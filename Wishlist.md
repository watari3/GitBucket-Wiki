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

### 1.1 Issue Priority

 - similar to the "labels" entity, a new entity "priority" is required where admin users can define the priorities for each project or organization.
 - minimal fields: 
    - id [numeric] 
    - name [string]
    - order [integer] - this defines the order, to be able to sort
 - an issue can have only one priority at a time