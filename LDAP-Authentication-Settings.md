GitBucket supports LDAP Authentication. <br />
GitBucket LDAP Authentication needs account that has read access to the LDAP.

## How to

1. Login as Admin Account.
2. Go to **Administration** and click **System Settings**.
3. Check **LDAP** and enter these values.

| Name | Description | Example Value |
|:-----|:------------|:--------------|
| LDAP Host | LDAP host name | 192.168.32.1 |
| LDAP Port | LDAP port | 389 (default 389) |
| Bind DN | Username that has read access to the LDAP | tarou (or allows email address type: tarou@example.com) |
| Bind Password | Password for Bind DN Account | password |
| Base DN | Top level DN of your LDAP directory tree(use for search user) | DC=example,DC=com |
| User name attribute | Name of the LDAP attribute. This is used as the GitBucket username | uid , sAMAccountName |
| Mail address attribute | Email address of LDAP attribute | mail |


## How to debug

NOTE: requires 1.7 or later. 1.6 has no debug log.

### log samples

```
15:43:56.529 [qtp1820788751-15] INFO  service.AccountService - LDAP Authentication Failed: System LDAP authentication failed.
15:43:37.386 [qtp1820788751-15] INFO  service.AccountService - LDAP Authentication Failed: User does not exist
15:49:34.565 [qtp1820788751-18] INFO  service.AccountService - LDAP Authentication Failed: User LDAP Authentication Failed.
15:41:09.370 [qtp1820788751-16] INFO  service.AccountService - LDAP Authentication Failed: Can't find mail address.
```

### LDAP Authentication log message

* Case: System LDAP authentication failed
    * Failed to access LDAP host/port by using Bind DN and Bind Password.
    * Wrong host, port, Bind DN or Bind Password
* Case: User does not exists
    * LDAP search username account from Base DN but not found user.
* Case: User LDAP Authentication Failed.
    * Found user but failed to login.(maybe not matched password)
* Case: Can't find mail address
    * Found user and user authentication success.
    * but not found email address (Email address attibutes). GitBucket requires email address attirbutes.
