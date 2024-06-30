# Abuse Mitigation

As with any public-facing service, there are several abuse scenarios that Confab administrators must be aware of. This page will outline these scenarios, alongside instructions on how to utilise the built-in mitigation techniques that have been implemented in Confab.

!!! tip
    Regular backups of your Confab instance is recommended to easily recover from various abuse scenarios, as you can simply restore the state of your Confab instance at an earlier point in time. See [backup guide](../backup/index.md) for more info.

## Content Abuse

As with any public-facing service that hosts user-generated content, users may generate content that you do not wish to associate with your website.

The first option for mitigating this risk is keeping [Admin comment notifications](../../core-functionality/admin-panel/index.md#email-comment-notification-settings) enabled. If you decide to not use the Manual Moderation Queue, this will ensure that you receive immediate notifications of any new edits/comments, so that you may take action in a timely manner.

If you want to be certain that any content that you do not deem acceptable will not be presented on your website at all, or your website is a high-traffic site where visitors may see unacceptable content before you have a chance to remove it, it is recommended that you enable the [Manual Moderation Queue](../../core-functionality/manual-moderation/index.md) using the [backend configuration](../../config/index.md#manual-moderation), where all comments will require Admin approval before becoming publicly visible on the site.

!!! note
    If you have a high traffic site, Confab may not be the right choice for you. See [design philosophy](../../about/design.md#operation-scale).

### Custom Usernames

Users are able to set custom usernames which replace the default randomly generated username. Admins do not have any moderation functionality for custom usernames beyond banning offending users. 

As such, if you prefer, custom usernames can be disabled altogether using the [backend configuration](../../config/#custom-usernames).

## Resource Abuse

### Comment Spam

To prevent a bad actor from flooding your comments with spam, Confab has features to limit the number of comments that a user can post at one time. Two options are available, depending whether or not you have chosen to use the [Manual Moderation Queue](../../core-functionality/manual-moderation/).


=== "Manual Moderation Enabled"

    In this scenario, all new comments will be sent to the Manual Moderation Queue. A maximum limit on the number of comments by each user that can be in the Manual Moderation Queue can be set using the [backend configuration](../../config/index.md#manual-moderation).

=== "Manual Moderation Disabled"

    To limit the number of comments that a user can generate within a given timespan, use the [backend configuration](../../config/index.md#rate-limiting) to set a comment rate limit.

### Email Spam

Emails are [sent to users for various reasons](../../core-functionality/emails/index.md). To prevent your sending addresses being used for spam, and to prevent your SMTP quota being used up, Confab lets you set limits on the number of emails that can be sent.

To prevent large numbers of login authentication code emails being sent (to email addresses that may not even belong to the user requesting the authentication code), use the [backend configuration](../../config/index.md#user-authentication-parameters) to set limits on the maximum number of consecutive auth code emails that can be sent.

Confab also sends reply notifications to users when their own comments receive replies. To prevent this feature being used to generate spam emails, you may choose to [turn this feature off](../../core-functionality/admin-panel/index.md#user-own-replies), or, implement measures to prevent comment spam, as outlined [above](#comment-spam).