ActivityPub Plugin for GNU Social Doc
=====================================

## Contents

- [Methods](#methods)
  - [Followers Collection](#followers-collection)
  - [Liked Collection](#liked-collection)
  - [Profiles](#profiles)
- [Entities](#entities)
  - [Attachment](#attachment)
  - [Error](#error)
  - [Image](#image)
  - [Notice](#notice)
  - [Profile](#profile)
  - [Tag](#tag)

###### Selecting ranges

For most `GET` operations that return arrays, the query parameters `max_id` and `since_id` can be used to specify the range of IDs to return.
API methods that return collections of items can return a `Link` header containing URLs for the `next` and `prev` pages.
See the [Link header RFC](https://tools.ietf.org/html/rfc5988) for more information.

###### Pretty output

For most operations if the `pretty` parameter is set a formated output will be generated (useful for learning about the API or debuging purposes).

###### Errors

If the request you make doesn't go through, the plugin will usually respond with an [Error](#error).

___

## Methods

### Followers Collection

#### Getting an Actor's Followers Collection:

    GET :nickname/followers.json

Query parameters:

| Field       | Description                                          | Optional   | Type       |
| ----------- | ---------------------------------------------------- | ---------- | ---------- |
| `page`      | Followers page index                                 | no         | int32      |

Return:

| Field           | Description                         | Type    |
| --------------- | ----------------------------------- | ------- |
| `id`            | URL for current endpoint            | string  |
| `type`          | OrderedCollectionPage               | string  |
| `totalItems`    | Total number of followers           | int32   |
| `orderedItems`  | The URL of each follower            | string  |

### Liked Collection

#### Getting an Actor's Liked Collection:

    GET :nickname/liked.json

Query parameters:

| Field       | Description                                          | Optional   | Type       |
| ----------- | ---------------------------------------------------- | ---------- | ---------- |
| `max_id`    | Get a list of likes with ID less than this value     | yes        | int32      |
| `since_id`  | Get a list of likes with ID greater than this value  | yes        | int32      |
| `limit`     | Maximum number of likes to get (Default 40, Max 80)  | yes        | int32      |

Return:

| Field           | Description                         | Type                         |
| --------------- | ----------------------------------- | ---------------------------- |
| `id`            | URL for current endpoint            | string                       |
| `type`          | OrderedCollection                   | string                       |
| `totalItems`    | Number of elements in orderedItems  | int32                        |
| `orderedItems`  | Array of [Notices](#notice)         | Array of [Notices](#notice)  |

### Profiles

#### Fetching an Actor's profile:

    GET :nickname/profile.json

Returns a [Profile](#profile).

___

## Entities

> **Note:** Some attributes attributes in the entity payload can have ``null`` value and are marked as _nullable_ on the tables below. Attributes that are not nullable are guaranteed to return a valid value.

### Attachment

| Attribute                | Description                                     | Nullable | Type    |
| ------------------------ | ----------------------------------------------- | -------- | ------- |
| `id`                     | ID of the attachment                            | no       | int32   |
| `mimetype`               | Mimetype                                        | no       | string  |
| `url`                    | URL of the locally hosted version of the image  | no       | string  |
| `meta`                   | See **attachment metadata** below               | yes      | Array   |
| `title`                  | Attachment title                                | no       | string  |

**Attachment metadata:**

Images may contain `width`, `height`, `size`.

### Error

The most important part of an error response is the HTTP status code. Standard semantics are followed. The body of an error is a JSON object with this structure:

| Attribute                | Description                        | Nullable | Type    |
| ------------------------ | ---------------------------------- | -------- | ------- |
| `error`                  | A textual description of the error | no       | string  |

### Image

| Attribute                | Description     | Nullable | Type    |
| ------------------------ | --------------- | -------- | ------- |
| `type`                   | "Image"         | no       | string  |
| `width`                  | Image's width   | no       | int32   |
| `height`                 | Image's height  | no       | int32   |
| `url`                    | Image URL       | no       | string  |

### Notice

| Attribute                | Description                                       | Nullable | Type                                 |
| ------------------------ | ------------------------------------------------- | -------- | ------------------------------------ |
| `id`                     | Notice's URL                                      | no       | string                               |
| `type`                   | Notice's Type                                     | no       | string                               |
| `actor`                  | URL of Notice owner profile page (can be remote)  | no       | string                               |
| `published`              | DateTime of notice creation                       | no       | datetime                             |
| `to`                     | To                                                | no       |                                      |
| `cc`                     | CC                                                | no       |                                      |
| `content`                | Notice's Content in plain text                    | no       | string                               |
| `rendered`               | Notice's Content in HTML                          | no       | string                               |
| `url`                    | Notice's URL                                      | no       | string                               |
| `reply_to`               | ID of the notice this replies                     | yes      | int32                                |
| `is_local`               | Boolean, true if local, false otherwise           | no       | bool                                 |
| `conversation`           | Notice conversation id                            | no       | int32                                |
| `attachment`             | Array of [Attachments](#attachment)               | no       | Array of [Attachments](#attachment)  |
| `tag`                    | Array of [Tags](#tag)                             | no       | Array of [Tags](#tag)                |

### Profile

| Attribute                | Description                                      | Nullable | Type             |
| ------------------------ | ------------------------------------------------ | -------- | ---------------- |
| `@context`               | Standard compliance                              | no       |                  |
| `id`                     | Actor's id                                       | no       | int32            |
| `type`                   | "Person"                                         | no       | string           |
| `nickname`               | Actor's nickname                                 | no       | string           |
| `is_local`               | True if local, false otherwise                   | no       | bool             |
| `inbox`                  | URL to Actor's inbox endpoint                    | no       | string           |
| `outbox`                 | URL to Actor's outbox endpoint                   | no       | string           |
| `display_name`           | The Actor's display name                         | no       | string           |
| `followers`              | URL to Actor's followers endpoint                | no       | string           |
| `followers_count`        | Total number of followers                        | no       | int32            |
| `following`              | URL to Actor's following endpoint                | no       | string           |
| `following_count`        | Total number of following                        | no       | int32            |
| `liked`                  | URL to Actor's Liked collection endpoint         | no       | string           |
| `liked_count`            | Total number of favorites                        | no       | int32            |
| `summary`                | Actor's biography                                | no       | string           |
| `url`                    | URL of the Actor's profile page (can be remote)  | no       | string           |
| `avatar`                 | Actor's avatar                                   | no       | [Image](#image)  |

### Tag

| Attribute                | Description                                  | Nullable | Type       |
| ------------------------ | -------------------------------------------- | -------- | ---------- |
| `name`                   | The hashtag, not including the preceding `#` | no       | string     |
| `url`                    | The URL of the hashtag                       | no       | string     |
