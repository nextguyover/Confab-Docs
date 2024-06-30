# User Functionality

Users form an important part of core functionality, since all activities require a visitor to be logged in. Users are uniquely identified by their email address. 

## Administrators

Admins are set using the [backend server config](../../config/index.md#user-roles). Each Confab instance can have one or more Admins. Admins have a higher level of privilege required to moderate comments and users. Read more about [additional comment actions available to admins](../comments/index.md#administration-functionality).

## Reply Notifications

Reply notifications are enabled for users be default, where an email will be sent to them when someone replies to a comment they have left. This functionality can be disabled by the users themselves using the button on the bottom-left of the main Confab panel.

Additionally, this feature can be toggled off globally or for particular locations, using the [Admin Panel](../admin-panel/index.md#user-own-replies).

## Bans

Users can be banned by Admins. A banned user cannot login (will be informed at login that the provided email is banned), and will be rejected from performing all authenticated actions, including creating new comments, voting, editing, etc.

## Custom Usernames

Users are able to set custom usernames (2-15 characters in length), by clicking on "Anonymous" or their existing username on the main Confab panel. 

This feature can be disabled, and the cool-down duration can be customised using the [backend configuration](../../config/index.md#custom-usernames).

## Profile Pictures

Users are assigned a randomly generated identicon which serves to identify their comments (independently of usernames). Confab does not provide the ability to set custom profile pictures.

---

!!! note "Users Limitation"

    Confab currently does not have a central place to manage users, actions such as banning a user is only possible via a comment that a user has submitted. However this feature may be implemented in later versions. See the [future roadmap](../../about/roadmap.md).