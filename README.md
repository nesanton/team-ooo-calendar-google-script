# team-ooo-calendar-google-script

> Note: Works well with Google Workspace (formerly G Suite). Not sure if this is useful to non-commercial Google users. 

You've been through it - colleagues end up going on holidays at the same time. No one has visibility on when others are away. Manually populated OOO calendars, spreadsheets. Pain.

Suffer no longer! Google Apps Script to the rescue.

Well, it turned out that the whole thing is very easy. Google has made a [pretty good example](https://developers.google.com/apps-script/samples/automations/vacation-calendar) [1] on how to do exactly this. It works pretty straightforward: you feed it with the google group of your team and the ID of your team's OOO calendar, and it'll sync all events matching OOO criteria for every user in the team to this calendar. The good bits about it are:

* Automagically copies OOO events of all group users into one calendar every hour (tweakable)
* Does not touch events that have not been updated since the last script run
* Adjusts events that have been changed in the Users' calendars
* Deletes events that have been deleted in the Users' calendars
* Works well with Google's OOO functionality - the one that shows in the calendar nicely and rejects invites for you while you're away.

You can go ahead and follow the linked [1] vanilla example from Google or keep reading to get some of the [changes made in this repository](#changes-in-this-code).

## Changes in this code

### Verbose names

Google's example [1], creates records in the form "[username] - Original event name". The `username` may be not too verbose, especially for large teams.
This code added a best effort function to get users' Full Names. It's a best effort because Groups App and Calendar App do not provide access to User's Full Name (We may be wrong). So We look up the name in the Contacts App. Make sure that the user who runs the script has the whole team in their Contacts. Perhaps mangers are the best candidates. If you are on Google Workspace (formerly G Suite), it's very likely that the entire corporate directory is in everyone's contacts already.

### More Keywords

`KEYWORDS` list is extended with quite some words

### Sync-behind duration

`MONTHS_IN_ADVANCE` are set to `6`

## What is tweakable in the script?

* `GROUP_EMAIL` - that's your team's google group
* `TEAM_CALENDAR_ID` - that's the output calendar. Better create a new one and get its ID in `Settings -> Integrate Calendar`. Make sure to eventually share it with your team (google group), but I'd avoid giving everyone edit access - the script will not put back anything that was removed by hand.
* `KEYWORDS` - list of strings that will be matched against calendar events. Be creative - adding such things as "holiday" can sync something like "Public holiday", which may come handy if the team is distributed across countries.
* `MONTHS_IN_ADVANCE` - how many months from current date the script should crawl through
* Surely, you can change anything else - it's a script!

## Things you need to know

Script uses permissions and visibility of the user who runs it.

Events changed or deleted in the team calendar manually won't be repaired on the next script run. Due to the fact that this script skips events if they were not modified since the last run, any changes in the vars or the code of the script will only affect new or edited events:

E.g. if you added a new keyword you'd expect a matching event to be synced. But if this event was already present in person's calendar when the script executed last time - it won't be synced.

## Installation

Follow the instructions from the original [Google's example](https://developers.google.com/apps-script/samples/automations/vacation-calendar) [1], replacing the code with the contents of [team-ooo-calendar.gs](team-ooo-calendar.gs)

## Links

1. Google's example: Populate a team vacation calendar https://developers.google.com/apps-script/samples/automations/vacation-calendar

