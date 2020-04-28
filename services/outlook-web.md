# Outlook live

## Official documentation
There is no official documentation.

## Basic URL
`https://outlook.live.com/owa/`

[Add a test event](https://outlook.live.com/calendar/0/deeplink/compose?path=/calendar/action/compose&rru=addevent&startdt=2020-12-31T19:30:00Z&enddt=2020-12-31T22:30:00Z&subject=Birthday&body=With%20clowns%20and%20stuff&location=North%20Pole)

## Parameters

### path
required: yes

format: string

example: `path=/calendar/action/compose`

description: Internal application path.

### rru
required: yes

format: string (`addevent`)

example: `rru=addevent`

description: Action name.

### startdt
required: yes

format: datetime (`YYYY-MM-DD-THH:mm:SSZ`) or date (`YYYY-MM-DD`, for all-day events) in UTC

example: `startdt=2020-12-31T19:30:00Z`

description: The start date for the event.
You can omit trailing `Z`, in this case script assumes that time specified current user's timezone. 
To specify all-day events use the `YYYY-MM-DD` format.

### enddt
required: yes

format: datetime (`YYYY-MM-DD-THH:mm:SSZ`) or date (`YYYY-MM-DD`, for all-day events) in UTC

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

description: set location of the event.
