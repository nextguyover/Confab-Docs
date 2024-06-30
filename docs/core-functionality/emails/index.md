# Emails

Confab sends emails to users for authentication and various other notification scenarios. All the different types of emails that are sent by Confab are listed below.

!!! tip
    Sending emails has some risks for spam generation. See [Abuse Mitigation](../../admin-guide/abuse-mitigation/index.md#email-spam) for more information.

## Types of Emails

- Authentication code: one-time authentication code sent to a user's email used to login to Confab
- Reply notifications: notification email sent when a user's comment receives a reply

### Admin-only Emails

- Top-level comment notification: a new top-level comment (a comment that is **not** replying to another comment) is posted
- Comment notification: a new comment is posted **in reply** to an existing comment
- Edit notification: an edit is made to an existing comment
- Automoderation notification: an Automoderation rule is triggered that is [set to notify Admins](../auto-moderation/index.md#notify-admins)
- Manual Moderation Queue reminder: comment(s) have remained in the moderation queue for a duration(s) specified in the [backend configuration](../../config/index.md#moderation-queue-reminders)

## Templates

The included email templates are designed using [MJML](https://mjml.io/){:target="_blank"}. During the build process, these templates are [automatically compiled and placed in the correct directory](../../development/index.md#step-2-compile-email-templates) for the Confab server to have access to these files during runtime.

### Template Substitution

The relevant information is substituted by the Confab backend into the email templates before they are sent to users. Template variables in the email have the following pattern: `#TemplateVariable#`.

Different types of emails templates have different variables that Confab will attempt to substitute in. These are listed [below](#template-variables). Any nested section with the prefix `[Email]` is a type of email. A file with that name and file extension `.html` must be present in `Confab/Confab/App_Data/email_templates/` when Confab backend is compiled. (1)
{ .annotate }

1. The provided build scripts handle compiling and moving compiled files to the correct location automatically. 

Any variables prefixed by `+` means that template variable is available for **that section and its children**. Any variables prefixed by `-`, means that variable that has been inherited from a parent **stops being available for that section and its children**.

Following these guidelines, you may provide your own email templates with your own custom designs. Confab backend will do templating on any `.html` files in the correct directory with the correct file name.

### Template Variables

```
All Email {
    + #ServiceName#
    + #SiteUrl#
    + #UserEmail#
    + #Username#
    + #UserProfilePicUrl#
    + #ConfabUrl#
    + #EmailTimestamp#

    [Email] auth-code {
        + #AuthCode#
        + #AuthCodeAutoLoginURL#
    }

    Comment Notifications {
        + #CommentText#
        + #CommentUpvoteCount#
        + #CommentDownvoteCount#
        + #CommentUsername#
        + #CommentProfilePicUrl#
        + #CommentLink#
        + #CommentCreationTime#
        + #ParentCommentText#
        + #ParentCommentUpvoteCount#
        + #ParentCommentDownvoteCount#
        + #ParentCommentCreationTime#

        [Email] user-comment-reply-notif {
            + #NotifDisableLink#
        }

        Admin Comment Notification {
            + #CommentUserId#
            + #CommentUserEmail#
            + #CommentLocationInDb#

            [Email] admin-comment-notif-top-level {
                - #ParentCommentText#
                - #ParentCommentUpvoteCount#
                - #ParentCommentDownvoteCount#
                - #ParentCommentCreationTime#
            }

            [Email] admin-comment-notif {
                + #ParentCommentUsername#
                + #ParentCommentProfilePicUrl#
                + #ParentCommentUserId#
                + #ParentCommentUserEmail#
            }

            [Email] admin-edit-notif {
                - #ParentCommentText#
                - #ParentCommentUpvoteCount#
                - #ParentCommentDownvoteCount#
                - #ParentCommentCreationTime#
                + #EditPreviousContent#
            }

            [Email] admin-automod-notif {
                - #ParentCommentText#
                - #ParentCommentUpvoteCount#
                - #ParentCommentDownvoteCount#
                - #ParentCommentCreationTime#
                + #AutoModRuleRegex#
                + #AutoModRuleAction#
            }
        }
    }

    [Email] admin-mod-queue-reminder {
        + #ModQueueInactivityTime#
        + #ModQueueReminderTime#
        + #ModQueueCount#
        + #ModQueueOldestItemAge#
    }
}
```