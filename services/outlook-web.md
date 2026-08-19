# Outlook live

## Official documentation
There is no official documentation.

## Basic URL
Outlook Live:  
`https://outlook.live.com/calendar/deeplink/compose`

Office 365:  
`https://outlook.office.com/calendar/deeplink/compose`

An account index may be inserted before `deeplink`, for example
`https://outlook.live.com/calendar/0/deeplink/compose`. Both forms behave identically.

### Example

Outlook Live:  
`https://outlook.live.com/calendar/deeplink/compose?path=/calendar/action/compose&rru=addevent&startdt=2023-08-09T19:30:00Z&enddt=2023-08-09T22:30:00Z&subject=Birthday&body=With%20clowns%20and%20stuff&location=North%20Pole`

Office 365:  
`https://outlook.office.com/calendar/deeplink/compose?path=/calendar/action/compose&rru=addevent&startdt=2023-08-09T19:30:00Z&enddt=2023-08-09T22:30:00Z&subject=Birthday&body=With%20clowns%20and%20stuff&location=North%20Pole`

## How this was verified

Unlike Google Calendar, the Outlook web app does not parse the deep link in the browser.
None of the parameter names appear in the OWA JavaScript chunks that the compose page loads,
and the query string disappears from the address bar once the compose form is rendered,
so the mapping happens on the server.

That means the list below comes from replaying parameters against the live compose form and
observing the result, not from reading a parser.

Each parameter carries one of three confidence markers:

* **verified**: the parameter visibly changed the compose form.
* **no effect**: the parameter was replayed and the form did not change.
* **not observable**: the parameter may work, but the compose form gives no way to tell.

Last check on `outlook.live.com`: 2026-08-19. The Office 365 host was not re-tested.

## Parameters

### path
required: yes

format: string

example: `path=/calendar/action/compose`

confidence: verified

description: internal application path.

### rru
required: yes

format: string (`addevent`)

example: `rru=addevent`

confidence: verified

description: action name.

### startdt
required: yes

format: datetime (`YYYY-MM-DDTHH:mm:SSZ`) or date (`YYYY-MM-DD`, for all-day events) in UTC

example: `startdt=2020-12-31T19:30:00Z`

confidence: verified

description: the start date for the event.
You can omit the trailing `Z`, in which case the value is read in the current user's timezone.
To specify all-day events use the `YYYY-MM-DD` format.

### enddt
required: yes

format: datetime (`YYYY-MM-DDTHH:mm:SSZ`) or date (`YYYY-MM-DD`, for all-day events) in UTC

example: `enddt=2020-12-31T22:30:00Z`

confidence: verified

description: the end time of the event, format as for `startdt`.

### subject
required: yes

format: string

example: `subject=Birthday`

confidence: verified

description: event title. It also becomes the browser tab title.

### allday
required: no

format: boolean (`true`/`false`)

example: `allday=true`

confidence: verified

description: whether the event is all-day or not.
`allday=true` wins over the time part of `startdt` and `enddt`, so a full datetime range still collapses into an all-day event.
All-day events also default to "Free" availability.

### body
required: no

format: text or HTML

example: `body=With clowns and stuff`

confidence: verified

description: description of your event.
HTML is accepted and rendered, so `body=%3Cb%3Ebold%3C%2Fb%3E` produces bold text and an `<a href="...">` produces a real link.

### location
required: no

format: string

example: `location=North Pole`

confidence: verified

description: set the location of the event. The value is added as a free-text location entry, it is not resolved against Bing Maps.

### online
required: no

format: boolean (any truthy value, `1` and `true` both work)

example: `online=1`

confidence: verified

description: turns the online meeting toggle on. The toggle is labelled "Teams meeting" now, not "Skype meeting". Without this parameter the toggle stays off until an attendee is added.

### to
required: no

format: string

example: `to=santa@example.com,easter.bunny@example.com`

confidence: verified

description: a comma-separated list of emails of required attendees.

### cc
required: no

format: string

example: `cc=santa@example.com,easter.bunny@example.com`

confidence: verified

description: a comma-separated list of emails of optional attendees.

### freebusy
required: no

format: string (enum)

example: `freebusy=oof`

confidence: verified

options:
 - `free`
 - `tentative`
 - `busy`
 - `oof`
 - `workingelsewhere`
 - `nodata`

description: availability shown for the event. `oof` renders as "Out of office".

### reqresponse
required: no

format: boolean (`true`/`false`)

example: `reqresponse=true`

confidence: not observable

description: request responses from attendees. The compose form hides this behind a "Response options" menu, so the parameter could not be confirmed.

### allowfw
required: no

format: boolean (`true`/`false`)

example: `allowfw=true`

confidence: not observable

description: allow forwarding. Same "Response options" menu as above.

### hideattn
required: no

format: boolean (`true`/`false`)

example: `hideattn=true`

confidence: not observable

description: hide the attendee list. Same "Response options" menu as above.

### folderid
required: no

format: string

confidence: not observable

description: unknown, probably the target calendar folder.

## Parameters that do not work

These were replayed against the compose form and produced no change, so do not rely on them:

 - `private` and `sensitivity`: the privacy control stays on "Not private".
 - `categories`: no category is applied.
 - `reminder`: the reminder control keeps its default.
 - `charm`: no event charm is selected.
