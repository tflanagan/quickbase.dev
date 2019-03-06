---
title: Quick Base Developers

language_tabs: # must be one of https://git.io/vQNgJ
  - php
  - javascript--node
  - javascript--browser

toc_footers:
  - <a href='https://www.quickbase.com/' target='_blank'>Quick Base</a> | <a href='https://help.quickbase.com/api-guide/' target='_blank'>Quick Base API</a>
  - <a href='https://github.com/lord/slate' target='_blank'>Documentation Powered by Slate</a>
  - Developed by <a href='https://github.com/tflanagan' target='_blank'>Tristian Flanagan</a> | <a href='https://github.com/tflanagan/quickbase.dev' target='_blank'>GitHub</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to quickbase.dev! This site is dedicated to providing information on how to navigate Quick Base's API.

We have examples in PHP and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

We are continually trying to improve the information available! Please feel free to submit an issue or a pull request to help us improve!

# QuickBase

This is the low level Quick Base class, giving you direct access to Quick Base's API.

This class is utilized in the abstraction layers QBRecord and QBTable.

## Repository Links

* PHP: <a href='https://github.com/tflanagan/php-quickbase'>https://github.com/tflanagan/php-quickbase</a>
* JavaScript: <a href='https://github.com/tflanagan/node-quickbase'>https://github.com/tflanagan/node-quickbase</a>

## Installation & Loading

```php
<?php

// $ composer require tflanagan/quickbase

require_once(__DIR__.DIRECTORY_SEPARATOR.'vendor'.DIRECTORY_SEPARATOR.'autoload.php');

?>
```

```javascript--node
// $ npm install --save quickbase

const QuickBase = require('quickbase');
```

```javascript--browser
// <script type="text/javascript" src="quickbase.browserify.min.js"></script>
```

Installing the Quick Base library for your desired platform requires either including the browserified version of the library in your HTML page or install it via a package manager (npm or composer).

Including the library for use in your code depends on your platform.

## Initialization

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

<aside class="notice">
<i>(JavaScript Only)</i> This library makes API calls asychronously. As Quick Base cannot support `n` number of requests at a given time, the default limit is `10`. This throttles the requests sent to Quick Base in order to preserve the Quick Base applications integrity and performance.

If your application experiences a normal traffic rate that is higher than average, you may want to consider reducing the `connectionLimit` setting.
</aside>

The way we accomplish this is by exposing a single method `.api()`.

#### `.api(action[, options])`

```php
<?php

try {
  $results = $quickbase->api('SomeAPI_Action', array(
    'dbid' => 'bddnn3uz9',
    'someFutureProperty' => 'lorem ipsum'
  ));

  // Handle results
}catch(\Exception $err){
  // Handle error
}

?>
```

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

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
action | true | | The Quick Base API Action you wish to execute (ie: API_DoQuery)
options | false | | An object containing data pertaining to your API Action

## Quick Base API Endpoints

### API_AddField

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
}catch(\Exception $err){
  // Handle error
}

?>
```

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

<a href='https://help.quickbase.com/api-guide/add_field.html'>Quick Base Documentation</a>

Use API_AddField to add a new field to a table. You invoke this call on a table-level dbid.

When you add a field using API_AddField, you specify the field type, but no other field properties. After you've added the field, you can use API_SetFieldProperties to set the properties of the new field and any default values. (You can't set field type using API_SetFieldProperties; if you want to change the field type after adding the field, you must use the Quick Base UI.)

The amount of data space consumed by a field depends on the field type.

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
add_to_forms | false | false | Specifies whether the field you are adding should appear at the end of any form with form properties set to "Auto-Add new fields."
label | true | | Name of the new field
mode | Lookup/Formula only | | Specifies whether the field is a formula field or a lookup field
type | true | | The Quick Base field type.

Possible `type` values:

UI: TYPE | API: TYPE
Checkbox | checkbox
Date | date
Duration | duration
Email | Address email
File | Attachment file
Formula | (see the 'mode' param)
Lookup | (see the 'mode' param)
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

### API_AddRecord

### API_AddReplaceDBPage

### API_AddSubGroup

### API_AddUserToGroup

### API_AddUserToRole

### API_Authenticate

### API_ChangeGroupInfo

### API_ChangeManager

### API_ChangeRecordOwner

### API_ChangeUserRole

### API_CloneDatabase

### API_CopyGroup

### API_CopyMasterDetail

### API_CreateDatabase

### API_CreateGroup

### API_CreateTable

### API_DeleteDatabase

### API_DeleteField

### API_DeleteGroup

### API_DeleteRecord

### API_DoQuery

### API_DoQueryCount

### API_EditRecord

### API_FieldAddChoices

### API_FieldRemoveChoices

### API_FindDBByName

### API_GenAddRecordForm

### API_GenResultsTable

### API_GetAncestorInfo

### API_GetAppDTMInfo

### API_GetDBPage

### API_GetDBInfo

### API_GetDBVar

### API_GetGroupRole

### API_GetNumRecords

### API_GetSchema

### API_GetRecordAsHTML

### API_GetRecordInfo

### API_GetRoleInfo

### API_GetUserInfo

### API_GetUserRole

### API_GetUsersInGroup

### API_GrantedDBs

### API_GrantedDBsForGroup

### API_GrantedGroups

### API_ImportFromCSV

### API_ProvisionUser

### API_PurgeRecords

### API_RemoveGroupFromRole

### API_RemoveSubgroup

### API_RemoveUserFromGroup

### API_RemoveUserFromRole

### API_RenameApp

### API_RunImport

### API_SendInvitation

### API_SetDBVar

### API_SetFieldProperties

### API_SetKeyField

### API_SignOut

### API_UploadFile

### API_UserRoles

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

### _load

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
