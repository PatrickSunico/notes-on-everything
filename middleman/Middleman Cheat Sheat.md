# Middleman Cheat Sheat

#### Starting Middle Man project

```
middleman init my_new_project
Or
middleman init
```

#### Starting a middleman Server

```
middleman server
```

#### Build from the Source 

```
middleman build
```

#### config.ru 

Describes how the site should be loaded by a Rack-enabled web server. Enable hosting on Heroku and many more.

If you've already initialized a project and just want the config.ru for linking with pow or other development server its contents are simply:

```
require 'middleman/rack'
run Middleman.server
```



## Main Directories

Middleman makes use of the `source`, `build`, `data` and `lib` directories for specific purposes. Each of these directories are children of the main Middleman directory.

#### Source Directory

The `source` directory contains your main website source files to be built, including your templates javascript, CSS and images.

#### Build Directory

The `build` directory is where your static website files will be compiled and exported to.

#### Data Directory

Local Data allows you to create `.yml`, `.yaml` or `.json` files in a folder called `data` and makes this information available in your templates. The `data` folder should be placed in the root of your project i.e. in the same folder as your project's `source` folder. See the [Local Data](https://middlemanapp.com/advanced/data_files/) docs for more information.

#### Lib Directory

The `lib` directory enables you to include external Ruby modules which contain [helpers](https://middlemanapp.com/basics/helper_methods/) for building your application. If you use Rails then you will be familiar with this layout.





