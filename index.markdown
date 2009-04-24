---
layout: default
title: GitNest
---
GitNest is a small command-line tool managing cloned repositories in a file hierachy.. In short, GitNest aims to be a submodules-a-like scheme where one reposity sits inside the path of another repository. The GitNest command interface borrows heavily from [braid](http://github.com/evilchelu/braid/tree/master). If you have used either of those schemes in the past then should feel right at home with GitNest. - But don't worry if you aren't! Gitnest is fairly intuitive.

## Why use gitnest?

GitNest is a helper tool designed to complement and enhance the git-suite with an improved version of the git-submodules feature. If you are already using git, or are just starting out then it make for a great addition to the git. If you are cloning a git repository and have found a file named '.gitnest', you should really install gitnest, and then run 'gitnest-update' in the repository root.

You will want to use gitnest if you are: 

* Writing application-level code, or a module, a library which is versioned in git. 
* Writing a library to provide some of the functionality, or you would like to use a pre-existing library in your project which is already packaged a standalone and separate component.
* It doesn't make sense to merge the library's files in with your project, however they must or you would much prefer for them to reside in a subfolder within your project.
* The library you wish to use is versioned in git.
* Whilst working on your main project, you'd like to push any fixes or improvements for the library back upstream to its project maintainers.
* You have a documentation project, a website, other spin-off project which is related to your main software project. For technical / logistical reasons you require this to reside within the folder hierachy of the main project. However it is a distinct and logically separable sub-project.
* You have already looked into using git-submodules or are already. However certain aspects of git-submodules don't feel quite right or work the right way in practice.
* You want to check out somebody else's code, and they have organised their repositories with gitnest.

The following guide will help show you the various ins-and-outs of GitNest.

## Installation

    # Do you have rubygems installed? (GitNest requires RubyGems >= 1.3.1)
    gem --version
    
	# Add github as a source if you haven't already:
    gem sources -a http://gems.github.com
    
	# Install gitnest:
    gem install dreamcat4-gitnest

## Quickstart

In this example we have 2 repositories "rails-app" (the parent) and "ruby-lib" (a nested child).

	# Obtain any regular git repository - this one is a rails app.
	git clone git@github.com:username/rails-app.git
	
	# Adds, commits a new ".gitnest" configuration file 
	gitnest add git@github.com:username/ruby-lib.git "lib/ruby-lib"

	# Repository "ruby-lib.git" now checked-out on branch master
	# Path is "rails-app/lib/ruby-lib".

## Links

* [Indroduction](introduction.html)
* [Usage and examples](usage-and-examples.html)

