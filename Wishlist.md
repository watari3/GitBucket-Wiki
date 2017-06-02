Wishlist with Improvements
==========================

## 1. Issues

In the current version, GitBucket issues have now only **labels** and **milestones** attached to them.

Labels are *misused* for the various attributes an issue should normally have, but this is a very poor choice, since labels can't be made **must** and also can't be forced to take only one specific **value**.

In addition, an issue would need a least:
1. a **"priority"** - only one actual value at a time, if nothing is set than the default priority would be "normal"
1. a **"type"** - "bug", "feature request", etc. 
1. a **"resolution"** - "new", "open", "closed", "won't fix", etc.

All the above attributes should be single values, and also must fields (e.g. an issue is a "feature request" or a "bug", but not both at the same time: misusing "labels" for this task, such errors this happen all the time).

Some examples: https://bugs.opera.com/help/issueTypes.html
### 1.1 Issue Priority

 - similar to the **Labels** entity, a new entity **Priorities** is required where admin users can define the priorities for each repository.
 - Table `priority` fields: 
    - `user_name` [string]
    - `repository_name` [string]
    - `priority_id` [numeric] 
    - `priority_name` [string]
    - `priority_description` [string] - description for the users
    - `priority_order` [integer] - this defines the order, to be able to sort
    - `priority_default` [boolean] - the default priority for a project when the issue creator does not specifies a value.

 - an issue can have only one priority at a time (so the foreign key field must be in the ```issue``` table)
 - example default values:
   ```
    name          order      description
    -------------------------------------
    "normal"        4      "default"
    "important"     3      "Issues can be shipped with a final product, but should be reviewed before the next release"
    "high"          2      "Issues should be addressed before a final product is delivered. If the issue cannot be resolved before delivery, it should be prioritized for the next release"
    "very high"     1      "Issues must be addressed before a final product is delivered"
    "highest"       0      "All defects at this priority must be fixed before any public product is delivered"
   ```

--------

### 1.2 Issue Type
 - similar to the **Labels** entity, a new entity **Types** is required where admin users can define the priorities for each repository.
 - Table `type` fields: 
    - `user_name` [string]
    - `repository_name` [string]
    - `type_id` [numeric] 
    - `type_name` [string]
    - `type_description` [string] - description for the users
    - `type_default` [boolean] - the default type for a project when the issue creator does not specify a value.

 - an issue can have only one type at a time (so the foreign key field must be in the ```issue``` table), and usually only admins should be allowed to change the type of an issue.
 - example default values:
   ```
    name           description
    --------------------------
    bug            "A bug is a problem which impairs or prevents the functions of the product"
    change         "A change request is an improvement or enhancement to an existing feature or task"
    feature        "A feature request is a new feature of the product, which has yet to be developed"
    patch          "A patch is a workaround that should be applied"
    support        "A support request provides answers to a technical support question and is only relevant for customer projects"
    task           "A task directs work that needs to be done"
   ```

-----------

### 1.3 Issue Resolution
 - similar to the **Labels** entity, a new entity **Resolutions** is required where admin users can define the priorities for each repository.
 - similar to the **IssueLabels** a new entity **IssueResolutions** is required.
 - Table `resolution` fields: 
    - `user_name` [string]
    - `repository_name` [string]
    - `resolution_id` [numeric] 
    - `resolution_name` [string]
    - `resolution_description` [string] - description for the users
    - `type_default` [boolean] - the default type for a project when the issue creator does not specify a value.
 - Table `issue_resolution```
 - an issue can have only one resolution at a time (so the foreign key field must be in the ```issue``` table), but the history is important, since the change of the resolution and their timestamp reflects the progress of an issue.
