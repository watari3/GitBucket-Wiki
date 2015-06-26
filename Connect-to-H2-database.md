You can look GitBucket data in H2 via H2 Console integrated into GitBucket.

1. Logged-in to GitBucket with administrator user
2. Click ***Administration*** in the header
3. Click ***H2 Console*** in the side menu of administration page

![Administration Menu](admin_menu.png)

To connect GitBucket database, input connection information as following:

- Driver Class: org.h2.Driver
- JDBC URL: jdbc:h2:~/.gitbucket/data
- User Name: sa
- Password: sa

![H2 Console](h2console.png)