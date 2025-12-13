# Git Commit Message Guidelines

## Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

## Type

Must be one of the following:

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **build**: Changes that affect the build system or external dependencies (example scopes: npm, vite, package.json)
- **ci**: Changes to CI configuration files and scripts
- **chore**: Other changes that don't modify src or test files
- **revert**: Reverts a previous commit

## Scope

The scope should be the name of the component/module affected (as perceived by the person reading the changelog):

- **app**: Changes to App.svelte
- **section**: Changes to Section.svelte component
- **notification**: Changes to Notification.svelte component
- **mermaid**: Changes to Mermaid integration
- **build**: Changes to build configuration
- **deps**: Changes to dependencies

## Subject

The subject contains a succinct description of the change:

- Use the imperative, present tense: "change" not "changed" nor "changes"
- Don't capitalize the first letter
- No dot (.) at the end
- Maximum 72 characters

## Body

Just as in the subject, use the imperative, present tense: "change" not "changed" nor "changes".
The body should include the motivation for the change and contrast this with previous behavior.

## Footer

The footer should contain any information about **Breaking Changes** and is also the place to reference GitHub issues that this commit closes.

**Breaking Changes** should start with the word `BREAKING CHANGE:` with a space or two newlines.

## Examples

### Feature

```
feat(section): add ability to duplicate sections

Add a duplicate button to section header that creates a copy
of the section with all its tasks.

Closes #123
```

### Bug Fix

```
fix(section): correct button alignment in section header

Buttons in section header were not aligned properly with the
section name input. Added align-items: center to fix the layout.
```

### Documentation

```
docs: add git commit message guidelines

Create comprehensive commit message guidelines following
conventional commits specification for better changelog
generation and commit history clarity.
```

### Build Changes

```
build(deps): update mermaid to version 11.13.0

Update Mermaid.js to latest version for bug fixes and
improved Gantt chart rendering.
```

### Refactoring

```
refactor(app): simplify mermaid code generation logic

Extract code generation logic into separate function for
better maintainability and testability.
```

## Best Practices

1. **Keep commits atomic**: Each commit should represent a single logical change
2. **Write meaningful messages**: Future you (and others) should understand what and why
3. **Reference issues**: Always reference the issue number when applicable
4. **Test before commit**: Ensure your changes work and don't break existing functionality
5. **Keep the subject line short**: Aim for 50 characters or less
6. **Separate subject from body**: Use a blank line between subject and body
7. **Use the body to explain what and why**: Not how (the code shows how)

## Commit Message Template

You can set up a commit message template:

```bash
git config commit.template .gitmessage
```

Create `.gitmessage` file:

```
# <type>(<scope>): <subject> (Max 72 char)
# |<----  Using a Maximum Of 72 Characters  ---->|


# Body: Explain *what* and *why* (not *how*). Wrap at 72 chars.
# |<----   Try To Limit Each Line to a Maximum Of 72 Characters   ---->|


# Footer: Reference issues, breaking changes
# BREAKING CHANGE: 
# Closes #


# --- COMMIT END ---
# Type can be
#    feat     (new feature)
#    fix      (bug fix)
#    refactor (refactoring code)
#    style    (formatting, missing semi colons, etc; no code change)
#    docs     (changes to documentation)
#    test     (adding or refactoring tests; no production code change)
#    build    (changes to build system or dependencies)
#    ci       (changes to CI configuration)
#    chore    (updating grunt tasks etc; no production code change)
#    perf     (performance improvements)
#    revert   (revert a previous commit)
# --------------------
# Remember to
#   - Use the imperative mood in the subject line
#   - Do not end the subject line with a period
#   - Separate subject from body with a blank line
#   - Wrap the body at 72 characters
#   - Use the body to explain what and why vs. how
#   - Reference issues and PRs in the footer
# --------------------
```

## References

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Angular Commit Guidelines](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
