#### 1. RVM gemset list 

lists all available gemsets

```unix
rvm gemset list_all
```





#### 2. Upgrading Ruby with RVM

```unix
rvm install ruby-2.3.1
```

OR Specify which version 

```unix
rvm install ruby-"version number"
```



#### 3. Upgrading RubyGems Gem manager

This updates RubyGems to the stable and latest version

```unix
$ gem update --system
```

#### 4. Creating new Gemsets for Ruby, and Ruby on Rails version specific environments.

##### Gemset ruby 2.3.1@rails5.0

This creates a gemset which we can use if we are using ruby version 2.3.1 at rails version 5.0.0

```unix
rvm use ruby-2.3.1@rails5.0 --create
```

##### After that we need to install rails:

Rails using newest release candidate,

```unix
gem install rails --version=5.0.0
```

Check rails version if rails 5.0.0

```unix
rails -v
```

##### Gemset ruby 2.3.1@rails4.2(stable)

The same process as above

```unix
rvm use ruby-2.3.1@rails4.2.6 --create
```

Rails using the stable version, fallback for most applications

```unix
gem install rails --version=4.2.6
```

Check rails version if rails 4.2.6

```unix
rails -v
```

#### 5. Setting up a Default Gemset

Setting up Rails 5.0.0 as a default gemset when were creating a new project folder

```unix
rvm use ruby-2.3.1@rails5.0.0

rails -v // To check which version 
rvm use 2.3.1@rails5.0.0 --default
```

#### 6. New Rails Application(4.2.6 Version)

Creating a project spefic gemset with rvm

We can also specify which rails version to install

```unix
# No Quotes
mkdir "myapp"
cd myapp
rvm use ruby-2.3.1@rails4.2.6 --create
gem install rails or gem install rails --version="5.0.0" 
rails new . // generates the application folder in the root folder
```



#### Updating Global Gemsets (Always update Global Gemsets)

Here we can install all specific gems that get inherited by other gems

Use gemset global

```unix
rvm gemset use global
```

Then do a gem list to list which gems are installed

```unix
gem list
```

Lists all outdated gems

```unix
gem outdated
```

Update installed gems to latest versions

```unix
gem update
```





