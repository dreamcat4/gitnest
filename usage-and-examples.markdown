## Workflow

 * Hack, commit, hack, commit, etc, etc
 * `rake version:bump:patch release` to do the actual version bump and release
 * Have a delicious scotch
 * Install [gemstalker](http://github.com/technicalpickles/gemstalker), and use it to know when gem is built. It typically builds in a few minutes, but won't be installable for another 15 minutes.


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

