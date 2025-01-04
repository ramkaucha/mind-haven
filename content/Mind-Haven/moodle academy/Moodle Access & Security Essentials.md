---
title: Moodle Access & Security Essentials
tags:
  - moodle-academy
  - tutorial
cssclasses: 
author: Ram
---
## Overview of access control principles

Capability - description of a feature that users can be allowed to perform. e.g. 'add a new forum to the course', 'start new discussions', 'reply to posts'.
capabilities of each plugin are defined in it's `db/access.php` file
e.g. `grade/export/ods/db/access.php`
```php
$capabilities = [
	'gradeexport/ods:view' => [
		'riskbitmask' => RISK_PERSONAL,
		'captype' => 'read',
		'contextlevel' => CONTEXT_COURSE,
		'archetypes' => [
			'teacher' => CAP_ALLOW,
			'editingteacher' => CAP_ALLOW,
			'manager' => CAP_ALLOW,
		]
	],
	'gradeexport/ods:publish' => [
		'riskbitmask' => RISK_PERSONAL,
		'captype' => 'read',
		'contextlevel' => CONTEXT_COURSE,
		'archetypes' => [
			'manager' => CAP_ALLOW,
		]
	]
]
```

### contexts
moodle site is organised into a hierarchy of areas called `contexts` and users become students or teachers in individual contexts, such as courses
![[Pasted image 20241211221243.png]]
the root level of the hierarchy is the system context - represents whole site
on top of it there is a tree of course category contexts
every course has its own course context
within course contexts there are module contexts representing particular activity module instances, and block contexts representing blocks
the site front page is internally implemented as a course so the front page context is actually one of the course level contexts for that particular site course
user context represents a given user

e.g. something like a custom role 'spammer'  which prohibits the user from starting a new discussions and replying to forum posts. if this role is assigned to a user in the *system context* level, it prevents the user from posting anywhere on the site, if the role is assigned to the user in a particular course context level, the user can't post in forums of that particular course.

### Access control
should NEVER check what role the user has, e.g. 'if the user is student then, if the user is teacher then'
instead, declare these capabilities in the file `db/access.php` of your plugin together with sensible default values for the archetype roles.
then check if the user has the given capability via the `has_capability()` and `require_capability()` methods


## Plugin security basics
[Common types of security vulnerability](https://moodledev.io/general/development/policies/security#common-types-of-security-vulnerability)
*sesskey* token mechanism to protect against CSRF
data sanitisation on input and output to protect against XSS
avoiding SQL injections by using query argument placeholders

### cross-site request forgery
an attack that tricks a user to execute unwanted actions on a web application in which they are currently authenticated

use form api whenever possible for handle HTML forms, automatically checks the sesskey and request method.

manually checking and validating sesskey, using `require_sesskey()`
```php
$delete = optional_param('delete', null, PARAM_INT);

if ($delete) {
	require_sesskey();
	// etc.
}
```

### cross-site scripting
attacks are type of injection, in which malicious scripts are injected into otherwise benign and trusted websites

**how to avoid XSS**
important to clean all user-input and escape output.
get input values using `optional_param` or `required_param()` with an appropriate `PARAM_*` type, to ensure that only data of the type you expect is accepted
use `moodleforms` with appropriate `->setType` calls in the form defintion
clean or escape content appropriately on output
	use `s()` or `p()` to output plain text content
	use `format_string` to output content with minimal html like multi-lang
	use `format_text` to output all other content
any place where a user can input content that is output by `format_text` , `$options->noclean`, must be protected by a capability check, and the capability must be marked as `RISK_XSS`
when sending data to javascript code, use `$PAGE->requires->data_for_js` or `$PAGE->requires->js_function_call` methods

### SQL injections
consist of insertion or 'injection' of a SQL query via input data from the client to the application

**how to avoid SQL injection**
clean all user input by passing appropriate `PARAM_XXX` to `required_param()` or `optional_param()`
escape all input parameter
use higher level `dmllib` methods, like `get_record()`, don't create SQL yourself
when you have to inset value into SQL statements, use placeholders to insert safely
test code by using tool like `sqlmap` or by manually trying to tricky inputs
`< > & \&lt; \&gt; \&amp; ' \\ ' \\ \\`

## Plugin administration setting

### Configuration variables

setting a configuration variable is performed by called `set_config` core function
```php
set_config('myconfigname', 1, 'local_greetings');

// to get a configuration variable
if (get_config('local_greetings', 'myconfigname')) {
	//...
}
```

[Admin settings API](https://moodledev.io/docs/5.0/apis/subsystems/admin)

admin settings stored inside `settings.php` root of plugin folder

### Access control
not all config variables must be stored in the database, not all config variables must have a UI to control value

instead some can only be configured through `$CFG` object properties defined in the `config.php` file

Moodle stores most config variables in two databases tables `config` and `config_plugins`
core settings stored in the `config` table are exposed through `$CFG`
settings can be obtained via `get_config()` and stored via `set_config()` core functions call
administration UI allows you top modify the config values is implemented via subclasses of `admin_setting` class
UI for config of a plugin is found in the `setting.php` file located in the root directory
unless a custom storage mechanism is used, many `admin_setting` subclasses use the low level `get_config()` and `set_config()` to read and write configurable values
site admins is internally implemented as a tree structure of `admin_category` objects, the root category is known as the global `$ADMIN` object
admin pages can be `admin_settingpage` (container or related admin_setting instances) or `admin_externalpage)`
![[Pasted image 20241211230846.png]]

```php
$ADMIN->add(new admin_setting_configcolourpicker(
'local_greetings/messagebackgroundcolor',
 get_string('messagebackgroundcolor','local_greetings'),
get_string('messagebackgroundcolor_dec', 'local_greetings'),
'#FFFFFF', // DEFAULT VALUE,
));
```