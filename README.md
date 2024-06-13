# Code Review Workflow

## Setup Your Personal `.gitignore`

Everyone is allowed and encouraged to setup their development environment with the tools they prefer. Some tools and environments, however, generate files specific to their functioning, that aren't part of the repo being worked on. To prevent such files from accidentally being committed to repos, and rather than trying to account for every possible dev environment within individual repos' `.gitignore` files, developers should configure a [personal global `.gitignore`](https://stackoverflow.com/questions/7335420/global-git-ignore) that accounts for their specific choice of tools.

Broadly, the separation between repo vs personal `.gitignore`s is:

* **repo**: Anything generated as part of processes inherent to the repo that will appear across any/all environment/s, regardless of the choice of dev tools, e.g. build/test artifacts.
* **personal**: Anything generated as a result of the developer's choice, that will not always appear across all environments, e.g.:

   ```gitignore
   .DS_store # unique to macOS
   .idea
   .vscode
   .sublime-workspace
   ```

## Start Your Work

Before starting any work, create a new branch off of the `main` branch.

It is good practice to give the branch a name that is relevant to the work being done.

E.g. `fix-cli-printing-bug`, `add-component-of-feature`, etc.

1. Switch to the main branch

   ```shell
   git switch main
   ```

1. Update the main branch

   ```shell
   git pull
   ```

1. Create a new working branch

   ```shell
   git switch -c WORKING_BRANCH
   ```

## Save Your Work

Feel free to commit and push changes as often as you'd like to save any progress you've made.

As a general rule, try to make your commits as small and logical as possible. Leaving a trail of your thoughts for reviewers to read is the goal.

1. Add and commit changes

   ```shell
   git add FILES
   git commit
   ```

1. Push changes to GitHub

   ```shell
   git push
   ```

## Review Your Code

1. Commit and pushed any changes you want reviewed.

   ```shell
   git add FILES
   git commit
   git push
   ```

1. Go to your repository's GitHub page.
1. [Create a Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
1. Add reviewers (GitHub might make reviewer suggestions, add those as well).
1. Wait for comments and requests.

## Address Comments

Reviewers will undoubtedly have comments and/or make requests for your changes.

For each request, add a new commit to address that request, then push your new commits.

Every time you push, the Pull Request will be automatically updated and reviewers notified.


> [!TIP]
> You do not need to push each commit. Making several commits and pushing once is fine.


> [!IMPORTANT]
> Do **NOT** rebase or amend commits, i.e. no force pushes please.


## Merge Your Code

Once you have gotten enough approvals, use the *Squash and Merge* button to prepare your changes for merge.

> [!WARNING]
> Do **NOT** merge _other_ people's Pull Requests.


The _Squash and Merge_ button will present you with a text box to write a merge title and message.
Check that the merge title and message match your PR title and message.


> [!TIP]
> Make your commit message informative and helpful.


General rules for commit messages:

* For bug fixes, explain why this change fixes the bug.
* For feature work, explain how this part of the feature is supposed to work.

If you'd like to read more about writing good commit messages check out this [blog post](https://chris.beams.io/posts/git-commit/).

## Start Over

Now that you're finished, it's time to get back to work!

To start new work, follow the same steps as before:

1. Switch back to the main branch

   ```shell
   git switch main
   ```

1. Pull the latest changes

   ```shell
   git pull
   ```

1. Create a new branch for your work

   ```shell
   git switch -c NEW_WORKING_BRANCH
   ```

> [!NOTE]
> If you get a merge conflict or git tries to create a merge commit, see [below](#i-have-merge-conflicts)
 
Note, you do not need to do anything to your old branches, but if you do want to cleanup old Pull Request branches, checkout this [Stack Overflow answer](https://stackoverflow.com/a/6127884).

## Wait, But What If?

### Git won't let me pull main

If you are having trouble running `git pull` on the main branch, it may be that you accidentally made a commit on that branch before creating your working branch.

To fix this, create a "temporary" branch on your current main branch to make sure no commits are "lost".

```shell
git switch main
git branch USERNAME/save-main
git switch USERNAME/save-main 
```

Then re-checkout the main branch by deleting it and recreating it with `git switch`

```shell
git branch -D main
git fetch
git switch main
```

You can now safely delete your temporary branch

```shell
git branch -D USERNAME/save-main
```

### My branch and the main branch have diverged

As long as there are no merge conflicts (which GitHub will warn you about), there is nothing you need to do.

The _Squash and Merge_ button will handle the divergence like magic.

### I have merge conflicts!

Use Git's merge command to `merge` the main branch into your working branch, resolve all conflicts, and push.

```shell
git switch main
git pull

git switch WORKING_BRANCH
git merge main
```

Resolve merge conflicts

```shell
git commit
git push
```

### My branch is REALLY out of date

Same as above, use Git's `merge` command to update your working branch.

```shell
git switch main
git pull

git switch WORKING_BRANCH
git merge main

git push 
```

### My new work depends on my open Pull Request

In this case, create a branch off of your Pull Request branch and start working from that.

```shell
git switch PR_BRANCH
git pull

git switch -c NEW_WORKING_BRANCH
```

FYI, you can use a Draft Pull Request for work that is not yet ready.

### My new work depends on someone else's open Pull Request

This is identical to the solution above, except you'll need to use that user's Pull Request branch before creating your new branch.

```shell
git fetch
git switch COWORKERS_PR_BRANCH
git pull

git switch -c NEW_WORKING_BRANCH
```

Make sure to keep your branch up-to-date with changes from that user's Pull Request by periodically merging their branch into yours.

```shell
git pull origin COWORKERS_PR_BRANCH
```

### I'm halfway done, but now I'm stuck waiting for someone else's fix

This is identical to the last step of the solution above. You just pull the other user's Pull Request into an existing branch instead of creating a new one.

```shell
git pull origin COWORKERS_PR_BRANCH
```

### Someone else has pushed changes to my Pull Request

Sometimes it makes sense for a reviewer to push changes to your Pull Request. This is fine, but you will need to pull those changes locally before you will be allowed to push any of your changes.

With your working branch checked out, use the `pull` command with the remote and branch parameters to pull changes from your Pull Request branch.

```shell
git switch PR_BRANCH
git pull origin PR_BRANCH
```

## Useful Stuff

* [Git documentation](https://git-scm.com/doc)
* [Git basics](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
* [Git in Bash](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash)
* [GitHub desktop app](https://desktop.github.com/)
* [GitHub command line tool](https://cli.github.com/)
