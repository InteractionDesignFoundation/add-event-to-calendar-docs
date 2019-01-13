# Outlook live

## Official documentation
There is no official documentation.

## Basic URL
`https://outlook.live.com/owa/`

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

format: string

example: `startdt=20080101T143000`

description: The start and end dates for your event YYYYMMDDTHHmmSS,
e.g. 20080101T143000 would be half past two on the 1st Jan 2008.
You may need to convert them to the right time zone for your calendar,
I haven’t tested this.
You may also be able to specify all-day events using the YYYYMMDD format.

### enddt
required: yes

format: string

example: `enddt=20080101T143000`

description: Specify the end time of the event, format as for `startdt`.

### subject
required: yes

format: string

example: `subject=Bithday`

description: event title.

### allday
required: no

format: boolean (`true`/`false`)

example: `allday=true`

description: Where event is all-day or not.

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
