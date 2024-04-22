This repo is meant to serve as a tutorial for common usage of https://github.com/Skydio/revup

#### Motivation

1. Make stacked PR's easy to rebase and modify.
2. Make it easier for people to create small PR's (<500LOC) which are easier and quicker to review.
3. Reduce the overhead it takes to maintain long PR chains, have automated PR comment updates to keep track of changes across multiple PR's. See PR's #1 and #2 for an example.

#### Setup

1. If your `git --version` shows up `<2.39.0`, follow these [instructions](https://askubuntu.com/questions/568591/how-do-i-install-the-latest-version-of-git-with-apt) to upgrade git.
2. Follow first time setup instructions over at https://github.com/Skydio/revup, it's also useful to skim over Tutorials+rest of the README.
3. Create a `~/.revupconfig` file to store the following configuration variables for revup -

```
[revup]
github_oauth = <your-personal-access-token>
main_branch = <your-git-username>/dev

[upload]
rebase = true
upload_pr_body = false

```

4. To practice revup features, fork this repo to your local, and create a branch based off of the main branch named `<your-git-username>/dev`, and `git push -u origin <your-git-username>/dev`.
   This is a branch that you should rebase on main using `git checkout main`, `git pull --rebase`, `git checkout <username>/dev`, `git rebase main`) as needed.
   If this method is not used, it's harder to work on different machines using a revup workflow since there wouldn't be a remote version of your working commits.

   How revup functions is that you keep adding new _commits_ on top of the commits in `main`, and each commit has a related PR that is created/updated using `revup upload` and to make changes, `revup amend <HEAD~nth-commit>` is used to amend the PR with staged changes.

5. Now you can follow the `Tutorial` section of [Skydio/revup](https://github.com/Skydio/revup), however when doing `git commit`, format your commit body as

```
[component] Short description of change

- Change 1
- Change 2
...
- [try to follow these guidelines for wording good commits](https://www.freecodecamp.org/news/git-best-practices-commits-and-code-reviews/)

Topic: change-foo
Relative-branch: <branch-name if you're basing your PR off of someone's feature branch or PR>
```

Note that the last two lines of the commit are what tell `revup` how to structure our Pull Request.
For a PR we want to target for merging into `main`, our revup config already specifies that, however stacked PR's, which can be of the following form, will need to specify `Relative: <name-of-relative-topic>`.

To have your PR be based on someone else's PR branch, you can specify `Relative-branch: <specific-branch>` which is basically a more specific way to say what PR we want ours to be relative to.

```
main <------ change-foo <------- change-bar

     <------ <other-users-PR-branch> <-------- change-abc(Relative-branch: <other-users-PR-branch>
```

The advantage of this kind of setup is that you are only rebasing **ONE branch**, which is your WIP branch, and when you do `revup upload`, all PR's represented by commits in that branch are updated.

6. BEFORE you merge a branch in, make sure you change the PR to reflect that you want to merge it into `main` and **NOT** into `<your-git-username>/dev. And prior to doing that, please follow the instructions given in the **IMPORTANT** disclaimer at step **4.B**.

We want to merge relative branches in sequence with the PR closest to `main` merging soonest, so that all PR's up the chain can be rebased.
