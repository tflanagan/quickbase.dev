# Error Handling

When making an API call to Quick Base, Quick Base responds with at least three values, `errcode`, `errtext`, and `errdetail`.

After receiving a response, the libraries detailed automatically check and verify that the response does not contain an error. You do not have to explicity check if `errcode` is equal to 0.

In JavaScript, an `Error` is thrown and needs to be caught in a `catch()` along the `Promise` chain.

In PHP, an `Exception` is thrown and needs to be caught in a `try/catch` statement.

Here is a list of possible Quick Base errors.

Error Code | Error Text | Probable Cause
---------- | ---------- | --------------
0 | No error | No error
1 | Unknown error |
2 | Invalid input | You did not specify a name for the new application.<br /><br />To grant Group Record permissions, you must specify the Target Group name.<br /><br />This option is available to corporate workgroup users only.<br /><br />You cannot place more than .5 MB in this field.<br /><br />You do not have permission to access this table.<br /><br />You did not specify the field name.<br /><br />The field identifier (fid) does not refer to an existing field. Please provide a valid field identifier.
3 | Insufficient permissions | You do not have permission to perform the current operation.<br /><br />You do not have permission to clone (copy) an application. At a minimum, you must have View permission to copy an application.<br /><br />You do not have permission to query this table. At a minimum, you must have View permission to perform this operation.<br /><br />You do not have sufficient permission to delete the records you specified from this table.
4 | Bad ticket | No user account was found that matches the e-mail address (or user name) and password you entered.<br /><br />Your ticket has expired or is invalid. By default tickets expire after 12 hours of use. One-time use tickets expire after one use or after 5 minutes. Please try signing in again.
5 | Unimplemented operation |
6 | Syntax error | Your Quick Base API call does not have the correct syntax.
7 | API not allowed on this application table
8 | SSL required for this application table
9 | Invalid choice | You cannot set the specified field to "(choice)".
10 | Invalid field type |
11 | Could not parse XML input | Your XML request contains invalid XML.
12 | Invalid source DBID |
13 | Invalid account ID |
14 | Missing DBID or DBID of wrong type |
15 | Invalid hostname |
19 | Unauthorized IP address | Realm IP filtering is in place, and the user is not logged in from an authorized location.
20 | Unknown username/password | No user account was found that matches the e-mail address (or user name) and password you entered.
21 | Unknown user | No user account was found that matches the e-mail address or user name you entered.<br /><br />No group was found that matches the group name you entered.
22 | Sign-in required |
23 | Feature not supported |
24 | Invalid application token |
25 | Duplicate application token |
26 | Max count |
27 | Registration required | The user specified is someone who has been invited to join an application, but who has never used Quick Base before.
28 | Managed by LDAP |
29 | User on Deny list |
30 | No such record | There is no record in the table that matches the specified record identifier (rid).
31 | No such field | There is no field in this table that matches the specified field identifier (fid).
32 | The application does not exist or was deleted |
33 | No such query | There is no query in this table that matches the specified query identifier (qid).<br /><br />There is no query in this table that matches the specified query name (qname).
34 | You cannot change the value of this field |
35 | No data returned |
36 | Cloning error |
37 | No such report |
38 | Periodic report contains a restricted field |
50 | Missing required field | The default value property for the specified field is blank. Please supply a non-blank value for this field.<br /><br />You have either provided a blank value for one or more required fields, or you haven't supplied any value for one or more required fields whose default property is blank. |
51 | Attempting to add a non-unique value to a field marked "unique" |
52 | Duplicate field |
53 | The following required fields are missing from your import data: <field_1> [[<field_2>]...] |
54 | Cached list of records not found |
60 | Update conflict detected |
61 | Schema is locked | You cannot make development changes to an application because it is locked. Quick Base locks an application when you create a sandbox copy of the application.
70 | Account size limit exceeded |
71 | Database size limit exceeded |
73 | Your account has been suspended |
74 | You are not allowed to create applications | You do not have Create permissions on the billing account.
75 | View too large |
76 | Too many criteria | The limit on the number of criteria is currently set to 100 and you have exceeded this limit.
77 | API request limit exceeded | This can be returned if an API is called too frequently. The same API can't be called again before the time specified in the error message.
78 | Data limit exceeded |
80 | Overflow |
81 | Item not found |
82 | Operation took too long |
83 | Access denied |
84 | Database error |
85 | Schema update error |
87 | Invalid group | No group matched the group ID you provided.
100 | Technical Difficulties -- try again later |
101 | Quick Base is temporarily unavailable due to technical difficulties |
102 | Invalid request - we cannot understand the URL you specified |
103 | The Quick Base URL you specified contained an invalid srvr parameter |
104 | Your Quick Base app is experiencing unusually heavy traffic. Please wait a few minutes and re-try this command. |
105 | Quick Base is experiencing technical difficulties |
110 | Invalid role | The specified role does not exist in the Quick Base.
111 | User exists | The specified user already exists.
112 | No user in role | If you try to change a user’s role and no user is in the role you specify as the user’s current role, you’ll get this error.
113 | User already in role | The user has already been assigned the role you are trying to assign.
114 | Must be admin user |
150 | Upgrade plan |
151 | Expired plan |
152 | App suspended | The application is suspended, possibly due to nonpayment.
