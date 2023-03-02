
# Goal Setting Webinar

## Issue
An account was able to create more than 1 Masterplan for the same Journey. This caused them to store data in 1 Masterplan, while the other remained empty.

## Possible Causes
The UI displays a Create button when a Masterplan for a Journey doesn't exist and a Continue button when a Masterplan does exist.

The APIs involved include a Create Masterplan API and an Update Masterplan API. 

If the Create button is displayed in the UI, the Create Masterplan API would be used. If the Continue button is displayed in the UI, the Update Masterplan API would be used.

The only way to create another Masterplan is if the Create Masterplan API was hit twice.

The most probable cause of this bug would be the following:
* The Create Button in the UI may have been clicked twice consecutively
    * The first click would send the request to the API to create the Masterplan and subsequently begin redirecting the User to the new Masterplan's first screen. In the background, the second click would also be sent to the API which would generate the second Masterplan in the background
    * The user would then continue to update the first Masterplan while the second remains empty. Since the second Masterplan would technically be the latest, on their subsequent attempt to login another day, the second Masterplan would be returned with no data.

## Fixes
From the backend, a restriction has been placed where every user can only have 1 Masterplan per Journey. This would raise an error in the event that an attempt was made to create two. 

This error will likely pass silently in the event a double click was done by the User, but it would be a good idea to have preventative measures in place on the frontend that would prevent the double click in the first place.
