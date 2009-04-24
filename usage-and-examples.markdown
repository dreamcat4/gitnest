---
layout: default
title: GitNest
---

## Notes

* these instructions are for *gitnest version 0.0+*
* gitnest requires *git* and *rubygems*
* you must be in a repository with at least one commit in it

## Built in help

You can invoke the built in help to see all the available commands or the flags or a command:

  braid help
  braid help add # or braid add --help

## Starting point

There is only one type of repository in gitnest: a fully cloned, working repository. Whether any given repository is a root, a child, or a leaf node, all repositories are treated pretty much equally in gitnest. There are no complex bi-directional associations between repositories, or anything that cannot be seen by examining the .gitnest configuration file. 

## Adding a mirror

  # braid help add
  braid add git://github.com/rails/rails.git vendor/rails
  braid add -p git://github.com/mbleigh/seed-fu.git

## Adding mirrors with specific branches 

  braid add --revision bf1b1e0 git://github.com/rails/rails.git vendor/rails
  braid add --rails_plugin --type svn --revision 32 http://oauth-plugin.googlecode.com/svn/trunk/oauth_plugin

## Overriding default mirrors

  braid add --full --type svn --revision 403 http://oauth.googlecode.com/svn/code/ruby/oauth vendor/oauth
  braid add --full --rails_plugin git://github.com/mislav/will_paginate.git vendor/plugins/will_paginate

## Updating mirrors

  braid update vendor/plugins/cache_fu
  braid update

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

## Showing local changes made to mirrors

	braid diff vendor/rails

## Working in a team

	\# user 1
	gitnest add some/remote/blah.git lib/blah
	git push

	\# user 2
	gitnest update 


	# From the parent, update the sub-repository "ruby-lib" to branch "dev". (pulls and merges).
	# Also set "lib/ruby-lib" to follow branch "dev" whenever cloning or syncing. 
	gitnest update "lib/ruby-lib" --branch "dev"

	# Business as usual in the nested repository.
	cd "rails-app/lib/ruby-lib" && git-branch; git-commit; git-merge; git-push;

	# Or sync the "ruby-lib" repository back to the setting stored in .gitnest.
	cd "rails-app" && gitnest sync "lib/ruby-lib" 

	# "lib/ruby-lib" is now on branch "dev"

	# Move the repository "ruby-lib" to a different directory.
	gitnest move "lib/ruby-lib" "vendor/lib/ruby-lib"

	# Print to stdout a tree diagram showing the .gitnest file structure.
	gitnest show 

	# Remove the repository in lib/ruby-lib/. Update .gitnest, .gitignore.
	gitnest remove "lib/ruby-lib" 

	# Assign a quickreference label to be used in other gitnest commands.
	gitnest label --add "libs" "lib/ruby-lib/"
	gitnest label --add "libs" "lib/ruby-lib-2/"

For a detailed description of all commands, type 'gitnest help'.


## Operating on multiple repositories

	# Run command for a subset of repositories which are labeled.
	gitnest foreach --label "lib" git-diff 

	# Run arbitrary command in all of the child repositories sequentially.
	gitnest foreach git-diff 

	# Or use 'forall' to also run the command in the current-level directory.
	gitnest forall git-diff

## Workflow Example

    gitnest-add <arguments>

 * Executes the gitnest command 'add' with <arguments>
 * If command successful, updates .gitnest and .gitignore
 * Commits '.gitnest' and '.gitignore' to HEAD with msg "GitNest: Repository-X added in 'path/to/repository-x/'"

All gitnest commands perform roughly the above sequence; modifying the .gitnest file where applicable and recording the action as change history. However GitNest is unable to commit its changes if you have other pending changes. 'git-stash' will clean your index and working files prior to running a gitnest command (and 'git-stash apply' shall restore them). You can reset your working files and index with 'git reset --hard'.

## Updating with conflicts

If an update creates a conflict in one of the repositories, gitnest will leave the partially commited files in your working copy, just like a normal git merge conflict would. You will then have to resolve all conflicts and manually run 'git commit'. The commit message is already prepared.



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

## Pushing, Releasing 

Jeweler can also handle releasing to [RubyForge](http://rubyforge.org). There are a few steps you need to do before doing any RubyForge releases with Jeweler:

 * [Create an account on RubyForge](http://rubyforge.org/account/register.php)
 * Request a project on RubyForge. This involves waiting for a project approval, which can take any amount of time from a few hours to a week
   * You might want to create an umbrella project where you can publish your gems, instead of one project per gem
 * Install the RubyForge gem: sudo gem install rubyforge
 * Run 'rubyforge setup' and fill in your username and password for RubyForge
 * Run 'rubyforge config' to pull down information about your projects
 * Run 'rubyforge login' to make sure you are able to login

With this in place, you now update your Jeweler::Tasks to setup `rubyforge_project` with the RubyForge project you've just created. (Note, using `jeweler --rubyforge` when generating the project does this for you automatically.)

