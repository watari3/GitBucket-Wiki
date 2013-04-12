GitBucket persists all data into HOME/gitbucket. This directory has following structure:

 * /HOME/gitbucket
   * /repositoties
     * /USER_NAME
       * / REPO_NAME.git (substance of repository. GitServlet sees this directory)
   * /tmp
     * /USER_NAME
       * /branches
         * /REPO_NAME
           * /BRANCH_NAME (Repository browser sees these directories)
       * /init-REPO_NAME (used in repository creation and removed after it)