# Outlook live

## Official documentation
There is no official documentation.

## Basic URL
`https://outlook.live.com/owa/`

[Add a test event](https://outlook.live.com/owa/?path=/calendar/action/compose&rru=addevent&startdt=20201231T193000&enddt=20201231T223000&subject=Birthday&body=With%20clowns%20and%20stuff&location=North%20Pole)

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

format: datetime (`YYYYMMDDTHHmmSS`) or date (`YYYYMMDD`, for all-day events) in UTC

example: `startdt=20201231T193000`

description: The start date for the event.
A datetime will be automatically convetred to user's timezone.
To specify all-day events use the `YYYYMMDD` format.

### enddt
required: yes

format: datetime (`YYYYMMDDTHHmmSS`) or date (`YYYYMMDD`, for all-day events) in UTC

example: `enddt=20201231T223000`

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
