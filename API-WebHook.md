The gitbucket api/webhook is designed for compatibility of [the GitHub's it](https://developer.github.com/v3/).

Now implemented few api/webhook and few parameter yet. If you find some behavior difference from GitHub, maybe, it is gitbucket's bug or feature not implemented yet :).

## API

 * Issues
   * Comments
     * [List comments on an issue](https://developer.github.com/v3/issues/comments/#list-comments-on-an-issue)
     * [Create A Comment](https://developer.github.com/v3/issues/comments/#create-a-comment)
 * Pull Requests
   * [List pull requests](https://developer.github.com/v3/pulls/#list-pull-requests)
   * [Get a single pull request](https://developer.github.com/v3/pulls/#get-a-single-pull-request)
   * [List commits on a pull request](https://developer.github.com/v3/pulls/#list-commits-on-a-pull-request)
 * Repositories
   * [Get](https://developer.github.com/v3/repos/#get)
   * Status
     * [Create a Status](https://developer.github.com/v3/repos/statuses/#create-a-status)
     * [List Statuses for a specific Ref](https://developer.github.com/v3/repos/statuses/#list-statuses-for-a-specific-ref)
     * [Get the combined Status for a specific Ref](https://developer.github.com/v3/repos/statuses/#get-the-combined-status-for-a-specific-ref)
 * Users
   * [Get a single user](https://developer.github.com/v3/users/#get-a-single-user)
   * [Get the authenticated user](https://developer.github.com/v3/users/#get-the-authenticated-user)

## WebHook Events

 * [IssuesEvent](https://developer.github.com/v3/activity/events/types/#issuesevent)
 * [IssueCommentEvent](https://developer.github.com/v3/activity/events/types/#issuecommentevent)
 * [PullRequestEvent](https://developer.github.com/v3/activity/events/types/#pullrequestevent)
 * [PushEvent](https://developer.github.com/v3/activity/events/types/#pushevent)
