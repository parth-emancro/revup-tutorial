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
   This is a placeholder branch that you should rebase on main (e.g. with `git rebase origin main && git rebase main`) as needed since this will act as the base/main branch for revup development.
   If this method is not used, it's harder to work on different machines using a revup workflow since the local version of `main` can go out of sync across machines.

**IMPORTANT**: With `revup` usage, please ONLY `git push -u origin <your-git-username>/dev` after doing `git rebase origin main` , `git checkout <your-git-username>/dev`, and `git rebase main`. We don't want to push revup commits upstream since they represent PR's.

5. Now you can follow the `Tutorial` section of [Skydio/revup](https://github.com/Skydio/revup), however when doing `git commit`, format your commit body as

```
[component] Short description of change

- Change 1
- Change 2
...
- [try to follow these guidelines for wording good commits](https://www.freecodecamp.org/news/git-best-practices-commits-and-code-reviews/)

Topic: change-foo
Relative-branch: <your-git-username>/dev
```

Note that the last two lines of the commit are what tell `revup` how to structure our Pull Request.
For a PR we want to target for merging into `main`, we need to use `Relative-branch`, however stacked PR's, which can be of the following form, will need to specify `Relative: <name-of-relative-topic>`.

```
main <------ change-foo <------- change-bar
```

The advantage of this kind of setup is that you can easily use `revup amend` and `revup restack` to painlessly rebase all your PR's according to an updated `main` or relative topic branch.

To have your PR be based on someone else's PR branch, you can specify `Relative-branch: <specific-branch>` which is basically a more specific way to say what PR we want ours to be relative to.
