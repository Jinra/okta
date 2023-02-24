## Summary

I am releasing the offboarding flow from https://paper.dropbox.com/doc/Using-Okta-Workflows-to-Automate-User-Deprovisioning-via-Slack-orNKxky5yAdc7f2dNu5Dr for public use. Keep in mind this flow is very specific to the tools I used at that company and you may want only some, all, or more for you use case. The flow uses Slack as a platform to issue commands and post results, Airtable to store results of runs, and processes various actions for Google, Google Drive, Google Calendar, Okta, and Jamf. Adding more shouldn't be too difficult. I would take the time to fully understand the flow before you try to transition things to another service (such as Airtable to Google Sheets).

## Flow Explanation

The intake point is "1. Handle Command". This flow handles slash commands that call the tool. 2. Kick off deprovision deals with the task flows as well as button interactions.
The "Action" flows are flows that are called by "2. Kick off deprovision" which action various parts of offboarding. Lastly, the "UTIL" flows are utilities flows used by the action card.
There is also a special action card called "Action - Process Result.." which deals with updating Slack and Airtable with the results of the task flow that ran. This card is called by every task action card.

## Setup Connections

There are a number of connections you'll need to setup for everything to work. They are

1. API Connector - No Auth. This connection is basically a dummy connection for when you won't be passing authentication through the connection itself but rather as a bearer token through the header. You can use a "custom" connection type and leave the values blank
1. API Connector - Slack. I use a lot of generic POST/GET cards for Slack API calls because I find them more consistent and flexible to use than the built in Slack connection.
1. API Connector - Jamf. You'll want to use the "basic" authentication type and use the username and password for the Jamf service account user that will be calling Jamf API. Make sure the account has the right permissions.
1. API Connector - Airtable. This is for updating the cell values on the Airtable base when a task succeeds or fails. The built in Airtable connector cannot do this, however you will use the native connector for other things.
1. Airtable - This native connector is used for instantiating new records when the tool is run, and for reading row results in the case a flow needs to be retried due to an error.
1. Google Drive - This is for handling Google Drive transfers.
1. Google Calendar - This is for handling Google Calendar transfers.
1. Google Workspace - This is for handling the bulk of various Google Workspace tasks.
1. Okta - You should already have this, but this is for reading Okta information.
1. Slack - You'll be using the native Slack connector for reading data from slash commands on your app and maybe one or two other things.
1. Custom Google (via Connector Builder) - This is for calling commands that are on behalf of a user such as clearing a user's calendar. This is a bit tricky to setup but you can find instructions here https://developers.google.com/identity/protocols/oauth2/service-account#httprest. You can use the googleAuth.flow in this repo as a guideline on how it works to translate to Connector Builder. Unfortunately, you cannot export connector builder flows. Personally, I also provide scopes at run time, but you can hard code them if you wish.

## Setup cards and backend

1. Follow through the flow of the cards to get an idea of how the flow works.
1. Select the connections for all the API cards. Note that even though there can multiple connection cards of the same type (such as API Endpoint), it doesn't mean it always uses the same connection as the other cards. Look at the URL, or function of the card to see what connection you'll need to use. Note that some API Endpoint cards will use the "No Auth" connection, such as some of the Jamf cards, because they take a dynamically generated bearer token.
1. Follow the steps in the Paper guide (see next section) to setup some of the backend that's required such as the Slack app, Okta setup, and database setup.
1. The notes I included in the flows are also very useful to help setup the flows for your company, check them out

## How to add/remove tasks

You can add your task as an Action flow and create any respective UTIl flows if necessary. Generally this is necessary if your flow will iterate through a list such as wiping multiple devices a user owned via jamf. Once your flow is created, you can add it in "2. Kick off deprovision". You can follow the format of the surrounding cards to get an idea on how to construct it. Be mindful of the order of operations and make sure your task makes sense where it is. If you're using Airtable, you'll want to edit the if/else card just before the deprovisioning starts. This card will instantiate the row for the target user, or read data from the row in the case of a retry. You'll have to create a new column in Airtable (I recommend in the same order as the flow will run), then reload the cards in both if and else by clicking on the gear icon on the lower right and hitting "choose fields". Select the new column and save. Be sure to define the outputs respectively and link to the action if/elif card.

## More Info

More detailed information can be found at my [Using Okta Workflows to Automate User Deprovisioning in Slack](https://paper.dropbox.com/doc/Using-Okta-Workflows-to-Automate-User-Deprovisioning-via-Slack-orNKxky5yAdc7f2dNu5Dr) Paper guide.

## Support

I'm sure I missed a step or two, or more so feel free to ping me on the Mac Admins slack! @Pete Viri 