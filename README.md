ActivityPub Plugin for GNU Social Doc
=====================================

## Contents

- [Methods](#methods)
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

### Liked Collection

#### Getting an Actor's Liked Collection:

    GET :nickname/liked.json

Query parameters:

| Field       | Description                                                    | Optional   |
| ----------- | -------------------------------------------------------------- | ---------- |
| `max_id`    | Get a list of likes with ID less than this value               | yes        |
| `since_id`  | Get a list of likes with ID greater than this value            | yes        |
| `limit`     | Maximum number of likes to get (Default 40, Max 80)            | yes        |

Return:

| Field           | Description                                                    |
| --------------- | -------------------------------------------------------------- |
| `id`            | URL for current endpoint                                       |
| `type`          | OrderedCollection                                              |
| `totalItems`    | Number of elements in orderedItems                             |
| `orderedItems`  | An array of [Notices](#notice).                                |

### Profiles

#### Fetching an Actor's profile:

    GET :nickname/profile.json

Returns a [Profile](#profile).

___

## Entities

> **Note:** Some attributes attributes in the entity payload can have ``null`` value and are marked as _nullable_ on the tables below. Attributes that are not nullable are guaranteed to return a valid value.

### Attachment

| Attribute                | Description                                                                       | Nullable |
| ------------------------ | --------------------------------------------------------------------------------- | -------- |
| `id`                     | ID of the attachment                                                              | no       |
| `mimetype`               | Mimetype                                                                          | no       |
| `url`                    | URL of the locally hosted version of the image                                    | no       |
| `meta`                   | See **attachment metadata** below                                                 | yes      |
| `title`                  | Attachment title                                                                  | no       |

**Attachment metadata:**

Images may contain `width`, `height`, `size`.

### Error

The most important part of an error response is the HTTP status code. Standard semantics are followed. The body of an error is a JSON object with this structure:

| Attribute                | Description                        | Nullable |
| ------------------------ | ---------------------------------- | -------- |
| `error`                  | A textual description of the error | no       |

### Image

| Attribute                | Description             | Nullable |
| ------------------------ | ----------------------- | -------- |
| `type`                   | Image                   | no       |
| `width`                  | Image's width           | no       |
| `height`                 | Image's height          | no       |
| `url`                    | Image URL               | no       |

### Notice

| Attribute                | Description                                       | Nullable |
| ------------------------ | ------------------------------------------------- | -------- |
| `id`                     | Notice's URL                                      | no       |
| `type`                   | Notice's Type                                     | no       |
| `actor`                  | URL of Notice owner profile page (can be remote)  | no       |
| `published`              | DateTime of notice creation                       | no       |
| `to`                     | To                                                | no       |
| `cc`                     | CC                                                | no       |
| `content`                | Notice's Content in plain text                    | no       |
| `rendered`               | Notice's Content in HTML                          | no       |
| `url`                    | Notice's URL                                      | no       |
| `reply_to`               | ID of the notice this replies                     | yes      |
| `is_local`               | Boolean, true if local, false otherwise           | no       |
| `conversation`           | Notice conversation id                            | no       |
| `attachment`             | Attachment object                                 | no       |
| `tag`                    | Tag array                                         | no       |

### Profile

| Attribute                | Description                                                                        | Nullable |
| ------------------------ | ---------------------------------------------------------------------------------- | -------- |
| `@context`               | Standard compliance                                                                | no       |
| `id`                     | Actor's id                                                                         | no       |
| `type`                   | Person                                                                             | no       |
| `nickname`               | Actor's nickname                                                                   | no       |
| `is_local`               | Boolean, true if local, false otherwise                                            | no       |
| `inbox`                  | URL to Actor's inbox endpoint                                                      | no       |
| `outbox`                 | URL to Actor's outbox endpoint                                                     | no       |
| `display_name`           | The Actor's display name                                                           | no       |
| `followers`              | URL to Actor's followers endpoint                                                  | no       |
| `following`              | URL to Actor's following endpoint                                                  | no       |
| `liked`                  | URL to Actor's Liked collection endpoint                                           | no       |
| `liked_count`            | Total number of favorites                                                          | no       |
| `summary`                | Actor's biography                                                                  | no       |
| `url`                    | URL of the Actor's profile page (can be remote)                                    | no       |
| `avatar`                 | [Image](#image) object with the Actor's avatar                                     | no       |

### Tag

| Attribute                | Description                                  | Nullable |
| ------------------------ | -------------------------------------------- | -------- |
| `name`                   | The hashtag, not including the preceding `#` | no       |
| `url`                    | The URL of the hashtag                       | no       |
