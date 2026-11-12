---
name: dependency-checker
description: "Analyze Java Maven dependencies for version conflicts, outdated libraries, transitive dependencies, known security vulnerabilities, and compatibility risks. Use when reviewing a pom.xml, investigating dependency issues, or planning safe dependency updates."
---

# Dependency Checker

Analyze dependencies in Java Maven projects and recommend safe, minimal changes.

## Workflow

1. **Understand the project**
   - Find the root `pom.xml` and any module POMs.
   - Identify the Java version, Spring Boot version (if used), Maven parent, imported BOMs, and `dependencyManagement` entries.
   - Distinguish direct dependencies from transitive dependencies, and note their scopes.

2. **Inspect the resolved dependency graph**
   - Read the POM files first.
   - When a terminal is available and command execution is permitted, use the project's Maven wrapper (`./mvnw`) if present, otherwise `mvn`:
     ```bash
     ./mvnw dependency:tree -Dverbose
     ```
   - In multi-module projects, check affected modules individually when useful.
   - Identify duplicate artifacts, omitted versions, dependency mediation, unexpected transitive dependencies, and conflicting versions.
   - Explain *which direct dependency introduces* a problematic transitive dependency.
   - Do not assume that different versions mentioned in a tree are all present on the final runtime classpath.

3. **Check available updates**
   - If permitted and network/repository access is available, run:
     ```bash
     ./mvnw versions:display-dependency-updates
     ```
   - Check Maven plugins, version properties, and parent/BOM updates when relevant.
   - Separate patch, minor, and major updates where possible.
   - Do not recommend upgrading a Spring Boot-managed dependency independently without checking its BOM compatibility.
   - Verify compatibility with the project's Java version, framework version, and relevant library APIs.

4. **Check known vulnerabilities**
   - Prefer existing project security reports or an already configured scanner, such as OWASP Dependency-Check, Dependabot, or an organization-approved tool.
   - If the project has OWASP Dependency-Check configured and execution is permitted, use its `check` goal or an existing CI report.
   - Do not add a new scanning plugin, download external tools, or modify build configuration without permission.
   - For each confirmed finding, report the affected artifact and version, advisory/CVE identifier, source, severity (when provided), and known fixed version.
   - Do not invent CVEs, fixed versions, or compatibility claims. If no trusted scan or advisory is available, say that vulnerability status was not verified.
   - Treat automated findings as leads: account for false positives, affected version ranges, and real-world applicability.

5. **Recommend fixes**
   - Prioritize confirmed security issues, broken builds/runtime conflicts, and compatibility problems over cosmetic version updates.
   - Prefer minimal, supported fixes: upgrade the owning direct dependency, update the parent/BOM, or change dependency management as appropriate.
   - Propose exclusions or explicit version overrides only with a clear explanation of why they are needed and what could break.
   - State which tests or verification steps should follow any update.
   - Never modify `pom.xml`, upgrade dependencies, or commit changes unless the user explicitly asks.

## Output

Keep the report concise and actionable:

1. **Summary:** project/module checked and main findings.
2. **Findings:** dependency, resolved version, issue, evidence/source, and priority.
3. **Recommended changes:** exact dependency or BOM to update and the reason; mention compatibility risks.
4. **Verification:** Maven commands and tests to rerun.
5. **Limitations:** checks that could not be performed, especially unavailable vulnerability data or network access.

If no meaningful issues are found, say so. Never imply that a project is vulnerability-free solely because no findings were returned.

