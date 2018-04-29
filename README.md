ActivityPub Plugin for GNU Social Doc
=====================================

## Contents

- [Methods](#methods)
  - [Profiles](#profiles)
  - [Liked Collection](#liked-collection)
- [Entities](#entities)
  - [Profile](#profile)
  - [Notice](#notice)
  - [Image](#image)

###### Selecting ranges

For most `GET` operations that return arrays, the query parameters `max_id` and `since_id` can be used to specify the range of IDs to return.
API methods that return collections of items can return a `Link` header containing URLs for the `next` and `prev` pages.
See the [Link header RFC](https://tools.ietf.org/html/rfc5988) for more information.

###### Errors

If the request you make doesn't go through, the plugin will usually respond with an [Error](#error).

___

## Methods

### Profiles

#### Fetching an Actor's profile:

    GET :username/profile.json

Query parameters:

| Field      | Description                                                    | Optional   |
| ---------- | -------------------------------------------------------------- | ---------- |
| `pretty`   | If set it generates a formated output                          | yes        |

Returns a [Profile](#profile).

### Liked Collection

#### Getting an Actor's Liked Collection:

    GET :username/liked.json

Query parameters:

| Field      | Description                                                    | Optional   |
| ---------- | -------------------------------------------------------------- | ---------- |
| `pretty`   | If set it generates a formated output                          | yes        |
| `since`    | Starts from nth entry                                          | yes        |
| `limit`    | Maximum number of followers to get (Default 40, Max 80)        | yes        |

Return:

| Field           | Description                                                    |
| --------------- | -------------------------------------------------------------- |
| `id`            | URL for current endpoint                                       |
| `type`          | OrderedCollection                                              |
| `totalItems`    | Number of elements in orderedItems                             |
| `orderedItems`  | An array of [Notices](#notice).                                |


## Entities

> **Note:** Some attributes in the entity payload can have ``null`` value and are marked as _nullable_ on the tables below. Attributes that are not nullable are guaranteed to return a valid value.

### Profile

| Attribute                | Description                                                                        | Nullable |
| ------------------------ | ---------------------------------------------------------------------------------- | -------- |
| `@context`               | Standard compliance                                                                | no       |
| `id`                     | The ID of the Actor                                                                | no       |
| `type`                   | Person                                                                             | no       |
| `nickname`               | The username of the account                                                        | no       |
| `inbox`                  | URL to Actor's inbox endpoint                                                      | no       |
| `outbox`                 | URL to Actor's outbox endpoint                                                     | no       |
| `acct`                   | Equals `username` for local users, includes `@domain` for remote ones              | no       |
| `display_name`           | The Actor's display name                                                           | no       |
| `followers`              | URL to Actor's followers endpoint                                                  | no       |
| `followers`              | URL to Actor's following endpoint                                                  | no       |
| `liked`                  | URL to Actor's Liked collection endpoint                                           | no       |
| `liked_count`            | Total number of faves                                                              | no       |
| `summary`                | Actor's biography                                                                  | no       |
| `url`                    | URL of the Actor's profile page (can be remote)                                    | no       |
| `avatar`                 | [Image](#image) object with the Actor's avatar                                     | no       |

### Notice

| Attribute                | Description                                  | Nullable |
| ------------------------ | -------------------------------------------- | -------- |
| `id`                     | Notice's URL                                 | no       |
| `type`                   | Notice's Type                                | no       |
| `actor`                  | Notice owner's URL                           | no       |
| `published`              | DateTime of notice creation                  | no       |
| `to`                     | To                                           | no       |
| `cc`                     | CC                                           | no       |
| `content`                | Notice's Content in plain text               | no       |
| `rendered`               | Notice's Content in HTML                     | no       |
| `url`                    | Notice's URL                                 | no       |
| `reply_to`               | ID of the notice this replies                | yes      |
| `is_local`               | Equals 1 for local notices, 0 otherwise      | no       |
| `conversation`           | Notice conversation id                       | no       |
| `attachment`             | Attachment object                            | no       |
| `tag`                    | Tag array                                    | no       |

### Image

| Attribute                | Description             | Nullable |
| ------------------------ | ----------------------- | -------- |
| `type`                   | Image                   | no       |
| `width`                  | Image's width           | no       |
| `height`                 | Image's height          | no       |
| `url`                    | Image URL               | no       |
