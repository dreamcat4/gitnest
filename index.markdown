---
layout: default
title: GitNest
---
GitNest is a small command-line tool managing cloned repositories in a file hierachy, whereby one reposity sits inside the path of another repository. In short, GitNest aims to be a submodules-a-like scheme, however vastly improved. Also, the GitNest command interface borrows heavily from the braid project. So if you have used either of those in the past then should feel right at home with GitNest. - But don't worry! Its also our aim to provide the best possible experience for those who are not already familiar with such tools.

## Starting point

There are many ways that gitnest can be used in a broader computing / vcs environment. 
However the primary use for gitnest is as follows: 

* You are writing application-level code, or a module, a library which is versioned in git. 
* You would like to write a library to provide some of the functionality, or you would like to use someone else's library in your project which is already packaged a standalone and separate component.
* It doesn't make sense to include the library files directly into your project, however they must or you would much prefer for them to logically reside in a subfolder of your main project.
* The library you wish to use is also versioned in git.
* Whilst working on your main project, you'd also like to push any fixes or improvements of the library code back upstream to the library maintainers, which is an entirely separate project.
* You have a documentation project, a website, or spin-off project which is associated to your main software project. For certain technical / logistical reasons require this logically separable sub-project to reside within the folder structure of the main project hierachy.
* You have already looked into git-submodules or are using git-submodules already. However some aspects don't feel or work quite the right way.

The guide will assume the above use-case scenario and help show you the ins-and-outs.

## Installation

    # Do you have rubygems installed? You can check by typing:
    gem --version
    # Then run the following if you haven't done so before:
    gem sources -a http://gems.github.com
    # Install the gem:
    gem install dreamcat4-gitnest

## Quickstart

	# Obtain any regular git repository - happens to be a rails app.
	git clone git@github.com:username/rails-app.git
	
	# This will add/commit a ".gitnest" configfile, and also 
	# clone / instantiate "ruby-lib.git" as a nested, working repository.
	gitnest add git@github.com:username/ruby-lib.git "lib/ruby-lib"
	
	# From the parent, update the sub-repository to branch "dev". (pulls and merges).
	# Also set "lib/ruby-lib" to follow branch "dev" whenever cloning or syncing. 
	gitnest update "lib/ruby-lib" --branch "dev"
	
	# Business as usual in the nested repository in "rails-app/lib/ruby-lib".
	cd lib/ruby-lib && git-branch && git-commit; git-merge; git-push etc.
	
	# Or cd back and tell the parent to sync the child  
	cd rails-app && gitnest sync "lib/ruby-lib" --branch "dev"
	
	# Move sub-repo location to a slightly different place. Still nested.
	gitnest move "lib/ruby-lib" 
	
	# Print a pretty.
	gitnest show  
	
	# Remove the sub-repo. Deleted, gone.
	gitnest remove "lib/ruby-lib" 
	
	# Create a label for "lib/ruby-lib/"
	gitnest update --label "" "development"


* [Indroduction](introduction.html)
* [Usage and examples](usage-and-examples.html)













