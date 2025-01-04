How to use the page and the Output APIs in a simple plugin

AIM:
[x] structure of moodle local plugin type
[x] use moodle's page and output APIs to display text on a page
[x] how moodle handles localisation

Standard plugins occupy almost half of the Moodle installation on the file system. Other half consists of *core subsystems*, these subsystems provide the [core api](https://moodledev.io/docs/5.0/apis) that the plugins use.
![[Pasted image 20241210185109 1.png]]

## Types of plugins


| Plugin type   | Description                                                                                                                                                                                                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| activity      | essential moodle plugins, represent the activities through which the learning is happening in the courses                                                                                                                   |
| block         | content items that are added to the left, right, or center columns of any page                                                                                                                                              |
| report        | these plugin types are designed for reporting purposes                                                                                                                                                                      |
| course format | course formats determine the layout of the course main page                                                                                                                                                                 |
| filter        | filters are a way to automatically transform content before it is output                                                                                                                                                    |
| local         | local plugins are intended to be used for implementing site-specific or institution-specific functionality. they are designed as a generic plugin type for all kind of local customisations in a clean and maintainable way |
| theme         | theme define the look and feel of a moodle site                                                                                                                                                                             |
### local plugin type
often used to features that do not naturally fit into any other standard plugin type.

features of local plugin type
navigation - inject new items into the site navigation and administration tree and /or change the existing navigation
custom font page - a local plugin can be used to provide a custom site front page, make use of the `$CFG->customfrontpageinclude` configuration variable
custom web service functions - implementing custom external functions that can be exposed via the [web services](https://moodledev.io/docs/5.0/apis/subsystems/external) layer

### Full component name
for example, plugin type: local, folder name: greetings. *full component name*: local_greetings 

### Common plugin files
`version.php` - provides some meta-data about the plugin such as the version number, dependencies or the maturity level.  REQUIRED
`lang/en/{plugintype}_{pluginname}.php` - defines the english strings used by the plugin. REQUIRED
`db/install.xml` - defines the plugin's database tables. this file is only required if the plugin will create additional tables for sorting data.

## Core global variables in moodle

`$CFG` - contains configuration values of the Moodle setup, such as root directory, data directory, database details and other config values
`$SESSION` - moodle's wrapper round PHP's `$SESSION`
`$USER` - holds the user table record for the current user, this will be the 'guest' user record for people who are not logged in
`$SITE` - Frontage course record. this is the course record with id=1
`$COURSE`- holds the current course details, an alias for `$PAGE->course`
`$PAGE` - central store of information about the current page we are generating in response to the user's request
`$OUTPUT` - instance of `core_renderer` or one of its subclasses. used to generate `HTML` for output
`$DB` - holds the database connection details, used for all access to the database

declared in `setup.php`

### Page content
`$PAGE->set_context(context_system::instance());` - used to set the context of a page.
context that has been set for the current page can be accessed via the `$PAGE->context`

### Page layout
all supported layouts are defined in the `theme/{theme_name}/config.php` file of the currently used theme
commonly used layouts are base ( default ), standard, course, frontpage, mydashboard, and login.
`$PAGE->set_pagelayout('standard');` - used to set the page layout

### Page title and heading
`$PAGE->set_title(get_string('pluginname', 'local_greeting'));` - set the page title
`$PAGE->set_heading(get_string('pluginname', 'local_greetings'));` - define the heading

### page url
each page has unique url
```php
$PAGE->set_url(new_moodle_url('/local/greetings/index.php'));

# once current page's URL is set, we can access it via 
$baseurl = $PAGE->url;

# it can be also used as a base for additional URLs
$nextitemurl = new_moodle_url($PAGE->url, ['item' => $nextitemid]);
```

outputting the page html
once page has been set up , the output rendering mechanism has enough information to actually generate all the HTML for the page

to finish DOM initialisation and start outputting the HTTP headers and actual `<html>`, call:
`echo $OUTPUT->header()`
`$OUTPUT` - most useful global variable that moodle creates during setup, which is an instance of an output rendering class and its purpose is to generate the HTML representing the page and all the widgets on it.

e,.g. printing footer on the page `echo $OUTPUT->footer()`


### output helper functions
output helper functions to be called to process user data before including them in the page HTML source code
`s()` - wrapper for PHP native `htmlspecialchears()`, used for plain texts where we do not want to interpret any HTML. typical example might be values of HTML tag attributes
`echo '<input type'text' name='uname' value="' .s($uname) . '">';`
`format_string()` - display short strings without html formatting with text filtering applied
`echo format_string($course->name)`
`format_text()` - use to display longer formatted texts with all filter applied
```php
echo format_text($post->content, $post->contentformat, ['noclean' => true, 'context' => $context]);
```
Rich text - should the  content support rich text formatting
filter - should text filters be applied ? note that filtering is needed to support multi-language content
user data - does the text contain data submitted by a user?
javaScript - should inline javaScript be trusted and kept in the text

![[Pasted image 20241210194253 1.png]]

### HTML tags
moodle comes with an advanced output rendering engine that encourages developers to separate logic processing in PHP scripts from generating in HTML presentations via renderers and templates.
`html_writer` - helper class that provides a wide range of static methods to generate HTML from PHP.
```php
echo html_writer::tag('input', '' [
	'type' => 'text'
	'name' => 'username'
	'placeholder' => get_string('typeyourname', 'local_greetings'),
]);
```

### other helpful helper
`isloggedin()` - determines if a user is currently logged in
`isguestuser()` - determines if a user is logged in as a guest user with username 'guest'
`require_login()` - checks that the current user is logged in, if not then it redirects them to the site login
`fullname()` - returns the full name of the person


## User interface localisation

All texts displayed should not be hard-coded, instead defined in a language string file. (lang/en/file)
`$string['editingquiz'] = 'Editing quiz';`
`echo get_string('editingquiz, 'mod_quiz');`

### Substitute value
use `${a}` for substituting value, `$a` is an object string or number that can be used within translation strings.
```php
$string['answerno1'] = 'Answer {$a}'; // substituting string/integer
$string['answerno2'] = 'Answer {$a->name}'; // substituting object member

// passing value at runtime
get_string('answerno1', 'qtype_dragdrop', $number);
$user->number = 10;
get_string('answerno2', 'qtype_dragdrop', $user);
```

### Date and time
```php
$now = time();
echo userdate($now);
```
to manually specify display format, use *strftime formatting strings* defined in `core_langconfig` component
```php
echo userdate(time(), get_string('strftimedaydate', 'core_langconfig'));
```
can also use the `DateTime` class to obtain timestamp
```php
$date = new DateTime("tomorrow", core_date::get_user_timezone_object());
$date->setTime(0, 0, 0);
echo userdate($date->getTimestamp(), get_string('strftimedatefullshort', 'core_langconfig'));
```

### Decimal numbers
`format_float()` - to display decimal numbers nicely to the user (DO NOT USE FOR ACTUAL CALCULATIONS)
```php
$grade = 20.00 / 3;
echo format_float($grade, 2);
```

### Logos/Images
the directory for the icons used in a plugin is `plugintype/pluginname/pix/`
if there is a filed named `monologo.svg` or `monologo.png`, then moodle uses that as the plugin logo
before 4.0+, the plugin icons were named `icon.svg` or `icon.svg`

**Rendering icons in php**
using `pix_icon()`
```php
$chaptericon = $OUPUT->pix_icon('nav_text', get_string('navtext', 'mod_book'), 'mod_book');
```
Rendering icons in mustache templates
`{{#pix}} nav_text, mod_book, Next {{/pix}}`
