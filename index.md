## Welcome to UHPool

<a href="http://uhpool.meteorapp.com/"><i class="large github icon"></i>UHPool Meteor App</a>

# Table of contents

* [Overview: UHPool](#overview-uhpool)
  * [Walkthrough for UHPool](#walkthrough-uhpool)
* [Installation](#installation)
  * [Branches](#branches)
  * [Meteor Website](#meteor-website)
* [Application design](#application-design)
  * [Directory structure](#directory-structure)
  * [Import conventions](#import-conventions)
  * [Naming conventions](#naming-conventions)
  * [Data model](#data-model)
  * [CSS](#css)
  * [Routing](#routing)
  * [Authentication](#authentication)
  * [Authorization](#authorization)
  * [Configuration](#configuration)
  * [Quality Assurance](#quality-assurance)
    * [ESLint](#eslint)
  * [Survey](#survey)
* [Development history](#development-history)
  * [Initial Mockup Pages](#initial-mockup-pages)
  * [Milestone 1](#milestone-1)
  * [Milestone 2](#milestone-2)
  * [Milestone 3](#milestone-3)

### Overview UHPool

### Walkthrough UHPool

UHPool is a meteor app that allows people to find other carpoolers in the UH community. 
When first accessing UHPool, everyone will be greeted by the landing page showing why they should join to use the app and images that show a preview of the app.

Landing Page:
<img class="ui image" src="/images/landings2.png">

After loging in, new users will have to put in their personal information and if they're looking for a driver or passenger to carpool with. 

Profile Page:
<img class="ui image" src="/images/profile2.png">

The edit profile page allows any current users to change any information.

Edit Profile:
<img class="ui image" src="/images/edit2.png">

The listing page will show other users on the app that are also looking for other carpoolers. In this page you can search for a driver or pasenger at certain time periods that fit your schedule. 

Listing page:
<img class="ui image" src="/images/listings2.png">

Your listing page that to set up the time and dates when you are available to carpool with someone. 

My Listing Page:
<img class="ui image" src="/images/mylistings2.png">

### Installation

First, [install Meteor](https://www.meteor.com/install).

Second, [download a copy of UHPool](https://github.com/uhpool/uhpool.github.io/zipball/master), or clone it using git.
  
Third, cd into the app/ directory and install libraries with:

```
$ meteor npm install
```

Fourth, run the system with:

before using this code make sure that your package.json file contains this code:

```
{
  "name": "uhpool",
  "private": true,
  "scripts": {
    "start": "meteor --no-release-check --settings ../config/settings.json",
    "develop": "meteor --no-release-check --settings ../config/settings.development.json",
    "lint": "eslint ./imports",
    "jsdoc": "./node_modules/.bin/jsdoc -c ./jsdoc.json -r .",
    "test-watch": "meteor test --driver-package practicalmeteor:mocha --port 3100",
    "deploy": "DEPLOY_HOSTNAME=galaxy.meteor.com meteor deploy uhpool.meteorapp.com --owner ics314s17 --settings ../config/settings.json"
  }
```

This gives a shortcut for the command line into:

```
$ meteor npm run start
```

which is more perferable to type than:

```
meteor --no-release-check --settings ../config/settings.json",
```

If all goes well, the application will appear at [http://localhost:3000](http://localhost:3000). If you have an account on the UH test CAS server, you can login.  

### Branches

When creating a new branch for the project use issue-XX naming conventions. This prevents people from working on the master branch to prevent any future issues. Each issue is to work on different issues or problems that require attention such as "working on the landing page".

```
$ git checkout -b issue-XX
```

Try to avoid using the master branch as it is the primary focus that the app will run on.

To change branches:

```
$ git checkout [issue-XX or master]
```

Make sure to do:

```
$ git pull
```

in the master branch before adding or pushing any code to github. This prevents stepping on peoples toes or overriding other peoples codes that they worked on earlier which creates tension and problems. 

Lastly when pushing or pulling from github, merging with the master is required before pushing to github and merging with an issue is required if someone made a change to the master branch. 

```
$ git merge [master -Say if your current branch is issue-XX]
```

### Meteor Website

To use and make a meterapp website, first create an account on the [Galaxy website](https://galaxy.meteor.com/) After creating an account it will be costly in order to push any website onto galaxy so it's best to work with an organization that has limited slots but will be able to push your website for free. 

For more instructions please at [E54: Test deploy to Galaxy](http://courses.ics.hawaii.edu/ics314s17/morea/deployment/experience-test-deployment.html)

To avoid typing in 3-4 additional lines of codes put this code snippet into the ...app/package.json file:

```
{
  "name": "uhpool",
  "private": true,
  "scripts": {
    "start": "meteor --no-release-check --settings ../config/settings.json",
    "develop": "meteor --no-release-check --settings ../config/settings.development.json",
    "lint": "eslint ./imports",
    "jsdoc": "./node_modules/.bin/jsdoc -c ./jsdoc.json -r .",
    "test-watch": "meteor test --driver-package practicalmeteor:mocha --port 3100",
    "deploy": "DEPLOY_HOSTNAME=galaxy.meteor.com meteor deploy uhpool.meteorapp.com --owner ics314s17 --settings ../config/settings.json"
  }
```
This allows you to deploy your code onto your galaxy app by typing:

```
$ meteor npm run deploy
```

### Application Design

### Directory structure

The top-level directory structure contains:

```
app/        # holds the Meteor application sources
config/     # holds configuration files, such as settings.development.json
.gitignore  # don't commit IntelliJ project files, node_modules, and settings.production.json
```
However we will be focusing heavily on the app folder that contains nearly 90% of the file contents. The config/settings.development.json file contains the login information for the user that connects with the UH network. Each directory has its own sub-index that shows all the files in that directory which will be further explained in "Import conventions".

The app/ directory has this top-level structure:

```
client/
  lib/           
  head.html      # the <head>
  main.js        # import all the client-side html and js files (important if creating or deleting any directories)

imports/
  api/           # Define collection processing code (client + server side)
    base/        # BaseCollection is an abstract superclass of all RadGrad data model entities.
    interest/    # Represents a specific interest, such as "Software Engineering".
    profile/     # Profiles provide portfolio data for a user.
    user_accepted_listings/
  startup/       # Define code to run when system starts up (client-only, server-only)
    client/      # Contains the router and user account-configuration.js page to link other pages
    server/      # Initializes the database and publish the interest and profiles.
  ui/
    components/  # templates that appear inside a page template.
    layouts/     # Layouts contain common elements to all pages (i.e. menubar and footer)
    pages/       # Pages are navigated to by FlowRouter routes.
    stylesheets/ # CSS customizations, if any.

node_modules/    # managed by Meteor

private/
  database/      # holds the JSON file used to initialize the database on startup.

public/          
  images/        # holds static images for the website
  
server/
   main.js       # import all the server-side js files.
```

### Import conventions

This system adheres to the Meteor 1.4 guideline of putting all application code in the imports/ directory, and using client/main.js and server/main.js to import the code appropriate for the client and server in an appropriate order.

This system accomplishes client and server-side importing in a different manner than most Meteor sample applications. In this system, every imports/ subdirectory containing any Javascript or HTML files has a top-level index.js file that is responsible for importing all files in its associated directory.   

Then, client/main.js and server/main.js are responsible for importing all the directories containing code they need. For example, here is the contents of client/main.js:

```
import '/imports/startup/client';
import '/imports/ui/components/form-controls';
import '/imports/ui/components/directory';
import '/imports/ui/components/user';
import '/imports/ui/components/landing';
import '/imports/ui/layouts/directory';
import '/imports/ui/layouts/landing';
import '/imports/ui/layouts/shared';
import '/imports/ui/layouts/user';
import '/imports/ui/pages/directory';
import '/imports/ui/pages/filter';
import '/imports/ui/pages/landing';
import '/imports/ui/pages/listing';
import '/imports/ui/pages/mylistings';
import '/imports/ui/pages/user';
import '/imports/ui/pages/edit';
import '/imports/ui/stylesheets/style.css';
import '/imports/api/base';
import '/imports/api/profile';
import '/imports/api/interest';
import '/imports/api/user_accepted_listings';
```

Apart from the last line that imports style.css directly, the other lines all invoke the index.js file in the specified directory.

We use this approach to make it more simple to understand what code is loaded and in what order, and to simplify debugging when some code or templates do not appear to be loaded.  In our approach, there are only two places to look for top-level imports: the main.js files in client/ and server/, and the index.js files in import subdirectories. In those subdirectories they usually will contain any html, css and javascript files that the subdirectories will use. 

Note that this two-level import structure ensures that all code and templates are loaded, but does not ensure that the symbols needed in a given file are accessible.  So, for example, a symbol bound to a collection still needs to be imported into any file that references it. 
 
### Naming conventions

This system adopts the following naming conventions:

  * Files and directories are named in all lowercase, with words separated by hyphens. Example: accounts-config.js
  * "Global" Javascript variables (such as collections) are capitalized. Example: Profiles.
  * Other Javascript variables are camel-case. Example: collectionList.
  * Templates representing pages are capitalized, with words separated by underscores. Example: Directory_Page. The files for this template are lower case, with hyphens rather than underscore. Example: directory-page.html, directory-page.js.
  * Routes to pages are named the same as their corresponding page. Example: Directory_Page.

### Data model

The UHPool data model is implemented by three Javascript classes: [ProfileCollection](https://github.com/uhpool/UHPool/blob/master/app/imports/api/profile/ProfileCollection.js) and [AllListingsCollection](https://github.com/uhpool/UHPool/blob/master/app/imports/api/all_listings/AllListingsCollection.js). Both of these classes encapsulate a MongoDB collection with the same name and export a single variable (Profiles and AllListing)that provides access to that collection. 

Any part of the system that manipulates the UHPool data model imports the Profiles or AllListing variable, and invokes methods of that class to get or set data.

There are many common operations on MongoDB collections. To simplify the implementation, the ProfileCollection and AllListingCollection classes inherit from the [BaseCollection](https://github.com/uhpool/UHPool/tree/master/app/imports/api/base) class.

The [BaseUtilities](https://github.com/uhpool/UHPool/blob/master/app/imports/api/base/BaseUtilities.js) file contains functions that operate across both classes. 

Both ProfileCollection and AllListingCollection have Mocha unit tests in [ProfileCollection.test.js](https://github.com/uhpool/UHPool/blob/master/app/imports/api/profile/ProfileCollection.test.js) and [AllListingCollection.test.js](https://github.com/uhpool/UHPool/blob/master/app/imports/api/all_listings/AllListingsCollection.test.js).

### CSS

The application uses the [Semantic UI](http://semantic-ui.com/) CSS framework. To learn more about the Semantic UI theme integration with Meteor, see [Semantic-UI-Meteor](https://github.com/Semantic-Org/Semantic-UI-Meteor).

The Semantic UI theme files are located in [app/client/lib/semantic-ui](https://github.com/ics-software-engineering/meteor-application-template/tree/master/app/client/lib/semantic-ui) directory. Because they are located in the client/ directory and not the imports/ directory, they do not need to be explicitly imported to be loaded. (Meteor automatically loads all files into the client that are located in the client/ directory). 

Note that the user pages contain a menu fixed to the top of the page, and thus the body element needs to have padding attached to it.  However, the landing page does not have a menu, and thus no padding should be attached to the body element on that page. To accomplish this, the [router](https://github.com/uhpool/UHPool/blob/master/app/imports/startup/client/router.js) uses "triggers" to add an remove the appropriate classes from the body element when a page is visited and then left by the user. 

List of some of the Semantic UI used in this project:
-Dropdown
-Accordian
-Cards
-Grid
-Container
-Table

### Routing

For display and navigation among its four pages, the application uses [Flow Router](https://github.com/kadirahq/flow-router).

Routing is defined in [imports/startup/client/router.js](https://github.com/ics-software-engineering/meteor-application-template/blob/master/app/imports/startup/client/router.js).

UHPool defines the following routes:

  * The `/` route goes to the public landing page.
  * The `/directory` route goes to the public directory page.
  * The `/<user>/profile` route goes to the profile page associated with `<user>`, which is the UH account name.
  * The `/<user>/filter` route goes to the filter page associated with `<user>`, which is the UH account name.

### Authentication

For authentication, the application uses the University of Hawaii CAS test server, and follows the approach shown in [meteor-example-uh-cas](http://ics-software-engineering.github.io/meteor-example-uh-cas/).

When the application is run, the CAS configuration information must be present in a configuration file such as  [config/settings.development.json](https://github.com/ics-software-engineering/meteor-application-template/blob/master/config/settings.development.json). 

Anyone with a UH account can login and use UHPool to create a portfolio. The user will then create their own profile and image (and car image) in order to gain further access. 

### Authorization

The landing and directory pages are public; anyone can access those pages.

The profile and filter pages require authorization: you must be logged in (i.e. authenticated) through the UH test CAS server, and the authenticated username returned by CAS must match the username specified in the URL.  So, for example, only the authenticated user `johnson` can access the pages `http://localhost:3000/johnson/profile` and  `http://localhost:3000/johnson/filter`.

To prevent people from accessing pages they are not authorized to visit, template-based authorization is used following the recommendations in [Implementing Auth Logic and Permissions](https://kadira.io/academy/meteor-routing-guide/content/implementing-auth-logic-and-permissions). 

The application implements template-based authorization using an If_Authorized template, defined in [If_Authorized.html](https://github.com/uhpool/UHPool/blob/master/app/imports/ui/layouts/user/if-authorized.html) and [If_Authorized.js](https://github.com/uhpool/UHPool/blob/master/app/imports/ui/layouts/user/if-authorized.js).

### Configuration

The [config](https://github.com/uhpool/UHPool/tree/master/config) directory is intended to hold settings files.  The repository contains one file: [config/settings.development.json](https://github.com/uhpool/UHPool/blob/master/config/settings.development.json).

The [.gitignore](https://github.com/uhpool/UHPool/blob/master/.gitignore) file prevents a file named settings.production.json from being committed to the repository. So, if you are deploying the application, you can put settings in a file named settings.production.json and it will not be committed. If commited people trying to use your work or pulling your files will suffer dearly. 

UHPool checks on startup to see if it has an empty database in [initialize-database.js](https://github.com/uhpool/UHPool/blob/master/app/imports/startup/server/initialize-database.js), and if so, loads the file specified in the configuration file, such as [settings.development.json](https://github.com/uhpool/UHPool/blob/master/config/settings.development.json).  For development purposes, a sample initialization for this database is in [initial-collection-data.json](https://github.com/uhpool/UHPool/blob/master/app/private/database/initial-collection-data.json). 10 points if you can guess where all of these names come from for the profile collection.

### Quality Assurance

### ESLint

UHPool includes a [.eslintrc](https://github.com/uhpool/UHPool/blob/master/app/.eslintrc) file to define the coding style adhered to in this application. You can invoke ESLint from the command line as follows:

```
meteor npm run lint
```

ESLint should run without generating any errors.  

It's significantly easier to do development with ESLint integrated directly into your IDE (such as IntelliJ).

### Survey

We used an online survey to find suggestions and our target area for people to use our app @ https://goo.gl/forms/Sn69DvwC8G04OhqN2

So far as of april only 11 particpates filled out the survey and results were quite surprising:
According to the survey, a lot of people didn't know UH Manoa had a carpool system, people would consider giving carpooling a chance and people couldn't think of a solution to find someone to carpool with.

<img class="ui image" src="../images/pool survey 1.JPG">

<img class="ui image" src="../images/pool survey 2.JPG">

<img class="ui image" src="../images/pool survey 3.JPG">

<img class="ui image" src="../images/pool survey 4.JPG">

<img class="ui image" src="../images/pool survey 5.JPG">

## Development-History

### Initial Mockup Pages

Our initial mockup pages before starting our milestone 1, using the digits template. 

<img width="200px" src="images/landing.png"/>
<img width="200px" src="images/profile-page.png"/>
<img width="200px" src="images/listings-page.png"/>
<img width="200px" src="images/yourListings.png"/>

https://github.com/uhpool/UHPool/projects/1

### Milestone 1

For Milestone 1 five mockup pages were created for the project. The goal of milestone 1 was to create mockups of our app using the meteor app. The site allowed people to log in using their UH account however a lot of the functions in the website weren't fully operational. Milestone 1 started on April 5th 2017 and ended on April 13th 2017. 

<img width="200px" src="images/landings.png"/>
<img width="200px" src="images/profile.png"/>
<img width="200px" src="images/edit.png"/>
<img width="200px" src="images/listings.png"/>
<img width="200px" src="images/mylistings.png"/>

Milestone 1:
<a href="https://github.com/uhpool/UHPool/projects/1"><i class="large github icon"></i>Milestone Project 1</a>

<img class="ui image" src="/images/Milestone1.png">

Milestone2:

<a href="https://github.com/uhpool/UHPool/projects/2"><i class="large github icon"></i>Milestone Project 2</a>

<img class="ui image" src="/images/Milestone2.png">

Network:
<img class="ui image" src="/images/Network.png">

### Milestone 2
For Milestone 2 addtional fixes were added to the project, the landing page images were updated, created schema for user and listing page, connected various pages to the database, created a user and developer guide and created a footer and header.
Milestone 2 started on April 13th 2017 and ended on April 27, 2017

<img width="200px" src="images/landings2.png"/>
<img width="200px" src="images/profile2.png"/>
<img width="200px" src="images/edit2.png"/>
<img width="200px" src="images/listings2.png"/>
<img width="200px" src="images/mylistings2.png"/>

Milestone 2:
<a href="https://github.com/uhpool/UHPool/projects/2"><i class="large github icon"></i>Milestone Project 2</a>

<img class="ui image" src="/images/milestone2b.png">

Milestone 3:
<a href="https://github.com/uhpool/UHPool/projects/3"><i class="large github icon"></i>Milestone Project 3</a>

<img class="ui image" src="/images/milestone3b.png">

Network: 
<img class="ui image" src="/images/network2.png">

### Milestone 3

Milestone 3:
--add milestone 2 image here--

Network: 
--add network image here--
