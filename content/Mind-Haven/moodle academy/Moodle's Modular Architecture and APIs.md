---
title: Moodle Academy
tags:
  - moodle-academy
  - architecture
  - API
cssclasses: 
author: Ram
---
## Authentication
these plugins use hooks to enhance or replace parts of the user authentication flow
standard authentication method including authenticating against internally stored password, LDAP Or OAuth2 providers such as google/Facebook
installed in the `/auth/` directory

## Enrollment
controls who is enrolled into the courses and what role

common features:
roles - assigning a particular role in the given course, student or teacher etc..
installed in the `/enrol/` directory in moodle

## Course format
determines layout of the course main page

common features:
navigation - navigation tree inside the course, which is then displayed in the navigation widget
sections  - core-level concept that every course format can interpret and use in its own way
backup and restore - course format data is included in the course backup 
installed in the `/course/format` directory of moodle

## Admin tools
utilities for various site administration and maintenance tasks.

## Release and version numbers
properties in the plugin's `version.php` file
plugin release a name referring to the plugin release, given by the developer, '1.0', '2.0-beta', '5.8.12.3.4' etc... `$plugin->release`
version number is `$plugin->version` property, e.g. 2020092800, plays significant role in the plugin upgrade.

### version number format
`YYYYMMDDXX` where
`YYYY` - year
`MM` - month
`DD` day
`XX` additional counting starting at 00 and can be used up to 99 if needed


## Upgrade steps
Moodle does not downgrade.
the code to be executed on upgrade should be put into `db/upgrade.php` file within the plugin folder, the file is expected to define a single function called `xmldb_<plugin_component_name>_upgrade()`, the function should accept a single integer parameter with the older version number we are upgrading from

function body example
```php
<?php   
// This file is part of Moodle - [https://moodle.org/](https://moodle.org/)   
//   
// Moodle is free software: you can redistribute it and/or modify
// it under the terms of the GNU General Public License as published by  
// the Free Software Foundation, either version 3 of the License, or   
// (at your option) any later version.   
//   
// Moodle is distributed in the hope that it will be useful,   
// but WITHOUT ANY WARRANTY; without even the implied warranty of   
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the   
// GNU General Public License for more details.   
//   
// You should have received a copy of the GNU General Public License   
// along with Moodle. If not, see [https://www.gnu.org/licenses/](https://www.gnu.org/licenses/).   
/**   
* Provides the {@see xmldb_local_greetings_upgrade()} function.   
*   
* @package local_greetings   
* @category upgrade   
* @copyright 2022 Your Name <email@example.com>   
* @license [https://www.gnu.org/copyleft/gpl.html](https://www.gnu.org/copyleft/gpl.html) GNU GPL v3 or later   
* */   
defined('MOODLE_INTERNAL') || die();   
/**   
* Define upgrade steps to be performed to upgrade the plugin from the old version to the current one.   
*   
* @param int $oldversion Version number the plugin is being upgraded from.   
* */   
function xmldb_local_greetings_upgrade($oldversion) {   
	if ($oldversion < 2022092800) {   
		// Here goes the code that needs to be executed.   
		set_config('foo', 'bar', 'local_greetings');   
		upgrade_plugin_savepoint(true, 2022092800, 'local', 'greetings');   
	}   
	
	return true;   
}
```

## Common core APIs

### String API
how you get language text string to use in the user interface
e.g.
```php
echo get_strign('editingquiz', 'mod_quiz');
```

### Page API
used to set up the current page, add javascript and configure how things will be displayed to the user.
through the page api, it sets things like the title, initial heading, where the user is for the navigation
e.g.
```php
$PAGE->set_url('/mod/mymodulename/view.php', array('id' => $cm->id));
$PAGE->set_title('My module page title');
$PAGE->set_heading('My module page heading');
```

### Navigation API
allows for the manipulation of the navigation system used in moodle, available through  `$PAGE`
e.g.
code snippet below extends the navigation for the course
first, it finds the course node using a combination of the ID of the course, and the node type `navigation_node::TYPE_COURSE`
then, it adds to the navigation structure by adding a new node
```php
$coursenode = $PAGE->navigation->find($courseid, navigation_node::TYPE_COURSE);
$thingnode = $coursenode->add(get_string('thingname', new moodle_url('/a/link/if/you/want.php'));
$thingnode->make_active();
```

### Forms API
web forms are created using forms api
two parts:
	form definition, which define the elements of the form, including client-side validation
	using the form to capture and retrieve user input

e.g.
```php
// defines a form
// moodleform is defined in formslib.php

class myform extends moodleform {
	// add elements to form
	public function definition() {
		// reference to the form is stored in $this->form
		// common convention is to store it in a variable such as '$mform'
		$mform = $this->_form; // underscore

		// adding elements to form
		$mform->addElement('text', 'email', get_string('email'));

		// set type of element
		$mform->setType('email', PARAM_NOTAGS);
		
		// default value
		$mform->setDefault('email', get_string('enteremail'));
	}
}

```
```php
// how to use the form

// init the myform form from within the plugin
$mform = new \plugintype_pluginname\form\myform();

// form processing and displaying is done here.
if ($mform->is_cancelled()) {
	// if there is a cancel element on the form, and it was pressed,
	// then the `is_cancelled()` function will return true
	// handle cancel operation
} else if ($formform = $mform->get_data()) {
	// when the form is submitted, and the data is sucessfully validated
	// the `get_data()` function will return the data posted in the form
} else {
	// this branch is executed if the form is submitted but the data doesn't 
	// validate and the form should be redisplayed or on the first display of the form

	// set anydefault data (if any)
	$mform->set_data($toform);

	// display form
	$mform->display();
}
```


### Database access API
moodle supports multiple database, hence important that any database access code is compatible with the different databases supported.
the data definition API provides functions to create database tables, add or alter fields and indices, the functions in this library is mainly used in install and upgrade scripts

data manipulation API provides functions for manipulating the data, such as CRUD operations 
e.g. 
```php
// this is a code snippet to get the record for a user with id = 3
$user = $DB->get_record('user', ['id' = '3']);

// code snippet to insert a record in the `message` folder
$DB->insert_record('message', $messageobject);
```

### Upgrade API
how plugin installs and upgrades itself
moodle will automatically create your database tables for you when you visit the admin notification page `admin/index.php`
controller by three files within the plugin `version.php, db/install.xml and upgrade.php`

## Navigation
`$PAGE->navigation` - main navigation structure, contains items that will allow the user to browser to other available pages
`$PAGE->settingsnav` - contains items allowing users to edit settings
`$PAGE->navbar` - special structure for page breadcrumbs
- `$PAGE->[context]` is a Moodle context that immediately outlines the nature of the page the user is viewing.
- `$PAGE->course` is the course the user is viewing. This is essential if the context is CONTEXT_COURSE or anything within it. However, it is also useful in other contexts such as CONTEXT_USER.
- `$PAGE->cm` is the course module instance. This is essential if the context is CONTEXT_MODULE.
- `$PAGE->url` is used to match the active navigation item.

### Extending navigation tree
plugins can extend the navigation tree and insert custom links to it.
to add new items `$PAGE->navigation->add()`
several hooks (also known as callbacks) that plugins can implement to inject nodes into the navigation tree
```
extend_navigation_frontpage
extend_navigation_course
extend_navigation_user
```

to implement any of these hooks, a function must be present in the plugin's `lip.php` library file
e.g. if local_greetings needs to add a link to the navigation on the *Site Home* page, relevant hook implementation functions could look like
```php
/**
* insert a link to index.php on the site front page navigation menu
*
* @param navigation_node $frontpage Node representing the front page in the navigation tree
*/
function local_greetings_extend_navigation_frontpage(navigation_node $frontpage) {
	$frontpage->add(get_string('pluginname', 'local_greetings'), new moodle_url('/local/greetings/index.php'));
}

```

### Adding items to navigation block
in addition to navigation hooks, local plugins also support `extend_navigation` hook that can be effectively used to insert custom items into the navigation block
```php
/**
* add link to index.php into navigaton block
* 
* @param global_navigation $root Node representing the global navigation tree.
*/
function local_greetings_extend_navigation(global_navigation $root) {
	$node = navigation_node::create(
		get_string('greetings', 'local_greetings'), new moodle_url('/local/greetings/index.php'),
		navigation_node::TYPE_CUSTOM, null, null, new pix_icon('t/message', '')
	);

	$root->add_node($node);
}
```

### PHP scripts in moodle
two main kinds of PHP scripts:
**request handlers** - page controllers directly accessed by users' web browsers. these scripts are the endpoints for the http requests and they typically print the generated HTML and perform other actions
example - the script `/course/view.php` is accessed to display the course main page and also to handle some of the course-related actions such as moving sections
**libraries** - scripts are not supposed to be accessed directly via HTTP requests, they are loaded by other scripts and they provide library functions and class definition that implement all the logic
e.g. the course view script loads another library script `/lib/completionlib.php` which provides activity completed API-related functions
![[Pasted image 20241210222709 1.png]]

### User input
request handlers are responsible for reading and processing HTTP parameters,.
all input params coming from the user must be considered potentially malicious and must always be sanitised and validated.
```php
PARAM_INT - declares the parameter should be trated as an integer number
PARAM_ALPHA - for short strings that can contain ENGLISH ASCII letters only
PARAM_BOOL converts the input values like 0,1 to yes/no
PARAM_NOTAGS - strips all html tags from the submitted text
PARAM_TEXT - for longer plain texts, all html tags are stripped
```

### URL and redirects
uses `wwwroot` configuration variable into account for the URL
support renaming `admin/` folder to avoid collision with other web hosting platforms

convenient way of working with URLs in moodle is using `moodle_url` class,
	takes care of prefixing the url path with the defined host and root
	takes care of a renamed admin directory
	makes it easy to define the HTTP arguments to be submitted together with the request

`moodle_url` class instances can be passed to functions everywhere where moodle expects a url, the core function `redirect()` is an example of such function
e.g. displaying a link to a given course:
```php
$courseid = required_param('courseid', PARAM_INT);
$courseviewurl = new moodle_url('/course/view.php', ['id' => $courseid]);
echo `<a href="' .$courseviewurl . '">Back to the course</a>';`
```

## Database subsystem stack
Database schema definitions are stored in XML files that describe the database tables, columns, types, indexes and key.
database structure stored in syntax-independent XML files is then transformed to relevant data definition language statements
unless custom SQL, data manipulation API should be used to select insert, update, and delete records from the database tables
if custom SQL queries are needed, they must be written extremely carefully in a way that is database neutral (queries may only use syntax shared by all supported database systems), includes things like `UNIX_TIMESTAMP()` or quotes usage
![[Pasted image 20241211200454.png]]

### XMLDB editor
located in Site administration > development > XMLDB editor

`db/install.xml` is the XML file describing the latest version of all tables that your plugin provides. Do not modify the file manually - always use the XMLDB editor to edit the content of the file, this file is used when Moodle is installing the plugin the first name
`db/upgrade.php` - library file that controls changes to existing plugin installation, such as adding new tables, altering table's columns, adding and dropping keys


### Database design guidelines
all tables owned by a plugin should start with full component name of the plugin, e.g. `local_greetings, local_greetings_message`
activity modules are the exception to above rule
every table must have an auto-incrementing `id` field of the type `int(10)` set as its primary key, even if the table has other candidate keys
maximum length of table names is 28 char. 
maximum length of column names is 30 chars
column names should be always lower-case, simple and short.
table and column names should avoid using reserved words
columns that act as foreign keys and contain a reference to the `id` field in another table e.g. `widget-` should be called `widgetid`
boolean fields should be defined as integer fields `int(2)` and contain values 0 or 1 for false and true respectively 
dates and times are to be stored as UNIX timestamp in `int(10)` fields

## Database queries

### DB object
the data manipulation API is exposed via public methods of the `$DB` object
the `$DB` global object is an instance of the `moodle_database` class
moodle core takes care of setting up the connection to the database when the main `config.php` file is included
the `$DB` global object and its method calls can be accessed right after the main `config.php` is included.
to access `$DB` object in function
```php
<?php
function my_function() {
	global $DB;

	$DB->insert_record('tablename', $record);
}
```

### DB queries guidelines
actual tables are created with name prefix defined as `$CFG->prefix` in the `config.php` file
Do not use `AS` keyword for table aliases
Do not use table aliases at all for `DELETE` statements
Do use the `AS` keyword for column aliases

### Database calls for manipulating data

reading data from database
`$user = $DB->get_record('user', ['id' => '1']);`
`$DB->get_record()` used to fetch a single record from a table
there is also `get_records()` for fetching more than one record from a table
to execute complex queries `$DB->get_record_sql()`
```php
$user = $DB->get_record_sql('SELECT COUNT(*) FROM {user} WHERE deleted = 1 OR suspended = 1;');
```

**saving data to the database**
```php
$record = new stdClass;
$record->message = $message;
$record->timecreated = time();
$record->userid = $USER->id;

$DB->insert_record('local_greetings_messages', $record);
```

**updating data**
`$DB->update_record('local_greetings_messages', $updaterecord);`

**deleting data**
`$DB->delete_record('local_greetings_messages', $record);`


### Classes folder

`classes/event` sub directory within a plugin is where plugins implement their [Event API](https://docs.moodle.org/dev/Events_API)

`classes/privacy` is where plugins should implement [Privacy API](https://moodledev.io/docs/5.0/apis/subsystems/privacy)
plugins that do not store any personal user data implement the `core_privacy/local/metadata/null_provider` interface in plugin's provider
plugins which store data will need:
- describe the type of data that they store;
- provide a way to export that data
- provide a way to delete that data


### Third party libraries
if a plugin includes third party libraries, then it should be declared, third party refers to any library where the latest version of the code is not maintained and hosted by the plugin developer
- check the license to make sure library uses a `GPLv3` compatible license 
- if a library is not compatible, it cannot be distributed together with the plugin in one zip package, and hosted in the Moodle plugin directory
- details of third party should be declared in the `thirdpartylibs.xml` file
- create a `readme_moodle.txt` file detailing relevant information, including - download URLs and build instruction
- within the XML the `location` is a file, or directory, relative to your plugin's root
e.g. of `thirdpartylibs.xml`
```xml
<?xml version="1.0"?>
<libraries>
	<library>
		<location>javascript/html5shiv.js></location>
		<name>Html5Shiv</name>
		<version>3.6.2</version>
		<license>Apache</license>
		<licenseversion>2.0</licenseversion>
	</library>
	<library>
		<location>vendor/guzzle/guzzle/</location>
		<name>guzzle</name>
		<version>v3.9.3</version>
		<license>MIT</license>
		<licenseversion></licenseversion>
	</libray>
</libraries>
```

### AMD Javascript module
js in moodle is written in ESM format, and transpiled into AMD module for deployment
[Moodle javascript guide](https://moodledev.io/docs/guides/javascript)
the js file should be placed in `[path/to/moodle]/plugintype/pluginname/amd/src/`
e.g. `[path/to/moodle]/plugintype/pluginname/amd/src/example.js`
```javascript
/**
* Example module for the plugintype_pluginname plugin
*
* @module plugintype_pluginname/example
* @copyright Year, You name <your@email.address>
* @license http://www.gnu.org/copyleft/gpl.html GNU GPL v3 or later
*/

import { fetchThings } from './repository';

export const updateThings = (thingData) => {
	return fetchThings(thingData);
};
```
