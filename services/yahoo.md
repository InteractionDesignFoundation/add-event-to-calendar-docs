# Yahoo

## Official documentation
There is no official documentation.

## Basic URL

`https://calendar.yahoo.com/`

[Add a test event](https://calendar.yahoo.com/?v=60&TITLE=Birthday&ST=20201231T193000&ET=20201231T223000&DESC=With%20clowns%20and%20stuff&in_loc=North%20Pole&inv_list=John+Doe+%3Cjohn@example.com%3E,Jane+Doe+%3Cjane@example.com%3E)

## Parameters

### v
required: yes

format: number

example: `v=60`

description: Must be 60. Possibly a version number?

### TITLE
required: yes

format: text

example: `TITLE=Birthday`

description: Event title.
Line feeds will appear in the confirmation screen, but will not be saved.
May not contain HTML.

### ST
required: yes

format: datetime (ISO8601)

example: `ST=20201231T193000Z`

description: Event start time. Options:

 - `20201231T193000Z`: Event start time in UTC. Will be converted to the user's time zone.
 - `20201231T193000`: Event start time in user's local time
 - `20201231`: Event start time for an all day event. `DUR` value is ignored if this form is used.

### ET

required: yes

format: datetime (ISO8601)

example: `ET=20201231T193000Z`

description: Event end time. Options the same as for `st` parameter.

Note that `dur` parameter will be ignored if `et` is specified.

⚠️ This parameter appears to be broken with respect to timezones (need to confirm).

### dur

required: no

status: appears to no longer work

format: time (`HHmm`) or `allday`

example: `dur=0200`

description: Duration of the event.
Format is `HHmm`, zero-padded.
`mm` may range up to 99, and is converted into hours appropriately.
`HH` values over 24 hours appear to be modulated by 24.
Durations that span midnight behave strangely.
Leave this blank if the event has no specific end time, or if this is an all-day event.
Note that `dur` parameter will be ignored if `et` is specified.
Used only when `ed` is not specified OR value is `allday`.

Note that the maximum duration yahoo can except is 99 hours and 59 minutes due to the limitation of the format.

### ~type~
required: no

status: propably not supported anymore (or format is changed)

format: number

example: `type=10`

description: event's type
 - 11   Anniversay
 - 10   Appointment (default)
 - 12   Bill Payment
 - 13   Birthday
 - 27   Breakfast
 - 14   Call
 - 19   Chat
 - 37   Class
 - 26   Club Event
 - 24   Concert
 - 29   Dinner
 - 15   Graduation
 - 30   Happy Hour
 - 16   Holiday
 - 17   Interview
 - 28   Lunch
 - 18   Meeting
 - 23   Movie
 - 21   Net Event
 - 20   Other
 - 31   Party
 - 32   Performance
 - 33   Reunion
 - 34   Sports Event
 - 25   Travel
 - 22   TV Show
 - 35   Vacation
 - 36   Wedding

### DESC
required: no

format: text

example: `DESC=With clowns and stuff`

description: description of your event.
This may contain line breaks (encoded in the usual manner, these become %0A).
This may contain HTML, but all tags will be stripped out.
This appears to accept quite a large amount of text,
considering that it is being passed through on a query string.

### ~url~
required: no

status: propably not supported anymore (there is no input for URL in a form)

format: URL

example: `URL=https://example.com`

description: If present, the URL will be used to create a link out of the event title.

### in_loc
required: no

format: text

example: `in_loc=North Pole`

description: event location.

### in_st
required: no

format: text

example: `in_st=Main str.`

description: Street address.

### in_csz
required: no

format: text

example: `in_csz=Atlanta, GA, 30307`

description: City / State / Zip.

### in_ph
required: no

format: text

example: `in_ph=404-589-1228`

description: Phone.

### ~RPAT~
required: no

format: text

status: **deprecated** as of August 2021

example: `RPAT=01Wk`

description: Used to specify a recurring event.
If this is present, then `REND` is also required.
Examples:
 - Day: `01Dy`
 - Week: `01Wk`
 - Month: `01Mh`
 - Year: `01Yr`
 - Mon Wedn Fri: `01WkMoWeFr`
 - Tues Thurs: `01WkTuTh`
 - Mon – Fri: `01WkMoTuWeThFr`
 - Sat – Sun: `01WkSuSa`
 - Sat – Sun: `01WkSuSa`
 - Second Tuesday of every month: `01Mh2Tu`


### REND
required: no (yes if `RPAT` specified)

format: datetime (`YYYYMMDD`)

example: `REND=20191231`

description: Used to specify when a recurring event pattern ends.
Date format: `YYYYMMDD`


### invId

required: no

format: text

example: ``

description: ?

### inv_list

required: no

format: string

example: to=santa@example.com,easter.bunny@example.com

description: A comma-separated list of emails of attendees.

### recurId

required: no

format: text

example: ``

description: ?


### rem1
required: no

format: text (length 2 or 3): `{NUMBER}`[`D`|`H`|`M`]

example: `rem1=1D` (`1D` means 1day)

description: When reminder 1 should be sent. Set to 0D for no reminder.


### rem2
required: no

format: text (length 2 or 3): `{NUMBER}`[`D`|`H`|`M`]

example: `rem2=6H`

description: When reminder 2 should be sent. Cannot have a reminder 2 without a reminder 1.


### ~remadr~
required: no

status: deprecated

format: text

example: `remadr=some@example.com`

description: Probably setup an email address to send notification for a reminder.


### ~msngr~
required: no

status: deprecated. Only `true` makes sense, but it's `true` by default.

format: `true`/`false`

description: Probably enables messenger notification for a reminder.


### uid
required: no

format: text

example: `uid=750e0c92aa33a7382460a280c2dfb8e6`

description: Unique event ID. Using it, you can change existing events, do not add a new one.
