# Coding Standards and Best Practices

Since we are a method development group, we need to produce *production level* code. If you have not had much experience with any of the following, it is worth the time investment to get up to speed - not only so your project will be more stable, usable and maintainable, but **to give you skills that future employers want to see, guaranteed.**

## Paradigm: OOP

Object-oriented programming (OOP) powerful and the standard in production development environments. Reminder of the main features and benefits below. Ensure you are integrating the all of the following principles in your code!

- **Modularity and encapsulation:** OOP allows you to break down complex programs into smaller, more manageable pieces, or modules. Each module can have its own data and functionality, making it easier to work on and test in isolation. Encapsulation ensures that the internal workings of a module are hidden from other parts of the program, reducing the likelihood of bugs and making it easier to modify and update the code.
- **Code reuse:** OOP encourages the creation of reusable code, which can save time and effort in the long run. Objects can be created once and used multiple times, reducing the amount of code that needs to be written and maintained.
- **Inheritance and polymorphism:** OOP allows you to create new objects based on existing objects, through inheritance. This can save time and effort in creating new code, and also helps ensure consistency across the program. Polymorphism allows objects to take on multiple forms, depending on the context, which can make code more flexible and extensible. This allows you to easily iterate on your tool development to test different features.
- **Abstraction:** OOP allows you to create abstract data types that capture the essence of a concept, without getting bogged down in implementation details. This can make code more understandable and easier to reason about.

## Typing and Commenting

It is important for your code to be readable and maintainable. Ensure you are integrating all of the following:

- **Commenting**
    - Use comments to explain why code is written in a certain way, not what the code is doing.
    - Avoid commenting every line of code, instead focus on complex or non-obvious sections of the code.
    - Use descriptive names for variables and functions to reduce the need for comments.
    - Use inline comments sparingly and only when necessary.
    - Use a consistent commenting style throughout the codebase.
- **Typing**
    - Use type hints for function arguments, return values, and class attributes.
    - Use Union types to indicate that a function can accept multiple types of arguments.
    - Use Optional types for arguments that are not required.
- **Docstrings**
    - Use docstrings to describe the purpose and behavior of functions and classes.
    - Follow the [Google style guide](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) for writing docstrings.
    - Start the docstring with a one-line summary of the function or class.
    - Include information on function arguments, return values, and exceptions that can be raised.
    - Include examples of how to use the function or class.

💡

Circular imports can be a problem when typing. Use the following methods to avoid it:

1. Using the following type check

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from models import Book
```

1. Use `from __future__ import annotations` and **put your typing in single quotes**

## Branches and collaborating using Git and Github

### Repo organization

Most tools we develop should have the primary repo be on the [algo-cancer](https://github.com/algo-cancer?tab=repositories) account, and then you will fork this repo to your own account. When you want to push releases or changes, you submit a pull request to the algo-cancer repo.

### Branch Naming Conventions
- Use descriptive, hyphen-separated names that clearly indicate the purpose of the branch
- Follow the pattern: `type/description-of-change`
- Common types include:
  - `feature/` - For new features
  - `bugfix/` - For bug fixes
  - `hotfix/` - For urgent fixes to production
  - `docs/` - For documentation updates
  - `refactor/` - For code restructuring
  - `test/` - For adding or updating tests
- Examples:
  - `feature/add-user-authentication`
  - `bugfix/fix-memory-leak`
  - `docs/update-installation-guide`

### Branching Strategy (Git Flow)
Git Flow is our primary branching strategy. This creates a clean main that users see when initially visiting the repo, containing only release commits, while the actual development happens on the develop branch.

1. **Main Branches**
   - `main` (or `master`)
     - Production-ready code only
     - All commits tagged with version numbers
     - Never commit directly to main
   - `develop`
     - Primary development branch
     - Contains latest delivered development changes
     - Source branch for feature branches

2. **Supporting Branches**
   - `feature/*`
     - Branch from: `develop`
     - Merge back into: `develop`
     - Naming: `feature/description`
     - Used for new features and non-emergency fixes
     ```bash
     # Creating a feature branch
     git checkout develop
     git pull origin develop
     git checkout -b feature/new-feature
     
     # Completing a feature branch
     git checkout develop
     git merge --no-ff feature/new-feature
     git push origin develop
     git branch -d feature/new-feature
     ```

   - `release/*`
     - Branch from: `develop`
     - Merge back into: `main` AND `develop`
     - Naming: `release/version-number`
     - For release preparation and minor bug fixes
     ```bash
     # Creating a release branch
     git checkout develop
     git checkout -b release/1.2.0
     
     # Completing a release branch
     git checkout main
     git merge --no-ff release/1.2.0
     git tag -a v1.2.0
     git checkout develop
     git merge --no-ff release/1.2.0
     git branch -d release/1.2.0
     ```

   - `hotfix/*`
     - Branch from: `main`
     - Merge back into: `main` AND `develop`
     - Naming: `hotfix/version-number`
     - For urgent fixes to production code
     ```bash
     # Creating a hotfix branch
     git checkout main
     git checkout -b hotfix/1.2.1
     
     # Completing a hotfix branch
     git checkout main
     git merge --no-ff hotfix/1.2.1
     git tag -a v1.2.1
     git checkout develop
     git merge --no-ff hotfix/1.2.1
     git branch -d hotfix/1.2.1
     ```

3. **Branch Flow Overview**
   - Development of new features:
     1. Features branch off from `develop`
     2. When complete, merge back into `develop`
   - Creating a release:
     1. Branch off from `develop` to `release/x.x.x`
     2. When ready, merge into both `main` and `develop`
     3. Tag the merge in `main` with version number
   - Fixing production issues:
     1. Create `hotfix/x.x.x` from `main`
     2. When complete, merge into both `main` and `develop`
     3. Tag the merge in `main` with version number

4. **Key Principles**
   - No direct commits to `main`
   - `develop` branch always reflects latest delivered development changes
   - Feature branches should be short-lived
   - Release branches lock down versions and allow for last-minute fixes
   - Hotfix branches allow for emergency production fixes without disrupting development


### Push/Pull Best Practices
1. **Before Starting Work**
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feature/your-feature-name
   ```

2. **Regular Commits**
   - Commit frequently with clear, descriptive messages
   - Follow conventional commits format:
     ```
     type(scope): brief description
     
     Longer description if needed
     ```
   - Types: feat, fix, docs, style, refactor, test, chore

3. **Pushing Changes**
   ```bash
   git fetch origin main
   git rebase origin/main
   git push origin feature/your-feature-name
   ```

### Version Control Best Practices
1. **Semantic Versioning (SemVer)**
   - Format: MAJOR.MINOR.PATCH (e.g., 2.1.3)
   - MAJOR: Breaking changes
   - MINOR: New features, backward compatible
   - PATCH: Bug fixes, backward compatible

2. **Tags and Releases**
   ```bash
   git tag -a v1.0.0 -m "Version 1.0.0 release"
   git push origin v1.0.0
   ```

3. **Changelog Management**
   - Maintain a CHANGELOG.md file
   - Document all notable changes for each version
   - Include Added, Changed, Deprecated, Removed, Fixed sections

### Conflict Resolution
1. **Preventing Conflicts**
   - Regularly pull changes from main
   - Keep branches focused and short-lived
   - Communicate with team members about file changes

2. **Resolving Conflicts**
   ```bash
   # Method 1: Rebase
   git fetch origin main
   git rebase origin/main
   # Resolve conflicts in each commit
   git add .
   git rebase --continue

   # Method 2: Merge
   git fetch origin main
   git merge main
   # Resolve conflicts
   git add .
   git commit -m "Merge main and resolve conflicts"
   ```

3. **Best Practices for Conflict Resolution**
   - Understand both versions of the code before resolving
   - Consult with team members when necessary
   - Use visual merge tools (e.g., VS Code, GitKraken)
   - Test thoroughly after resolving conflicts
   - Document complex conflict resolutions in commit messages

### Pull Request Guidelines
1. **Creating PRs**
   - Use descriptive titles
   - Fill out PR template completely
   - Link related issues
   - Add appropriate labels
   - Request relevant reviewers

2. **PR Description Template**
   ```markdown
   ## Changes
   - Detailed list of changes

   ## Testing
   - Test cases covered
   - How to test

   ## Screenshots
   (if applicable)

   ## Checklist
   - [ ] Tests added/updated
   - [ ] Documentation updated
   - [ ] Code follows style guidelines
   - [ ] All checks passing
   ```

3. **Review Process**
   - Respond to feedback promptly
   - Make requested changes in new commits
   - Squash commits before merging
   - Use "Squash and merge" for clean history

### GitHub Actions and CI/CD
- Set up automated tests
- Configure linting checks
- Implement automatic version bumping
- Create deployment workflows
- Add security scanning


## Testing

WIP

## Docs

WIP