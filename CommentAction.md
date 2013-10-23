After the issue creation at GitBucket, users can add comments or close it.
The details are saved at ```ISSUE_COMMENT``` table.

To determine if it was any operation, you see the ```ACTION``` column.

|ACTION|
|--------|
|comment|
|close_comment|
|reopen_comment|
|close|
|reopen|
|commit|
|merge|

#####comment
This value is saved when users have made a normal comment.

#####close_comment, reopen_comment
These values are saved when users have reopened or closed the issue with comments.

#####close, reopen
These values are saved when users have reopened or closed the issue.
At the same time, store the fixed value(i.e. "Close" or "Reopen") to the ```CONTENT``` column.
Therefore, this comment is not displayed, and not counted as a comment.

#####commit
This value is saved when users have pushed including the ```#issueId``` to the commit message.
At the same time, store it to the ```CONTENT``` column with its commit id.
This comment is displayed. But it can not be edited by all users, and also not counted as a comment.

#####merge
This value is saved when users have merged the pull request.
At the same time, store the message to the ```CONTENT``` column.
This comment is displayed. But it can not be edited by all users, and also not counted as a comment.
