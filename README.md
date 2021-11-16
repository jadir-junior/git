# Getting Started - First-Time Git Setup

Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

1. `[path]/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories. If you pass a option `--system` to `git config`. It reads and writes from this file specifically. Because this is a system configuration file, you would need administrative or superuser privilege to make chages to it.
2. `~/.gitconfig` ot `~/.config/git/config` file: Values specific personally to you, the user. You can make Git read and write to this specifically by passing the `--gloabl` option, and this affects **all** of the repositories you work with on your system.
3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you're currently using: Specific to that single repository. You can force Git to read from and write to this file with the `--local` option, but that is in fact the default. Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly

Each level overrides values in the previous level, so values in `.git/config` trump those in `[path]/etc/gitconfig`

You can view all of your settings and where they are coming from using:

`$ git config --list --show-origin`
