# Getting Started - First-Time Git Setup

Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

1. `[path]/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories. If you pass a option `--system` to `git config`. It reads and writes from this file specifically. Because this is a system configuration file, you would need administrative or superuser privilege to make chages to it.
2. `~/.gitconfig` ot `~/.config/git/config` file: Values specific personally to you, the user. You can make Git read and write to this specifically by passing the `--gloabl` option, and this affects **all** of the repositories you work with on your system.
3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you're currently using: Specific to that single repository. You can force Git to read from and write to this file with the `--local` option, but that is in fact the default. Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly

Each level overrides values in the previous level, so values in `.git/config` trump those in `[path]/etc/gitconfig`

You can view all of your settings and where they are coming from using:

`$ git config --list --show-origin`

# Git Basic - Getting a Git Repository

### Getting a Git Repository

You typically obtain a Git repository in one of two ways

1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
2. You can **clone** an existing Git repository from elsewhere

### Initializing a Repository in an Existing Directory

`$ cd /home/user/my_project`

and type:

`$ git init`

This creates a new subdirectory named `.git` that contains all of your necessary repository files -- aGit repository skeleton.

### Cloning an Existing Repository

`$ git clone https://github.com/libgit2/libgit2`

If you want to clone the repository into a directory named something other than `libgit2`, you can specify the new directory name as additional argument?

`$ git clone https://github.com/libgit2/libgit2 mylibgit`

That command does the same thing as the previous one, but the target directory is called `mylibgit`

# Git Basics - Recording Changes to the Repository

### Recording Changes to the Repository

Remember that each file in your working directory can be in one of two state: **tracked** or **untracked**.

**Tracked** files are files that files that were in the last snapshot, as well as any newly staged files; they can be unmodified, modified, or staged. In short, **tracked** files are files Git knows about.

**Untracked** files are everything else -- any files in your working directory that were not in your last snapshot and are not in your staging area. When you first clone a repository, all of your files will be tracked and unmodified because Git just checked them out and you haven't edited anything

![The lifecycle of the status of your files](https://git-scm.com/book/en/v2/images/lifecycle.png)

### Checking the Status of Your Files

The main tool you use to determine which files are in which state is the `git status` command

### Short Status

If you run `git status -s` or `git status --short` you get a far more simplified output from the command

```
$ git status -s
  M README
 MM Rakefile
 A  lib/git.rb
 M  lib/simplegit.rb
 ?? LICENSE.txt
```

There are two columns to the output -- the **left-hand** column indicates the status of the staing area and the **right-hand** column indicates the status of the working tree

**??** New files that aren't tracked

**A** New files that have been added to the staging area

**M** Modified files have an M

for example:

In that output, the `README` file is modified in the working directory but not yeg staged, while the `lib/simplegit.rb` file is modified and staged. The `Rakefile` was modified, staged and then modified again, so there are changes to it that are both staged and unstaged

### Ignoring Files

`.gitignore`

```
touch .gitignore
->
*.[oa]
*~
```

### Viewing Your Staged and Unstaged Changes

You'll probably use it most often to answer theses two questiosn: What have you changed but not yet staged?

And what have you staged that you are about to commit?

Although `git status` answers those questions very generally by listing the file names, `git diff` shows you the exact lines added and removed -- the patch, as it were

Let's say you **edit** and **stage** the `README` file again and then **edit** the `CONTRIBUTING.md` file without **staging** it. If you run your `git status` command, you once again see something like this:

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified: README

Changes not staged for commit:
  (use "git add <file>..." to update what will be commited)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified: CONTRIBUTING.md
```

To see what you've changed but not yet staged, type `git diff` with no other arguments:

```
$ git diff
diff --git a/CONTRIBUTING.md b//CONTRIBUTING.md
index 8ebb911..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
Please include a nice description of your changes when you submit your PR; if we have to read the whole diff to figure out why you're contributing in the first place. you're less likely to get feedback and your change
-merged in.
+merged in. Also, split your changes into compreheensive chunks if your patch is
+longer than a dozen lines.

If you are starting to work a particular area, fell free to submit a PR that highliightss your work in progress (and note in the PR title that it's)
```

That command compares what is in your working directory with what is in your staging area. The result tells you the changes you've made that you haven't yet staged.

If you want to see wath you've staged that will go into your next commit, you can use `git diff --staged`. This command compares your staged changes to your last commit

```
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 000000...03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```

and `git diff --cached` to see what you've staged so far (`--staged` and `--cached` are synonyms)

### Comitting Your Changes

Config vim to commit

```
$ git config --global core.editor vim
```

The editor displays the following text (this example is a Vim screen):

```
#Please enter the commit message for your changes. Lines starting with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Your branch is up-to-date with 'origin/master'.
#
# Changes to be committed:
#   new file: README
#   modified: CONTRIBUTING.md
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

For an even more explicit reminder of what you've modified, you can passa the `-V (--verbose)` option to `git commit`. Doing so also puts the diff of your change in the editor so you can see exactly what changes you're commiting

Alternatively, you can type your commit message inline with the `commit` command by specifying it after a `-m` flag, like this:

```
$ git commit -m "Story 182: fix benchmarks for speed"
[mater 463dc4f] Story 182: fix benchmarks for speed
2 files changed, 2 insertions(+)
create mode 100644 README
```

### Skipping the Staging Area

Adding the `-a` option to the `git commit` command makes Git automatically stage every file that is already tracked before doing the commit, letting you skip the git add part:

```
$ git commit -a -m 'Add new benchmarks'
```

### Removing Files

To remove a file from Git, you have to remove it from your tracked files (more accurately, remove it from your staging area) and then commit. The `git rm` command does that, and also removes the file from your working directory so you don't see it as untracked file the next time around.

The next time you commit, the file will be gone and no longer tracked. If you modified the file or had already added it to the staging area, you must force the removal with the `-f` option. This is a **safety** feature to prevent accidental removal of data that hasn't yet been recorded in a snapshot and that can't be recovered from Git.

Another useful thing you may want to do is to keep the file in your working tree but remove it from your staging area. In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of `.a` compiled files. To do this, use the `--cached` option:

```
$ git rm --cached README
```

# Working with Remotes

### Fetching and Pulling from Your Remotes

To get data from your remote projects, you can run:

```
$ git fetch <remote>
```

If you clone a repository, the command automatically adds that remote repository under the name "origin". So, `git fetch origin` fetches any new work that has been pushed to that server since you cloned.

If your current branch is set up to track a remote branch, you can use the `git pull` command to automatically fecht and then merge that remote branch into your current branch.

**Note**

- From git version 2.27 onward, `git pull` will give a warning if the `pull.rebase` variable is not set. Git will keep warning you until you set the variable.
- If you want the default behavior of git (fast-forward if possible, else create a merge commit): `git config --global pull.rebase "false"`

# Tagging

### Listing Your Tags

Listing the existing tags in Git is straightforward. Just type `git tag` (with optional `-l` or `--list`):

```
$ git tag
v1.0
v2.0
```

You can also search for tags mathc a paticular pattern, If you're interested only in looking at the 1.8.5 series:

```
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
```

### Creating Tags

Git supports two types of tags: **lightweight** and **annotated**.

A **lightweight** tag is very muck like a branch that doesn't change -- it's just a pointer to a specific commit.

**Annotated** tags, however, are stored as full objects in the Git database. They're checksummed; container the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It's generally **recommended that you create annotated tags** so you can have all this information

### Annotated Tags

Creating an annotated tag in Git is simple. The eaiest way is to specify `-a` when you run `tag` command:

```
$ git tag -a v1.4 -m "my version 1.4"
$git tag
v0.1
v1.3
v1.4
```

the `-m` specifies a tagging message, which is stored with the tag. If you don't specify a message for an annotated tag, Git launches your editor so you can ype it in.

### Tagging Later

You can also tag commits after you've moved past them. Suppose your commit history looks like this:

```
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 Create write support
0d52aaab4479697da7686c15f77a3d64d9165190 One more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc Add commit function
4682c3261057305bdd616e23b64b0857d832627b Add todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a Create write support
9fceb02d0ae598e95dc970b74767f19372d61af8 Update rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc Commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a Update readme
```

Now, suppose you forgot to tag project at v1.2, wich was at the "Upadte rakefile" commit. You can add it after the fact. To tag that commit, you specify the commit checksum (or part of it) at the end of the command:

```
$ git tag -a v1.2 9fceb02
```

You can see that you've tagged the commit:

```
$ git tag
v0.1
v1.2
v1.3

$ git show v1.2
tag v1.2
Tagger: Scott Cachon <schacon@gee-mail.com>
Date: Mon Feb 9 15:32:16 2009 - 0800

version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon <mchacon@gee-mail.com>
Date:   Sun Apr 27 20:43:35 2008 -0700

    Update rakefile
...
```

### Sharing Tags

By default, the `git push` command doesn't transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches -- you can run `git push origin <tagname>`

If you have a lot of tags that you want to push up at once, you can alse use the `--tags` options the `git push` command. This will transfer all of your tags to the remote server that are not alerady there.

### Deleting Tags

To delete a tag **on your local repository**, you can use `git tag -d <tagname>`

Note that this does not remote the tag from any remote servers

```
$ git push origin --delete <tagname>
```
