# Git and Github

## Github Notifications

Add your work email address to github so that relevant notifications don't go to your personal inbox:

1. Add your work email address at the [top of this page](https://github.com/settings/emails)
2. Select your company email address for `nerevu` in the `Custom Routing` section at the [bottom of this page](https://github.com/settings/notifications)
3. That's it! You rock ;)

## Basic Git Flow

When working with a Nerevu repository, it is important that you create your own branch for updates, fixes, or changes. When a change needs to be implemented, you simply commit and push your changes to your own branch and then submit a pull request to request merging your branch with origin. This prevents your changes from adversely affecting production code.

1. Clone repository

```bash
git clone https://github.com/nerevu/<REPO_NAME>.git
```

2. Create your branch to add your fix/feature

```bash
git checkout -b <BRANCH_NAME>
```

3. Prettify your code

*Python*

```bash
manage prettify
```

*JavaScript*

```bash
npm run prettify
```

4. Push your commits to Github and create a [Pull Request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)

```bash
git commit -m "<COMMIT_MESSAGE>"
git push --set-upstream origin <BRANCH_NAME>
```

*NOTE: As a general rule, don't merge code that purposefully raises errors and kills the application. Talk to Reuben about how to gracefully handle situations where you want to throw an error.*

If master changes while you are fixing a pull request, rebase to master (please ask questions if you do not feel confident rebasing)

```bash
git fetch origin
git rebase origin/master
```

Fix conflicts and force push to YOUR BRANCH on Github.

```bash
git push -f origin <BRANCH_NAME>
```

**Never force push anything to `MASTER`! If you feel you must, speak with Reuben in great detail first!**

## Project Management

### Work Backlog

You can find issues that are being worked at the [Nerevu Group Github Projects page](https://github.com/orgs/nerevu/projects). If you click on the project you are working on, you will find a Kanban Board with all the issues (some of them have milestones with due dates).

### Kanban Board

When you start working on a new issue, ensure you immediately do the following to keep the Kanban Board up to date:

1. Create a [Pull Request (PR)](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request) and select what `Project` it belongs to. This automatically adds your PR to the `In Progress` column on the Kanban Board. The `Assignees`, `Labels`, and `Milestone` sections are only needed for issues (the issue needs `Project` as well.

2. Go to the Kanban Board for the respective project and move the related issue from the `To Do` column to the `In Progress` column.

3. Once done fixing the issue, rebase to squash the work into one commit `git rebase -i` and add the words `(closes <ISSUE_NUMBER>)` to the commit message. This will automatically close the corresponding issue once your PR gets merged.

4. When your PR is merged into master, both the Issue and the PR in the Kanban Board will automatically move to the `Done` column.

## Misc

### Commit Messages

Your commit message should adhere to the following:

- be no more than 50 characters
- give a descriptive summary of the work accomplished
- begin with a present tense verb
- use sentence case

Examples:

```text
Remove unused attributes
Refactor widgets
Update models and db initialization
```

If your commit fits one of the following categories, add the appropriate prefix.

- `[FIX]`: Fixes a bug
- `[CHANGE]`: Changes existing behavior or external API
- `[ENH]`: Enhances existing behavior
- `[NEW]`: Adds new behavior
- `[CVE]`: Patches a security vulnerability

Examples:

```text
[FIX] Set default cache timeout
[CHANGE] Update college data model
[ENH] Optimize data fetching
[NEW] Make number of months configurable
[CVE] Patches elliptic CVE-2020-13822
```

If your commit fixes an existing issue, add the suffix `(closes <ISSUE_NUMBER>)`.

Example:

```text
[ENH] Standardize number format (closes #10)
```

### Commit Frequency

You should be committing your code and pushing to GitHub often so that your changes are always available for others to see. Even if the commits don't have completed code, still commit them. You should strive to have GitHub up-to-date with your local branch at all times. Once you are ready to merge your code into the master branch, rebase your commits into more logical groups. For example:

1. All commits that fixed an issue should be grouped into one commit.
2. Other code changes that are not related to an issue should be grouped into their specific commit tasks.

### When to Rebase

There are several times that you should consider rebasing in Git.

1. Periodically to the master branch to make sure your code doesn't break the new changes to the master branch.
2. When you make a change that a reviewer requested in a PR.
3. Any other time that makes sense (you'll learn this over time)

### How to Rebase

Rebasing is necessary whenever history needs to be changed. This can be for changing commit messages or for changing the order and contents of commits. For example, if you want to change commit messages follow these steps:

1. Run `git rebase -i master`
2. Change the `pick` field to `edit` next to the commit you intend to change
3. Save the file
4. Run `git rebase --continue`
5. This will reopen the rebase file where you can change the commit text and exit to save

Similarly, you can edit the contents of the files themselves when under edit mode. After step 2 above, the local file system will reflec the changes of the selected commit. Here you can change the files using a text editor and then recommit them like so:

1. Run `git rebase -i master`
2. Change the pick field to `edit`
3. Make changes to the file system
4. Run `git status` to see edited files
5. Run `git add .` followed by `git commit --ammend`
6. Complete the rebase using `git rebase --continue`

This will effectively change the edits included in that specific commit.

On the other hand, if you want to erase a commit entirely you can simply comment out the commits you wish to erase using `#` and then continue the rebase. This will edit the commit history to erase any commits you erase.

### .gitignore File

- don't commit `cache` folders and files (add them to `.gitignore`)
