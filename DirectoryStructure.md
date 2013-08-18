GitBucket persists all data into HOME/gitbucket. This directory has following structure:

```
* /HOME/gitbucket
  * /repositoties
    * /USER_NAME
      * / REPO_NAME.git (substance of repository. GitServlet sees this directory)
      * / REPO_NAME.wiki.git (wiki repository)
  * /data
    * /USER_NAME
      * /files
        * avatar.xxx (image file of user avatar)
  * /tmp
    * /_upload
      * /SESSION_ID (removed at session timeout)
        * yyyyMMddHHmmSSsss (temporary file for file uploading)
    * /USER_NAME
      * /init-REPO_NAME (used in repository creation and removed after it)
      * /REPO_NAME.wiki (working directory for wiki repository)
      * /REPO_NAME
         * /download (temporary directories is created under this directory)
```