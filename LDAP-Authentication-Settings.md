GitBucket supports LDAP Authentication. <br />
GitBucket LDAP Authentication needs account that has read access to the LDAP.

## Settings

1. Login as Admin Account.
2. Go to **Administration** and click **System Settings**.
3. Check **LDAP** and enter these values.

| Name | Description | Example Value |
|:-----|:------------|:--------------|
| LDAP Host | LDAP host name | 192.168.32.1 |
| LDAP Port | LDAP port | 389 (default 389) |
| Bind DN | Username that has read access to the LDAP | tarou (or allows email address type: tarou@example.com) |
| Bind Password | Password for Bind DN Account | password |
| Base DN | Top level DN of your LDAP directory tree(use for search user) | DC=example.com,DC=com |
| User name attribute | Name of the LDAP attribute. This is used as the GitBucket username | uid , sAMAccountName |
| Mail address attribute | Email address of LDAP attribute | mail |
