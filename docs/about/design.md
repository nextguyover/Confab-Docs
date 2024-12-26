# Design Philosophy

## Authentication Flow

Guests on your site can freely view any comments on your site. However, to perform any interactive actions such as voting on comments, or submitting new content, they must be logged in.

As soon as a new guest performs any action that requires authentication, the anonymous login process happens transparently in the background, allowing the guest to engage with comments frictionlessly. (1)
{ .annotate }

1. A CAPTCHA may be presented if other visitors have logged in anonymously from the same IP address recently. Read more about [anonymous commenting](../core-functionality/users/index.md#anonymous-users), and learn how to configure this feature through the [backend config](../config/index.md#anonymous-commenting).

To enable additional features such as comment reply notifications, a user must login with their email address. Confab was initially designed without anonymous commenting, hence, the email authentication flow was designed from the ground-up to be as frictionless for visitors as possible. 

Upon entering an email address, a link is presented to take a visitor directly to their mailbox(1). Furthermore, in the authentication code email that users are sent, there is a direct link that can be clicked to navigate back to your site and automatically login to Confab. In a best-case scenario, the login process takes just 3 clicks. 
{ .annotate }

1. Jump to mailbox feature currently supports mail providers Gmail and Outlook 

## Passwordless Login

A passwordless authentication design was implemented to make the authentication process faster, as outlined in the previous section. Fundamentally, a user shouldn't be required to store a password for a small-scale blog or similar sites that Confab is intended for. An additional bonus to this is in the event of a data breach, passwords cannot be leaked.

## Operation Scale

Confab was designed for use on a small-scale site such as a personal blog. This allows the feature set and codebase to be simpler, allowing for easier maintainability. The SQLite database was also chosen for this reason, since a higher performance database is unnecessary for a low level of traffic.

If you have a high-traffic site, Confab (at least in its current state) may not be the best choice for you.