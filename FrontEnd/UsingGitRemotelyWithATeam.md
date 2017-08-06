# Cloning A Remote Repository  

As a developer you will be working in solo projects and collaborative projects. In a collaborative setting, your team will be working off a master mold, the `master branch`. Think of this branch as the finished product. It is highly unlikely that you will be making changes to the code in the master branch. In a given project you will be assigned responsibility over a specific piece of code. In order to keep the master branch intact, you will have to copy the project locally and create your own working branch. When you finish your task, the changes from your branch will be merged into the master branch. The first step in collaborating in a project is to clone the remote repository.

## Cloning Terminology  

Cloning is straight forward. Learning the following Git commands will help you clone in just a couple of steps.

`git clone`: Copy a git repository in order to collaborate.

`git checkout`: Switch to a new branch.

`git checkout -b`: Create and immediately switch to a branch.

## How To Clone A Remote Repository  

> Important: replace `[project]`, and `[branch-name]`

### Cloning a remote repository  

1. `$ mkdir [project]` Creates a folder in local machine with the name `[project]`.

2. `$ cd [project]`

3. Navigate to the main page of the repository on Github.

4. Click on the green button that says "Clone or download" on the right-hand side of the page.

5. In the `Clone with HTTPS` section, copy the code URL for the repository.

6. Type `git clone` and paste the url.

#### Example

```sh
$ git clone https://github.com/USERNAME/REPOSITORY
```

#### Output Example

```sh
$ git clone https://github.com/schacon/simplegit.git
Initialized empty Git repository in /private/tmp/simplegit/.git/
remote: Counting objects: 100, done.
remote: Compressing objects: 100% (86/86), done.
remote: Total 100 (delta 35), reused 0 (delta 0)
Receiving objects: 100% (100/100), 9.51 KiB, done.
Resolving deltas: 100% (35/35), done.
```

### Creating a branch and switching into it  

`$ git checkout -b [branch-name]`

#### Example

```sh
$ git checkout -b branching-out
Switched to a new branch 'branching-out'
```

> Another way of creating a branch and switching into it is to: `$ git branch [branch-name]` and then `$ git checkout [branch-name]`

#### Going back to the master branch

`$ git checkout master`

#### Example

```sh
$ git checkout master
Switched to branch 'master'
```

#### Deleting a branch

`$ git branch -d [branch-name]`

#### Example

```sh
$ git branch -d delete-this-branch
Deleted branch delete-this-branch (was 78b2670).
```

#### Practice makes perfect  

[Practice creating a branch here](https://try.github.io/levels/1/challenges/18)

## Git Practice  

> Got to [learngitbranching.js.org](http://learngitbranching.js.org/) and complete exercise 1 under the 'remote' tab.

#### Conclusion  

Whether you just want to look at somebody's code or you need to work in a group project, you will need to get the code into your machine. In order to do so, you use `git clone` to get a local copy of a Git repository.

---

# Branches  

When developing an application, you might want to experiment with a new feature. This new feature might require pretty dramatic changes in the code-base that you are developing. Or you might be working on a feature that is likely to break some of the code that one of your peers is developing. If we only have a single code base, then experimentation and potential overlap might make evolving the code base difficult.

To overcome this problem, Git allows us to create "branches" of code. Like tree branches, this allows you to spin off a new copy of the code in parallel with the main branch of code. You've likely seen (and typed) "master" into the command line several times by now. "master" is the default branch that Git sets up when you initialize a repo. So, no matter what, you are always working on a branch.

This article will discuss how to create new branches, how to manage multiple branches, and how to merge them back together when.

## Merging Branches  

Incorporating changes from an isolated branch into the main branch is common practice in the world of version control. Doing so is fairly simple using the git command, `git merge`. Below we will use the 'merge-example' branch to demonstrate a merge. If we create a branch and remove files from it and commit the removals, then this deletions are isolated from the main 'master' branch. In order to include deletions in the 'master' branch, you must merge in the 'merge-example' branch.

## Git Branching & Merging Commands  

* `git branch`: List all of your branches

* `git branch <branch-name>`: Creates a new branch with a new name

* `git checkout <branch-name>`: Switches to the new branch

* `git merge <branch>`: Merges a branch context into a current branch.

* `git merge --abort`: Returns to state prior to merge.

* `git reset --hard`: Rolls back to the commit before the merge.

## Merging Two Branches  

> This article assume you already know how to manage a local repo with Git. For the following tutorial, you will first need to create a local repo called "branches". Create a README.md file inside the "branches" repo. Add the text "Branches test file" to the README file. Stage and commit the file to "branches". All of the following code should occur inside the "branches" repo.

In this example, we'll branch off of our `master` branch by creating a new one. We'll then make a small change. After committing the changes, we'll merge it back into our `master` branch. Before you start, you should run `git status` and ensure you are on the master branch.

```sh
# Creating a branch named "bug-fix"
# You will still be in the master branch after running
# this command
$ git branch bug-fix

# Switching to that branch
# Now you are in the bug-fix branch
# Any changes you make and commit will be isolated
# to this branch and will not appear
# in the master branch until
# you merge them back together
$ git checkout bug-fix
```

Now that we are on a new branch, let's go ahead and make some changes. Change the text in the README file to "This text is in the bug-fix branch".

```sh
$ git add README.md
$ git commit -m "Changed the text"
```

Now that we've added and committed to our `bug-fix` branch, we can go back to `master` and merge

```sh
# switches back to our "master" branch
# If you look in the README file the text will be "Branches test file"
# This is because you made the changes in the bug-fix branch
# So the master branch doesn't yet know about those changes
$ git checkout master

# merges "bug-fix" into "master"
# this will take any changes in "bug-fix" and transfer them to "master"
$ git merge bug-fix
```

## How To Resolve A Merge Conflict  

If both branches contain an identical block of code, a merge conflict will take place. If both the 'master' branch and the 'merge-example' branch contained a link, in the same code block, with different link text, then the merge would fail. When a merge conflicts happens there is no need to panic. First, Git is great at helping us with resolving a merge conflict. Second, when merging from a remote repository, it **does not** affect the remote repository.

When a merge conflict is detected, you will get a merge conflict message similar to the one below.

#### Example

```sh
$ git merge merge-example
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

We now know the file location of the merge conflict. Now go to your editor and open that file. You will notice that Git has very nicely marked the merge conflict as follows.

#### Example

```
Please visit us here
<<<<<<< HEAD
<a href="https://theironyard.com">The Iron Yard</a>
=======
<a href="https://theironyard.com">theironyard.com</a>
>>>>>>> other-branch
```

The conflict is wrapped between `<<<<<<<` and `>>>>>>>` and divided by `========`. To resolve the conflict, delete the conflicting code (It is always best to discuss this with the other developer when working in a group project) and markers. After you have done so, simply save your work and stage the changes, i.e., `$ git add index.html` and commit, `$ git commit -m 'resolve Iron Yard link merge conflict'`.

At this point you can either push or continue working and push at a later time since the merge conflict has been resolved!

## Aborting And Reseting A Merge  

There is always a was of returning to the state before a merge was executed. This means that you cannot really break anything. You can abort by running the following command:

`git merge --abort`

If you incorrectly resolved a conflict, you can easily undo it by rolling back to the commit prior to the merge. You can reset your code using the following command:

`git reset --hard`

> WARNING: A reset will rewrite commit history. Use it with caution. Mostly this is so because other people you are collaborating with may already have one version of the git history, and if you rewrite it, their current work may be affected very negatively. In general, it is recommended to only use this method (reset) on your own branch (just you) before merging. Read about git revert below for an alternative. 1

## Git Practice  

> Got to [learngitbranching.js.org](http://learngitbranching.js.org/) and complete exercise 2 under 'To Origin And Beyond' found in the 'remote' tab.

## Conclusion  

`git merge` will combine another branch's context into the current working branch. Git does a lot of work under the hood in order to figure out how to best combine both sets of code into a new state with the *unique* code from both. Merge conflicts do happen, but thankfully Git makes it easy to fix these. Good coding practices and good communication with other developers can help make merge conflicts rare.

##### Lesson Footnotes

* 1: [Revert vs Reset](https://www.atlassian.com/git/tutorials/undoing-changes)

---

# Git Team Best Practices  

Working on a solo project is fairly straight forward, but working on a large project with other developers can be complicated. One aspect of software development that can be a challenge is keeping track of who is doing what. Imagine the following. You are working on a cool webapp with two other developers, all from the same branch. You are responsible for the JavaScript. The other two developers are responsible for the HTML and CSS, respectively. The HTML developer changes the JS code without telling you. Your code no longer works, delaying the project three days. Avoiding this kind of situation is a must. Developers must follow good version control practices to keep development productive and deployment smooth.

## Workflow  

Git and Github are flexible tools that allow for many versioning workflows. The most popular ones are: long-running branches, topic branches, `GitHub flow` and `GitFlow`. Which one you use is up to you and your team. The most important things is for everybody to agree on one and stick to the work-flow. Below is a basic introduction to the `GitHub flow` and `GitFlow` branch models.

## GitHub flow Model  

GitHub flow offers a robust and yet lightweight branch-based model well suited for handling projects, both large and small. Github also offers powerful tools to help support teams that rely on regular deployments. Here is a rundown of the GitHub flow model:

### Create A branch  

Creating a branch will give you an environment where you can work on different ideas and features without harming the code in the `master` branch. In your own branch you are free to try things and commit changes, with the peace of mind that your code will not be merged until it is *tested, reviewed and approved for a merge*.

> The `master` branch is the deployable branch, keep this in mind. This is a working copy of the code. It should be thoroughly tested and reviewed.

Keep your branch names descriptive. 'my-branch' is not a good branch name when working on user authentication. 'user-authentication-module' is a better choice.

### Commit Often  

Think of committing as saving a video game - you want to save and save often! Adding commits is how git keeps track of your code changes. These changes are accessible to you and everybody else working on the project. Just like using 'time-machine', a commit lets you go back to a safe point if a bug is found. Each commit is a separate unit of change.

Keep commit messages descriptive and succinct. This will make it easy for you and others to follow along and possibly provide feed back. When writing a commit message, keep it at around 50 characters. The commit message should answer what was the motivation behind the change and how it is different from the previous change. Also, use the present tense. For example, instead of "changed function variable names" to "change function variable names".

It is also recommended to commit small changes, compared to broad code changes. This will make it easier to handle bugs. If you do end up making large changes, consider using `$ git add -p`. This will let you interactively choose parts of a patch between the index and the work tree, and add them to the index. This will also give you a chance to review the difference before adding changes to the index.

> If you have added many changes to the stage, it is recommended to view the differences between commits. Use `$ git diff --staged` or alternatively, use `$ git commit -v`. The latter one will bring up the vim editor, letting you edit commit messages. It will also let you view the 'diff' about to be committed.

### Push Often  

By commit often, you are creating safe-points in your local machine, but if you computer bites the dust, so do your commits. It is good practice to `push` your commits often. This will keep your commits in the authoritative upstream repository. **Do not** use Git as a backup tool. 'committing' should be done semantically.

### Pull Requests  

Integrating you code is the next step. Once you have reviewed and tested your code, open a `pull request`. A Pull Request initiates a venue for communication in regards to your commits. This gives others the ability to see what changes would be merged, if accepted. Even if you have no code changes, use a Pull Request to share ideas, problems and screenshots. Use `@mention` to ask for feedback from your team or a specific developer.

### Review And Discussion  

Think of a Pull Request as 'code therapy'. Your code might be great and ready to be merged, or it might need polishing (you might need to add unit testing, change the style, etc). A Pull Request should start a healthy conversation geared towards productive and seamless integration of your code. While a Pull Request is open, you may continue committing and pushing. Github will show your new commits.

### Deployment  

Deployment is when the rubber meets the road. Once you have reviewed and tested your code, you can deploy your branch. Deploying your code will verify your changes in a production environment.

## Tips  

### DO's  

* Pull the master branch daily. Other people's branches most likely will be merged while you are working on your code. This might affect your current/future code. Therefore, pull in the master branch. Make sure to commit and push your branch changes and checkout into the master branch before pulling. Once you are ready to start new work, go back to the beginning of the `GitHub flow` cycle by creating a branch, etc.

* Test your code thoroughly. Make sure there are no side effects.

* Commit related code changes. An HTML change and a JavaScript change should be committed together if the changes are related.

* Commit small changes. Large commits make it hard to track down issues.

* Be careful when using `git reset --hard`. This will throw away all your uncommitted changes.

### DON'Ts  

* Do not develop on the master branch.

* Do not merge the upstream master branch with your branch. Remember your code must be tested, reviewed and approved in order to be merged. You merging your branch does not conform with the Gitub-flow model. Rely on Pull Requests for code integration.

* Do not create a branch to track more than one code change. Each code change should have its own branch.

* Do not use Git as a backup tool.

* Do not use `git pull --rebase` in a public environment. Performing a git pull -rebase in a private branch is fine 1, but doing the same on a branch where many developers are working on in a bad idea. This is because it changes the starting point of the branch to your newest commit, effectively removing everybody else's commits! This will create merge conflicts, hurt egos, and in some cases, termination.

* Do not use `git push --force` in a public environment. When your local repository is out of sync with the authoritative repository `git push` will fail. **Do not be tempted to do a** `git push --force`. This will override the structure and sequence of commits of the authoritative repository, deleting other people's commits. Keep your repository synced by performing a `git pull` daily and/or starting a new branch when needed.

## GitFlow  

Git branches are really powerful because you can use them for lots of things. But historically there wasn't really any information on how to use git branches when working on a team of developers. And most people just made up their own model.

In 2010 Vincent Driessen wrote an article called "A successful Git branching model". This article was aptly named because most people found that working with git branches on their team was not "successful". Vincent described how to properly use git, with great detail, even up to the point of showing the commands you should run at various points in time.

Driessen's branch model is centered around two main branches, `master` and `develop`, and other supporting branches, such as `hotfix`, `feature`, and `release`. Here's the diagram from his article that shows how his new approach organizes your repo's history:

![Driessen's branch model](https://github.com/rickmurdock/notes/blob/master/FrontEnd/images/driessensBranchModel.png)

We encourage you to read [Vincent's article](http://nvie.com/posts/a-successful-git-branching-model/) and here's some additional tips:

## Know The Lingo  

There is a plethora of Git commands, each with their own set of options. It is imperative for a developer to be familiar with all the Git commands related to creating a project, staging, branching, merging, sharing, updating, inspecting and comparing. See [Git Reference](https://git-scm.com/docs) for details.

## Git Lifecycle Review  

1- Create a repo by cloning or initting (only done once per project)

```sh
cd <projectsDirectory>
git init .

# if it exists already on GitHub and you need to pull files
hub remote add <projectname>
git pull

# if it doesn't exist on GitHub (default -- new project)
hub create <projectname>
```

2- Always check the status before each step.

```sh
git status
```

3- If you add files to the directory... nothing is stored in Git until you add it to the staging area.

```sh
git add <filename> <filename>

# or add everything in the directory

git add .
```

4- Nothing in the staging area is logged into Git until you commit.

```sh
git commit -m "a message about the commit"
```

5- Nothing will be on GitHub until you push.

```sh
git push origin master
```

6- You can create publicly visible websites on GitHub with `gh-pages`.

GitHub allows you to create a branch called `gh-pages`. These files (like an index.html file) will then be accessible at `<github-username>.github.io/<projectname>/index.html`.

Here's a Bookmark URL you can use to easily convert project page URLs to gh-pages URLs:

```js
javascript: (function() {    var s = location.href;    var r = /^((http[s]?|ftp):\/)?\/?([^:\/\s]+)((\/\w+)*\/)([\w\-\.]+[^#?\s]+)(.*)?(#[\w\-]+)?$/g;    location.href = s.replace(r, "http:/$5.github.io/$6");})();
```

7- Pushing to `gh-pages`.

```sh
# first time pushing to gh-pages
git branch gh-pages
git fetch . master:gh-pages
git push origin --all

# after first time...
git fetch . master:gh-pages
git push origin --all
```

8- Always modify your README.md file and the GitHub link and description of your repos.

If someone has to type out the gh-pages link to visit it, they won't (including employers). Always put your gh-pages link in the Website field at the top of your repo page.

## Git Practice  

### Complete Git lifecycle practice  

#### Prerequisites

Ensure that you have followed the "Computer Setup" notes. If you're not sure, you can run the following commands and make sure they run. If you see `command not found`, you need to follow the instructions to install the command line tools.

* `brew --version`, should output `0.9.5` or similar.

* `git --version`, should output git version `2.7.1` or similar.

#### Git immersion

* Go to gitimmersion.com.

* Complete Labs 1-12, 24, and 25.

* Create a repository on GitHub named `git-immersion` and push your local repository to GitHub.

##### Lesson Footnotes

1: `git pull --rebase`
