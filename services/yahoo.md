# Yahoo

## Official documentation
There is no official documentation.

## Basic URL

`https://calendar.yahoo.com/`

[Add a test event](https://calendar.yahoo.com/?v=60&TITLE=Birthday&ST=20201231T193000&ET=20201231T223000&DESC=With%20clowns%20and%20stuff&in_loc=North%20Pole&inv_list=john@example.com,jane@example.com)

AOL Calendar (`https://calendar.aol.com/`) runs the same application bundle, so the parameters below apply there too.

## How this was verified

Yahoo Calendar parses the query string in the browser. The whole mapping lives in one function
(`qm`) inside `https://s.yimg.com/nq/ep/*/bundle/bundle_epoch_calendar.js`, next to the table that
lists every parameter the application knows about:

```js
{ST:"startTime", ET:"endTime", uid:"uid", recurId:"recurId", TITLE:"summary",
 in_loc:"location", DESC:"description", inv_list:"attendees", DUR:"allDay",
 REM1:"reminders", REM2:"reminders", TYPE:"type", RPAT:"recurrence",
 VIEW:"view", REND:"recurrence_end"}
```

That table is exhaustive: any other name in the query string is ignored.
Each parameter was then replayed against the live event composer.

Last check: 2026-08-19.

### Parameter name casing

For every entry the parser first looks for the name exactly as written above, then for its
all-lowercase form. So `TITLE` and `title` both work, `in_loc` is always lowercase, and a mixed
form such as `Title` is ignored.

`REND` is the one exception: the lowercase spelling is recognised but the value is then read back
under the uppercase name, so only `REND` actually does anything.

## Parameters

### v
required: yes

format: number

example: `v=60`

description: without it the event composer does not open at all, the URL just lands on the calendar.
It is not read by the parameter parser, so its role is to select the legacy compose entry point.

### TITLE
required: yes

format: text

example: `TITLE=Birthday`

description: event title.
`+` is decoded as a space, then the value is URL decoded.
Line feeds will appear in the composer but are not saved.
HTML is not rendered.

### ST
required: yes

format: datetime (`YYYYMMDDTHHmmss`) or date (`YYYYMMDD`)

example: `ST=20201231T193000`

description: event start time. Options:

 - `20201231T193000`: start time in the user's local time;
 - `20201231`: start of an all-day event. `DUR` is ignored in this form;
 - `20201231T193000Z`: **the trailing `Z` is currently not honoured**. The value is stored verbatim and rendered as a local time, so `19:30Z` shows up as `19:30` regardless of the user's timezone. Convert to the user's local time yourself.

Yahoo has no timezone parameter at all.

### ET

required: yes

format: datetime (`YYYYMMDDTHHmmss`) or date (`YYYYMMDD`)

example: `ET=20201231T223000`

description: event end time. Same options and the same `Z` limitation as `ST`.

When `ET` is present, `DUR` is ignored.

### DUR

required: no

format: time (`HHmm`) or `allday`

example: `dur=0200`

description: duration of the event. Only used when `ET` is absent.

 - `allday` marks the event as all-day;
 - `HHmm` is added to `ST`, where `HH` is parsed from the first two characters and `mm` from the next two. Non numeric parts count as zero, and `0000` becomes 30 minutes.

The maximum is therefore 99 hours and 59 minutes, a limit of the format itself.
If `ST` carries a `Z` suffix, the computed end time carries it too.

### TYPE
required: no

format: number (zero based index into the list below)

example: `TYPE=7`

description: the event charm, which the composer shows as "Type".
The old numeric table (Anniversary 11, Appointment 10, and so on) no longer applies, and the
string form (`TYPE=birthday`) is not accepted either. Current values:

| value | charm |
| --- | --- |
| 0 | General |
| 1 | Invite |
| 2 | Work |
| 3 | School |
| 4 | Red |
| 5 | Yellow |
| 6 | Green |
| 7 | Birthday |
| 8 | Anniversary |
| 9 | Date |
| 10 | Vacation |
| 11 | Fun |
| 12 | Bills |
| 13 | Phone |
| 14 | Doctor |
| 15 | Flag |
| 16 | Pet |

### DESC
required: no

format: text

example: `DESC=With clowns and stuff`

description: description of your event.
`+` is decoded as a space, then the value is URL decoded.
Line breaks (`%0A`) are preserved.
HTML is not rendered: tags arrive as literal text in the description box.
The field accepts a large amount of text.

### in_loc
required: no

format: text

example: `in_loc=North Pole`

description: event location, stored as free text. It is not resolved against a maps provider.

### inv_list

required: no

format: comma-separated plain email addresses

example: `inv_list=santa@example.com,easter.bunny@example.com`

description: guests.
The value is split on commas and each part is used as an email address as is, so the
`Name <email>` form no longer works: the display name ends up inside the address and the
entry is either mangled or dropped.
Note that `+` is **not** decoded as a space for this parameter, unlike `TITLE` and `DESC`.

### RPAT
required: no

format: text

example: `RPAT=01Wk`

description: recurrence pattern. This parameter works again, it is not deprecated.

The value is uppercased first, so the case of the letters does not matter.
It starts with an interval and a unit:

 - Day: `01Dy`
 - Week: `01Wk`
 - Month: `01Mh`
 - Year: `01Yr`

Anything after that is read as a list of weekdays taken from `SU`, `MO`, `TU`, `WE`, `TH`, `FR`, `SA`:

 - Mon Wed Fri: `01WkMoWeFr`
 - Tue Thu: `01WkTuTh`
 - Mon to Fri: `01WkMoTuWeThFr`
 - Sat and Sun: `01WkSuSa`
 - Second Tuesday of every month: `01Mh2Tu`

For the monthly unit the weekday must carry an ordinal of `1`, `2`, `3`, `4` or `-1`.
A literal `5` in the value is rewritten to `-1`, which is how "last weekday of the month" is expressed.
If the tail of the value is present but matches no weekday, the whole recurrence is dropped.

### REND
required: no (only used together with `RPAT`)

format: `+YYYYMMDD`, a 10 digit unix timestamp, or `-Nt`

example: `REND=%2B20270331`

description: when the recurrence ends. It is ignored unless `RPAT` produced a valid pattern.

Three forms are accepted:

 - `+20270331`: end date. The leading plus is part of the value, so it must be encoded as `%2B` in a URL;
 - `1774915200`: the same thing as a unix timestamp in seconds;
 - `-10t`: stop after 10 occurrences.

A bare `REND=20270331` (the form documented previously) is **not** recognised and is silently dropped.
Only the uppercase spelling works.

### REM1
required: no

format: `{NUMBER}`[`M`|`H`|`D`]

example: `rem1=15M`

description: first reminder, expressed as an offset before the event.
Leading zeros are stripped and the value is uppercased, then it must land exactly on one of the
offsets the composer supports:

`5M`, `15M`, `30M`, `1H`, `2H`, `3H`, `6H`, `12H`, `1D`, `2D`, `3D`, `4D`, `5D`, `6D`, `7D`, `8D`, `9D`, `10D`, `11D`, `12D`, `13D`, `14D`

Anything else falls back to "No Reminder".
Both an alert and an email are enabled for the reminder.

### REM2
required: no

format: same as `REM1`

example: `rem2=6H`

description: second reminder. It is appended to the same list as `REM1`, so in practice you can
also use `REM2` on its own.

### uid
required: no

format: text

example: `uid=750e0c92aa33a7382460a280c2dfb8e6`

description: unique event id. With it the link opens an existing event for editing instead of creating a new one.

### recurId
required: no

format: text

description: identifies one occurrence of a recurring event, used together with `uid`.

### VIEW
required: no

format: string

possible values: `today`, `day`, `week`, `month`, `year`, `list`

example: `VIEW=month`

description: calendar view. The parser reads it, but the effect could not be confirmed while the
event composer is open.

## Parameters that no longer exist

None of these appear in the parameter table, so the application drops them:

 - `in_st`, `in_csz`, `in_ph`: street, city/state/zip and phone. Put the whole address in `in_loc` instead.
 - `URL`: used to turn the event title into a link.
 - `invId`
 - `remadr`: reminder email address. Reminders always enable email now.
 - `msngr`: messenger notification.
