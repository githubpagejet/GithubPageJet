# GithubPageJet - easy way to build a Github page
![Logo](https://raw.githubusercontent.com/githubpagejet/GithubPageJet/master/resources/pageJet-logo.png)

GithubPageJet is an application which can be used to build a website on the top of "Github pages" service *(check: https://pages.github.com)*. It covers the following goals:
- **Easy way to build a website on "Github pages"**.
- To work **as light as possible**, without additional heavy libraries and dependencies.

Also:
- Implemented **jQuery** and option to execute **AJAX requests**.
- Implemented **java script Routing**.
- Implemented **front-end template engine - Handlebars**.
- Implemented **grunt**.
- **Multilingual** templating.
- Option to use the application on **your own server**.

**Table of Contents**
- [Used technologies](#used-technologies)
- [Installation](#installation)
  - [Use with Github Pages](#use-with-github-pages)
    - [First step](#first-step)
    - [Build a development environment](#build-a-development-environment)
      - [Checkout](#checkout)
      - [Install services](#install-services)
      - [Configuration](#configuration)
      - [File permissions](#file-permissions)  
      - [Web service](#web-service)
        - [Nginx](#nginx)	
      - [Task runners and package managers](#task-runners-and-package-managers)
    - [Development](#development)
  - [Use with your own server](#use-with-your-own-server)  
- [Structure of the application](#structure-of-the-application)
  - [File structure](#file-structure)
- [Contributing](#contributing)
- [License](#license)
	
# Used technologies
The following technologies and libraries have been used:

**Front-end:**
- JavaScript:
	- jQuery
	- JS app (router /Url: [http://krasimirtsonev.com/blog/article/A-modern-JavaScript-router-in-100-lines-history-api-pushState-hash-url](http://krasimirtsonev.com/blog/article/A-modern-JavaScript-router-in-100-lines-history-api-pushState-hash-url)/, controllers, data manager, template engine, translations).
- Templates:
	- HTML5
	- CSS3
	- LESS or SASS
	- Handlebarsjs

**Server-side services:**
If you plan to use Handlebars and the minification option for CSS and JS, you might be interested in the services below:
- npm
- grunt
- npm task "handlebars" used for precompilation of the handlebar templates
- npm task "uglify" used for javascript minimization
- npm task "less" used for the compilation of LESS to CSS
- npm task "sass" used for the compilation of SASS to CSS
- npm task "watch" used to watch for changes in particular directories and files and if there are changes, it will automatically call the default grunt task which will call all other tasks (handlebars, uglify, less, sass)*.

# Installation

## Use with "Github Pages"
### First step
Go to https://pages.github.com and follow the instructions describing how to create a repository which can host your website. Skip this step if you already have done this.

### Build a development environment
If you want to use the full capacity of the application you will have to install a development environment on your own machine.

This will allow you to use - Handlebars template engine and to minify the CSS and JavaScript file. Skip this chapter if you don't need these option and go to ...

#### Checkout
Checkout the project on your machine:
```
git clone https://github.com/githubpagejet/GithubPageJet
```
For the current example, the name of the directory where we will clone and install the project is “example”.

Go to the directory of the project:
```
cd example
```

#### Install services
Install the following services:
- **npm**, follow the instructions from here: [https://github.com/npm/npm](https://github.com/npm/npm)
- **grunt**, follow the instructions from here: [http://gruntjs.com/getting-started](http://gruntjs.com/getting-started)
- also the following npm tasks for grunt are required: 
	- **uglify**, follow the instructions from here: [https://github.com/gruntjs/grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify )
	- **handlebars**, follow the instructions from here: [https://github.com/gruntjs/grunt-contrib-handlebars](https://github.com/gruntjs/grunt-contrib-handlebars) 
	- **less**, follow the instructions from here: [https://github.com/gruntjs/grunt-contrib-less](https://github.com/gruntjs/grunt-contrib-less) 
	- **sass**, follow the instructions from here: [https://github.com/gruntjs/grunt-contrib-sass](https://github.com/gruntjs/grunt-contrib-sass)	
	- **watch**, follow the instructions from here: [https://github.com/gruntjs/grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch)

Sometimes during the installation of grunt and the npm tasks it is possible to encounter errors and missing dependencies. A possible solution is to delete the folder “node_modules” and to start the process of installation again.
```
rm -R node_modules/
```

#### Configuration
Go to /js/config.js and set up the following:
```js
...
	"data_manager": {        
        "api": "URL-OF-THE-API"
	},
...
```
Example:
```js
...
	"data_manager": {        
        "api": "https://api.somewebsite.com"
	},
...
```
Save and close.

When we finish with your configurations, you can add the config files to .gitignore which will protect them to be rewritten. Add this to .gitignore:
```js
	/js/config.js
```

#### File permissions
Execute the following:
```
chmod -R 0755 example/
chown -R www-data example/
chgrp -R www-data example/
```

#### Web service
For the development of our application, we will need to configure a domain name which points to our server *(make sure this is already done before you continue)* and also to configure the web aplication which will make a relation between our domain name and our directory where we keep the files of our application.

##### Nginx
Use the configurations from files:
```
nginx-80.conf
nginx-443.conf
```
Check the validity of the configuration and reload:
```
nginx -t
service nginx reload
```

#### Task runners and package managers
Grunt and all npm tasks which we’ve already installed, have the following purpose.

```
grunt less
```
Compiles the LESS file */css/style.less* and converts it to the CSS file */css/style.css*.

```
grunt sass
```
Compiles the SASS file */css/style.scss* and converts it to a the CSS file */css/style.css*.

```
grunt uglify
```
Takes all javascript files, minimize them and unites them in one single file named */js/lib.min.js*.

```
grunt handlebars
```
Takes all handlebar templates located in *"/templates"* and pre-compiles them. The pre-compiled templates are saved in */js/templates.js*.

```
grunt
```
Executes all npm tasks listed above.

```
grunt watch
```
If you run this task in the terminal, grunt will start a script which will watch for changes in all directories and files which are related to all above tasks. If there are changes, the script will automatically run the default grunt task which on other hand will run all other tasks. As a consequence, all LESS/SASS files will be regenerated, all js files will be minified, all handlebars templates will be pre-complited, etc.

### Development
If we follow the steps above, we will have a working environment which can be accessed with our web browser and domain name which is configured for the purposes.

Once we are ready with the development, we have to generate Handlebars JS templates and to minify the CSS and JavaScript files:

```
grunt
```

Now we can commit and deploy to our github repository.

## Use with your own server
If you want to use the applicatin on your own server, simply follow the steps above except the one where we deploy to Github.

# Structure of the application

More detail document will be provided in the future.

## File structure
Before we start with the development of our own application, we will need to know the purpose of each file and folder part of the framework.

```
assets/ - assets like images, fonts, etc.

css/ - CSS and LESS/SASS files

js/

  controllers/ - here are all controllers used by the javascript application

	lib/ - contains important javascript libraries like jquery, handlebars, routers, etc.
			
		dataManager.js - data manager used to executed AJAX requests to the API or other service
			
		handlebars.min.js - Handlebars front-end template engine
			
		jquery.min.js - jQuery
			
		router.js - Router
		
	translations/ - translations required by the javascript application
		
	app.js - starting point of the javascript application
		
	config.js - configuration of the js application
		
	custom.min.js - this is minimized js file generated by grunt
		
	lib.min.js - this is minimized js file generated by grunt
		
	routes.js - routes required by the javascript application
		
	templates.js - handlebars pre-compiled templates

templates/ - Handlebars templates used by the front-end
	
.gitignore - ignore file for git

contact-us.html - example page (you can delete it)

page.html - example page (you can delete it)

index.html - example page (you can reuse it)
	
robots.txt - robots file for the search engines

Gruntfile.js - grunt configuration

package.json - grunt configuration

```

# Contributing
How can you contribute:

1. Fork it.
2. Create your feature branch (git checkout -b my-new-feature).
3. Commit your changes (git commit -am 'Added some feature').
4. Push to the branch (git push origin my-new-feature).
5. Create a new Pull Request.

# License
GithubPageJet is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).
