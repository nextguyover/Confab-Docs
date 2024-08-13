# Backend Configuration Reference

Confab's main configuration is done by editing the `appsettings.json` file. The location of this file depends on your install method (1). 
{ .annotate }

1. 
    === "Docker"
        Run this command to copy the file out of the container:
        
        ```bash
        docker cp confab:/confab-config/appsettings.json ./appsettings.json
        ```

        Edit the file on your host system as required, then copy the file back into the container with:

        ```bash
        docker cp ./appsettings.json confab:/confab-config/appsettings.json
        ```

    === "Bare Metal"
        The `appsettings.json` file is found in the same directory as your Confab executable. 

This page provides a reference for the configuration file.

!!! note
    All of the following code snippets on this page are for the `appsettings.json` file. This will not be specified per-block.

## Database Location

Specify the location that the SQLite database will be accessed (or created, if it doesn't exist).

```json
{   
    "ConnectionStrings": {
        "DefaultConnection": "Data Source=Database/sqlite.db" //(1)!
    }
}
```
{ .annotate }

1. E.g. this would name the database file "sqlite.db" and it would be placed at directory `Database/`


## Logging

Configure logging levels for various parts of the application. Logging level can be set to `"Trace"`, `"Debug"`, `"Information"`, `"Warning"`, `"Error"`, `"Critical"`, and `"None"`. Learn more about [ASP.Net Logging](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/logging/?view=aspnetcore-6.0){:target="_blank"}.

```json
{   
    "Logging": {
        "LogLevel": {
            "Default": "Information", //(1)!
            "Microsoft.EntityFrameworkCore.Infrastructure": "Warning", //(2)!
            "Microsoft.EntityFrameworkCore.Database.Command": "Warning", //(3)!
            "Microsoft.AspNetCore.HttpLogging": "Warning", //(4)!
            "Confab": "Information" //(5)!
        }
    }
}
```
{ .annotate }

1. Default logging level for all categories
2. Database infrastructure messages
3. Individual database commands
4. Individual HTTP requests and their contents. Set to `"Trace"` for detailed logging of HTTP requests
5. Confab functionality log messages

!!! warning
    `"Trace"` and `"Debug"` should only be used when troubleshooting. `"Trace"` level may contain sensitive information, and may generate very large log files due to  verbosity.

## Confab Parameters

Miscellaneous parameters related to Confab operation.

```json
{
    "ConfabParams": {
        "ExternalUrl": "", //(1)!
        "CommentsAtLocation": [ "" ], //(2)!
        "Server": {
            "Port": 2632 //(3)!
        }
    }
}
```
{ .annotate }

1. The public location where your Confab backend instance will be accessible. For example, this may be a subdomain of your main site. E.g. `"https://comments.confabcomments.com"`
2. CORS is used to prevent your Confab instance being embedded into sites that you don't allow. This parameters sets the CORS allowed locations. E.g. `"https://confabcomments.com"`. More than one location can be set.
3. Port on your local system that the Confab backend will bind to.


## Emails

Use this section to configure settings related to SMTP and emails.

```json
{
    "Emails": {
        "SMTP": {
            "Server": "", //(1)!
            "Port": 465, //(2)!
            "UseTLS": true //(3)!
        },
        "TemplateParameters": { //(4)!
            "ServiceName": "", //(5)!
            "SiteUrl": "", //(6)!
            "ConfabBackendApiUrl": "" //(7)!
        },
        "SendingAddresses": { //(8)!
            "AuthCodeEmails": { //(9)!
                "Address": "", //(12)!
                "Username": "", //(13)!
                "Password": "" //(14)!
            },
            "UserNotificationEmails": { //(10)!
                "Address": "",
                "Username": "",
                "Password": ""
            },
            "AdminNotificationEmails": { //(11)!
                "Address": "",
                "Username": "",
                "Password": ""
            }
        }
    }
}
```
{ .annotate }

1. SMTP server URL
2. SMTP server port
3. Whether the SMTP server and port you have specified uses TLS encryption (recommended)
6. These are template variables that will be inserted into the email templates
5. Name of the site that Confab will be facilitating comments for.
6. Location of the site that Confab will be embedded into (e.g. `"https://example.com"`)
7. The public location where your Confab backend instance will be accessible. For example, this may be a subdomain of your main site. E.g. `"https://comments.confabcomments.com"`
8. This section allows different SMTP sending addresses to be specified for different email categories
9. [One-time use authentication code emails](../core-functionality/emails/index.md#types-of-emails)
10. [Notification emails sent to users (reply notifications)](../core-functionality/emails/index.md#types-of-emails)
11. [All emails sent to Admins](../core-functionality/emails/index.md#admin-only-emails)
12. Sending email address for this category
13. SMTP username for this address. With some providers, this may be the same as sending email address.
14. SMTP password for this address.

### Moderation Queue Reminders

Specify the number of hours of inactivity after which, a [Moderation Queue Reminder email](../core-functionality/emails/index.md#admin-only-emails) will be sent. Multiple hour numbers can be specified here, and an email will be sent to Admins after each number of hours of inactivity has elapsed

```json
{
    "Emails": {
        "AdminModQueueRemindersHrsAfterInactivity": [ //(1)!
            48,
            168
        ]
    }
}
```
{ .annotate }

1. By default, send a reminder email after `48` hours of inactivity, then a second reminder after `168` hours. 

## User Authentication Parameters

Configure parameters related to the user authentication process and login authentication code emails.

```json
{
    "UserAuthParams": {
        "VerificationCodeExpirySeconds": 300, //(1)!
        "MaxVerificationCodeAttempts": 3, //(2)!
        "MaxVerificationCodeEmails": 3, //(3)!
        "MaxVerificationCodeEmailResetDurationHours": 24, //(4)!
        "MaxNewSignups": -1, //(5)!
        "MaxNewSignupsDurationMinutes": 60 //(6)!
    }
}
```
{ .annotate }

1. Number of seconds before the one-time authentication code sent to a user's email is invalidated
2. Number of incorrect attempts before the one-time authentication code sent to a user's email is invalidated
3. Maximum number of consecutive authentication code emails to send to a particular user. This value resets upon a successful login. See [abuse mitigation](../admin-guide/abuse-mitigation/index.md#email-spam) for more info.
4. Cooldown hours duration to reset the unsuccessful verification code email count. See [abuse mitigation](../admin-guide/abuse-mitigation/index.md#email-spam) for more info.
5. Maximum number of new user sign ups within a certain time duration. Set `-1` to disable limit.
6. Maximum user sign up limit duration. Sign ups will be temporarily disabled if number of user sign ups within this duration is `>MaxNewSignups`.

## Comment Settings

This section provides some customisation options for comment behaviour.

### Rate Limiting

Rate limiting sets a cap on the number of comments that can be posted by a user during a certain time duration. It is a good idea to enable this if the Manual Moderation Queue is disabled, see [Abuse Mitigation](../admin-guide/abuse-mitigation/index.md#comment-spam).

```json
{
    "CommentSettings": {
        "RateLimiting": {
            "Enabled": false, //(1)!
            "TimeDurationMins": 360, //(2)!
            "MaxCommentsPerTimeDuration": 5 //(3)!
        }
    }
}
```
{ .annotate }

1. Set `true` to enable rate limiting
2. Time duration in minutes to use for rate limiting.
3. The maximum number of comments that a user can make within the rate limit time duration.

### Manual Moderation

Enabling manual moderation sends all new comments to the [Manual Moderation Queue](../core-functionality/manual-moderation/index.md) to be reviewed before they are publicly visible.

```json
{
    "CommentSettings": {
        "Moderation": {
            "ManualModerationEnabled": true, //(1)!
            "MaxModQueueCommentCountPerUser": 5 //(2)!
        },
    }
}
```
{ .annotate }

1. Set `false` to disable manual moderation
2. Sets a limit on the maximum number of comments from any user that can be in the [Manual Moderation Queue](../core-functionality/manual-moderation/index.md) at one time. This option only applies if `"ManualModerationEnabled": true`. If you have an [Automoderation rule](../core-functionality/auto-moderation/index.md) that sends comments to the Manual Moderation Queue, this limit **will not be respected**.

### Edits

Control when edits can be made after a new comment is posted.

```json
{
    "CommentSettings": {
        "Edits": {
            "Mode": "Always",/*(1)!*/ //Disabled, DurationAfterCreation, WhileAwaitingModeration, Always (case sensitive)
            "DurationAfterCreationMins": 10, //(2)!
            "ShowEditBadgeOnComment": "Timestamp",/*(3)!*///None, Badge, Timestamp (case sensitive)
            "ShowEditHistory": true //(4)!
        }
    },
}
```
{ .annotate }

1.  - `"Disabled"`: Editing is disabled
    - `"DurationAfterCreation"`: Editing is allowed for a certain duration after comment creation
    - `"WhileAwaitingModeration"`: Editing is allowed while a comment is in the [Manual Moderation Queue](../core-functionality/manual-moderation/index.md)
    - `"Always"`: Editing is always enabled
2. Set the duration after creation that a comment can be edited. This value is only active if `Edits:Mode: "DurationAfterCreation"`.
3. Whether to show an edit badge on comments that have been edited. Admins always see edit timestamps.
    - `"None"`: Do not show edit badge on comment
    - `"Badge"`: Show badge on comments that have been edited
    - `"Timestamp"`: Show badge and timestamp on comments that have been edited
4. Enable or disable edit history visibility. Always visible to Admins.

    !!! note
        Edit history of a comment prior to moderation approval is not shown publicly. See [edit history](../core-functionality/edit-history/index.md) for more info.

    !!! warning
        If this value is `true`, but `"ShowEditBadgeOnComment": "None"`, Confab backend will throw an error during startup


!!! warning
    Learn about the [Content Risks](../admin-guide/content-risks/index.md#blocking-edits) of allowing edits on your Confab instance.


### Page Detection Regex

Specifies the RegEx used to convert a given URL location on your site to a location string, used to identify a [comment location object](../core-functionality/location/index.md) in the database. The default value below extracts the pathname of the URL (e.g. `https://confabcomments.com/blog/post-1#section` → `blog/post-1`).

```json
{
    "CommentSettings": {
        "PageDetectionRegex": "(?<=(?:(?:[^@:\\/\\s]+):\\/?)?\\/?(?:(?:[^@:\\/\\s]+)(?::(?:[^@:\\/\\s]+))?@)?(?:[^@:\\/\\s]+)(?::(?:\\d+))?(?:(?:\\/\\w+)*))(?:\\/[\\w\\-\\.]*[^#?\\s]*)(?=(?:.*)?(?:#[\\w\\-]+)?$)"
  },
}
```

!!! warning
    Changing this RegEx is not recommended, since several features, including comment links in emails, relies on the location being the pathname of the URL.

## Custom Usernames

Usernames are randomly generated by default. Use this section to control whether setting custom usernames is enabled for users. Admins can always set custom usernames.

```json
{
    "Usernames": {
        "CustomUsernamesEnabled": true, //(1)!
        "UsernameChangeCooldownTimeMins": 60 //(2)!
    }
}
```
{ .annotate }

1. Set `false` to disable setting custom usernames
2. Set cooldown time for username changes

!!! warning
    Allowing custom usernames poses some risks. See [Abuse Mitigation](../admin-guide/abuse-mitigation/index.md#custom-usernames) for more information.

## User Roles

Administrator user accounts are set here by specifying the email addresses that you would like to receive Admin privileges.

```json
{
    "UserRoles": {
        "Admin": [ //(1)!
            "admin1@example.com",
            "admin2@example.com",
        ]
    }
}
```
{ .annotate }

1. Specify any number of Admin users by entering email addresses