## Welcome to ICS491Lucio - OHA Website

<a href="https://ohagrants.herokuapp.com/"><i class="large github icon"></i>OHA website</a>

# Table of contents

* [Overview: OHA website](#overview-OHA-Website)
  * [Walkthrough for OHA website](#walkthrough-OHA-Website)
* [Installation](#installation)
  * [Branches](#branches)
  * [Heroku Website](#heroku-website)
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
  * [Survey](#survey)
* [Development history](#development-history)
  * [Initial Mockup Pages](#initial-mockup-pages)
  * [HACC 2017](#hacc-2017)
  * [Milestone 1](#milestone-1)
  * [Milestone 2](#milestone-2)
  * [Intial User Study](#initial-user-study)
  * [Conclusion](#conclusion)
* [Personal-Professional-Portfolios](#personal-professional-portfolios)

### Overview OHA Website

### Walkthrough OHA Website

Access the website through: https://ohagrants.herokuapp.com/

OHA website is a angular app that allows the community and government officials to gain access to grants and scholarships for Hawaiian descendants. 
When first accessing th OHA website, everyone will be greeted by the landing page showing any news and updates. It then explains about our team and the website. 

Landing Page:
<img class="ui image" src="/images/landings2.png">

Listing Page:
<img class="ui image" src="/images/edit2.png">

The listing page shows all the grants and scholarships in a list. It also allows the user to filter out and search through all the grants and show data analysis for the selected amount of grants and scholarships. 

QA Page:
<img class="ui image" src="/images/profile2.png">

Answers any pages that a user may have, plan to include tutorial videos or images in the near future.

Contact Us page:
<img class="ui image" src="/images/listings2.png">

Includes how to contact our team and it also includes a google survey form that users can fill out to give back their experience on the website.

### Installation

Optional, [install Atom](https://atom.io/).

First, [install Angular](https://angular.io/guide/quickstart).

Second, [download a copy of Lucio](https://github.com/ICS491TeamLucio/ICS491Lucio.git), or clone it using git.
  
Third, [install Flask](http://flask.pocoo.org/)

Please note you may have to install [the latest version of python](https://www.python.org/downloads/).

If all goes well, the application will appear at [http://localhost:3000](http://localhost:3000) or whatever default server that will be printed onto the screen. 

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

in the master branch before adding or pushing any code to github. This prevents stepping on peoples toes or overriding other people’s codes that they worked on earlier which creates tension and problems. 

Lastly when pushing or pulling from github, merging with the master is required before pushing to github and merging with an issue is required if someone made a change to the master branch. 

```
$ git merge [master -Say if your current branch is issue-XX]
```

### Heroku Website

To use and make a meteor app website, first create an account on the [Heroku website](https://www.heroku.com/) After creating an account, you should have one website that you can deploy for free.

For better instructions please follow this [website](https://progblog.io/How-to-deploy-a-Flask-App-to-Heroku/).

You should then be able to access the website online on: https://nameofapp.herokuapp.com/

### Application Design

### Directory structure

The top-level directory structure contains:

```
src/        # holds the Angular application sources
.gitignore  # doesn't commit .DS_Store, .htaccess, *.pyc
```
However, we will be focusing heavily on the src folder that contains nearly 90% of the file contents. Each directory has its own sub-index that shows all the files in that directory which will be further explained in "Import conventions".

The src/ directory has this top-level structure:

```  
data/
  OHA Test Data Grant_2013_2014_2015_2016_Table.xlsx # function to parse excel file           
  
static/
  bower_components/  # bootstrap, angular ui, suave ui, etc.
  components/        # backend for the listing and visualizers
  css/               # style.css
  images/            # contains all the images used for the site
  js/                # javascript
    app.js/        # javascript for the app
    listing.js/    # javascript for the listing page
    route.js/      # route that connects each webpage to each other

templates/
  contactus.html/	# contact us page
  index.html/	    # landing page
  listings.html/	 # listing page
  qa.html/	       # QA page
     
app.py/           # impoty os - needed for Flask
graph.png/        # generate plot as image passable to frontend
parse_to_json.py/ # used to parse information from excel to data
test.html/        # generate plot as image passable to frontend
tests.py/         # generate plot as image passable to frontend
```

### Import conventions

This system adheres to the Meteor 1.4 guideline of putting all application code in the imports/ directory, and using client/main.js and server/main.js to import the code appropriate for the client and server in an appropriate order.

This system accomplishes client and server-side importing in a different manner than most Meteor sample applications. In this system, every imports/ subdirectory containing any JavaScript or HTML files has a top-level index.js file that is responsible for importing all files in its associated directory.   

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

We use this approach to make it simpler to understand what code is loaded and in what order, and to simplify debugging when some code or templates do not appear to be loaded.  In our approach, there are only two places to look for top-level imports: the main.js files in client/ and server/, and the index.js files in import subdirectories. In those subdirectories, they usually will contain any html, CSS and JavaScript files that the subdirectories will use. 

Note that this two-level import structure ensures that all code and templates are loaded, but does not ensure that the symbols needed in each file are accessible.  So, for example, a symbol bound to a collection still needs to be imported into any file that references it. 
 
### Naming conventions

This system adopts the following naming conventions:

  * Files and directories are named in all lowercase, with words separated by hyphens. Example: accounts-config.js
  * "Global" JavaScript variables (such as collections) are capitalized. Example: Profiles.
  * Other JavaScript variables are camel-case. Example: collectionList.
  * Templates representing pages are capitalized, with words separated by underscores. Example: Directory_Page. The files for this template are lower case, with hyphens rather than underscore. Example: directory-page.html, directory-page.js.
  * Routes to pages are named the same as their corresponding page. Example: Directory_Page.

### Data model

The OHA grant data model is implemented by three JavaScript classes: [ProfileCollection](https://github.com/uhpool/UHPool/blob/master/app/imports/api/profile/ProfileCollection.js) and [AllListingsCollection](https://github.com/uhpool/UHPool/blob/master/app/imports/api/all_listings/AllListingsCollection.js). Both classes encapsulate a MongoDB collection with the same name and export a single variable (Profiles and AllListing)that provides access to that collection. 

Any part of the system that manipulates the UHPool data model imports the Profiles or AllListing variable, and invokes methods of that class to get or set data.

There are many common operations on MongoDB collections. To simplify the implementation, the ProfileCollection and AllListingCollection classes inherit from the [BaseCollection](https://github.com/uhpool/UHPool/tree/master/app/imports/api/base) class.

The [BaseUtilities](https://github.com/uhpool/UHPool/blob/master/app/imports/api/base/BaseUtilities.js) file contains functions that operate across both classes. 

Both ProfileCollection and AllListingCollection have Mocha unit tests in [ProfileCollection.test.js](https://github.com/uhpool/UHPool/blob/master/app/imports/api/profile/ProfileCollection.test.js) and [AllListingCollection.test.js](https://github.com/uhpool/UHPool/blob/master/app/imports/api/all_listings/AllListingsCollection.test.js).

### CSS

The application uses the [Semantic UI](http://semantic-ui.com/) CSS framework. To learn more about the Semantic UI theme integration with Meteor, see [Semantic-UI-Meteor](https://github.com/Semantic-Org/Semantic-UI-Meteor).

The Semantic UI theme files are in [app/client/lib/semantic-ui](https://github.com/ics-software-engineering/meteor-application-template/tree/master/app/client/lib/semantic-ui) directory. Because they are in the client/ directory and not the imports/ directory, they do not need to be explicitly imported to be loaded. (Meteor automatically loads all files into the client that are in the client/ directory). 

Note that the user pages contain a menu fixed to the top of the page, and thus the body element needs to have padding attached to it.  However, the landing page does not have a menu, and thus no padding should be attached to the body element on that page. To accomplish this, the [router](https://github.com/uhpool/UHPool/blob/master/app/imports/startup/client/router.js) uses "triggers" to add a remove the appropriate classes from the body element when a page is visited and then left by the user. 

List of some of the Semantic UI used in this project:
-Dropdown
-Accordion
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

When the application is run, the CAS configuration information must be present in a configuration file such as [config/settings.development.json](https://github.com/ics-software-engineering/meteor-application-template/blob/master/config/settings.development.json). 

Anyone with a UH account can login and use UHPool to create a portfolio. The user will then create their own profile and image (and car image) to gain further access. 

### Authorization

The landing and directory pages are public; anyone can access those pages.

The profile and filter pages require authorization: you must be logged in (i.e. authenticated) through the UH test CAS server, and the authenticated username returned by CAS must match the username specified in the URL.  So, for example, only the authenticated user `johnson` can access the pages `http://localhost:3000/johnson/profile` and `http://localhost:3000/johnson/filter`.

To prevent people from accessing pages they are not authorized to visit, template-based authorization is used following the recommendations in [Implementing Auth Logic and Permissions](https://kadira.io/academy/meteor-routing-guide/content/implementing-auth-logic-and-permissions). 

The application implements template-based authorization using an If_Authorized template, defined in [If_Authorized.html](https://github.com/uhpool/UHPool/blob/master/app/imports/ui/layouts/user/if-authorized.html) and [If_Authorized.js](https://github.com/uhpool/UHPool/blob/master/app/imports/ui/layouts/user/if-authorized.js).

### Configuration

The [config](https://github.com/uhpool/UHPool/tree/master/config) directory is intended to hold settings files.  The repository contains one file: [config/settings.development.json](https://github.com/uhpool/UHPool/blob/master/config/settings.development.json).

The [.gitignore](https://github.com/uhpool/UHPool/blob/master/.gitignore) file prevents a file named settings.production.json from being committed to the repository. So, if you are deploying the application, you can put settings in a file named settings.production.json and it will not be committed. If committed people trying to use your work or pulling your files will suffer dearly. 

UHPool checks on startup to see if it has an empty database in [initialize-database.js](https://github.com/uhpool/UHPool/blob/master/app/imports/startup/server/initialize-database.js), and if so, loads the file specified in the configuration file, such as [settings.development.json](https://github.com/uhpool/UHPool/blob/master/config/settings.development.json).  For development purposes, a sample initialization for this database is in [initial-collection-data.json](https://github.com/uhpool/UHPool/blob/master/app/private/database/initial-collection-data.json). 10 points if you can guess where these names come from for the profile collection.

### Survey

We used an online survey to find suggestions and our target area for people to use our app @ https://goo.gl/forms/mlk02azs1h2AXxCS2

So far as of April only 11 participants filled out the survey and results were quite surprising:
According to the survey, a lot of people didn't know UH Manoa had a carpool system, people would consider giving carpooling a chance and people couldn't think of a solution to find someone to carpool with.

<img class="ui image" src="../images/pool survey 1.JPG">

<img class="ui image" src="../images/pool survey 2.JPG">

<img class="ui image" src="../images/pool survey 3.JPG">

<img class="ui image" src="../images/pool survey 4.JPG">

<img class="ui image" src="../images/pool survey 5.JPG">

## Development-History

### Initial Mockup Pages

Our initial mockup pages before starting our HACC 2017, it contains the brainstorm and paper prototype of our website.

<img width="200px" src="images/landing.png"/>
<img width="200px" src="images/profile-page.png"/>
<img width="200px" src="images/listings-page.png"/>

https://github.com/uhpool/UHPool/projects/1

## HACC 2017

All of this was from our DevPost page for the HACC 2017:

Inspiration:
This was our first hackathon for all of us so we wanted to gain experience in the HACC, so even if we didn't win, we'll still get experience which can be more valuable than the prize. Our team name: Lucio is comes from the game Overwatch that one of our teammate loves to play and that no one else had a better name to come up with. 

What it does:
It's supposed to show all the grants from 2013-2016 in a table that can be filtered with a search bar and a graphical representation (pie chart, bar graph,...) will also appear to give more useful data on the grants. Such as the amount of grants that was awarded in 2013 or the type of different grants that are around one hundred thousand dollars. This allows any users such as a government official get a better idea on the grants being issued or to a grant recipient trying to find which grant fits their needs.  

How we built it:
For our editor we used Atom, for our front-end and back-end we used Angular, for our web framework we used Flask and for our deployment we used Heroku. We mostly used HTML, Python, and other basic languages used to build a website.

Challenges we ran into:
A lot of the team was busy with school and work so it was hard for everyone to get together to work on the project. Some folks were so busy that they had little or no time to do any work on the project which cut down our manpower. Some of the smaller challenges that we faced were implementing some of our ideas into the project such as creating analysis tools or even deploying the project on Heroku. Other challenges was the understanding of using angular and Flask. For example a lot of us were unfamiliar with using Angular however fortunately we had one teammate who was a professional at using Angular. For Flask we were unable to solve the problem where we couldn't get the website to automatically refresh after we made some changes to our project. 

Accomplishments that we are proud of:
Being able to complete a functional website by the due date, there were a couple of times where we thought we wouldn't be able to make the deadline due to some difficulties. However we're proud to meet the requirements of having a functional website such as our search engine through the multiple grants in the table and the analysis tools that display useful information to the user. We know that our project may be limited and unappealing to the eyes but we're proud of our project. 

What we learned:
That college students are busy with school, work and life. We had a hard time trying to balance between schedules and this hackathon. In fact we had some folks that were unable to meet up until a week after the first check in. However some of us did learn a lot about project management, new applications, new software's and a good refresher for some of us. For the most part a lot of us learned about tools and languages that we didn't know about until the hackathon. Such as Angular, Flask and even Python for some of us. Other new things that we learned was how to create analysis tools such as pie charts or bar graphs for our database in our website. 

What is next for Lucio:
Learning from our mistakes to improve ourselves in the future or the next HACC. We do have two people that plan on continuing this project after the HACC. We're hoping to expand on this project and implement ideas that we weren't able to be completed during the HACC. Such as adding more information on the grants that are being issued, tutorial videos that explain how the website work and making the overall appearance of the website better. 

HACC 2017:
<a href="https://devpost.com/software/lucio">HACC 2017 Devpost</a>

<img class="ui image" src="/images/DevPost.jpg">

Milestone 1:

<a href="https://github.com/ICS491TeamLucio/ICS491Lucio/projects/1"><i class="large github icon"></i>Milestone Project 1</a>

<img class="ui image" src="/images/Milestone2.png">

Network:
<img class="ui image" src="/images/Network.png">

HACC 2017 began on August 26th, 2017 and ended on September 23rd, 2017

<img width="200px" src="images/landings2.png"/>
<img width="200px" src="images/profile2.png"/>
<img width="200px" src="images/edit2.png"/>
<img width="200px" src="images/listings2.png"/>
<img width="200px" src="images/mylistings2.png"/>

## Personal Professional Portfolios

Before we begin here's our personal professional portfolios:
<a href="https://jjhna.github.io/">Jonathan Na's professional portfolio

<a href="">Vincent professional portfolio
