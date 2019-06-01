---
description: Represents timestamps.
---

# Date

## API

The Date API is still preliminary and requires importing the `Date` object from the host \(as `Date`\).

### Static

* Date.**now**\(\): `i64` Returns the current UTC timestamp in milliseconds
* Date.**UTC**\(year: `i32`, month?: `i32`, day?: `i32`, hour?: `i32`, minute?: `i32`, second?: `i32`, millisecond?: `i64`\): `i64` Returns the UTC timestamp in milliseconds of the specified date.

### Constructor

* new **Date**\(value: `i64`\) Constructs a new date object from an UTC timestamp in milliseconds.

### Instance

* Date\#**getTime**\(\): `i64` Gets the UTC timestamp of this date in milliseconds.
* Date\#**setTime**\(value: `i64`\): `i64` Sets the UTC timestamp of this date in milliseconds and returns the timestamp.



