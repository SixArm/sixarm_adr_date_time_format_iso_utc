# SixArm.com → Architecture Decision Records →<br> Date time format using ISO8601 and UTC time zone

We want to be able to use a consistent date format and time format that are useful for our typical needs.

  * We need this because some of our systems do not have a specification of a date format or time format.

  * For example, JSON messages do not have a native timestamp, so we must choose a serialization text format.

We choose the format "YYYY-MM-DDTHH:MM:SS.NNNNNNNNN+HH:MM", which is using ISO8601, and we use UTC time zone whenever possible.

  * The format shows the year, month, day, hour, minute, second, nanoseconds, and timezone offset.

  * The format is generally human-readable, sort-orderable, and machine-interoperable.

Examples:

  * Date: "2018-01-01" is 2018 January 1st.

  * Time: "00:00:00.000000000" is midnight.

  * Timezone: "+00:00" is UTC.

  * Timestamp: "2018-01-01T00:00:00.000000000+00:00" is 2018 January 1st, midnight, UTC.

Details:

  * ISO8601 standard format.

  * UTC time zone. This is Universal Coordinated Time, a.k.a. Greenwich Mean Time, Zulu Time Zone.

  * Human-friendly readability, by using separator characters when possible, such as "HH:MM", rather than "HHMM".

  * Sort-friendly orderability, such as "YYYY-MM-DD", rather than than "MM-DD-YYYY".

  * Machine-friendly interoperability, by using a format that favors parsing into fields. The nanosecond separator is using a period "." rather than a comma "," because a comman may interfere with some spreadsheet comma-separated-value parsing.
    
Reasoning:

  * For typical use, we value easy to read/write by humans, more than raw speed/size.

  * For typical use, we want a format that works fine in machine systems, and also works well manually, such as writing sample data, reading JSON output, grepping a log file, etc.

  * For atypical use, such as high performance computing, we expect we'll want to optimize any text format we choose by converting the the text to a faster format, such as a programming language's built-in date object type. So the text format doesn't matter much for HPC.

  * For the time, we use nanoseconds, rather than just seconds, because we want our timestamps to be fully-compatible with high-precision systems that use nanoseconds.

  * For the time nanoseconds decimal, we use a period, rather than a comma, because a period looks more like a decimal, and is also easier to use in comma separated values (CSV) files.

  * For the timezone, we use hours and minutes, rather than "Z" or "UTC", because we want timezone syntax to sort correctly among multiple timezones.

  * For the timezone hours and minutes, we use a separator, rather than no separator, because we want the timezone format to be akin to the time format, because this improves human readability and can help machine parseability.

Known issues:

  * For the time nanoseconds, note that as of this writing, the date command on the BSD operating system does not have a nanoseconds option. Consider installling GNU date.
