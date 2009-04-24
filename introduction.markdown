---
layout: default
---

## Implementation, contribution

For historical reasons, gitnest has first been implemented as a ruby gem, following on from dchelimsky's excellent rake tasks in the RSpec project. The interface of GitNest was also heavily influenced by the braid RubyGem. Braid has a similar goal, however braid converts the inner repositories into subtrees and merges them with the parent, optionally including the commit histories.

If you would like to contribute to the gitnest _ruby-gem_, please fork and push / send pull request to git://github.com/dreamcat4/gitnest.git. Or email dreamcat4@gmail.com.

<!-- Another possibiliy would be for GitNest to be re-writen as a compiled tool ( in C, C-lib/or C++ boost ). This would remove the necessity for rubygems to be installed. Another possibility would be to incorporate GitNest into a larger, multi-function toolset. The original author can help with such porting efforts. If you would like to use or re-write gitnest project in this way, then please get in touch! -->

## Contributors

[Braid](http://github.com/evilchelu/braid/tree/master) - Copyright (c) 2007-2008 Cristi Balan, Norbert Crombach

[RSpec](http://github.com/dchelimsky/rspec/tree/master) - Copyright (c) 2005-2009 The RSpec Development Team

## License

(The MIT License)

Copyright (c) 2009 dreamcat4

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

<!-- ## Todo, feature improvement, suggestions (FIS)

Its fair to say that the original author of GitNest has little intention to implement any of these features himself. However they are suggested features and currently missing from the RubyGem implementation. For certain users, then many items on this list might be considered 'must haves'.

	* Implement foreach and forall actions.
	* Support for converting from git-submodules - > GitNest.
	* Support for converting from GitNest - > git-submodules.
	* Rudimentary support for converting from GitNest to braid-managed subtrees and back again (harder).
	* A way to import from nested subversion repositories (a script around git-svn clone)
	* A way to update and sync subversion repositories (probably foreach git-svn).
	* A way to automatically fork all the nested repositories for editing/pushing in one action (eg like 'jeweler --create-repo'). This would override the official repositories and re-write the .gitnest file.
	* Command to set-up and maintain global configuration options. 
	* Command to alias git-clone in command-line environment (add to ~/.profile).
	* User-definable path quick-references in global configuration options instead of just -r, -p and -g.

If you have any other feature improvement suggestion, or wish to comment on an existing suggestion please msg your thoughts across to me on GitHub. I'll probably ask you to implement your proposed features, or respond in a non-timely fashion however. -->

<!-- ## Failover

Then sourcing repositories for the first time, gitnest will first look in ~/.gitnest and ./.gitnest for source repository configuration options. This allows you to customize repository search on a per user or a per-project basis. Gitnest then looks for source repositories in the following locations, and in the following priority order

	/local/path/git/repos/
	github.com
	projectlocker.com
	unfuddle.com
	gitorious
	repo.or.cz
	git.savannah.gnu.org

	codebasehq.com (git clone git@gitbase.com:account/project/repo.git)
	gitosis (user hosted site)
	http://git.savannah.gnu.org/gitweb/
	git://git.sv.gnu.org/a2ps.git
	http://git.sv.gnu.org/r/a2ps.git
	ssh://git.sv.gnu.org/srv/git/a2ps.git -->
	
    
<!-- ## Is it a gitnest repository?

The usual convention is to check out a new repository by typing "git-clone < url >" or "git clone  >url >". However with the standard git this will only ever fetch the parent repository. If you are unsure whether a repository is a gitnest repository, then you may run 'gitnest-clone' instead. Gitnest will notice if there are meant to be nested repositories, and automatically download them to the appropriate places.

    gitnest clone http://somesite/git-repo.git

	# Or export it to your bash .profile
	echo "alias git-clone=\"gitnest-clone\"" >> ~/.profile
    
So this will perform all of the necessary initialization steps, including the regular 'git-clone', and to correctly fetch and initialize any or all nested repositories. A .gitnest file is used to store the repository references, very similar to .gitmodules and simply contains a list of each child repository, the url to fetch from, path to its desired location / mount point, etc.

If you have already cloned the repository and notice it has a .gitnest file, then you can just run

    gitnest update

Which will perform the remaining initialization steps and ensure that all of the sub-repositories are up-to-date. -->

<!-- ## The .gitnest

GitNest handles generating the `.gitnest` for your project. Normally you do not need to edit this file by hand. Use gitnest commands to manipulate your repository structure. 

A real-life `.gitnest` would look something like this:

	--- 
	lib: 
	  branch: master
	  type: git-clone
	  url: git@github.com:username/railsapp-lib.git
	vendor/gems/geokit-gem: 
	  branch: development
	  type: git-clone
	  url: git@github.com:username/geokit-gem.git
	vendor/plugins/geokit-rails: 
	  type: git-clone
	  url: git@github.com:username/geokit-rails.git
	  label: x
	/config/: 
	  branch: release
	  path: git@github.com:username/

Here are the corresponding commands:

	gitnest add git@github.com:username/railsapp-lib.git lib/
	gitnest add -g git@github.com:username/geokit-gem.git --branch development
	gitnest add -p git@github.com:username/geokit-rails.git
	gitnest config --branch release
	gitnest config --add-path "git@github.com:username/" -->


<!-- ## ~/.gitnest configuration

Configuration can be stored on a per-repository and on a User-level basis. Configuration in `~/.gitnest` override local config in `.gitnest`. The special block `/config/:` is marked by a leading slash whereas any other entry is a 'path/to' repository.

/config/:

* Paths to search for git repositories (overriding the official repository locations)
* Whether to track repositories, equivalent of specifying --track and --no-track 
* The path prefixing switches, -r -g -p, when adding new repositories. -->

<!-- ## Merging subtrees, non-gitnest repositories, path conflicts  

Usually this is a consideration when merging between and old branch and a new branch, or moving from a pre-existing system to gitnest. The 3 common Scenarios are as follows:

1) A repository with git-submodules, and a gitnest repository
2) A managed-subtrees repository (braid), and a gitnest repository
3) A repository with paths and files conflicting with the gitnest repository

* There are no specially managed subtrees in gitnest. In gitnest, all merging is handled separately within each repository and on a per-repository basis. If attempting to merge a braided repository with a gitnest repository, there are 2 scenarios. 1) If the working repository is gitnested then contains an ignored subfolder housing another .git working repository. The subfolder part cannot be merged from running git-merge in the parent. It will simply be ignored / discarded during the merge. However you may attempt to cd into the child repository and then merge the subtree - part afterward as a second action. -->

<!-- ## Comparison to submodules, subtrees

There are some important differences. 

* Remotes may be flexibly-sourced, and dont need to be hard-coded into the .gitmodules file. Instead, a path-checking failover scheme is adopted whereby GitNest will check through popular repository locations, and clone from the first match found. This is especially useful if you are checking out your own copies of an official 'set' of repositories. Popular locations are GitHub, local (on-disk), and G

* Commit SHA-1's are no longer tracked in the base tree. Instead branches or tags may be used for enforcing a dependency of the parent repository on a required version of the subrepository. Its optional to specify a particular branch or tag for any given repository. The default behaviour which is to sync with the master branch. This makes it easier when commiting changes, as you no longer have to commit the SHA-1 of the sub-repository into the parent repository every time you have made a commit in the sub-repository.This is a departure from the submodules methodology of keeping everything in check, every commit. However the benefits of moving dependency tracking to a per-branch basis are thought to be substantial. Or for any given child repository, you may specify the --no-branch option, which will mean no branch information need be recorded. Then the sync will have no effect, and not try to pull in changes automatically from the master.

* GitNest also records repository paths in the .gitignore file. This means that merging two branches of a repository that has been switched over to GitNest is much safer operation on the parent repository. After the merge 'gitnest update' will detect.

* There are no special kinds of subtrees in gitnest, except as per normal business for any regular git repository. A subtree exists only in relation to its' own .git repository, and a subtree cannot be shared between, or straddle repositories. Its intended that a whole git repository is the atomic unit here, which may be arbitrarily nested. The whole nest structure is recursive, so a single gitnest-clone or gitnest-update will iteratively dril down through all of the nested repositories. So if you are including a library, and that library also includes its own gitnest dependancies, those will be pulled in also.

* When the same repository must exist twice, three times in a gitnest, and mounted at different paths. 

1) If all such working copies are meant to follow the same branch and commits: Then only one copy should be managed by .gitnest map the remaining paths with a symlink. Otherwise it would be necessary to push up each commit to the shared remote and gitnest-update will pull back (attemptting to merge) onto the other local copies. (Not recommended).

2) If your working repositories have the same remote but are following different branches: This situation should be fine, and the repositories will not conflict with each other in gitnest. If you decide to merge those branches, then we are back at 1). 

3) You are actively working on only one branch, in one copy. Then consider using tags to freeze the other copies, or applying the --no-track option. This is probably the safest aproach. -->





