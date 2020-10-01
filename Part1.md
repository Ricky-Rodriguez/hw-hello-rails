# Part 1: Set up the new app

## Verify you have Ruby and Rails installed

Open a terminal window in your development environment.

`ruby -v` tells you the installed Ruby version; if it fails, you 
need to install Ruby--we recommend using [rvm (Ruby Version Manager)](https://rvm.io), 
which allows you to maintain multiple installed versions of Ruby and 
gem sets, useful if you're working on multiple apps that use different Ruby versions.
You should have Ruby 2.6 or later.

`rails -v` tells you the installed version of the Rails framework. 
If none is installed, say `gem install rails --version=4.2.10`, which is
the Rails version that this assignment was tested with.  (It may work with 
Rails 5 or 6 but is not guaranteed to.)

You will also need the `bundler` gem.  If `bundle -v` fails, `gem install bundler`
to install it.

## Create a new Rails app

Now that you have Ruby and Rails installed, create a new, empty
Rails app with the command `rails new rottenpotatoes --skip-test-unit --skip-turbolinks`.

The options tell Rails to omit three aspects of the new app:

* Rather than Ruby's `Test::Unit` framework, in a future assignment we will instead
create
tests using the RSpec framework.

* Turbolinks is a piece of trickery that uses AJAX behind-the-scenes to speed
up page loads in your app.  However, it causes so many problems with JavaScript
event bindings and scoping that we strongly recommend against using it.  A well
tuned Rails app should not benefit much from the type of speedup Turbolinks provides.

If you're interested, `rails new --help` shows more options available
when creating a new app.



If all goes well, you'll see several messages about files being created,
ending with `run bundle install`, which may take a couple of minutes to complete.  
You can now `cd` to the
newly-created `rottenpotatoes` directory, called the **app root**
directory for your new app.  From now on, unless we say otherwise, all
file names will be relative to the app root.  Before going further,
spend a few minutes examining the contents of the new app directory
`rottenpotatoes` to remind yourself with some of
the directories common to all Rails apps.

What about that message `run bundle install`?
Recall that Ruby libraries are packaged as "gems", and the tool
`bundler` (itself a gem) tracks which versions of which libraries a
particular app depends on.
Open the file called `Gemfile` --it might surprise you that there are 
already gem names in this file even though you haven't written any
app code, but that's because Rails itself is a gem and also depends on
several other gems, so the Rails app creation process creates a 
default `Gemfile` for you.  For example, 
you can see that `sqlite3` is listed, because the default
Rails development environment expects to use the SQLite3 database.

## Issues with bundler versions
You may get the following error message:
```
Bundler could not find compatible versions for gem "bundler":
  In Gemfile:
    rails (= 4.2.10) was resolved to 4.2.10, which depends on
      bundler (< 2.0, >= 1.3.0)

  Current Bundler version:
    bundler (2.1.4)
```

To resolve it, we recommend running the following commands:
```
gem uninstall bundler   # To remove all existing bundler installations eg. 2.1.4
gem install bundler -v '1.17.3'  # To install compartible bundler version
```

You may have a default bundler version that will not allow you to uninstall with the following error:
```
Gem bundler-2.1.4 cannot be uninstalled because it is a default gem
```

Run the following command to find and delete files that match the format `bundler*`:
```
cd $(ruby -e "puts Gem::Specification.default_specifications_dir") && rm -rf bundler*
```

If you had issues with bundler earlier you may want to run `bundle install` now.


## Work around the SQLite3 gem bug in v1.4

Rails uses the SQLite3 database as the default for development and testing.  
Unfortunately, versions of the SQLite3 gem starting with 1.4.0 introduced
a bug that make it no longer work properly with Rails.  To work around this,
we must force Rails to use an earlier version, namely any version beginning
with 1.3.  Locate the line in the `Gemfile` that specifies the `sqlite3` gem
and change it to read as follows:

`gem 'sqlite3', '~> 1.3.0'`

Then run `bundle update` and verify that its output contains "Using sqlite3 1.3.x" 
where x is any minor version.


## Check your work

To make sure everything works, change into the app's root directory and
start the app locally by typing `rails server`, which starts the WEBrick app server
listening on port 3000.  Then in a web browser
visit `localhost:3000` and you should see the generic Ruby on Rails landing page, 
which is actually being served by your app.  Later we will define our own routes
so that the "top level" page does not default to this banner.


## Commit your work
At this stage, everything is working and you should initialize a git repo in your app's folder and commit your app.
Here is an [example `.gitignore` file for Rails on Github](https://github.com/github/gitignore/blob/master/Rails.gitignore).
You should frequently commit your code from now on.
