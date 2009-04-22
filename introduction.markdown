## Comparison to submodules, subtrees

The whole behaviour and interface of GitNest has been made very similar to git-submodules, so if you are familiar with submodules then you should feel right at home using GitNest. However its also our aim to make the experience as streamlined as possible for the novice - user, and do away with the more cumbersome parts. So there are some important differences. 

* Remotes may be flexibly-sourced, and dont need to be hard-coded into the .gitmodules file. Instead, a path-checking failover scheme is adopted whereby GitNest will check through popular repository locations, and clone from the first match found. This is especially useful if you are checking out your own copies of an official 'set' of repositories. Popular locations are GitHub, local (on-disk), and G

* Commit SHA-1's are no longer tracked in the base tree. Instead branches or tags may be used for enforcing a dependency of the parent repository on a required version of the subrepository. Its optional to specify a particular branch or tag for any given repository. The default behaviour which is to sync with the master branch. This makes it easier when commiting changes, as you no longer have to commit the SHA-1 of the sub-repository into the parent repository every time you have made a commit in the sub-repository.This is a departure from the submodules methodology of keeping everything in check, every commit. However the benefits of moving dependency tracking to a per-branch basis are thought to be substantial. Or for any given child repository, you may specify the --no-branch option, which will mean no branch information need be recorded. Then the sync will have no effect, and not try to pull in changes automatically from the master.

* GitNest also records repository paths in the .gitignore file. This means that merging two branches of a repository that has been switched over to GitNest is much safer operation on the parent repository. After the merge 'gitnest update' will detect 

## Implementation, contribution

For historical reasons, gitnest has first been implemented as a ruby gem, following on from dchelimsky's excellent rake tasks in the RSpec project. This gem was also heavily influenced by the excellent braid RubyGem. Braid has a much similar interface, however implements the inner repositories as merged subtrees rather than separate repositories / submodules within the parent.

It may be beneficial for the wider community that GitNest be re-writen as a generic command-line tool (bash, C/C++). This would remove the ruby requirement and to have rubygems installed.

If you would like to contribute to GitNest, either the command line version or the Ruby Gem, please see "Todo, feature improvement, suggestions (FIS)" section following.

## Todo, feature improvement, suggestions (FIS)

Its fair to say that the original author of GitNest has little intention to implement any of these features himself. However they are suggested features and currently missing from the RubyGem implementation. For certain users, then many items on this list might be considered 'must haves'.

	* Implement foreach and forall actions.
	* Support for converting from git-submodules -> GitNest.
	* Support for converting from GitNest -> git-submodules.
	* Rudimentary support for converting from GitNest to braid-managed subtrees and back again (harder).
	* A way to import from nested subversion repositories (a script around git-svn clone)
	* A way to update and sync subversion repositories (probably foreach git-svn).
	* A way to automatically fork all the nested repositories for editing/pushing in one action (eg like 'jeweler --create-repo'). This would override the official repositories and re-write the .gitnest file.
	* Command to set-up and maintain global configuration options. 
	* Command to alias git-clone in command-line environment (add to ~/.profile).
	* User-definable path quick-references in global configuration options instead of just -r, -p and -g.

If you have any other feature improvement suggestion, or wish to comment on an existing suggestion please msg your thoughts across to me on GitHub. I'll probably ask you to implement your proposed features, or respond in a non-timely fashion however.

## Failover

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
	ssh://git.sv.gnu.org/srv/git/a2ps.git
	
## Installing

    # Do you have rubygems installed? You can check by typing:
    gem --version
    # Then run the following if you haven't done so before:
    gem sources -a http://gems.github.com
    # Install the gem:
    gem install dreamcat4-gitnest
    
## If you dont know whether the repository is a gitnest repository

Usually you will obtain a repository to work on by typing "git-clone <url>" or "git clone <url>" on the command line. However this will only fetch the parent repository. If you are unsure whether a repository contains nested repositories, then run the following command always:

    gitnest clone <url>
    
This will execute "git-clone", and that attempt to read the .gitnest file recursively in each repository and fetch / initialize any nested repositories. A .gitnest file is very similar to a .gitmodules file. It resides in the root folder of the parent repository and simply contains a list of each child repository, the url to fetch it from, and path specifying its location.

## Using in an existing gitnest repository

If you have cloned a repository already and notice that it contains a .gitnest file, then you can just run an update. Gitnest will notice that there are no nested repositories yet, and automatically download them to the appropriate places.

    gitnest update


## Sample .gitnest configuration file
## .gitnest

GitNest handles generating a '.gitnest' file for your project. Normally you do not edit this file by hand, but instead use "gitnest add" and "gitnest remove" to manipulate your parent repository. However a vaid .gitnest file will usually look something like this:

	--- 
	lib: 
	  branch: master
	  type: git-clone
	  url: git@github.com:dreamcat4/railsapp-lib.git
	vendor/gems/geokit-gem: 
	  branch: development
	  type: git-clone
	  url: git@github.com:dreamcat4/geokit-gem.git
	vendor/plugins/geokit-rails: 
	  type: git-clone
	  url: git@github.com:dreamcat4/geokit-rails.git
	  label: x
	/options/: 
	  branch: master
	  type: git-clone
	  url: git@github.com:dreamcat4/geokit-rails.git

The above specification file defines the following properties:

 * 1 nested sub-repository named 'railsapp-lib' mounted in folder lib/
 * 1 is tracking the latest developments from branch master

 * 2 Ruby gem named 'geokit-gem' in residing in vendor/gems/geokit-gem/ 
 * 2 is tracking a branch named 'development' rather than the default master branch.

 * 3 Ruby plugin 'geokit-rails' in vendor/plugins/geokit-rails/ 
 * 3 has a quickreference label 'x' 
 * 3 has no branch associated with it, so 'git-pull' will not be executed on a 'gitnest update'.

 * 1 was created by running "gitnest add git@github.com:dreamcat4/railsapp-lib.git lib/"
 * 2 was created by running "gitnest add -g git@github.com:dreamcat4/geokit-gem.git --branch development"
 * 3 was created by running "gitnest add -p git@github.com:dreamcat4/geokit-rails.git"

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

## Releasing to GitHub

Jeweler handles releasing your gem into the wild:

    rake release

It does the following for you:

 * Regenerate the gemspec to the latest version of your project
 * Push to GitHub (which results in a gem being build)
 * Tag the version and push to GitHub

