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

| Field      | Description                                                    | Optional   |
| ---------- | -------------------------------------------------------------- | ---------- |
| `since`    | Starts from nth entry                                          | yes        |
| `limit`    | Maximum number of followers to get (Default 40, Max 80)        | yes        |

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
| `type`                   | One of: "image", "video", "gifv", "unknown"                                       | no       |
| `url`                    | URL of the locally hosted version of the image                                    | no       |
| `remote_url`             | For remote images, the remote URL of the original image                           | yes      |
| `preview_url`            | URL of the preview image                                                          | no       |
| `text_url`               | Shorter URL for the image, for insertion into text (only present on local images) | yes      |
| `meta`                   | See **attachment metadata** below                                                 | yes      |
| `description`            | A description of the image for the visually impaired (maximum 420 characters), or `null` if none provided  | yes      |

**Attachment metadata:**

May contain `small` and `original` (referring to the preview and the original file). Images may contain `width`, `height`, `size`, `aspect`, while videos (including GIFV) may contain `width`, `height`, `frame_rate`, `duration` and `bitrate`. There may be another top-level object, `focus` with the coordinates `x` and `y`. These coordinates can be used for smart thumbnail cropping, [see this for reference](https://github.com/jonom/jquery-focuspoint#1-calculate-your-images-focus-point).

> **Note**: When the type is "unknown", it is likely only `remote_url` is available and local `url` is missing

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
| `is_local`               | Equals 1 for local notices, 0 otherwise           | no       |
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
| `inbox`                  | URL to Actor's inbox endpoint                                                      | no       |
| `outbox`                 | URL to Actor's outbox endpoint                                                     | no       |
| `acct`                   | Equals `nickname` for local users, includes `@domain` for remote ones              | no       |
| `display_name`           | The Actor's display name                                                           | no       |
| `followers`              | URL to Actor's followers endpoint                                                  | no       |
| `followers`              | URL to Actor's following endpoint                                                  | no       |
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
