## Contributing to Boost.Geometry

Copyright © 2014 Adam Wulkiewicz

Distributed under the Boost Software License, Version 1.0. (See accompanying file LICENSE_1_0.txt or copy at >[http://www.boost.org/LICENSE_1_0.txt](http://www.boost.org/LICENSE_1_0.txt))

### Table of Contents

* [Prerequesites](#prerequesites)
* [Testing the development branch of Boost.Geometry](#testing-the-development-branch-of-boostgeometry)
* [Fork Boost.Geometry repository](#fork-boostgeometry-repository)
    * [Verify if your fork works (optional)](#verify-if-your-fork-works-optional)
* [Working with ModularBoost](#working-with-modularboost)
    * [Preparation](#preparation)
    * [Add new remote repository](#add-new-remote-repository)
    * [GitFlow workflow](#gitflow-workflow)
    * [Create new branch for your work](#create-new-branch-for-your-work)
    * [Verify](#verify)
* [Modify the branch](#modify-the-branch)
    * [Commits](#commits)
    * [Test](#test)
    * [Push](#push)
* [Request a pull](#request-a-pull)
* [Done!](#done)

### Prerequesites

* git (Git BASH on Windows)
* a compiler to run Boost tests - gcc, clang, Visual Studio Express, etc.
* configured GitHub account
* every step mentioned here: [TryModBoost](https://svn.boost.org/trac/boost/wiki/TryModBoost) understood and tested.
 
Other helpful links:

* http://git-scm.com/documentation
* https://help.github.com/articles/set-up-git
* https://help.github.com/articles/setting-your-email-in-git
* https://help.github.com/articles/generating-ssh-keys

### Testing the development branch of Boost.Geometry

Those are the steps which must be done to check out the Boost.Geometry repository and run tests
(also described here: https://svn.boost.org/trac/boost/wiki/TryModBoost#Developing).

Check out the develop branch

    cd libs/geometry
    git checkout develop
    git branch -vv
    git pull

Run the tests

    b2 test

If everything works for you, you may move forward.

### Fork Boost.Geometry repository

You can't work directly in the original Boost.Geometry repository, therefore you should create your fork of this library. This way you can modify the code and when the job is done send a pull request to merge your changes with the original repository.

![Fork button](fork.png)

1. login on GitHub
2. go to [Boost.Geometry repository](https://github.com/boostorg/geometry)
3. click the 'Fork' button
4. choose your profile
5. wait
6. be happy!

More info: [Forking Projects](https://guides.github.com/activities/forking/)

#### Verify if your fork works (optional)

Go out of `modular-boost/libs/geometry/` directory

    cd ../../..

clone your repository and checkout develop branch

    git clone git@github.com:username/geometry geometry
    git checkout develop
    git branch -vv
    git pull

see commits

    git log
    gitk

For now you should see exactly the same commits as in Boost.Geometry repository.

### Working with ModularBoost

You could of course stop on the previous point and work on this local clone of the fork of Boost.Geometry however there is a catch. You won't be able to use b2 (run tests or build documentation) if the library is not inside the Boost directory. It's therefore convenient to use your fork inside modular-boost which basically means adding new remote for Boost.Geometry and using it aside the original Boost.Geometry repository.

#### Preparation

If you did the optional steps mentioned in the section [**Verify if your fork works (optional)**](#verify-if-your-fork-works-optional), go back to the Geometry module directory `modular-boost/libs/geometry/`.

    cd ../modular-boost/libs/geometry

For now there is only one remote repository set for this local copy. You may check it by running

    git remote -v

you should see something like this

![origin git@github.com:boostorg/geometry.git](remote_origin.png)

There is one remote repository added, the original Boost.Geometry repository at [boostorg/geometry](https://github.com/boostorg/geometry)

#### Add new remote repository

Add another remote repository, your fork, give it some memorable name

    git remote add my_fork git@github.com:username/geometry

now after running

    git remote -v

you should also see the remote you just added

![my_fork git@github.com:awulkiew/geometry](remote_fork_origin.png)

#### GitFlow workflow

Boost is using the [GitFlow](http://nvie.com/posts/a-successful-git-branching-model/) workflow or branching model if you like. It's because it is very well suited to collaboration and scaling the development team. Each repository using this model should contain two main branches:

* master - release-ready version of the library
* develop - development version of the library
 
and could contain various supporting branches for new features and hotfixes. Those supporting branches have specific purposes, there are also rules describing which branches they may originate from and which branches they may be merged with. Those issues aren't covered in this tutorial.

As a contributor you'll most likely be adding new features or fixing bugs in the development version of the library. This means that for each contribution you should create a new branch originating from the develop branch, modify it and send a pull request in order to merge it, again with the develop branch.

#### Create new branch for your work

Make sure you're in develop branch running

    git branch -vv

you should see

![Develop branch picked](branch_vv_develop.png)

Now pick a name for your new branch. Try to choose the name which doesn't already exist. To check the names of existing remote branches run

    git branch -a

![List of branches](branch_a.png)

or check them directly on GitHub.

Lets say that you'd like to add some new feature. To reflect that, and because Boost is using the [GitFlow](nvie.com/posts/a-successful-git-branching-model/) branching model, you could name your branch `feature/example`. Choosing the name for a branch is up to you since it won't be created in the original Boost.Geometry repository.

Create new local branch

    git branch feature/example
    git checkout feature/example

push it to your fork

    git push -u my_fork feature/example

This should create a remote branch for you. The `-u` switch also sets up the tracking of the remote branch.

#### Verify

Now after running

    git branch -vv

you should see

![Feature branch picked](branch_vv_feature.png)

Have in mind that if you didn't use the `-u` switch you wouldn't see the tracking info for your new branch.

You should also be able to see your newly created remote branch on GitHub

![Fork branches list](remote_branch.png)

or while running

    gitk

![gitk develop and feature/example](gitk_branches.png)

### Modify the branch

If you plan to add a new feature, or a bugfix, or some parts for the docs, or an example, it is often wise to consult the [mailing list](http://lists.boost.org/mailman/listinfo.cgi/geometry) if that is a good idea. Other people may be working on the same thing. If it is a larger feature it is good to post a sort of implementation plan to check if the feature, and the implementation, is what is expected.

#### Commits

A word about commits, run

    git log

or

    gitk

and see what the commit messages convention is used.

In Boost.Geometry the commit messages should contain:

1. some name of the modified part of the library in the square brackets, e.g. an algorithm name (tip: look at the namespaces and directories),
2. a short title,
3. an extended comment in the new lines (tip: to add more lines to the commit message use git commit without the `-m` switch).

As it's mentioned in the GIT documentation (http://git-scm.com/book/en/Distributed-Git-Contributing-to-a-Project):

> It's also a good idea to use the imperative present tense in these messages. In other words, use commands. Instead of "I added tests for" or "Adding tests for," use "Add tests for."

#### Test

As you already know tests are placed in the `test/` directory. Add tests for the newly added feature and if you added new files modify the Jamfile as well.

Run the tests (assuming you're in `modular-boost/libs/geometry/`)

    b2 test

#### Push

At the end, push your changes to the remote branch

    git push my_fork feature/example

or if your local branch is tracking the remote one, just

    git push

### Request a pull

After pushing your work you should be able to see it on GitHub.

Click "Compare and pull request" button

![compare and pull button](compare_and_pull.png)

By default GitHub wants to request a pull to the boostorg/geometry:master branch

![default pull branches](default_pull.png)

Change it by clicking the "Edit" button on the right side and pick develop branch

![change base branch](change_pull_base.png)

add some nice title and description

![Pull request title](pull_request_title.png)

and click the "Send pull request" button.

### Done!

After the review your contribution will be merged into Boost.Geometry or you'll see some comments about issues that should be fixed before merging.
