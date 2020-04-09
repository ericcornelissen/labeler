# Pull Request Labeler

Pull request labeler triages PRs based on the paths that are modified in the PR.

Note that only pull requests being opened from the same repository can be labeled.  This action will not currently work for pull requests from forks -- like is common in open source projects -- because the token for forked pull request workflows does not have write permissions.

## Usage

### Create `.github/labeler.yml`

Create a `.github/labeler.yml` file with a list of labels and [minimatch](https://github.com/isaacs/minimatch) globs to match to apply the label.

The key is the name of the label in your repository that you want to add (eg: "merge conflict", "needs-updating") and the value is the path (glob) of the changed files (eg: `src/**/*`, `tests/*.spec.js`)

#### Basic Examples

> Unchanged from [actions/labeler](https://github.com/actions/labeler).

```yml
# Add 'label1' to any changes within 'example' folder or any subfolders
label1:
  - example/**/*

# Add 'label2' to any file changes within 'example2' folder
label2: example2/*
```

#### Common Examples

> Unchanged from [actions/labeler](https://github.com/actions/labeler).

```yml
# Add 'repo' label to any root file changes
repo:
  - ./*

# Add '@domain/core' label to any change within the 'core' package
@domain/core:
  - package/core/*
  - package/core/**/*

# Add 'test' label to any change to *.spec.js files within the source dir
test:
  - src/**/*.spec.js
```

#### New Features

> These highlight new features compared to [actions/labeler](https://github.com/actions/labeler).

```yml
# Add a `new` label for any newly added files
new:
  - ./**/*
  - on: added

# Add a 'changes' label for any changed files
changes:
  - ./**/*
  - on: modified

# Add a 'removing' label for any removed files
removing:
  - ./**/*
  - on: removed

# Add an 'updates' label for any newly added or changed files
updates:
  - ./**/*
  - on:
    - added
    - modified
```



### Create Workflow

Create a workflow (eg: `.github/workflows/labeler.yml` see [Creating a Workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file)) to utilize the labeler action with content:

```
name: "Pull Request Labeler"
on:
- pull_request

jobs:
  triage:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/labeler@v2
      with:
        repo-token: "${{ secrets.GITHUB_TOKEN }}"
```

_Note: This grants access to the `GITHUB_TOKEN` so the action can make calls to GitHub's rest API_
