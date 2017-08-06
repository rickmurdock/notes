As you become a better programmer, the complexity and scope of your projects will expand. You will begin collaborating with other programmers or working more on the detailed aspects of a program. Creating software incorporates several layers of tests, logic, structure, and interfaces. To keep your bearing, you will need to learn how to manage and track code in a consistent and standardized way. The first step is to start tracking the changes to your code locally.

Local version control refers to keeping a running log of all the changes you've made to your code over time. Several tools exist to keep track of these changes. This article and this class will focus on one in particular.

Git is a powerful tool that allows you to control versions of your code through the CLI. Git lets you create several working versions of your code so that you can try out ideas without breaking critical features. Git keeps a complete history of your code by letting you commit checkpoints as you make changes. You can compare and contrast code and you can even use Git to roll your code back to an earlier state if things go awry.

This article will get you up and running with Git. Learning to properly use Git is the first step toward managing your code like a professional.

# A Brief History of Git  

![git logo](https://github.com/rickmurdock/notes/blob/master/FrontEnd/images/gitLogo.png)

The development of the Linux Kernel, a Unix-like system, is an inspiration to the development of Git. Linux maintenance updates were passed around as patches and archived files (1991-2002). In 2002 the Linux project began using BitKeeper. This was a proprietary free-of-charge Distributed Version Control System. This DVCS simplified the development and maintenance process. Compared to patches and archived files, a DVCS relies on developers “cloning” a copy of a repository. The cloned 'repository' has a full history of the project with all the metadata of the original. From here developers 'pull' and 'push' changes in order update the code.

In 2005, the relationship between the Linux Kernel Project and BitKeeper developer dissolved. BitKeeper became a paid service, prompting the Linux community - including Linus Torvalds - to develop new version control software. Git emerged from that effort.

Git offers a simple, fully distributed, and speedy design. It has strong support for large projects and core document management features for collaboration.

# Git Commands  

Once installed, `git` ships with a handful of commands that you can use to track the changes you make to your files.

* `init`: This creates a git repository in the current directory.

* `add`: Adds a file(s) to `staging`.

* `reset`: This will remove a file from the `staging` without changing anything else.

* `status`: See the current status of your directory and repository.

* `commit`: Saves a snapshot of the staging area.

* `commit -m`: Same as just `commit` but takes a string that will be used as the commit message

# Using Git Locally  

Using Git at a local level is a fairly simple process. In order to use git, you must first install it.

## Install Git from Git-SCM  

The simplest way is to download and install from their website: [Download Link](https://git-scm.com/download).

## Install Git with Homebrew  

If you have [Homebrew](https://brew.sh/) installed, you can also install `git` with this command:

```sh
$ brew install git
```

> Git can be installed several ways. Most of those ways depend on the platform (Mac, Windows, Linux) that you are using. As long as you install the latest version, it doesn't matter how you get it installed.

## Configure Git  

Git is associated with another common tool called GitHub. Git (a command line tool) and GitHub (a website to store remote repositories) are different things. They have similar names and confusion between them is common. Be sure to recognize the differences between Git and Github.

For now, configure Git with the email address that is associated with your GitHub account. Don't worry, if you don't have a GitHub account. You can go ahead and configure Git with the email address you will use later to sign up for GitHub.

If you are not sure, you can [check here](https://github.com/settings/emails) to ensure you use one of the emails listed.

```sh
$ git config --global user.email "<Your email>" # configures your email
$ git config --global user.name "<Your name>" # configures your Full Name
```

By default, Git will use a built-in text editor like Vim or Nano. Those editors are advanced tools and are beyond the scope of this class. Instead, we'll configure Git to use [Atom](https://atom.io/). The following command will configure Git to use Atom. Whenever Git opens a file, it will do so using Atom.

```sh
$ git config --global core.editor "atom --wait"
```

> Git can be configured to use other editors. If you choose to use an editor other than Atom, please check out this guide.

## Create a Local Repository  

Git novices often have a hard time understanding what Git is doing in the background. The truth is that it takes time to pick up the processes that run when you execute Git commands. The best remedy for ignorance is practice.

Now that you have Git installed, let's go ahead and practice. Follow the steps below to track your very first file.

1. Navigate to your "Documents" folder in Finder. Once in the "Documents" folder, create another folder called "Projects".

2. Navigate to the "Projects" folder using your command line tool (Terminal, iTerm, etc).

3. Create a test repo using the following commands:

```sh
# Create a folder called "test-repo" where you will work on your project
# The folder can be called anything. In this case, "test-repo" is just the
# name we chose for our demo
$ mkdir test-repo

# Change Directory into your project folder
$ cd test-repo

# Initialize Git inside of the project folder
# so that Git will watch the folder and can track changes
# This command turns our project folder into a "repository" (repo)
$ git init

# Create a file for Git to track
# The contents of this file don't matter,
# we are just using it for demo purposes
$ touch README.md
```

4. Open the README.md file with your text editor and add the following text to the file.

```sh
# Hello World

This is a little bit of markdown to keep things interesting.
```

5. After creating the README file, Git will know about the file. However, Git isn't 'tracking' changes to the README file. What that means is that Git won't record changes that occur to `README.md`. First, you must add `README.md` to your 'staging area'. When you stage a file, you tell Git that the file's changes should be recoreded during the next 'commit'. Add the "README" file to your staging area with the `git add` command.

    `git add` works like this:

```sh
# git add [FILE]

# To add README.md
git add README.md

# To add index.html
git add index.html

# To add all the files in the repository
git add .
```

This will stage the changes to the repo and prepare Git to commit those changes.

```sh
# Stage this file to be committed
$ git add README.md
```

6. Now that the file has been staged, Git will record changes to that file in the next 'commit'. A commit is a "snapshot" of the current state of our repo. Git will save a copy of all the staged files *exactly as they are* when you make the commit.

    It will save a repo with one file (README.md) and will track the contents of that file (in this case, the two lines of text above).

```sh
# Create your first commit
# Always add a commit message (in quotes) to every commit
# That message tells other developers what the commit contains
# In this case, the commit message is "initial commit"
# As you make additional changes to this repo,
# The commit messages should change accordingly
$ git commit -m "initial commit"
```

**Let's Recap Our Progress:** Over the last 6 steps, we did a few things:

* We created a new project folder and added a file to it

* Next, we initialized the project folder with Git

* We then added some edits to the file

* Finally, we staged the file changes and committed that file

If we make additional changes to that file, we have a time-stamp in history that we can go back to if we make a mistake.

## Practice, Practice, Practice  

Now that this project is up and running, make a few additional changes to the README file. Save the file in your text editor, then add it to staging and commit another change. Do this a few times to get some hands-on practice.

Other things you can do to practice:

* Create another file in the repository, then add that file to staging and commit it.

* Delete files from the repository, then add and commit.

* Create sub-folders in the repository, then create files inside of those folders.

* Create different types of files (images, word files, .js files, etc.) in your repository

## Use the repository  

1. Add a file(s) to staging.

  * `$ git add <file-name>`. Add single file.

  * `$ git add .` To track all files in the current directory. You can do this by adding the current directory.

2. `$ git status`

3. `$ git commit -m <description>`

>  * use `$ git reset <file-name>` to remove file from staging.
>  * It is good practice to check the `status` of the repository after adding a file(s).
>  * Think of a `commit` as saving a game. You want to save and save often.

## Conclusion  

Regardless of how big or small the project, being able to keep track of the history of code changes is paramount. Version control makes fixing and modifying the code a much simpler process. If we use Git to commit often and check the status of our commits, our code will always be safe and secure.

> *Lastly, practice makes perfect! Go to [try.github.io](https://try.github.io/levels/1/challenges/1) and complete the challenges 1-9.*

---

# Managing Remote Repositories  

More often than not, you will collaborate with other developers on large projects. You might need to share code with them, or you might need to work on the same body of code at the same time. This type of collaboration can get pretty messy if you don't stay organized. It can get especially messy if everyone involved isn't tracking their changes. We want to make our local code repos (managed by Git) available to other collaborators with a "remote" repo.

> Additionally, you might want to store your code in a remote location in case of an emergency. If your computer gets dropped in the ocean but you have used a remote repo, you won't all your work.

This article will talk about "remote" repositories and why we need to maintain them.

## Local/Remote Workflow  

To start, let's discuss some of the motivations for maintaining a remote.

Let's pretend that I am working on a major project for an international client. I'm developing a part of application and collaborating with several other developers. We are all working on the same body of code but developing different features. We have coordinated on how we will manage our code through Git (locally) and a shared remote.

A typical workflow is as follows:

* Before I begin working on my code, I "pull" down any changes that my colleagues made to the remote repo. This will sync my local repo with any new work that my peers have done.

* I then work locally (on my computer) to make changes to my code.

* I track these changes with Git, staging and committing them as necessary. I may do this several times (and should). I write detailed commit messages so that my peers can see what changes have been made in each commit.

* Once I am satisfied with my work, I "push" those changes up to the "remote" location (a server). Pushing my changes syncs the code in the remote to the code in my local repo. This ensures that other developers have access to my changes to the code.

I have to assume that my peers will follow the same workflow. If we all pull down one another's changes *first*, we won't constantly overwrite one another. Plus, if we track each change with a commit, then our remote will keep a complete log of *everyone's* changes. It's pretty neat stuff.

This should illuminate the value of maintaining a remote. It is the central "hub" where all collaborators intersect.

## Github and Managing Remotes  

Several sites exist for creating remote repos, but we'll focus on one. Github is the standard place to store remote Git repos. We can use Git to "push" our local changes to our remote Github repo. Git offers several commands to manage our remotes. This section will talk about how to use those commands, how to hook up a connection between local and remote repos, and how to sync data between the two.

## Terminology and Commands  

* `git remote add`: Add a new remote repository of to project.

* `git remote`: View remote alias.

* `git remote -v`: View alias's URL.

* `git remote rm [ALIAS]`: Remove an existing remote alias.

* `git remote rename [old-alias] [new-alias]`: Rename remote aliases.

* `git remote set-url`: Change an existing remote URL

* `git push`: Push changes to remote repository.

## Adding A Remote Repository  

> This article assumes you already know how to manage a local repo with Git. For the following tutorial, you will first need to create a local repo called "local-remote". Create a README.md file inside the "local-remote" repo. Add the text "Local Remote Practice File" to the README file. Stage and commit the file to "local-remote".

To connect our local repo to a remote repository, we must first create the remote repo. Navigate to https://github.com/new. Then enter a "local-remote" in `Repository name`. Next click on `Create repository`. This will create a remote repo on your Github account.

On the next screen, you will see your remote's URL. You will need this to add your remote repository to your local repository. Copy this code and then run the following Git command replacing the sample URL with your remote's url.

```sh
$ git remote add origin git@github.com:[USERNAME]/[REPO-NAME].git
```

# Other Useful Commands  

What follows are several useful commands for managing remotes connected to your local repos.

## Listing Remote Repositories  

This will show all the remote aliases associated with the local Git repo. This will typically return "origin" when associated with Github.

```sh
# Will list the Remote Alias
$ git remote
```

This will show the URLs to the remote repo. It's useful for fetching and pushing content changes (More on that elsewhere).

```sh
# Will the Remote URL
$ git remote -v
```

## Removing A Remote Repository  

If you are not using a remote repository anymore, you can delete it.

```sh
$ git remote rm <ALIAS>
Renaming A Remote Repository  
```

You may want to rename a remote alias without having to delete it and add it again. To do so, use the following Git command.

```sh
$ git remote rename <old-alias> <new-alias>
```

## Changing A Remote URL  

If you ever need to change the URL of a remote repository, use the following git command.

```sh
$ git remote set-url <alias> <the-remote-url>
```

## Conclusion  

Git makes it easy to add a remote repository once you are ready to put your code on Github.com. Git also makes it easy to list remotes, view a remote's URL, change a remote's URL, rename a remote, and even remove a remote.

> Practice makes perfect! Go to [try.github.io](https://try.github.io/levels/1/challenges/10) and complete challenges 10-12.

---

# Git Fetch And Pull and Push  

In version control environment, code changes daily. As a developer, you will work on a team where many individuals work on different parts of a project. Keeping your local repository up-to-date is of the utmost importance. Git offers a set of tools to help you with this process, `git pull` and `git fetch`. Both their purpose is similar, but their implementation and execution are different.

## Git Terminology  

* `git fetch`: Synchronizes local repository with a remote repository and:

  * It pulls down any data not found locally.

  * It provides bookmarks to where each branch was on the remote after synchronization.

  * It *does not*  merge the data.
  
* `git pull`: Fetches and merges remote data into current branch

  * Runs a `git fetch` pulling down data from a remote repository

  * Immediately runs a `git merge`. This merges data from the remote repository with the local repository.

* `git push`: Sends all the local commits and changes to the remote branch

  * Merges those changes with the remote branch content, thereby syncing them together
  
## Fetch Vs. Pull  

Both Git commands pull down data from a remote repository. The difference is the handling of that data on a local level. Deciding which command to use depends on the situation.

### When To Use Fetch And Pull  

`git fetch`: This is a good one to use when working in a collaborative environment. Performing a `fetch` will pull down the data and let you see what has changed. For example, new branches might have been created. This would give you a chance to look at each branch individually and then decide which one to merge. In a nutshell, `fetch` serves as a data preview.

`git pull`: It is recommended to use `pull` when starting with a **clean working copy**. There should be no uncommitted local changes before performing a `pull`. If you do have local changes and want to avoid conflicts, use `git stash` to save local changes *temporarily*. Keep in mind that `git pull` might cause a `merge conflict` when merging remote changes with local ones.

## Git Practice  

> *Go to [learngitbranching.js.org](http://learngitbranching.js.org/) and complete exercises 3 and 4 under the 'remote' tab.*

## Conclusion  

When working on code collaboratively you will most likely have to update your code on a daily basis. Git offers two different tools to get this done. With `git pull` you will be able to update your local repository in one step. Use `git fetch` to preview code changes and to cherry pick which changes you merge locally.

---

# Stashing  

A lot of times as a developer you will be in the middle of a project when suddenly you have to switch to a different branch. Switching is easy, but you will not always want to commit changes before making the switch. For example, you have not had a chance to test the code. `git stash` will save these changes temporarily so that you can continue work later.

## Stash Terminology  

* `git stash`: Adds current changes to the stack.

* `git stash list`: View stashes currently on the stack.

* `git stash apply`: Applies an item from the stash list to current working directory.

* `git stash drop`: Removes items from the stash list.

> In the following examples anytime you see a series of `xxxxx` it would normally be replaced with a unique id.

## Git Stash  

`git stash` also give you back a clean working directory and it leaves the code at the state of the last commit. It **will not** include untracked files. To do so, use `git stash -u` (version 1.7.7 and later).

Here is an example of stashing some changes. First, let's assume we have made some changes. You'll see those when you run the following:

```sh
$ git status # will show the status
```

Let's say we don't want any of those changes to be in this branch. This is where stash comes in.

```sh 
$ git stash
  Saved working directory and index state WIP on master: xxxxxxx we are trying git stash
  HEAD is now at xxxxxxx we are trying git stash
$ git status
  # On branch master
  nothing to commit (working directory clean)
```

## Git Stash List  

If you want to see a list of the stashes in your stack, you can do the following:

```sg
$ git stash list
  stash@{0}: WIP on master: xxxxxxx we are trying git stash
```

Incremental stashes will be noted inside the `{ }`. `stash@{0}` represents the **last** stashed item.

```sh
$ git stash list
  stash@{0}: WIP on master: xxxxxxx we added one stash
  stash@{1}: WIP on master: xxxxxxx we added two stashes
```

## Git Stash Apply  

When you want to apply a stash to the branch you are in, you can do the following:

```sh
$ git stash apply
  # On branch master
  # Changes not staged for commit:
  #   (use "git add <file>..." to update what will be committed)
  #   (use "git checkout -- <file>..." to discard changes in working directory)
  #
  # modified:   hello-world.html
  #
  no changes added to commit (use "git add" and/or "git commit -a
```

By default, it will reapply the last added stash item to the working directory, stash@{0}. Change stash number to apply a different item.

## Git Stash Drop  

Let's say you want to drop one of the stashes on the stack. You should first run `git stash list` to get the number, then:

```sh
$ git stash drop stash@{1}
  Dropped stash@{1} (xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx)
```

## Conclusion  

Quickly saving changes you are not yet ready to commit or save is easy using `git stash`. This git command will give you the ability to come back to your code at a later time while you work on something else.
