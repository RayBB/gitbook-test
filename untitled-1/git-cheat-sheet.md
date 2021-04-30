# Git-Cheat-Sheet

`Git` adds/changes its feature-set regularly, so make sure to keep it up-to-date! These notes created with `git 2.22`.

Contents:

* [Setting Up](git-cheat-sheet.md#setting-up)
* [Creating a Pull Request](git-cheat-sheet.md#creating-a-pull-request)
* [Making Updates to Your Pull Request](git-cheat-sheet.md#making-updates-to-your-pull-request)
* [Commit History Manipulation](git-cheat-sheet.md#commit-history-manipulation)
* [Resolving Rebase Conflicts](git-cheat-sheet.md#resolving-rebase-conflicts)

## Setting Up

1. Fork the Open Library repository using the GitHub UI by logging in to Github, going to [https://github.com/internetarchive/openlibrary](https://github.com/internetarchive/openlibrary) and clicking the Fork button in the upper right corner: ![GitHub Fork](https://archive.org/download/screenshot20191211at11.12.56/fork.jpg)
2. Clone your fork to your local machine:

```bash
git clone git@github.com:USERNAME/openlibrary.git
git remote add origin https://github.com/USERNAME/openlibrary.git
```

1. Add 'upstream' repo to list of remotes

```bash
git remote add upstream https://github.com/internetarchive/openlibrary.git
```

1. Verify the new remote named 'upstream'

```bash
git remote -v
```

## Creating a Pull Request

1. Make sure master is up-to-date:

```bash
git checkout master
git pull upstream master
```

1. [Create a new branch for the feature or issue you plan to work on](https://github.com/internetarchive/openlibrary/blob/master/CONTRIBUTING.md#development-practices) and check it out.

```bash
git checkout -b 1234/fix/fix-the-thing
```

\(specifying `-b` creates a new branch, and `checkout` checks it out\).

1. Make changes/commit:

```bash
git add the-file.html
git commit
```

A commit message should answer three primary questions;

* Why is this change necessary?
* How does this commit address the issue?
* What effects does this change have?
* Push the branch:

```bash
git push origin HEAD
```

\(note HEAD refers to your current branch; so make sure you're on the right branch!\)

1. Test your changes:

```bash
docker-compose exec web make test
```

1. Go to [https://github.com/internetarchive/openlibrary/pulls](https://github.com/internetarchive/openlibrary/pulls) and make new pull-request from branch in your forked repository and provide the information requested in the template.

   ![GitHub pull request](https://archive.org/download/screenshot20191211at11.12.56/pull-request.png)

Your code is now ready for review!

## Making Updates to Your Pull Request

Pull requests often receive feedback; to make changes to your existing pull requests:

1. Make sure your branch is up-to-date with master

```text
git checkout master
git pull upstream master
```

1. Rebase your branch onto master

```text
git checkout 1234/fix/fix-the-thing
git rebase master
```

| Info |
| :--- |
| Rebasing is the equivalent of "lifting" all the commits in your branch, and placing them on top of the latest master. It effectively changes the _base_ of your branch/commits. |

| Info |
| :--- |
| Sometimes there will be changes in the master branch to the same lines in your branch. This results in a conflict, because `git` can't decide which changes to use. See [Resolving rebase conflicts](git-cheat-sheet.md#resolving-rebase-conflicts). |

1. Make your edits and commit \(same as steps 3 in [Creating a Pull Request](git-cheat-sheet.md#creating-a-pull-request)\).
2. Push your changes up.

```text
git push origin HEAD
```

**Note**: If the master rebase pulled in new changes, you will have to _force push_: `git push origin HEAD -f`.

| Info |
| :--- |
| Force pushing _replaces_ the commits on the remote branch with the commits on your local branch. Non-force pushing just adds new commits. Whenever you perform a rebase, you will have to force push to your branch. |
| You should only force push if working on one of your own branches. If working on a branch which other people are also pushing to, force pushing is dangerous because it can override others' work. In that case, use `--force-with-lease`; this will force push _only_ if someone else hasn't made any changes to the branch. |

## Commit History Manipulation

**WARNING:** You can cause yourself some headaches with this feature! But it's easily one of the most powerful things about using `git`, so it's worth learning :\)

Sometimes you'll want to rearrange/reword/combine commits to keep the history neat. To do this, on your branch, run:

```text
git rebase -i master
```

| Info |
| :--- |
| The `-i` is for interactive. The command is also specified often as something like `git rebase -i HEAD~2`. `HEAD` refers to the current, latest commit in your branch; `~2` goes back 2 in the history, so you'll be manipulating the last 2 commits. `git rebase -i master` lets you manipulate all the commits on your branch. |

| Info |
| :--- |
| By default, `git` will open up an editor in your terminal \(likely `vim`\). If you would rather use VS Code, run `git config --global core.editor "code"` once, and then `git` will always use VS Code when prompting for a rebase, or a commit message. |
| If you happen to find yourself stuck in `vim` and don't know how to get out, press `ggdG:wq` \(in order: `g` for "go to", `g` for "top of file", `d` for delete, `G` for "to bottom of file". So `ggdG` is for "go to the top of the file, and delete everything". This is how you cancel `git rebase -i`. Then: `:` for "enter command-line mode", `w` for "save", `q` for "quit". So `wq` is "save and quit". If you're interested in learning more about `vim`, see [https://vim.fandom.com/wiki/Tutorial](https://vim.fandom.com/wiki/Tutorial) |

This will open a text editor, and let you edit all the commits that your branch has. It will look something like this:

```text
pick eb8ab51 Made footer's HTML translatable
pick a18d382 Fixed some typos
pick 76b9883 Made footer's HTML translatable: added the missing translatable strings
pick 73c78b7 Fix git hash version i18n
pick 377e121 Added a missing translatable string
pick caf9507 Reverted unrelated changes to the PR
pick 23961be Clean up trailing whitespace

# Rebase ef7d551..23961be onto ef7d551 (7 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
```

If you decide you want to cancel the rebase, delete everything, and then save. That tells `git` to do nothing.

To continue with the rebase, save the file. `git` will then replay all the instructions/commits in that file. If there is a conflict, it will pause to let you fix them. See [Resolving rebase conflicts](git-cheat-sheet.md#resolving-rebase-conflicts).

## Resolving Rebase Conflicts

Sometimes when you `rebase` your branch, you will get conflicts! This happens when the master branch edited some of the same lines that your branch has edited, and `git` can't determine which changes to use. So it asks you to decide :\)

If it hits a conflict during the rebase, it will stop, and let you fix it. I find fixing conflicts in VS Code to work well. In VS Code, open up the Source Control panel \(Ctrl-Shift-G\). Conflicts will be shown in red at the top with a "C" next to them. Click on the file to find and resolve the conflicts. Once you've resolved them all, press the "+" button next to the file in the source control panel; this is the same as running `git add FILE`. Once you've resolved all the conflicts, run `git rebase --continue` to continue the rebase.

If the conflicts look too much, and you want to abandon the rebase and go back to where your code was before, run `git rebase --abort`.

## References

* Getting Started flow roughly based on [https://gist.github.com/Chaser324/ce0505fbed06b947d962](https://gist.github.com/Chaser324/ce0505fbed06b947d962)

