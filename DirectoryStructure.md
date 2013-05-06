GitBucket persists all data into HOME/gitbucket. This directory has following structure:

 * /HOME/gitbucket
   * /repositoties
     * /USER_NAME
       * / REPO_NAME.git (substance of repository. GitServlet sees this directory)
   * /tmp
     * /USER_NAME
       * /init-REPO_NAME (used in repository creation and removed after it)
       * /REPO_NAME.wiki
       * /REPO_NAME
          * /download