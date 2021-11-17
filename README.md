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
