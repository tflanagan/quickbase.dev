---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php
  - javascript

toc_footers:
  - <a href='https://www.quickbase.com/'>Quick Base</a>
  - <a href='https://www.datacollaborative.com/'>Data Collaborative</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to quickbase.dev! This site is dedicated to providing information on how to navigate Quick Base's API.

We have examples in PHP and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

We are continually trying to improve the information available! Please feel free to submit an issue or a pull request to help us improve!

# QuickBase

## Initiation

## api

### API_AddField

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

## Initiation

## clear

## delete

## get

## getDBID

## getFid

## getFids

## getField

## getFields

## getTableName

## load

## loadSchema

## save

## set

## setDBID

## setFid

## setFids

## toJson

# QBTable

## clear

## deleteRecord

## deleteRecords

## getAppID

## getDateFormat

## getDBID

## getChildTables

## getFid

## getFids

## getField

## getFields

## getNRecords

## getOptions

## getPlural

## getQueries

## getQuery

## getRecord

## getRecords

## getSingular

## getSList

## getTableName

## getTimezone

## getVariable

## getVariables

## load

## _load

## loadNRecords

## loadSchema

## save

## setDBID

## setFid

## setFids

## setOptions

## setQuery

## setSList

## toJson

## upsertRecord

## upsertRecords

## NewRecord

## NewRecords

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

