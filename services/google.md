# Google

## Official documentation

There is no official documentation for the current version, but Google
had some documentation in the past.
You can see it using web.archive wayback machine (it still works!):
[Share your events with an individual, a group, or the whole world](https://web.archive.org/web/20120313011336/http://www.google.com/googlecalendar/event_publisher_guide.html). 

## Basic URL
`https://calendar.google.com/calendar/render`

[Add a test event](https://calendar.google.com/calendar/render?action=TEMPLATE&text=Birthday&dates=20201231T193000Z/20201231T223000Z&details=With%20clowns%20and%20stuff&location=North%20Pole)

## Parameters

### action
required: yes

format: string/eval

possible values: `TEMPLATE`

example: `action=TEMPLATE`

description: A default required parameter.

### text
required: yes

format: text

example: `text=Birthday`

description: event title.

### dates
required: yes

format: `YYYYMMDDToHHmmSSZ/YYYYMMDDToHHmmSSZ`

example: `dates=20201231T193000Z/20201231T223000Z`

description: gives the start and end dates and times (in Greenwich Mean Time) for the event.
Dates must have both start and end time or it won't work.
The start and end date can be the same (if appropriate).
Special cases:
 - to use the user's timezone: `20201231T193000/20201231T223000` (don't specify a timezone);
 - to use UTC timezone, convert datetime to UTC, then use `Z`  suffix: `20201231T193000Z/20201231T223000Z`;
 - for all-day events use `20201231/20210101`. You must use the following date as the end date for a one day all day event, or +1 day to whatever you want the end date to be.

### ctz
required: no

format: [timezone name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

example: `ctz=America/New_York`

description: custom timezone.

### details
required: no

format: text

example: `details=With clowns and stuff`

description: description of your event.

### location
required: no

format: text

example: `location=North Pole`

description: set location of the event.
Make sure it's an address google maps can read easily.

### crm

required: no

possible values: `AVAILABLE`, `BUSY`, `BLOCKING`

format: string

example: `crm=AVAILABLE`

description: if Free, Busy, or Out of Office respectively.

### trp

required: no

possible values: `true`, `false`

format: string

example: `trp=false`

description: Show event as busy (true) or available (false).
Stands for [RFC 5545 transparency](https://tools.ietf.org/html/rfc5545#section-3.8.2.7).
It's ignored for all day events, please refer to `crm` instead.

    
### sprop
required: no

format: key-value

example: `sprop=website:www.santa.org&sprop=name:Sata-online`

description: identify the website or event source

### add
required: no

format: text (comma-separated emails)

example: `add=elf1@example.com,elf2@example.com`

description: a list of guests (comma-separated).

### src
required: no

format: text (email)

example: `src=santa@example.com`

description: add an event to a shared calendar rather than a user's default.
    
    
### recur
required: no

format: text ([RFC-5545 specs](https://icalendar.org/iCalendar-RFC-5545/3-8-5-3-recurrence-rule.html))

example: `recur=RRULE:FREQ=DAILY`

description: set recurring events.

    
## Tools
1. [Google calendar link generator](http://kalinka.tardate.com/)
1. [Google calendar link generator](http://output.jsbin.com/xujuluw)

