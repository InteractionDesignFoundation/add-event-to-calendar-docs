# Outlook live

## Official documentation
There is no official documentation.

## Basic URL
Outlook Live:  
`https://outlook.live.com/calendar/deeplink/compose`

Office 365:  
`https://outlook.office.com/calendar/deeplink/compose`

### Example

Outlook Live:  
`https://outlook.live.com/calendar/deeplink/compose?path=/calendar/action/compose&rru=addevent&startdt=2023-08-09T19:30:00Z&enddt=2023-08-09T22:30:00Z&subject=Birthday&body=With%20clowns%20and%20stuff&location=North%20Pole`

Office 365:  
`https://outlook.office.com/calendar/deeplink/compose?path=/calendar/action/compose&rru=addevent&startdt=2023-08-09T19:30:00Z&enddt=2023-08-09T22:30:00Z&subject=Birthday&body=With%20clowns%20and%20stuff&location=North%20Pole`

## Parameters

### path
required: yes

format: string

example: `path=/calendar/action/compose`

description: internal application path.

### rru
required: yes

format: string (`addevent`)

example: `rru=addevent`

description: action name.

### startdt
required: yes

format: datetime (`YYYY-MM-DDTHH:mm:SSZ`) or date (`YYYY-MM-DD`, for all-day events) in UTC

example: `startdt=2020-12-31T19:30:00Z`

description: The start date for the event.
You can omit trailing `Z`, in this case script assumes that time specified in current user's timezone.
To specify all-day events use the `YYYY-MM-DD` format.

### enddt
required: yes

format: datetime (`YYYY-MM-DDTHH:mm:SSZ`) or date (`YYYY-MM-DD`, for all-day events) in UTC

example: `enddt=2020-12-31T22:30:00Z`

description: The end time of the event, format as for `startdt`.

### subject
required: yes

format: string

example: `subject=Birthday`

description: event title.

### allday
required: no

format: boolean (`true`/`false`)

example: `allday=true`

description: whether event is all-day or not.

### body
required: no

format: text

example: `body=With clowns and stuff`

description: description of your event

### location
required: no

format: string

example: `location=North Pole`

description: set the location of the event.

### online
required: no

format: boolean (any value means `true`)

description: toggle the "Skype meeting" button.

example: `online=1`

### to
required: no

format: string

description: A comma-separated list of emails of required attendees.

example: `to=santa@example.com,easter.bunny@example.com`

### cc
format: string

description: A comma-separated list of emails of optional attendees.

example: `cc=santa@example.com,easter.bunny@example.com`
