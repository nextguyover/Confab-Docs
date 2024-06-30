# Design Philosophy

## Mandatory Authentication 

Requiring user authentication to create or vote on comments was chosen to alleviate the possible abuse scenarios associated with anonymous commenting. If you are interested in anonymous commenting as a features, visit the [Confab Github repo](https://github.com/{{variables.CONFAB_GITHUB_LOCATION}}) and let us know. 

During the design of Confab, every effort was taken to make the authentication process as frictionless for visitors as possible. 

Upon entering an email address, a link is presented to take a visitor directly to their mailbox. Furthermore, in the authentication code email that users are sent, there is a direct link that can be clicked to navigate back to your site and automatically login to Confab. In a best-case scenario, the login process takes just 3 clicks. 

## Passwordless Login

A passwordless authentication design was implemented to make the authentication process faster, as outlined in the previous section. Fundamentally, a user shouldn't be required to store a password for a small-scale blog or similar sites that Confab is intended for. An additional bonus to this is in the event of a data breach, passwords cannot be leaked.

## Operation Scale

Confab was designed for use on a small-scale site such as a personal blog. This allows the feature set and codebase to be simpler, allowing for easier maintainability. The SQLite database was also chosen for this reason, since a higher performance database is unnecessary for a low level of traffic.

If you have a high-traffic site, Confab (at least in its current state) may not be the best choice for you.