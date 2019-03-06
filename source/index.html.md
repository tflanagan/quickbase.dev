---
title: Quick Base Developers

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript--node
  - javascript--browser
  - php

toc_footers:
  - <a href='https://www.quickbase.com/' target='_blank'>Quick Base</a> | <a href='https://help.quickbase.com/api-guide/' target='_blank'>Quick Base API</a> | <a href='https://github.com/tflanagan/quickbase.dev' target='_blank'>GitHub</a>
  - <a href='https://github.com/lord/slate' target='_blank'>Documentation Powered by Slate</a>
  - Developed by <a href='https://github.com/tflanagan' target='_blank'>Tristian Flanagan</a>

includes:
  - errors

seo: 

search: true
---

# Introduction

Welcome to quickbase.dev! This site is dedicated to providing information and help on how to navigate Quick Base's API.

The featured libraries are all designed to be flexible, portable, easy, and fun to use. We have examples in PHP and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

All the featured libraries are hosted on GitHub and are open and welcome to submissions.

Additionally, all the featured libraries are available for use under their respective <a href='http://www.apache.org/licenses/LICENSE-2.0' target='_blank'>Apache-2.0 licenses</a> found in each respective repository.

We are continually trying to improve the information available! Please feel free to submit an issue or a pull request to help us improve!

## Support

The featured libraries are open-source projects open and welcome to all submissions.

Please keep all support requests to their respective GitHub repositories. For example, for a PHP issue please do not open an issue in the node repository, open it in the PHP repository.

## FAQ

### How do I debug?

```javascript--node
// Windows
// $ SET DEBUG=* && node path/to/script.js

// Unix
// $ DEBUG=* node path/to/script.js
```

```javascript--browser
window.localStorage.debug = '*';
window.location.reload(); // You have to refresh for the changes to take hold
```

```php
<?php

$quickbase = new \QuickBase\QuickBase(array( ... ));

$quickbase->debug = true;

?>
```

In JavaScript, the libraries make good use of the `debug` library.

If you are in the browser, to engage the debug functionality, open your developer tools console and enter `window.localStorage.debug = '*';`. Then refresh the page. Any API calls will now be logged directly to the console so you know exactly what is being sent to Quick Base and what is being sent back.

If you are use Nodejs, you'll have to set the DEBUG environment variable, for Windows `SET DEBUG=*`, for unix based systems `DEBUG=*`. Any API calls will now be logged directly to stdout so you know exactly what is being sent to Quick Base and what is being sent back.

In PHP, the library has a very rudamentary system. If you set the `QuickBase` instance public property `debug` to `true`, then the library will `var_dump` all outgoing requests, but not incoming responses. You can accomplish the same feat by `var_dump`'ing the result of your `api()` method call.

# Requirements

Below are the requirements for various platforms.

Also, while not a requirement, if you are working on a Windows machine, I highly recommend using `cmder` to interact with Nodejs or PHP via the command line.

You can find `cmder` here: <a href='https://cmder.net/' target='_blank'>https://cmder.net/</a>

## Browser

The libraries detailed in this document are transpiled to and made available in ECMAScript2015 ("ES 5"), thus any browser that supports ES 5 or greater, supports these libraries.

You can find out what browsers support ES 5 here: <a href='https://kangax.github.io/compat-table/es5/' target='_blank'>https://kangax.github.io/compat-table/es5/</a>

<i>(Hint: All modern browsers support ES 5.)</i>

## Server

### Nodejs

Version 4.0.0 or greater

To install Nodejs, please visit their website here: <a href='https://nodejs.org/en/' target='_blank'>https://nodejs.org/en/</a>

You will also require `npm`, which is Nodejs's package manager. Depending on how you install Nodejs, `npm` is most likely included. If it wasn't, you can find help here: <a href='https://www.npmjs.com/get-npm' target='_blank'>https://www.npmjs.com/get-npm</a>

### PHP

Version 5.4.0 or greater

PHP extensions CURL and XML are also required

To install PHP, please find help here: <a href='https://secure.php.net/manual/en/install.php' target='_blank'>https://secure.php.net/manual/en/install.php</a>

You will also require `composer`, which is a PHP package manager. To install `composer`, please visit here: <a href='https://getcomposer.org/download/' target='_blank'>https://getcomposer.org/download/</a>

# QuickBase

This is the low level Quick Base class, giving you direct access to Quick Base's API.

This class is utilized in the abstraction layers QBRecord and QBTable.

## Repository Links

* PHP: <a href='https://github.com/tflanagan/php-quickbase' target='_blank'>https://github.com/tflanagan/php-quickbase</a>
* JavaScript: <a href='https://github.com/tflanagan/node-quickbase' target='_blank'>https://github.com/tflanagan/node-quickbase</a>

## Installation & Loading

```javascript--node
// $ npm install --save quickbase

const QuickBase = require('quickbase');
```

```javascript--browser
// <script type="text/javascript" src="quickbase.browserify.min.js"></script>
```

```php
<?php

// $ composer require tflanagan/quickbase

require_once(__DIR__.DIRECTORY_SEPARATOR.'vendor'.DIRECTORY_SEPARATOR.'autoload.php');

?>
```

Installing the Quick Base library for your desired platform requires either including the browserified version of the library in your HTML page or install it via a package manager (npm or composer).

Including the library for use in your code depends on your platform.

## Initialization

```javascript--node
const quickbase = new QuickBase({
  realm: 'subdomain/realm',
  userToken: 'user token',
  appToken: 'application token',
  flags: {
    msInUTC: true,
    encoding: 'ISO-8859-1'
  },
  connectionLimit: 10,
  errorOnConnectionLimit: false
});
```

```javascript--browser
var quickbase = new QuickBase({
  realm: 'subdomain/realm',
  userToken: 'user token',
  appToken: 'application token',
  flags: {
    msInUTC: true,
    encoding: 'ISO-8859-1'
  },
  connectionLimit: 10,
  errorOnConnectionLimit: false
});
```

```php
<?php

$quickbase = new \QuickBase\QuickBase(array(
  'realm' => 'subdomain/realm',
  'userToken' => 'user token',
  'appToken' => 'application token',
  'flags' => array(
    'msInUTC' => true,
    'encoding' => 'ISO-8859-1'
  )
));

?>
```

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
realm | true | | Quick Base Realm (or subdomain)
appToken | false | | Quick Base Application Token
userToken | false | | Quick Base User Token
flags | false | | Object containing a collection of API parameters
  - msInUTC | false | true | Interpret all timestamps as milliseconds in UTC rather than using the local application time
  - encoding | false | ISO-8859-1 | The encoding used to make requests to and parse responses from Quick Base
connectionLimit | true | 10 | <i>(JavaScript Only)</i> Maximum number of simultaneous API requests
errorOnConnectionLimit | false | false | <i>(JavaScript Only)</i> Throw an error if the connectionLimit is exceeded

## Making an API Call

The Quick Base librarys are built in such a way as to be as future-proof as possible. 9/10 times, a new API endpoint will automatically be supported if you're using these libraries.

The way we accomplish this is by exposing a single method `api()`.

#### `.api(action[, options])`

```javascript--node
quickbase.api('SomeAPI_Action', {
  dbid: 'bddnn3uz9',
  someFutureProperty: 'lorem ipsum'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('SomeAPI_Action', {
  dbid: 'bddnn3uz9',
  someFutureProperty: 'lorem ipsum'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('SomeAPI_Action', array(
    'dbid' => 'bddnn3uz9',
    'someFutureProperty' => 'lorem ipsum'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
action | true | | The Quick Base API Action you wish to execute (ie: API_DoQuery)
options | false | | An object containing data pertaining to your API Action

In JavaScript, the `api()` method returns a Promise, powered by <a href='http://bluebirdjs.com/docs/getting-started.html' target='_blank'>bluebirdjs</a>. If an error occurs during the request, you can handle the error by using `catch()`. Otherwise, you can continue processing with the results passed into a `then()`.

In PHP, the `api()` method returns the resulting JSON object from the API call. If an error occurs, it will be thrown and needs to be caught with a `try/catch` statement.

<aside class="notice">
<i>(JavaScript Only)</i><br />
This library makes API calls asychronously. As Quick Base cannot support n number of requests at a given time, the default limit is 10. This throttles the requests sent to Quick Base in order to preserve the Quick Base applications integrity and performance.<br />
<br />
If your application experiences a normal traffic rate that is higher than average, you may want to consider reducing the `connectionLimit` setting.
</aside>

## Quick Base API Endpoints

The following is a list of Quick Base API Endpoints mostly compiled from Quick Base's own help section.

Each endpoint has an overview of the endpoint and what it supports, an example of using it in code, what the response from Quick Base looks like in JSON, and a link to Quick Base's help section for that specific endpoint.

This list may not contain everything that is supported. As these libraries are future-proof, if Quick Base comes out with a new endpoint, it will be automatically supported - so long as Quick Base hasn't changed too much. We will try to keep this update to date, time allowing. To that note, Quick Base's documentation is the ultimate authority in regards to what is supported.

### API_AddField

```javascript--node
quickbase.api('API_AddField', {
  dbid: 'bddnn3uz9',
  add_to_forms: true,
  label: 'Label',
  mode: 'virtual',
  type: 'formula'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_AddField', {
  dbid: 'bddnn3uz9',
  add_to_forms: true,
  label: 'Label',
  mode: 'virtual',
  type: 'formula'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_AddField', array(
    'dbid' => 'bddnn3uz9',
    'add_to_forms' => true,
    'label' => 'Label',
    'mode' => 'virtual',
    'type' => 'formula'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_AddField",
  "errcode": 0,
  "errtext": "No error",
  "fid": 8,
  "label": "Label"
}
```

<a href='https://help.quickbase.com/api-guide/add_field.html' target='_blank'>Quick Base Documentation</a>

Use API_AddField to add a new field to a table. You invoke this call on a table-level dbid.

When you add a field using API_AddField, you specify the field type, but no other field properties. After you've added the field, you can use API_SetFieldProperties to set the properties of the new field and any default values. (You can't set field type using API_SetFieldProperties; if you want to change the field type after adding the field, you must use the Quick Base UI.)

The amount of data space consumed by a field depends on the field type.

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Table DBID that you want to add a field to
add_to_forms | false | false | Specifies whether the field you are adding should appear at the end of any form with form properties set to "Auto-Add new fields."
label | true | | Name of the new field
mode | Lookup/Formula only | | Specifies whether the field is a formula field or a lookup field (possible values for formula: 'virtual', and lookup: 'lookup')
type | true | | The Quick Base field type.

Possible `type` values:

UI: TYPE | API: TYPE
-------- | ---------
Checkbox | checkbox
Date | date
Duration | duration
Email Address | email
File Attachment | file
Formula | any other type
Lookup | text or float
List - User | multiuserid
Multi-Select Text | multitext
Numeric | float
Numeric - Currency | currency
Numeric - Percent | percent
Numeric - Rating | rating
Phone Number | phone
Report  Link | dblink
Text | text
Time Of Day | timeofday
URL | url
User | userid

### API_AddGroupToRole

```javascript--node
quickbase.api('API_AddField', {
  dbid: 'bddnn3uz9',
  gid: '345889.ksld',
  roleid: 12
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_AddField', {
  dbid: 'bddnn3uz9',
  gid: '345889.ksld',
  roleid: 12
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php
nodetry {
  $results = $quickbase->api('API_AddField', array(
    'dbid' => 'bddnn3uz9',
    'gid' => '345889.ksld',
    'roleid' => 12
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```jsonnode{
  "action": "API_AddGroupToRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-node/api_addgrouptorole.html' target='_blank'>Quick Base Documentation</a>

Use API_AddGroupToRole to add a group to a role in an app.

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID
gid | true | | The id of the group
roleid | true | | The id of the role

### API_AddRecord

```javascript--node
quickbase.api('API_AddRecord', {
  dbid: 'bddnn3uz9',
  fields: [
    { fid: 6, value: 'Hello World!' }
  ],
  disprec: false,
  fform: false,
  ignoreError: false,
  msInUTC: false,
  msAsDurationDefault: false
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_AddRecord', {
  dbid: 'bddnn3uz9',
  fields: [
    { fid: 6, value: 'Hello World!' }
  ],
  disprec: false,
  fform: false,
  ignoreError: false,
  msInUTC: false,
  msAsDurationDefault: false
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_AddRecord', array(
    'dbid' => 'bddnn3uz9',
    'fields' => array(
      array( 'fid' => 6, 'value' => 'Hello World!' )
    ),
    'disprec' => false,
    'fform' => false,
    'ignoreError' => false,
    'msInUTC' => false,
    'msAsDurationDefault' => false
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_AddRecord",
  "errcode": 0,
  "errtext": "No error",
  "rid": 21,
  "update_id": 1206177014451
}
```

<a href='https://help.quickbase.com/api-guide/add_record.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Table DBID
fields | true | | Array of objects with fid/value parameters
disprec | false | false | Set this parameter to true to specify that the new record should be displayed within the Quick Base application. An application login required before the record can be displayed. If you use this parameter, Quick Base, returns the normal Quick Base HTML page that displays the record.<br /><br />Omit this property if you don't want the new record to display within the Quick Base application.
fform | false | false | Set this parameter to true if you are invoking API_AddRecord from within an HTML form that has checkboxes and want those checkboxes to set Quick Base Checkbox fields.<br /><br />Omit this property if you don't need Quick Base to set Checkbox fields based on your HTML page.
ignoreError | false | false | Set this parameter to true to specify that no error should be returned when a built-in field (for example, Record ID#) is written-to in an API_AddRecord call.<br /><br />If you do not set this parameter, Quick Base returns an error when API_AddRecord writes to a built-in field.
msInUTC | false | global instance setting | Allows you to specify that Quick Base should interpret all date/time stamps passed in as milliseconds using Coordinated Universal Time (UTC) rather than using the local application time.<br /><br />Set this parameter to true if you want to use Coordinated Universal Time.
msAsDurationDefault | false | false | Set this parameter to true to specify milliseconds, instead of days, as the default units for a duration field value sent without any units.<br /><br />If you set this parameter to false or omit it, then any duration field values sent without a unit will default to being interpreted as days.

### API_AddReplaceDBPage

```javascript--node
// Adding a new DB Page
quickbase.api('API_AddReplaceDBPage', {
  dbid: 'bddnn3uz9',
  pagename: 'newpage.html',
  pagetype: 1,
  pagebody: '<html></html>'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});

// Updating an existing DB Page
quickbase.api('API_AddReplaceDBPage', {
  dbid: 'bddnn3uz9',
  pagename: 'newpage.html',
  pageid: 12,
  pagetype: 1,
  pagebody: '<html></html>'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
// Adding a new DB Page
quickbase.api('API_AddReplaceDBPage', {
  dbid: 'bddnn3uz9',
  pagename: 'newpage.html',
  pagetype: 1,
  pagebody: '<html></html>'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});

// Updating an existing DB Page
quickbase.api('API_AddReplaceDBPage', {
  dbid: 'bddnn3uz9',
  pagename: 'newpage.html',
  pageid: 12,
  pagetype: 1,
  pagebody: '<html></html>'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

// Adding a new DB Page
try {
  $results = $quickbase->api('API_AddReplaceDBPage', array(
    'dbid' => 'bddnn3uz9',
    'pagename' => 'newpage.html',
    'pagetype' => 1,
    'pagebody' => '<html></html>'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

// Updating an existing DB Page
try {
  $results = $quickbase->api('API_AddReplaceDBPage', array(
    'dbid' => 'bddnn3uz9',
    'pagename' => 'newpage.html',
    'pageid' => 12,
    'pagetype' => 1,
    'pagebody' => '<html></html>'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_AddReplaceDBPage",
  "errcode": 0,
  "errtext": "No error",
  "pageID": 12
}
```

<a href='https://help.quickbase.com/api-guide/add_replace_dbpage.html' target='_blank'>Quick Base Documentation</a>

Use API_AddReplaceDBPage to add a new database page or replace an existing page with a new page. You invoke this call on an application (dbid).

Quick Base allows you to store various types of pages at the application level. These pages can be various text or rich text or HTML page that you store and link to buttons in the Quick Base UI. They can also be XSL templates used for customizing the Quick Base application. They can also be Exact Forms, which are forms (form letters, invoices, etc.) created in Microsoft Word using the Quick Base Exact Form template that gets data from Quick Base tables.

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID
pagename | true | | The name of the DB Page
pageid | true, if updating | | The id of the DB Page you wish to update
pagetype | true | | Page type value, possible values are 1 for XSL stylesheets or HTML Pages and 3 for Exact Forms
pagebody | true | | Contains the contents of the page you are adding.

### API_AddSubGroup

```javascript--node
quickbase.api('API_AddSubGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_AddSubGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_AddSubGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_AddSubGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_AddSubGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_AddUserToGroup

```javascript--node
quickbase.api('API_AddUserToGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_AddUserToGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_AddUserToGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_AddUserToGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_AddUserToGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_AddUserToRole

```javascript--node
quickbase.api('API_AddUserToRole', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_AddUserToRole', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_AddUserToRole', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_AddUserToRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/add_user_to_role.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Authenticate

```javascript--node
quickbase.api('API_Authenticate', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Authenticate', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Authenticate', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Authenticate",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/authenticate.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_ChangeGroupInfo

```javascript--node
quickbase.api('API_ChangeGroupInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_ChangeGroupInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_ChangeGroupInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_ChangeGroupInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_ChangeGroupInfo.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_ChangeManager

```javascript--node
quickbase.api('API_ChangeManager', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_ChangeManager', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_ChangeManager', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_ChangeManager",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_ChangeManager.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_ChangeRecordOwner

```javascript--node
quickbase.api('API_ChangeRecordOwner', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_ChangeRecordOwner', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_ChangeRecordOwner', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_ChangeRecordOwner",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/change_record_owner.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_ChangeUserRole

```javascript--node
quickbase.api('API_ChangeUserRole', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_ChangeUserRole', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_ChangeUserRole', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_ChangeUserRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/change_user_role.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_CloneDatabase

```javascript--node
quickbase.api('API_CloneDatabase', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_CloneDatabase', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_CloneDatabase', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_CloneDatabase",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/clone_database.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_CopyGroup

```javascript--node
quickbase.api('API_CopyGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_CopyGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_CopyGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_CopyGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_CopyGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_CopyMasterDetail

```javascript--node
quickbase.api('API_CopyMasterDetail', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_CopyMasterDetail', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_CopyMasterDetail', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_CopyMasterDetail",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_CopyMasterDetail.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_CreateDatabase

```javascript--node
quickbase.api('API_CreateDatabase', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_CreateDatabase', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_CreateDatabase', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_CreateDatabase",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/create_database.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_CreateGroup

```javascript--node
quickbase.api('API_CreateGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_CreateGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_CreateGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_CreateGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_CreateGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_CreateTable

```javascript--node
quickbase.api('API_CreateTable', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_CreateTable', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_CreateTable', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_CreateTable",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/create_table.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_DeleteDatabase

```javascript--node
quickbase.api('API_DeleteDatabase', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_DeleteDatabase', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_DeleteDatabase', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_DeleteDatabase",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/delete_database.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_DeleteField

```javascript--node
quickbase.api('API_DeleteField', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_DeleteField', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_DeleteField', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_DeleteField",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/delete_field.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_DeleteGroup

```javascript--node
quickbase.api('API_DeleteGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_DeleteGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_DeleteGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_DeleteGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_DeleteGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_DeleteRecord

```javascript--node
quickbase.api('API_DeleteRecord', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_DeleteRecord', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_DeleteRecord', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_DeleteRecord",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/delete_record.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_DoQuery

```javascript--node
quickbase.api('API_DoQuery', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_DoQuery', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->API_DoQuery('API_', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_DoQuery",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/do_query.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_DoQueryCount

```javascript--node
quickbase.api('API_DoQueryCount', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_DoQueryCount', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_DoQueryCount', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_DoQueryCount",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/do_query_count.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_EditRecord

```javascript--node
quickbase.api('API_EditRecord', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_EditRecord', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_EditRecord', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_EditRecord",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/edit_record.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_FieldAddChoices

```javascript--node
quickbase.api('API_FieldAddChoices', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_FieldAddChoices', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_FieldAddChoices', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_FieldAddChoices",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/field_add_choices.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_FieldRemoveChoices

```javascript--node
quickbase.api('API_FieldRemoveChoices', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_FieldRemoveChoices', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_FieldRemoveChoices', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_FieldRemoveChoices",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/field_remove_choices.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_FindDBByName

```javascript--node
quickbase.api('API_FindDBByName', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_FindDBByName', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_FindDBByName', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_FindDBByName",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/find_db_by_name.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GenAddRecordForm

```javascript--node
quickbase.api('API_GenAddRecordForm', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GenAddRecordForm', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GenAddRecordForm', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GenAddRecordForm",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/gen_add_record_form.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GenResultsTable

```javascript--node
quickbase.api('API_GenResultsTable', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GenResultsTable', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GenResultsTable', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GenResultsTable",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/gen_results_table.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetAncestorInfo

```javascript--node
quickbase.api('API_GetAncestorInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetAncestorInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetAncestorInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetAncestorInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getancestorinfo.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetAppDTMInfo

```javascript--node
quickbase.api('API_GetAppDTMInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetAppDTMInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetAppDTMInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetAppDTMInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/get_app_dtm_info.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetDBPage

```javascript--node
quickbase.api('API_GetDBPage', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetDBPage', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetDBPage', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetDBPage",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/get_db_info.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetDBInfo

```javascript--node
quickbase.api('API_GetDBInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetDBInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetDBInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetDBInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/get_db_page.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetDBVar

```javascript--node
quickbase.api('API_GetDBVar', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetDBVar', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->API_GetDBVar('API_', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetDBVar",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getdbvar.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetFieldProperties

```javascript--node
quickbase.api('API_GetFieldProperties', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetFieldProperties', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->API_GetFieldProperties('API_', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetFieldProperties",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_GetFieldProperties.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetGroupRole

```javascript--node
quickbase.api('API_GetGroupRole', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetGroupRole', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetGroupRole', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetGroupRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_GetGroupRole.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetNumRecords

```javascript--node
quickbase.api('API_GetNumRecords', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetNumRecords', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetNumRecords', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetNumRecords",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getnumrecords.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetSchema

```javascript--node
quickbase.api('API_GetSchema', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetSchema', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetSchema', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetSchema",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getschema.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetRecordAsHTML

```javascript--node
quickbase.api('API_GetRecordAsHTML', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetRecordAsHTML', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetRecordAsHTML', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetRecordAsHTML",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getrecordashtml.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetRecordInfo

```javascript--node
quickbase.api('API_GetRecordInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetRecordInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetRecordInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetRecordInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getrecordinfo.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetRoleInfo

```javascript--node
quickbase.api('API_GetRoleInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetRoleInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetRoleInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetRoleInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getroleinfo.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetUserInfo

```javascript--node
quickbase.api('API_GetUserInfo', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetUserInfo', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetUserInfo', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetUserInfo",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getuserinfo.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetUserRole

```javascript--node
quickbase.api('API_GetUserRole', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetUserRole', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetUserRole', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetUserRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/getuserrole.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GetUsersInGroup

```javascript--node
quickbase.api('API_GetUsersInGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GetUsersInGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GetUsersInGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GetUsersInGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_GetUsersInGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GrantedDBs

```javascript--node
quickbase.api('API_GrantedDBs', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GrantedDBs', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GrantedDBs', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GrantedDBs",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/granteddbs.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GrantedDBsForGroup

```javascript--node
quickbase.api('API_GrantedDBsForGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GrantedDBsForGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GrantedDBsForGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GrantedDBsForGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_GrantedDBsForGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_GrantedGroups

```javascript--node
quickbase.api('API_GrantedGroups', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_GrantedGroups', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_GrantedGroups', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_GrantedGroups",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_GrantedGroups.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_ImportFromCSV

```javascript--node
quickbase.api('API_ImportFromCSV', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_ImportFromCSV', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_ImportFromCSV', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_ImportFromCSV",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/importfromcsv.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_ProvisionUser

```javascript--node
quickbase.api('API_ProvisionUser', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_ProvisionUser', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_ProvisionUser', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_ProvisionUser",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/provisionuser.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_PurgeRecords

```javascript--node
quickbase.api('API_PurgeRecords', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_PurgeRecords', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_PurgeRecords', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_PurgeRecords",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/purgerecords.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_RemoveGroupFromRole

```javascript--node
quickbase.api('API_RemoveGroupFromRole', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_RemoveGroupFromRole', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_RemoveGroupFromRole', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_RemoveGroupFromRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_RemoveGroupFromRole.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_RemoveSubgroup

```javascript--node
quickbase.api('API_RemoveSubgroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_RemoveSubgroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_RemoveSubgroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_RemoveSubgroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_RemoveSubgroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_RemoveUserFromGroup

```javascript--node
quickbase.api('API_RemoveUserFromGroup', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_RemoveUserFromGroup', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_RemoveUserFromGroup', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_RemoveUserFromGroup",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_RemoveUserFromGroup.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_RemoveUserFromRole

```javascript--node
quickbase.api('API_RemoveUserFromRole', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_RemoveUserFromRole', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_RemoveUserFromRole', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_RemoveUserFromRole",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/removeuserfromrole.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_RenameApp

```javascript--node
quickbase.api('API_RenameApp', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_RenameApp', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_RenameApp', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_RenameApp",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/renameapp.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_RunImport

```javascript--node
quickbase.api('API_RunImport', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_RunImport', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_RunImport', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_RunImport",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/runimport.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_SendInvitation

```javascript--node
quickbase.api('API_SendInvitation', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_SendInvitation', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_SendInvitation', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_SendInvitation",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/sendinvitation.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_SetDBVar

```javascript--node
quickbase.api('API_SetDBVar', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_SetDBVar', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->API_SetDBVar('API_', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_SetDBVar",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/setdbvar.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_SetFieldProperties

```javascript--node
quickbase.api('API_SetFieldProperties', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_SetFieldProperties', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_SetFieldProperties', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_SetFieldProperties",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/setfieldproperties.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_SetKeyField

```javascript--node
quickbase.api('API_SetKeyField', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_SetKeyField', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_SetKeyField', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_SetKeyField",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/setkeyfield.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_SignOut

```javascript--node
quickbase.api('API_SignOut', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_SignOut', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->API_SignOut('API_', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_SignOut",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/signout.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_UploadFile

```javascript--node
quickbase.api('API_UploadFile', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_UploadFile', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_UploadFile', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_UploadFile",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/uploadfile.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_UserRoles

```javascript--node
quickbase.api('API_UserRoles', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_UserRoles', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_UserRoles', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_UserRoles",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/userroles.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Webhooks_Activate

```javascript--node
quickbase.api('API_Webhooks_Activate', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Webhooks_Activate', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Webhooks_Activate', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Webhooks_Activate",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_Webhooks_Activate.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Webhooks_Copy

```javascript--node
quickbase.api('API_Webhooks_Copy', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Webhooks_Copy', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Webhooks_Copy', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Webhooks_Copy",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_Webhooks_Copy.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Webhooks_Create

```javascript--node
quickbase.api('API_Webhooks_Create', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Webhooks_Create', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Webhooks_Create', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Webhooks_Create",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_Webhooks_Create.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Webhooks_Delete

```javascript--node
quickbase.api('API_Webhooks_Delete', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Webhooks_Delete', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Webhooks_Delete', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Webhooks_Delete",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_Webhooks_Delete.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Webhooks_Deactivate

```javascript--node
quickbase.api('API_Webhooks_Deactivate', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Webhooks_Deactivate', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Webhooks_Deactivate', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Webhooks_Deactivate",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_Webhooks_Deactivate.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

### API_Webhooks_Edit

```javascript--node
quickbase.api('API_Webhooks_Edit', {
  dbid: 'bddnn3uz9'
}).then((results) => {
  // Handle results
}).catch((error) => {
  // Handle error
});
```

```javascript--browser
quickbase.api('API_Webhooks_Edit', {
  dbid: 'bddnn3uz9'
}).then(function(results){
  // Handle results
}).catch(function(error){
  // Handle error
});
```

```php
<?php

try {
  $results = $quickbase->api('API_Webhooks_Edit', array(
    'dbid' => 'bddnn3uz9'
  ));

  // Handle results
}catch(\Exception $error){
  // Handle error
}

?>
```

> The above returns JSON structured like this:

```json
{
  "action": "API_Webhooks_Edit",
  "errcode": 0,
  "errtext": "No error"
}
```

<a href='https://help.quickbase.com/api-guide/API_Webhooks_Edit.html' target='_blank'>Quick Base Documentation</a>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
dbid | true | | Application DBID

# QBRecord

## Initialization

## Instance Methods

### clear

### delete

### get

### getDBID

### getFid

### getFids

### getField

### getFields

### getTableName

### load

### loadSchema

### save

### set

### setDBID

### setFid

### setFids

### toJson

# QBTable

## Initialization

## Instance Methods

### clear

### deleteRecord

### deleteRecords

### getAppID

### getDateFormat

### getDBID

### getChildTables

### getFid

### getFids

### getField

### getFields

### getNRecords

### getOptions

### getPlural

### getQueries

### getQuery

### getRecord

### getRecords

### getSingular

### getSList

### getTableName

### getTimezone

### getVariable

### getVariables

### load

### loadNRecords

### loadSchema

### save

### setDBID

### setFid

### setFids

### setOptions

### setQuery

### setSList

### toJson

### upsertRecord

### upsertRecords

## Static Methods

### NewRecord

### NewRecords

# Formatting Data for Quick Base

Quick Base has very specific patterns and formats for various field types that you will need to pay attention to.

Below is a table of Quick Base field types and their acceptable values.

UI: Field Type | Acceptable Values
---------------|------------------
Text | Any characters, special characters, numbers, or symbols. Note that non-alphanumeric characters (anything other than A-Z, a-z, and 0-9) may need to be encoded to appear as intended in your data.
Text - Multi-line | Any characters, special characters, numbers, or symbols. Note that non-alphanumeric characters (anything other than A-Z, a-z, and 0-9) may need to be encoded to appear as intended in your data.
Text - Multiple Choice | A valid choice that has been set up for the multiple choice field. Note that Quick Base does not validate case here; if you enter "ford" and, in your application, the choice is "Ford," the choice will be accepted.<br /><br />If you enter an invalid choice, Quick Base generates an error.
Multi-select Text | Up to 20 valid choices from the list set up for the field, separated by semi-colons. The set of choices provided must not contain duplicates.<br /><br />Note: Choices added to a multi-select text field are limited to 60 characters, and the total number of choices in the field may not exceed 100.
Numeric | Positive or negative numbers. Quick Base ignores any non-numeric characters you enter here, but will not generate an error.<br /><br />If you've specified decimal places using API_SetFieldProperties, the value will be truncated  or lengthened accordingly.
Numeric - Currency | Positive and negative numbers, with or without decimals. The decimal character should match the decimal character set in the field's properties.
Numeric - Percent | A number that represents the percentage. Note that, if you want to indicate 80%, you should enter 80 in this field.
Numeric - Rating | A numeric rating, from 1 - 5. Quick Base displays ratings as stars; if you enter 3 in a Numeric-Rating field, Quick Base displays 3 out of 5 stars selected.
Date | A date in the format specified in the app's properties.<br /><br />Alternatively, a date in milliseconds since January 1, 1970 00:00:00 UTC.  Note that the Quick Base HTTP API returns dates in this format, which is the same internal representation used by JavaScript.
Date/Time | A date and time. Dates should be in the format specified in the app's properties.<br /><br />This field is an extended date field that can also contain the time, in the format HH:MM AM/PM. If you don't specify AM or PM, Quick Base defaults to AM. If you don't specify a time, Quick Base defaults to 12:00 AM.
Time of Day | A time in this format: HH:MM AM/PM.<br /><br />If you do not specify AM or PM, Quick Base defaults to AM.
Duration | A number that indicates a period of time. Note that you must use API_SetFieldProperties to set the unit of measure.<br /><br />If you enter a non-numeric value here, Quick Base ignores the value (no error is generated.)
Checkbox | A string that indicates whether the checkbox is checked or not.<br /><br />To specify that a checkbox is checked, enter any of these values:<ul><li>1</li><li>yes</li><li>true</li><li>on</li></ul>To specify that a checkbox is not checked, enter any string other than those listed above, or leave the parameter blank. If a Checkbox field is required, and does not default to "checked," you must enter some value to be able to save the record.
Phone Number | A phone number, with or without an extension. Enter a 10-digit string of numbers. You are not required to enter special characters (parentheses or dashes).<br /><br />Example: For this phone number: (123) 456-7890<br /><br />...enter  1234567890<br /><br />If you want to include an extension, you can enter x after the last digit of the phone number, followed by the numeric characters that make up the extension. There is no minimum or maximum character limit on extensions.<br /><br />Example: For this phone number:(123) 456-7890 x9876<br /><br />...enter  1234567890x9876<br /><br />Quick Base ignores any non-numeric character you enter here (except for the x used for extensions).
Email Address | An email address (joeuser@example.com).<br /><br />Note that if you enter an invalid email address, Quick Base does not generate an error.
User | A Quick Base user's email address or Quick Base user name.
List-User | Quick Base users' email addresses, Quick Base user names, or hashed user IDs, separated by semi-colons.<br /><br />Example: joe@example.com;sue@example.com
File Attachment | A base64-encoded file.<br /><br />Note that you must not use MIME encoding and must not include MIME headers. Note that many base64 encoders or base64 encoding methods are for MIME type encoding and will not work with Quick Base.
URL | A Web address. If you don't enter "http://", Quick Base adds it for you.<br /><br />Note that if you enter an invalid Web address, Quick Base does not generate an error.
Report Link | Report links are derived from other fields. You can update which report is linked to by updating the field that the report link refers to. You can't write to this type of field directly. If your API writes to a Report Link field, Quick Base ignores the call.
iCalendar | iCalendar fields are derived from other fields. You can update this type of field only by updating the fields to which it refers. You can't write to this type of field directly. If your API writes to an iCalendar field, Quick Base returns an error.
vCard | vCard fields are derived from other fields. You can update this type of field only by updating the fields to which it refers.  You can't write to this type of field directly If your API writes to a vCard field, Quick Base returns an error.
Predecessor | The Record ID of the predecessor record. Note that if you enter an invalid Record ID here, Quick Base returns an error.
Formula | Formula fields are derived from other fields. You cannot write to this type of field directly. If your API writes to a formula field, Quick Base returns an error.
