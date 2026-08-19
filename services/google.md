# Google

## Official documentation

Google has updated their official [Google Calendar API documentation](https://developers.google.com/calendar) for v3 including an updated [events reference](https://developers.google.com/calendar/api/v3/reference/events). The updated documentation also includes a [guide for creating events](https://developers.google.com/calendar/api/guides/create-events).

Other helpful resources:
* [Google Calendar Help](https://support.google.com/calendar/?hl=en#topic=10509740)
* [Share your calendar with someone](https://support.google.com/calendar/answer/37082)
* [Add a Google calendar to your website](https://support.google.com/calendar/answer/41207)

## Basic URL
`https://calendar.google.com/calendar/render` or `https://calendar.google.com/calendar/r/eventedit`

[Add a test event](https://calendar.google.com/calendar/render?action=TEMPLATE&text=Birthday&dates=20201231T193000Z/20201231T223000Z&details=With%20clowns%20and%20stuff&location=North%20Pole)

The `render` URL is a thin entry point: it redirects to `/calendar/u/0/r/eventedit` and keeps the whole query string.
Everything below therefore applies to both URLs.

Prefer `render` when you do not control the device the link is opened on. It is the form Google itself
hands out, and it is reported to be the only one that reaches the event editor on Android, where the
`eventedit` path opens the Google Calendar app without starting event creation (see issue #56).

## How this was verified

The parameters below were recovered by reading the Google Calendar web client bundle
(the `Sbh` deep link parser inside the `calendar-web` JavaScript modules) and then replaying
each parameter against the live event editor.

Each parameter carries one of two confidence markers:

* **verified**: the parameter visibly changed the event editor.
* **from code**: the parameter is read by the client parser, but the effect was not confirmed in the UI.

Last check: 2026-08-19.

## Parameters

### action
required: yes (only for the `render` URL)

format: string/eval

possible values: `TEMPLATE`

example: `action=TEMPLATE`

confidence: verified

description: a default required parameter. (If you're using `https://calendar.google.com/calendar/r/eventedit`, this parameter is not required.)

### text
required: yes

format: text

example: `text=Birthday`

confidence: verified

description: event title.

### dates
required: yes

format: `YYYYMMDDTHHmmSSZ/YYYYMMDDTHHmmSSZ`

example: `dates=20201231T193000Z/20201231T223000Z`

confidence: verified

description: gives the start and end dates and times (in Greenwich Mean Time) for the event.
Dates must have both start and end time or it won't work.
The start and end date can be the same (if appropriate).
Special cases:
 - to use the user's timezone: `20201231T193000/20201231T223000` (don't specify a timezone);
 - to use UTC timezone, convert datetime to UTC, then use `Z` suffix: `20201231T193000Z/20201231T223000Z`;
 - for all-day events use `20201231/20210101`. You must use the following date as the end date for a one day all day event, or +1 day to whatever you want the end date to be;
 - the literal value `dates=now` opens the editor at the next half hour or full hour slot, using the current time.

Note: the parser treats the pair as a UTC range only when **both** halves end with `Z`.

### ctz
required: no

format: [timezone name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

example: `ctz=America/New_York`

confidence: verified

description: custom timezone. It is used for both the start and the end of the event, unless `stz` or `etz` override it.

### stz
required: no

format: [timezone name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

example: `stz=Asia/Tokyo`

confidence: verified

description: start timezone. Takes priority over `ctz`.

### etz
required: no

format: [timezone name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

example: `etz=America/Los_Angeles`

confidence: verified

description: end timezone. Combined with `stz` it produces an event that starts in one timezone and ends in another.

### details
required: no

format: text (basic HTML is accepted)

example: `details=With clowns and stuff`

confidence: verified

description: description of your event.
Simple markup such as `<b>` and `<a href="...">` survives into the description editor, so remember to URL encode it.

### location
required: no

format: text

example: `location=North Pole`

confidence: verified

description: set location of the event.
Make sure it's an address google maps can read easily.

### location_name
required: no

format: text

example: `location_name=Empire State Building`

confidence: verified

description: structured location. When present, Google builds a rich location object instead of a plain string, and the companion `location_*` parameters below fill in its fields.

### location_address_formatted_address, location_address_country, location_address_locality, location_address_region, location_address_post_office_box_number, location_address_street_address
required: no

format: text

example: `location_address_formatted_address=20 W 34th St, New York`

confidence: verified (for `location_address_formatted_address`), from code (for the rest)

description: address parts of the structured location. They are only read when `location_name` is present.

### location_place_id, location_maps_cluster_id
required: no

format: text (Google Maps identifiers)

example: `location_place_id=ChIJaXQRs6lZwokRY6EFpJnhNNE`

confidence: from code

description: link the structured location to a Google Maps place.

### location_geo_latitude, location_geo_longitude
required: no

format: float

example: `location_geo_latitude=40.7484&location_geo_longitude=-73.9857`

confidence: from code

description: coordinates of the structured location.

### location_url
required: no

format: URL

example: `location_url=https://example.com`

confidence: from code

description: a link attached to the structured location.

### crm

required: no

possible values: `AVAILABLE`, `BUSY`, `BLOCKING`, `UNKNOWN`

format: string

example: `crm=AVAILABLE`

confidence: verified

description: if Free, Busy, or Out of Office respectively. `UNKNOWN` clears the value.

### icc
required: no

possible values: `DEFAULT`, `PUBLIC`, `PRIVATE`, `SECRET`, `CONFIDENTIAL`

format: string

example: `icc=PRIVATE`

confidence: verified

description: event visibility.

### trp
required: no

format: string

example: `trp=false`

confidence: verified as **no longer supported**

description: used to toggle transparency ([RFC 5545 transparency](https://tools.ietf.org/html/rfc5545#section-3.8.2.7)).
The current web client does not read this parameter at all, and the event stays "Busy". Use `crm` instead.

### sprop
required: no

format: repeatable `key:value` pair

example: `sprop=goo.allowModify:false&sprop=goo.allowInvitesOther:false&sprop=goo.showInvitees:false`

confidence: verified

description: guest permissions. Only three keys are read by the current client:
 - `goo.allowModify`: let guests modify the event;
 - `goo.allowInvitesOther`: let guests invite others;
 - `goo.showInvitees`: let guests see the guest list.

Values are compared against the literal string `true`, so anything else counts as `false`.
The old `sprop=website:...` and `sprop=name:...` pairs (source attribution) are parsed into the same map but no longer used.

### pprop
required: no

format: repeatable `key:value` pair

example: `pprop=eventColor:5`

confidence: from code

description: private properties of the event. The only key the client looks up is `eventColor`.

### add
required: no

format: repeatable, text (comma-separated emails)

example: `add=elf1@example.com,elf2@example.com`

confidence: verified

description: a list of guests. The parameter may be repeated, and each occurrence may hold a comma-separated list.
Append `_o` (or `_O`) to an address to add that person as an **optional** guest: `add=elf1@example.com,elf2@example.com_o`.

### src
required: no

format: text (email)

example: `src=santa@example.com`

confidence: from code

description: add an event to a shared calendar rather than a user's default.

### targ
required: no

format: text (email)

example: `targ=santa@example.com`

confidence: from code

description: same purpose as `src`, and it wins when both are present. The value must contain `@`, otherwise it is ignored.

### recur
required: no

format: text ([RFC-5545 specs](https://icalendar.org/iCalendar-RFC-5545/3-8-5-3-recurrence-rule.html))

example: `recur=RRULE:FREQ=DAILY`

confidence: verified

description: set recurring events. Note that this one is **not** handled by the client side deep link parser, so it behaves independently of the parameters above.

### vcon
required: no

possible values: `meet`

format: string

example: `vcon=meet`

confidence: verified

description: add a video meeting link. The client compares the value against the literal `meet`, so no other conferencing provider can be requested this way. A real Meet link is generated immediately.

### eid
required: no

format: text (base64 event id)

example: `eid=<event id>`

confidence: from code

description: opens an existing event instead of creating a new one.

### elid
required: no

format: text

example: `elid=5`

confidence: from code

description: event color id. It is only read when an internal feature flag is on, otherwise the client falls back to `pprop=eventColor:...`.

### erem
required: no

format: repeatable, three numbers separated by `:`

example: `erem=1:60:0`

confidence: partially verified

description: switches the event to custom reminders and adds one notification row per occurrence.
The third component must not be `-1`, otherwise the entry is dropped.
In the current UI the rows are added but the values themselves are not applied, so treat this parameter as unreliable.

### csid, ctid
required: no

format: text

example: `csid=<conference solution id>&ctid=<conference type id>`

confidence: from code

description: conference identifiers. `ctid` is only read when `csid` is present.

### scfdata, scfdataop
required: no

format: base64 encoded protobuf

confidence: from code

description: structured conference data attached to the event, plus the operation to perform with it.

### gdoc-attachment
required: no

format: repeatable, space separated `id title url extra`

example: `gdoc-attachment=<id>+<title>+<url>+empty`

confidence: from code

description: attaches Google Drive documents. Each part is URL encoded individually, and the literal `empty` marks a missing part. Between three and five parts are accepted.

### cls
required: no

format: integer

example: `cls=1`

confidence: from code

description: undocumented classification field.

### ab
required: no

format: boolean (`true` or `1`)

example: `ab=true`

confidence: from code

description: undocumented boolean flag.

### egep
required: no

format: integer (`0`, `1` or `2`)

example: `egep=1`

confidence: from code

description: undocumented guest permission preset. Values outside the enum are ignored.

### taskId, taskBundleId, ddl
required: no

format: text (`taskId`, `taskBundleId`), datetime (`ddl`)

confidence: from code

description: these switch the editor into task mode rather than event mode. `ddl` is the task deadline.

## Tools
1. [Google calendar link generator](http://kalinka.tardate.com/)
1. [Google calendar link generator](http://output.jsbin.com/xujuluw)
