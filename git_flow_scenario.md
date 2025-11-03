# Git Flow: A Step-by-Step Guide

Git Flow is a branching model for Git that provides a structured approach to software development, particularly well-suited for projects with scheduled release cycles or those requiring support for multiple versions of software. It was first published by Vincent Driessen and has been widely adopted for its robust framework in managing larger projects.

While some modern continuous delivery practices favor simpler workflows like Trunk-Based Development or GitHub Flow, Git Flow remains a valuable model for specific project needs.

## Key Branches in Git Flow

Git Flow revolves around five main types of branches, each with a specific role:

1.  **`main`**: This branch stores the official release history and always reflects production-ready code. Direct modifications to `main` are generally avoided to maintain its integrity.
2.  **`develop`**: This serves as an integration branch for features. All feature branches are merged into `develop`, and it acts as the basis for release branches. It contains the complete history of the project, while `main` contains an abridged version.
3.  **`feature` branches (`feature/*`)**: These branches are used for developing new features. They branch off from `develop` and are merged back into `develop` once the feature is complete. Features should never interact directly with `main`.
4.  **`release` branches (`release/*`)**: Created from `develop` when enough features are ready for a new release. This branch is used for bug fixes, documentation generation, and other release-oriented tasks. No new features are added to a `release` branch. Once ready, it's merged into `main` and `develop`, and tagged with a version number.
5.  **`hotfix` branches (`hotfix/*`)**: Used to quickly patch critical bugs in production releases. Hotfix branches are based on `main` and are merged back into both `main` and `develop` once the fix is complete, and `main` is tagged.

## Git Flow Workflow Steps

Here's a step-by-step breakdown of the Git Flow workflow:

### 1. Initialization

To start using Git Flow in a repository, you first need to initialize it. This typically involves creating the `develop` branch from `main`.

*   **Create `develop` branch**:
    ```bash
    git branch develop
    git push -u origin develop
    ```
*   **Using `git-flow` extension**: If you have the `git-flow` extension installed, you can initialize it, and it will prompt you for branch names.
    ```bash
    git flow init
    ```
    (When prompted for the production branch name, use `main`. For the development branch, use `develop`.)

### 2. Feature Development

Each new feature should be developed in its own dedicated branch.

*   **Start a new feature**:
    ```bash
    # Without git-flow extension
    git checkout develop
    git checkout -b feature/my-new-feature
    
    # With git-flow extension
    git flow feature start my-new-new-feature
    ```
*   **Work on the feature**: Commit changes regularly to your feature branch.
*   **Finish a feature**: Once the feature is complete and tested, merge it back into `develop`. It's best practice to use pull requests for code review before merging.
    ```bash
    # Without git-flow extension
    git checkout develop
    git merge feature/my-new-feature
    git push origin develop
    git branch -d feature/my-new-feature # Delete local branch
    git push origin --delete feature/my-new-feature # Delete remote branch
    
    # With git-flow extension
    git flow feature finish my-new-feature
    ```

### 3. Release Management

When `develop` has accumulated enough features for a release, a `release` branch is created.

*   **Start a release**:
    ```bash
    # Without git-flow extension
    git checkout develop
    git checkout -b release/1.0.0
    
    # With git-flow extension
    git flow release start 1.0.0
    ```
*   **Prepare for release**: On the `release/1.0.0` branch, perform bug fixes, update documentation, and conduct final testing.
*   **Finish a release**: Once the release is stable, merge it into `main` and `develop`, and tag `main`.
    ```bash
    # Without git-flow extension
    git checkout main
    git merge release/1.0.0
    git tag -a 1.0.0 -m "Release 1.0.0"
    git checkout develop
    git merge release/1.0.0
    git push origin main develop --tags
    git branch -d release/1.0.0 # Delete local branch
    git push origin --delete release/1.0.0 # Delete remote branch
    
    # With git-flow extension
    git flow release finish 1.0.0
    ```

### 4. Hotfix Management

If a critical bug is found in the production version (`main`), a `hotfix` branch is created to address it immediately.

*   **Start a hotfix**:
    ```bash
    # Without git-flow extension
    git checkout main
    git checkout -b hotfix/fix-critical-bug
    
    # With git-flow extension
    git flow hotfix start fix-critical-bug
    ```
*   **Fix the bug**: Implement the fix on the `hotfix/fix-critical-bug` branch.
*   **Finish a hotfix**: Merge the hotfix into `main` and `develop`, and tag `main`.
    ```bash
    # Without git-flow extension
    git checkout main
    git merge hotfix/fix-critical-bug
    git tag -a 1.0.1 -m "Hotfix 1.0.1"
    git checkout develop
    git merge hotfix/fix-critical-bug
    git push origin main develop --tags
    git branch -d hotfix/fix-critical-bug # Delete local branch
    git push origin --delete hotfix/fix-critical-bug # Delete remote branch
    
    # With git-flow extension
    git flow hotfix finish fix-critical-bug
    ```

## Best Practices for Using Git Flow

*   **Consistent Branch Naming**: Adhere to a consistent naming convention for your branches (e.g., `feature/my-feature`, `release/1.0.0`, `hotfix/bug-fix`).
*   **Small, Frequent Commits**: Keep your commits small and focused on a single logical change.
*   **Regular Syncing**: Regularly merge changes from `develop` into your `feature` branches to stay up-to-date and minimize merge conflicts.
*   **Code Review and Testing**: Always review and test code before merging, especially into `develop` and `main`. Utilize pull requests for this purpose.
*   **Automate Workflow**: Consider using the `git-flow` extension or other tools to automate branching and merging processes.

This document outlines the core principles and steps of the Git Flow workflow, providing a solid foundation for its implementation in your projects.
