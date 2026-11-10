# Commit Message Instructions

Generate commit messages following the Conventional Commits format:

type(component): description

## Rules

- Use one of the following types: build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test.
- Always include a component (scope) that represents the affected part of the project.
- Use lowercase for the type and component.
- Write a concise, meaningful description in English using imperative mood.
- Do not include Jira ticket IDs.
- Do not include additional explanations or a commit body.
- Return only the commit message.

## Examples

fix(worker): prevent duplicate task creation
feat(api): implement task cancellation
refactor(client): simplify HTTP request handling
fix(config): increase job worker timeout
test(worker): add integration tests for task creation
chore(deps): update Camunda dependencies

