---
layout: default
title: GitNest
---

## Notes

* these instructions are for *gitnest version 0.0+*
* gitnest requires *git* and *rubygems*
* you must be in a repository with at least one commit in it

## Starting point

  git init project && cd project
  touch README
  git commit -a -m "new repository"

There is only one type of repository in gitnest: a fully cloned, separate working repositories. Most people also have a hosted public repository that you push your changes to. We assume that your public repositor(y/ies) are bare, and so shall only ever be pushed the .gitnest reference file.

Whether any given repository is a root, a child, or a leaf node, all repositories are treated with equally in gitnest. And there are no complex associations between repositories, or anything that cannot be determined by looking at the .gitnest reference file. 


Full mirrors are useful when you want to view imported history in your own project. You usually want this if the mirror is also a repository you have access to, for example, when using shared code across projects.

Please note that you *cannot change* between mirror types after the initial add. You'll have to remove the mirror and add it again.

h2. Built in help

You can invoke the built in help to see all the available commands or the flags or a command:

  braid help
  braid help add # or braid add --help

h2. Adding a mirror

  # braid help add
  braid add git://github.com/rails/rails.git vendor/rails
  braid add -p git://github.com/mbleigh/seed-fu.git

h2. Adding mirrors with revisions

  braid add --revision bf1b1e0 git://github.com/rails/rails.git vendor/rails
  braid add --rails_plugin --type svn --revision 32 http://oauth-plugin.googlecode.com/svn/trunk/oauth_plugin

h2. Adding mirrors with full history

  braid add --full --type svn --revision 403 http://oauth.googlecode.com/svn/code/ruby/oauth vendor/oauth
  braid add --full --rails_plugin git://github.com/mislav/will_paginate.git vendor/plugins/will_paginate

h2. Updating mirrors

  braid update vendor/plugins/cache_fu
  braid update

h2. Updating mirrors with conflicts

If a braid update creates a conflict, braid will stop execution and leave the partially commited files in your working copy, just like a normal git merge conflict would.

You will then have to resolve all conflicts and manually run @git commit@. The commit message is already prepared.


## Locking and unlocking mirrors

  braid update --revision 6c1c16b vendor/rails
  braid update --head vendor/rails

## Showing local changes made to mirrors

	braid diff vendor/rails

## Working in a team

	\# user 1
	gitnest add some/remote/blah.git lib/blah
	git push

	\# user 2
	gitnest update 


## Workflow
 * 
 * Hack, commit, hack, commit, etc, etc
 * `rake version:bump:patch release` to do the actual version bump and release
 * Have a delicious scotch
 * Install [gemstalker](http://github.com/technicalpickles/gemstalker), and use it to know when gem is built. It typically builds in a few minutes, but won't be installable for another 15 minutes.


## Operating on multiple repositories

	# Run command for a subset of repositories which are labeled.
	gitnest foreach --label "lib" git-diff 

	# Run arbitrary command in all of the child repositories sequentially.
	gitnest foreach git-diff 

	# Or use 'forall' to also run the command in the current-level directory.
	gitnest forall git-diff


## Advanced usage and configuration

	# Run command for a subset of repositories which are labeled.
	gitnest add --config -p "" 

	# Run arbitrary command in all of the child repositories sequentially.
	gitnest foreach git-diff 

	# Or use 'forall' to also run the command in the current-level directory.
	gitnest forall git-diff


### Commands

add
remove
update
sync
move
foreach
forall
diff 
status
setup

### Flags

-b --branch
-t --tag
--no-branch
--no-track
-s --stash
-r --rails
-g --rails_gem
-p --rails_plugin
-f --force

## Update

Jeweler gives you tasks for building and installing your gem:

    rake build
    rake install

## Sync

Jeweler tracks the version of your project. It assumes you will be using a version in the format `x.y.z`. `x` is the 'major' version, `y` is the 'minor' version, and `z` is the patch version.

Initially, your project starts out at 0.0.0. Jeweler provides Rake tasks for bumping the version:

    rake version:bump:major
    rake version:bump:minor
    rake version:bump:patch

## Releasing to RubyForge

Jeweler can also handle releasing to [RubyForge](http://rubyforge.org). There are a few steps you need to do before doing any RubyForge releases with Jeweler:

 * [Create an account on RubyForge](http://rubyforge.org/account/register.php)
 * Request a project on RubyForge. This involves waiting for a project approval, which can take any amount of time from a few hours to a week
   * You might want to create an umbrella project where you can publish your gems, instead of one project per gem
 * Install the RubyForge gem: sudo gem install rubyforge
 * Run 'rubyforge setup' and fill in your username and password for RubyForge
 * Run 'rubyforge config' to pull down information about your projects
 * Run 'rubyforge login' to make sure you are able to login

With this in place, you now update your Jeweler::Tasks to setup `rubyforge_project` with the RubyForge project you've just created. (Note, using `jeweler --rubyforge` when generating the project does this for you automatically.)

