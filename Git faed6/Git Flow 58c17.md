# Git Flow

# What is git

- Collaboration

# What is git flow

- Set of guidelines for developers using version control
- Referred to as a Branching Model

- Not rules but only guide

## How it works

- Central repository
- `master`, `develop` is for recording project history
- `master` is representing the official release history

## Master

- Contains the latest version

## Develop branch

- Unstable

---

![Untitled.png](Git%20Flow%2058c17/Untitled.png)

## Feature branch

- Merge into develop when it is tested by developer.

## Release branch

- Limited amount of features
- A Senior developer Forked develop
- Should be tested by QA (Quality Assurance) test team
- Commits are only bug fixes
- Could be deployed on staging server
- At the end it will be merged into `develop` & `master`
- Tagging the merge commit in the master branch as official release

## Hotfix branch

- Very minor fixes
- Fork `master` and then merge into `master` again

---

# Git flow commnad

```bash
$ git flow -h
usage: git flow <subcommand>

Available subcommands are:
   init      Initialize a new git repo with support for the branching model.
   feature   Manage your feature branches.
   bugfix    Manage your bugfix branches.
   release   Manage your release branches.
   hotfix    Manage your hotfix branches.
   support   Manage your support branches.
   version   Shows version information.
   config    Manage your git-flow configuration.
   log       Show log deviating from base branch.

Try 'git flow <subcommand> help' for details.
```

## Git flow initializing

```bash
$ git flow init
No branches exist yet. Base branches must be created now.
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Bugfix branches? [bugfix/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
Hooks and filters directory? [C:/Users/Amin/test/.git/hooks] 

$ git branch
  develop
* master
```

## Feature branches as an example

```bash
$ git flow feature -h
usage: git flow feature [list] [-h] [-v]

    Lists all the existing feature branches in the local repository.

    -h, --help            Show this help
    -v, --verbose         Verbose (more) output
```

To list the feature branches

```bash
$ git flow feature list
No feature branches exist.

You can start a new feature branch:

    git flow feature start <name> [<base>]
```

To create new feature branches

```bash
$ git flow feature start new_feature
Switched to a new branch 'feature/new_feature'

Summary of actions:
- A new branch 'feature/new_feature' was created, based on 'develop'
- You are now on branch 'feature/new_feature'

Now, start committing on your feature. When done, use:

     git flow feature finish new_feature

$ git branch
  develop
* feature/new_feature
  master

$ git flow feature list
* new_feature
```

To finish and merge a feature to develop branch

```bash
$ git checkout develop
Switched to branch 'develop'

$ git flow feature finish new_feature
Already on 'develop'
Updating 60833e6..4191553
Fast-forward
 amin | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 amin
Deleted branch feature/new_feature (was 4191553).

Summary of actions:
- The feature branch 'feature/new_feature' was merged into 'develop'
- Feature branch 'feature/new_feature' has been locally deleted
- You are now on branch 'develop'
```