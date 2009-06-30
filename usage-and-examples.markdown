---
layout: default
---

## Notes

* these instructions are for *gitnest version 0.0+ in development*
* the first *stable* version of gitnest will be *version 0.5.0+*
* gitnest requires *git* and *rubygems*
* you must be in a repository with at least one commit in it

<!-- ## Built in help

You can invoke the built in help to see all the available commands or the flags or a command:

  gitnest help
  gitnest help add # or braid add --help -->

## Starting point

A git nest is a structure of nested git repositories, much like submodules. There is only one type of repository in gitnest: a fully cloned, working repository. Whether any given repository is a root, a child, or a leaf node, all repositories are treated equally. There are no complex bi-directional associations between repositories, or anything that cannot be examined in the file .gitnest.

## Add

	# Adds, clones repo, then commits the new ".gitnest" configuration  
	gitnest add git@github.com:username/ruby-lib.git "lib/ruby-lib"

	# Prefixing -g path becomes "vendor/gems/ruby-gem/". "prod" is a branch of ruby-gem.git.
	gitnest add -g git@github.com:username/ruby-gem.git --branch "prod"

## Update
	
	# From the parent, update the sub-repository "ruby-lib" to branch "dev". (pulls and merges).
	# Also set "lib/ruby-lib" to follow branch "dev" whenever cloning or syncing. 
	gitnest update "lib/ruby-lib" --branch "dev"
	
	# Gitnest should warn on commits from the parent repository.
	# Unsaved updates in a child and which files in which repositry of the child.
	
	# Business as usual in the nested repository.
	cd "rails-app/lib/ruby-lib" && git-branch; git-commit; git-merge; git-push;

## Remove

	# Remove the repository in lib/ruby-lib/. Update .gitnest, .gitignore.
	gitnest remove "lib/ruby-lib" 


## Workflow

    gitnest-add <arguments>

 * Executes the gitnest command `add` with `<arguments>`
 * If command successful, updates .gitnest and .gitignore
 * Commits `.gitnest` and `.gitignore` to HEAD with msg `"GitNest: Repository-X added in 'path/to/repository-x/'"`

All gitnest commands perform roughly the above sequence; modifying the .gitnest file where applicable and recording the action as change history. However GitNest is unable to commit its changes if you have other pending changes. `git-stash` hides your index and working files prior to running a gitnest command (and `git-stash apply` shall restore them). You can wipe away all working files and index with `git reset --hard`.
	
<!-- ## Track

Gitnest can track the version of your repositories. By default gitnest assumes that you will be staying up-to date with the master branch. gitnest-update --track and --no-track will switch on and off tracking for any given repository.

	# Sync the "ruby-lib" repository back to the setting you stored in .gitnest.
	gitnest sync "lib/ruby-lib" 

	# "lib/ruby-lib" is now on branch "dev" -->

<!-- ## Sync

Gitnest can track the version of your repositories. By default gitnest assumes that you will be staying up-to date with the master branch. gitnest-update --track and --no-track will switch on and off tracking for any given repository.

	# Sync the "ruby-lib" repository back to the setting you stored in .gitnest.
	gitnest sync "lib/ruby-lib" 

	# "lib/ruby-lib" is now on branch "dev" -->

<!-- ## Prefixing paths -g -p and -r

You can configure the 3 prefixing switches for gitnest-add. They will be remembered in your user-level configuration file ("~/.gitnest"). The defaults are shown below.

	gitnest config -g --quickpath "vendor/gems/"
	gitnest config -p --quickpath "vendor/plugins"
	gitnest config -r --quickpath "vendor/rails" -->

<!-- ## Overriding default mirrors

	gitnest config --add-path "/local/repos/" 
	gitnest config --add-path "git@github.com:username/"  -->

<!-- ## Diff

	# Cd into each repostiory, and run 'git-diff'
	gitnest diff  -->

<!-- ## Move

	# Move the repository "ruby-lib" to a different directory.
	gitnest move "lib/ruby-lib" "vendor/lib/ruby-lib" -->

<!-- ## Show

	# Print to stdout a tree diagram showing the .gitnest file structure.
	gitnest show  -->

<!-- ## Label

	# Assign a quickreference label to be used in other gitnest commands.
	gitnest label --add "libs" "lib/ruby-lib/"
	gitnest label --add "libs" "lib/ruby-lib-2/" -->

<!-- ## Operating on multiple repositories

	# Run command for a subset of repositories which are labeled.
	gitnest foreach --label "lib" git-diff 

	# Run arbitrary command in all of the child repositories sequentially.
	gitnest foreach git-diff 

	# Or use 'forall' to also run the command in the current-level directory.
	gitnest forall git-diff -->

<!-- ## Pushing, releasing

Guide to pushing code up to repositories

## Working in a team

	\# user 1
	gitnest add some/remote/blah.git lib/blah
	git push

	\# user 2
	gitnest update  -->

<!-- ## Updating with conflicts

If an update creates a conflict in one of the repositories, gitnest will leave the partially commited files in your working copy, just like a normal git merge conflict would. You will then have to resolve all conflicts and manually run 'git commit'. The commit message is already prepared. -->


<!-- ### More Commands

sync
config
migrate
move
foreach
forall
diff 
status -->

<!-- ### Flags
--path
-b --branch
-t --tag
--track
--no-track
-g --rails_gem
-p --rails_plugin
-r --rails
-s --stash
--no-merge (calls git-fetch instead of git-pull)
-f --force -->



