You can look GitBucket data in H2 via H2 Console integrated into GitBucket.

1. Logged-in to GitBucket with administrator user
2. Click ***Administration*** in the header
3. Click ***H2 Console*** in the side menu of administration page

![Administration Menu](admin_menu.png)

Then, you can see the connection form of the H2 console. To connect GitBucket database, input connection information as following:

- Driver Class: org.h2.Driver
- JDBC URL: jdbc:h2:~/.gitbucket/data
  - Note: Replace "~/.gitbucket" with your HOME directory if you changed the directory (e.g. "jdbc:h2:C:\GitBucket_HOME\data").
- User Name: sa
- Password: sa

![H2 Console](h2console.png)