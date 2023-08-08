# My Notes from [The Git and GitHub Bootcamp](https://www.udemy.com/course/git-and-github-bootcamp/) Course

---

## Config

Set your username and email:

```
git config --global user.name “First name and last name”
git config --global user.email  username@domain.extension
```

To check:

```
git config user.name
git config user.email
git config --global --list
```

Use `git help` or `git help command_name`to get help!

## Getting and Creating Projects

`git init`: to instantiate a new git repository in the current directory. Make sure there are no existing .git folders found in the current directory or any of its subdirectories, using the following command which should not return any result:

`find . -type d -name ".git"`

To create a new repository with a specific project folder name, you can do:

`git init some_folder_name`: creates a folder with that name and creates a new repo there.

## Basic commands

`git status`: display the current state of the repository

`git log`: shows the list of commits. Press q to exit the log view.

`git log --oneline`: displays a condensed list of commits with each commit's abbreviated hash and commit message on a single line. Press q to exit.

`git add  filename1.ext1  filename2.ext2` : add one or more files to the staging area

`git add . ` : adds all modified and untracked files in the current directory to the staging area, preparing them to be included in the next commit.

`git commit`: opens the default editor for entering the commit message (recommended when commit message is longer than 50 chars). You can set your default editor to Vscode using git config --global core.editor "code --wait" and verify the change using git config --global core.editor. It should now display "code --wait."

`git commit -m “message”`: Enables you to include a commit message while creating a commit.

`git commit -am “message”`: add previously tracked file(s) that are modified into the staging area and commit in one line! Won’t work on untracked files that you just created after the last commit.

**Commit message best practices:**

A well-formed commit message typically consists of a short subject line (around 50 characters or less) written in the imperative mood (e.g., “fix bugs”; not “fixes bugs” or “fixed bugs”), summarizing the main change. Optionally, it may be followed by a more detailed body providing additional context and explaining the reasons for the change, using bullet points for lists if necessary. Including references to relevant issues or tickets is helpful, and it's crucial to stay consistent with the project's style (go by your organization’s guidelines for writing commit messages).

Example:

```
git commit -m "Add new feature for user authentication
# a blank line here is necessary!
- Implement user login and registration functionality
- Add password hashing for enhanced security
- Update user model to include additional fields
Resolves #123"
```

### Amending commits

Imagine a situation where you've just executed a commit using `git commit -m "Add all files"`, but inadvertently left out a particular file. As a result, this file was not incorporated into the commit. If you encounter this scenario, you have the option to correct it by making an amendment to the commit.

```
git add forgotten_file
git commit –amend
```

Amend only works with the most recent commit not earlier ones. Ammend also provides another functionality: You can modify the message of the **most recent commit**. This is particularly useful for fixing typos or making other necessary adjustments to the commit message.

### Ignoring files

To prevent Git from tracking specific files or directories—such as log files, sensitive data like secret keys and passwords, or certain folders like Node module dependencies—you can compile a list of their names within a .gitignore file situated in the project folder.

Here's an example of the contents of a .gitignore file:

```
secrets.txt                  # a file
bin/                         # a directory
node_modules/                # a directory
*.log                        # you can use regular expressions/wildcards
```

While we want Git to ignore these files and directories and doesn't track them, we do want Git to keep track of the .gitignore file itself. Hence, we avoid including its name within the file.

An additional resource you might find beneficial is gitignore.io. This website offers .gitignore templates tailored to the programming language(s) you're using, streamlining the process of generating an appropriate .gitignore file for your project.

### Git Branching

Allows working on multiple ideas in parallel.

`git branch`: lists all the branches, with the current active branch marked by an asterisk (\*). Press q to exit.

`git branch branch_name`: create a new branch based upon the current HEAD (but it won’t switch you to that branch).

`git switch branch_name`: switches to the branch_name or makes HEAD point there (Like checkout).

`git switch -c branch_name`: to create a new branch and switch to it all in one go (like git checkout -b).

**Delete and rename branches**

`git branch -d` (or -D for forcing deletion) branch_name: for deleting a branch.

`git branch -m` for moving/renaming a branch. First, switch to the branch that you want to rename, then `git branch -m new_name`.

### Merging

We merge branches, not specific commits. We always merge to the current HEAD branch.

```
git switch main
git merge bugfix
```

**Dealing with merge conflicts**: Open the files that are encountering conflicts. Within these files, carefully address the conflicts by making necessary adjustments. Eliminate the conflict markers, save your modifications and then add the resolved files and commit the changes, finalizing the merge.

`git diff`: without additional options, it lists all the changes in our working directory that are not staged.

`git diff HEAD`: lists all the changes since your last commit (both staged and unstaged changes)

`git diff --staged` (or `git diff --cached`): lists the changes between the staging area and the last commit.
We can also narrow down the diff to a specific file(s), or even compare files in branches or comparing two commits:

```
git dif HEAD file1 file2
git diff --staged file1
git diff main..other_branch  # alternatively, use a single space instead of ..
git diff HEAD..HEAD~1        # or simply git diff HEAD~1 to see the diff between the two last commits.
git diff 09cbcc9..15db960    # use commit hashes (abbreviated, copied from git log --oneline)
```

### stashing

`git stash`: Saves unfinished changes (staged or unstaged) and hides them away, reverting the changes in your working copy and allowing you to switch to another branch without taking the uncommitted changes along or risking conflicts. Ideal for temporarily storing work that isn't ready for an official commit.

`git stash pop`: is used to remove the most recently stashed changes from your stash (LIFO order) and apply them to your current working copy. However, if you wish to apply the changes in multiple branches without removing them from the stash, you can use git stash apply instead of pop. This allows you to apply the changes across multiple branches and retain the stash for future use.

Example commands:

`git stash list`,

`git stash apply stash@{stash_id_from_list}`,

`git stash drop stash@{stash_is}`,

`git stash clear`

### Checkout

`git checkout`: use it to create branches, switch to new branches, restore files, undo history, etc.

`git checkout <commit_hash>`: travel back to a specific commit! HEAD will point to that commit (detached HEAD!) rather than pointing to a branch reference as it normally would. Instead of the full hash, you can use the first 7 characters of it, or you can use HEAD~ to travel back to previous commit with respect to where HEAD is currently at: `git checkout HEAD~1` checks out one commit earlier to where HEAD is now and `git checkout HEAD~2` travels back to its grandparent commit and so on! Now, some available options to do here:

    1-Poke around!
    2- Checkout back to a branch you were on before,
    3- Create a new branch and switch to it.

### Revert changes

`git checkout HEAD <filename(s)>`: reverts a modified file to its state in the previous commit. For example, you can run `git checkout -- filename1 filename2` to discard changes in the mentioned files and revert them to their state in the last commit.

`git restore <filename>`: Also helps undoing or discarding changes and restores the file to the way it looked like in the last commit. It uses HEAD as the default source, but we can change that using --source option:

`git restore --source HEAD~2 filename.html`

`git restore --staged <filename>`: unstage a file.

`git reset <commit_hash>`: will reset the repo back to a specific commit. The commits made after that specific commit will be lost but the changes will still be in our working area (we don’t lose the changes; we lose the commits only). Then we can create a new branch and switch to it, and the changes will be carried with us, and we can add and commit those changes in the new branch.

`git reset --hard <commit_hash>`: a hard rerset (both commits and changes we made will be gone!).

`git revert <commit5_hash>`: similar to git reset but instead of eliminating commits, it creates a brand-new commit that reverses the changes from commit5! If you are collaborating with others who have copies of the bad commit, use revert rather than reset to reverse the bad commit. If you haven’t pushed the bad commit and no one else has a copy of it, you can do either revert or reset.

## Using a Remote Repository

`git clone <url>`: Copies all files from a remote repository, not limited to gitHub, to your local machine. When running git branch, it displays only the main branch, but the remote repository may have several other branches. To obtain a local copy of any of those branches (for example origin/branch2), switch to it using `git switch branch2` without including the origin/ as if some branch called branch2 already exists on your local machine. Your branch2 will now track the remote branch (origin/branch2).

`git remote -v`: For viewing any existing remotes.

`git remote add <name> <url>`: used to add a new remote repository. The <name> serves as an arbitrary label, with "origin" being commonly used as the default name.

`git remote show origin`: More details and info on the remote repo (including branches).

`git remote get-url origin`: Shows origin’s url

`git remote set-url origin <new_reomte_url>`: sets the origin to point to a new remote repo.

`git push origin branch_name`: push a specific branch to a remote repo.

`git push origin local_branch_name`: remote_branch_name: pushes a local branch to a remote branch!

**The -u option**: After configuring the upstream branch for a local branch (e.g., git push -u origin branch1), subsequent pushes can utilize the shorthand: `git push`

`git branch -r`: lists the remote branches in a git repository (e.g., origin/main).

If you clone a repo and make a few local commits, to go back and check how a file looked like when we first cloned, you can checkout the origin/main (git checkout origin/main).

`git fetch <remote>`: downloads the latest information from the specified remote repo, typically gitHub, without integrating the changes into your local working directory. If you don't specify the remote, it defaults to 'origin.' After fetching, you can view the updates made since your last clone or fetch using the checkout command: e.g., `git checkout origin/main`

`git fetch <remote> <branch_name>`: we can fetch a specific branch as well.

`git pull <remote> <branch>`: is analogous to the combination of 'git fetch' followed by 'git merge'. This operation involves fetching the most recent changes from the specified remote and integrating them into the current local branch. So, it matters where we run this command from. Whatever branch we run this command from is where the changes will be merged into (also, resolve any conflicts that arise during this merging process).
