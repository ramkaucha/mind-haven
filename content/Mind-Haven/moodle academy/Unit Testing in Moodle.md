
## Types of testing

### Manual testing
conducted by humans
testing own code
#### Code testing
development, peer review, integration reviews
#### QA testing
look at the functionality of moodle from a user's POV
real users systematically try each feature in moodle and test that it works in the current version of moodle code
#### Integration functional testing
Moodle HQ devs manually test the functionality of all the issues that have been integrated that week.

### Automated testing
tests conducted using software tools or computer scripts
#### Unit testing
automated tests of very low-level code functionality that a developer write as part of any new code using [PHPUnit](https://moodledev.io/general/development/tools/phpunit)
#### Acceptance testing
automated testing of the user interface using `Behat` framework
#### Continuous integration testing
continuous integration server tests the new code for:
	coding guidelines
	PHPUnit tests
	SimpleTest unit tests on older versions of Moodle
	Detect unresolved merge conflicts
	Compare databases upgraded from previous versions
	check `version.php` is correct

#### Regression testing
every day, automated build in a test server runs a large number of tests concerning key functions of Moodle, to make sure that everything still works, and that some new fix in Moodle hasn't caused problems elsewhere
these tests must pass completely before new release can be made:
	acceptance testing using Behat framework
	performance testing using JMeter



## Unit testing
testing an individual unit of a code against expected results
	unit of code - function, method or a class
	unit is piece of code that is repeatable, testable and functional
	unit testing is usually automated by using a unit testing framework



### Best practice for unit testing
write tests before or during development
unit tests should be deterministic
each unit test should focus on only one use case
test for different scenarios
each unit test should be independent of other tests

### PHPUnit
advanced unit testing framework, installed as a composer dependency

## Moodle PHPUnit integration

```bash
# navigate to moodle directory
cd /path/to/moodle

sudo nvim /etc/php/php.ini
# uncomment
extension=gd
extension=intl
extension=iconv
extension=sodium

sudo systemctl restart php-fpm

php composer.phar install

# in config.php
$CFG->phpunit_dbtype = getenv('MOODLE_DOCKER_DBTYPE');
$CFG->phpunit_dblibrary = 'native'; 
$CFG->phpunit_dbhost = 'db'; 
$CFG->phpunit_dbname = 'moodle_phpunit'; 
$CFG->phpunit_dbuser = getenv('MOODLE_DOCKER_DBUSER');
$CFG->phpunit_dbpass = getenv('MOODLE_DOCKER_DBPASS');
$CFG->phpunit_dboptions = ['dbcollation' => getenv('MOODLE_DOCKER_DBCOLLATION')];

$CFG->phpunit_prefix = 'phpu_';
$CFG->phpunit_dataroot = '/var/www/phpunit_data';

# init phpunit
# create bash script (db-moodle.sh)
$CFG->phpunit_dbtype = getenv('MOODLE_DOCKER_DBTYPE');
$CFG->phpunit_dblibrary = 'native'; 
$CFG->phpunit_dbhost = 'db'; 
$CFG->phpunit_dbname = 'moodle_phpunit'; 
$CFG->phpunit_dbuser = getenv('MOODLE_DOCKER_DBUSER');
$CFG->phpunit_dbpass = getenv('MOODLE_DOCKER_DBPASS');
$CFG->phpunit_dboptions = ['dbcollation' => getenv('MOODLE_DOCKER_DBCOLLATION')];

# run test (using alias in ~/.zshrc)
alias moodle-test='MOODLE_DOCKER_WWWROOT="/path/to/your/moodle" MOODLE_DOCKER_DBTYPE=pgsql MOODLE_DOCKER_DB=pgsql MOODLE_DOCKER_WEB_HOST=localhost bin/moodle-docker-compose exec webserver vendor/bin/phpunit'

# run specific test file
moodle-test [test.php]

# run specific test method
moodle-test --filter test_name path/to/test.php
```

### Writing PHPUnit tests
all tests are located in the `pluginname/tests` folder
all test case class name should match the file name, for example, the class name for `mod/forum/test/this_test.php` should be `this_test`
all test case classes should end with `_test` suffix
all test case classes should use the namespace they belong to, e.g. the namespace for `mod/forum/test/this_test.php` should be `mod_forum` with sub-namespaces (and corresponding sub-directories) allowed to better match what is being tested

e.g.
```php
<?php

namespace mod_pluginname;

class complex_test extends \advanced_testcase {
	public function test_isadmin() {
		global $DB;

		$this->resetAfterTest(true); // reset all changes
		$this->assertFalse(is_siteadmin()); // by default no user is logged-in
		$this->setUser(2); // switch $USER
		$this->assertTrue(is_siteadmin()); // admin is logged-in now

		$DB->delete_records('user', []);
		$this->resetAllData();
		$this->assertTrue($admin = $DB->records_exists('user', ['id'=>2]));

		$this->assertFalse(is_siteadmin());
	}
}
```

`moodle-test /var/www/html/local/greetings/tests/lib_test.php`

