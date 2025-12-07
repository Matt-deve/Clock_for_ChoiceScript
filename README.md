# Clock_for_ChoiceScript
An open source Clock Module for *ChoiceScript* projects.
By Matt-deve aka CodedQuill

## Description

#### In Brief
A clock module allowing you to easily change the time and date with a single call to a subroutine, and then dynamically write it into your text. Users can define their own calendrical systems, date and time stamp formatting, and even use a pseudo-unix timestamp.

#### Contributing
Any feedback is more than welcome, you can message me on the ***Choice of Games*** forum at https://forum.choiceofgames.com/u/codedquill/summary or search for the '*clock_for_choicescript*' topic thread.

Any donations can be provided via my ***Patreon*** at https://patreon.com/CodedQuill

Wish to contribute? Via ***GitHub*** just fork the repo and submit a request. For anything else just message me.

## Setup
Download the file and add to your project with your other scene files.

Locate the section in the module for `*create` variables. Cut and paste these into 'startup.txt'

Call `*gosub_scene clock stamp` to initialise the timestamps before using them in your text. (This only needs to be done once)
### References
The clock uses a set of special reference variables prefixed with `ref_` in order to calculate the pseudo-unix time. They also act as default values in some instances. These are preset to the standard UTC datetime of 1970.01.01 00:00:00 i.e. midnight at the beginning of the first of January 1970.
If you wish to set a different reference time or date, you can simply change these manually, it's ok, everything will still work!
### Month and Day Names
Two arrays `month_names` and `day_names` determine the sequence of month and day names. They are preset to the standard English language forms of January to December and Monday to Sunday.
However, you can simply change the string values in the arrays to whatever you like! 
Change language? Swap Monday for Poniedziałek or Mandag or Isnin... etc...
Change date system? Add Octoday and update the array length to 8. or. Remove Monday (yay! no more Mondays!) and update the array length to 6.
### Month Lengths
An array `month_len` determines the number of days in each month, which in turn determines the length of the year (obviously). But programmatically this is how year lengths are calculated in the module as well... as the sum of month lengths + leap day (if a leap year - more later).
You can of course change any and all of these values to any integer above 0. (Just remember the first digit is the array length! This should match the length of month_names array as well!)
### Leap Years
Two variables `leap_month` and `leap_freq` control in which month a leap day is added, and how frequently leap years come around. These are preset to `leap_month = 2` and `leap_freq = 4` i.e. February and every four years.
Modify these to match any custom calendar system you can think of, within the limits of 1 leap day added per leap year frequency. Alternatively, you can set either of these values to 0 and no more leap years.
### Time Lengths
Adjust the variables `day_len` `hour_len` and `min_len` to alter the length of those time units. Each time unit uses the next smaller as it's unit of measure i.e. `hour_len = 60` means 'an hour is 60 minutes' (3600 seconds), set it to `hour_len = 100` and now 'an hour is 100 minutes' (6000 seconds).

## Usage
### Setting the Time and Date
#### Basics
There are 2 methods to set an absolute time and date, by either using the manual method e.g. `*set sec 30` or, preferably by calling the `set_current` subroutine: for example, `*gosub_scene clock set_current "1/4/2001 12:34:27" false` will set the date to the first of April 2001 and the time to 34 minutes and 27 seconds past 12.
But you don't need to set a full datetime code everytime. Using `1/4/2000` will just set the date and leave the time unchanged, and using `12:34` would change the hour and minute but leave the seconds and date unchanged.

*NOTE!* Dates are parsed in **ascending** units i.e. day month year! Times are parsed in **descending** units i.e. hour minute seconds!

The `set_current` subroutine also requires a `boolean` as a second parameter. This tells the parser whether it should resolve any datetime overflows in the datetime code you passed. For example, `*gosub_scene clock set_current "1/4/2001 12:34:66" true` would resolve to *April 1st 2001, 12:35:06*.
#### Formatting Marks
When using `set_current` (or `travel` - more later) certain formatting marks denote the unit of the preceding number. 

`.` `,` `-` or `/` indicates the numbers relate to a date, 

`:` indicates a time, and

` ` `;` or `_` indicates a switch from parsing a date (or time) to parsing a time (or date).

*NOTE!*  If no formatting marks are used the routine will assume you mean seconds and then set the time accordingly.

However, if you want to make it explicitly clear to the parser what is going on, you can use a number followed by a letter `s` `m` `h` `D` `M` or `Y` to denote the datetime unit to change. These can then be in any order.
#### Missing Units Commands
There is also method of telling the parser what to do with any datetime units you haven't specified. 

`%0` will set all missing values to `0` if an hour, minute or second, or `1` if it is a day, month or year. 

`%r` will set missing values to the reference datetime which is defined in startup.txt. 

For example, `27s32m5h%r` will set the time to 05:32 and 27 seconds on the first of January 1970 (as this is the preset reference date). Whereas, `27s32m5h%0` will set the date to the first of January in year 1.
### Getting the Time and Date
#### Basics
When you need a formatted date or time stamp, `${stamp_date}` or `${stamp_time}` will write it directly into your text. If you would like the pseudo-unix timestamp, use `${stamp_unix}`. The stamps are updated automatically when setting the time or date, or when changing the stamp formatting, but only if using the built-in subroutines! 

To update the stamps manually use : `*gosub_scene clock stamp`.

*NOTE!* When using the stamps in your text for the first time, they won't be set unless you have called one of the other subroutines. In this case you must set them manually as above.
#### Formatting
The subroutines `set_format_date` and `set_format_time` sets how the date and time should be written when you need a datetime stamp. This follows a similar procedure as setting the current time and date, but now we omit the numbers and add more letters and formatting marks to show how it will be written.

`w` or `W`   To denote a weekday element. `w` writes the first letter, `ww` first two letters, `www` three letter abbreviation, `wwww` full weekday name.

`s` or `S` To denote seconds.

`m` or `M` To denote minutes.

`h` or `H` To denote hours.

`d` or `D` To denote days.

`m` or `M` To denote months.

For the above, a single letter will write the number, a double letter will write the number with leading zeroes if below 10.

`hhh` or `HHH` will write the hour in a 12-hour clock format appending 'am' or 'pm' as appropriate.

`hhhh` or `HHHH` will write the hour in a 12-hour clock format with leading zeroes, appending 'am' or 'pm' as appropriate.

`ddd` or `DDD` will write the ordinal number i.e. `1st`, `2nd` `3rd` etc

`mmm` or `MMM` will write the three-letter abbreviation for the month.

`mmmm` or `MMMM` will write the full name of the month.

`y` or `Y` To denote years.

A single or double letter writes the last two digits of the year. A triple or quadruple letter writes the full year number.

`, . _ - : ; ' / of` etc are added 'as is' into the format.

*NOTE!* Adding words and numbers to the formatted stamp can be done, BUT care should be taken not to duplicate any key letters which will be interpolated into numbers!
### Travelling in Time
Use `travel` and pass a formatted time code as a parameter to travel forwards or back in time. The amounts specified will be added to the corresponding variables, so use negative values to go backwards in time! The values passed are also applied to the individual units, so passing `1M` as a parameter will move the time forward 1 whole month, not 30.4 days. The stamps will then update accordingly.

## Limitations
Known limitations as of 2025.12.06. Future updates may remove these issues in the future!

*NOTE!* No support for BC/BCE dates. Years below 1 will return a negative number.

*NOTE!* No escape character support when formatting timestamps.

*NOTE!* This is a *pseudo-unix* system emulating how the unix system works but is not, and cannot be, integrated with it using vanilla *ChoiceScript*. For this reason, it is impossible to get the current *actual* time the user is engaging with the work. Also, *leap seconds* have not been included in the pseudo-unix timestamp parser. To overcome these limitations a *JavaScript* solution is required. 
