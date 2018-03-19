# Product lifecycle state diagram

![shot_180319_151628](https://user-images.githubusercontent.com/14268190/37584546-81a68064-2b88-11e8-8ed2-16b3cf66b9e4.png)

## Draft
The draft state for a Product or API is when a Product or API definition is not deployed and is not associated with any Catalog

## Staged
When we stage a Product, a copy of the Product version is deployed to the target Catalog. Staged is the initial state when we publish a Product.
When a Product is in the staged state, it is not yet visible to, or subscribable by, any developers.
Staging of Product versions is usually carried out (tiến hành) in the API Designer.

## Published
when we publish a Product, a fixed copy of the Product version is deployed to the target Catalog. The Product version is visible to and subscribable by, the targeted developers or communities.
when a Product is published in a Catalog, the visibility and subscription settings can be changed for the published version of that Product.

## Deprecated (phản đối)
when we deprecate a Product, the Product version is visible only to developers whose applications are currently subsribed. No new subscriptions to the Plans in the Product are possible.

## Retired (Nghỉ hưu)
When we retire a Product, the Product version can neither be viewed nor can its Plans be subscribed to, and all of the associated APIs are taken offline.
