The gitbucket api/webhook is designed for compatibility of [the GitHub's it](https://developer.github.com/v3/).

Now implemented few api/webhook and few parameter yet. If you find some behavior difference from GitHub, maybe, it is gitbucket's bug or feature not implemented yet :).

## Authentication

 Yet, [personal token in header](https://developer.github.com/v3/#oauth2-token-sent-in-a-header) only.

## API

 * Issues
   * Comments
     * [List comments on an issue](https://developer.github.com/v3/issues/comments/#list-comments-on-an-issue)
     * [Create A Comment](https://developer.github.com/v3/issues/comments/#create-a-comment)
   * Labels
     * [List all labels for this repository](https://developer.github.com/v3/issues/labels/#list-all-labels-for-this-repository) (since v3.11)
     * [Get a single label](https://developer.github.com/v3/issues/labels/#get-a-single-label) (since v3.11)
     * [Create a label](https://developer.github.com/v3/issues/labels/#create-a-label) (since v3.11)
     * [Update a label](https://developer.github.com/v3/issues/labels/#update-a-label) (since v3.11)
     * [Delete a label](https://developer.github.com/v3/issues/labels/#delete-a-label) (since v3.11)
 * Pull Requests
   * [List pull requests](https://developer.github.com/v3/pulls/#list-pull-requests)
   * [Get a single pull request](https://developer.github.com/v3/pulls/#get-a-single-pull-request)
   * [List commits on a pull request](https://developer.github.com/v3/pulls/#list-commits-on-a-pull-request)
 * Repositories
   * [Get](https://developer.github.com/v3/repos/#get)
   * [Create](https://developer.github.com/v3/repos/#create)
   * [Enabling and disabling branch protection](https://developer.github.com/v3/repos/#enabling-and-disabling-branch-protection) (since v3.11)
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
 * [PullRequestReviewCommentEvent](https://developer.github.com/v3/activity/events/types/#pullrequestreviewcommentevent)
 * [PullRequestEvent](https://developer.github.com/v3/activity/events/types/#pullrequestevent)
 * [PushEvent](https://developer.github.com/v3/activity/events/types/#pushevent)
