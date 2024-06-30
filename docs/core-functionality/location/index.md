# Comment Locations

Each comment has a location property which defines which location on your site that they are found. This location is a string that (by default) is the [**pathname**](https://developer.mozilla.org/en-US/docs/Web/API/Location#location_anatomy){:target="_blank"} of any given page URL.

Comments at a given location must be manually enabled, this is to prevent new comments being posted at a location which doesn't correspond to an actual page on your site. 

!!! info
    The intended usage of this functionality for a site such as a blog would be:
    
    1. Making a new blog post
    
    2. Going to Confab on the new blog post page and enabling commenting at the location

## Enabling New Location

To enable commenting at a new location, that particular location must be initialised. If commenting is not enabled at a particular location, Confab will present an error that "Comments are not available".

To initialise a new location, click "Go to login" on the top-right of the Confab widget, login with an Admin account, then open the [Admin Panel](../admin-panel/index.md#commenting-settings), and select the option under Local Commenting Settings to initialise the current location.