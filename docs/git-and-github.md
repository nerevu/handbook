# Git and Github

## Github Notifications

Add your work email address to github so that relevant notifications don't go to your personal inbox:

1. Add your work email address at the [top of this page](https://github.com/settings/emails)
2. Select your company email address for `nerevu` in the `Custom Routing` section at the [bottom of this page](https://github.com/settings/notifications)
3. That's it! You rock ;)

## Basic Git Flow

Clone repository

```bash
git clone https://github.com/nerevu/<REPO_NAME>.git
```

Create a branch to add your fix/feature

```bash
git checkout -b <BRANCH_NAME>
```

Prettify your code

*Python*

```bash
manage prettify
```

*JavaScript*

```bash
npm run prettify
```

Push your commits to Github and create a [Pull Request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)

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

*NEVER FORCE PUSH ANYTHING TO **MASTER**! IF YOU FEEL YOU MUST, TALK EXTENSIVELY TO REUBEN FIRST!*

## Project Management

### Work Backlog

You can find issues that are being worked at the [Nerevu Group Github Projects page](https://github.com/orgs/nerevu/projects). If you click on the project you are working on, you will find a Kanban Board with all the issues that need worked (many of them have milestones with due dates).

### Keeping the Kanban Board up to date

When you start working on a new issue and you create a new branch, you should immediately do the following to keep the Kanban Board up to date:

1. Commit your new branch to Github and add the words `(closes #{issue_number})` to the commit message. This will automatically close the corresponding issue in Github when you go to merge your code into master with a Pull Request (PR).
   ![Closes](https://user.fm/files/v2-d3f896981e40c790ac8fc6cb66a3e627/closes.png)

2. Create a PR immediately in Github and choose what `Project` it belongs to. This automatically adds your PR to the `In Progress` column on the Kanban Board. The `Assignees`, `Labels`, and `Milestone` sections are only needed for issues (the issue needs `Project` as well (right hand side of the image below).
   ![Projects](https://user.fm/files/v2-d3fe2b5084f7823c1c3fbe100fadfb3b/PR.png)

3. Go to the Kanban Board and move the correlated issue from the `To Do` column to the `In Progress` column.
   ![Kanban Board](https://user.fm/files/v2-4f9e2e3d18c03a7e48898b33fc1cd24a/kanban.png)

4. Once your PR is merged into master, both the Issue and the PR in the Kanban Board should automatically be moved to the `Done` column.

## Misc

### Commit Messages

If your commit fits one of the following categories, add the appropriate prefix.

- `[FIX]`: Fixes an existing bug
- `[CHANGE]`: Changes existing behavior or external API
- `[NEW]`: Adds new behavior

### Commit Frequency

You should be committing your code and pushing to GitHub often so that your changes are always available for others to see. Even if the commits don't have completed code, still commit them. You should strive to have GitHub up-to-date with your local branch at all times. Once you are ready to merge your code into the master branch, rebase your commits into more logical groups. For example:

1. All commits that fixed an issue should be grouped into one commit.
2. Other code changes that are not related to an issue should be grouped into their specific commit tasks.

### When to Rebase

There are several times that you should consider rebasing in Git.

1. Periodically to the master branch to make sure your code doesn't break the new changes to the master branch.
2. When you make a change that a reviewer requested in a PR.
3. Any other time that makes sense (you'll learn this over time)

### .gitignore File

- don't commit `cache` folders and files (add them to `.gitignore`)
