# Clock_for_ChoiceScript
An open source Clock Module for Choicescript projects.
By CodedQuill aka Matt-deve

## Description


## Setup
Download the file and add to your project with your other scene files.
Locate the section in the module for *create variables. Cut and paste these into 'startup.txt'
### References
The clock uses a set of special reference variables prefixed with 'ref_' in order to calculate the pseudo-unix time. They also act as default values in some instances. These are preset to the standard UTC datetime of 1970.01.01 00:00:00 i.e. midnight at the beginning of the first of January 1970.
If you wish to set a different reference time or date, you can simply change these manually, it's ok, everything will still work!
### Month and Day Names
Two arrays 'month_names' and 'day_names' determine the sequence of month and day names. They are preset to the standard English language forms of January to December and Monday to Sunday.
However, you can simply change the string values in the arrays to whatever you like! 
Change language? Swap Monday for Poniedziałek or Mandag or Isnin... etc...
Change date system? Add Octoday and update the array length to 8. or. Remove Monday (yay! no more Mondays!) and update the array length to 6.
### Month Lengths
An array 'month_len' determines the number of days in each month, which in turn determines the length of the year (obviously). But programmatically this is how year lengths are calculated in the module as well... as the sum of month lengths + leap day (if a leap year - more later).
You can of course change any and all of these values to any integer above 0. (Just remember the first digit is the array length! This should match the length of month_names array as well!)
### Leap Years
Two variables 'leap_month' and 'leap_freq' control in which month a leap day is added, and how frequently leap years come around. These are preset to 'leap_month = 2' and 'leap_freq = 4' i.e. February and every four years.
Modify these to match any custom calendar system you can think of, within the limits of 1 leap day added per leap year frequency. Alternatively, you can set either of these values to 0 and no more leap years.
### Time Lengths
Adjust the variables 'day_len' 'hour_len' and 'min_len' to alter the length of those time units. Each time unit uses the next smaller as it's unit of measure i.e. 'hour_len = 60' means 'an hour is 60 minutes' (3600 seconds), set it to 'hour_len = 100' and now 'an hour is 100 minutes' (6000 seconds).

## Usage
### Setting the Time and Date
#### Basics
There are 2 methods to set an absolute time and date, by either using the manual method e.g. '*set sec 30' or, preferably by calling the 'set_current' subroutine: for example, '*gosub_scene clock set_current "1/4/2001 12:34:27" ' will set the date to the first of April 2001 and the time to 34 minutes and 27 seconds past 12.
But you don't need to set a full datetime code everytime. Using '1/4/2000' will just set the date and leave the time unchanged, and using '12:34' would change the hour and minute but leave the seconds and date unchanged.
#### Formatting Marks
When using 'set_current' (or 'travel' - more later) certain formatting marks denote the unit of the preceding number. Using '.' ',' or '/' indicates the numbers relate to a date. Using ':' indicates a time. And using ' ' ';' or '_' indicates a switch from parsing a date (or time) to parsing a time (or date).
Warning, if no formatting marks are used the routine will assume you mean seconds and then set the time accordingly.
However, if you want to make it explicitly clear to the parser what is going on, you can use a number followed by a letter 's' 'm' 'h' 'D' 'M' or 'Y' to denote the datetime unit to change. These can then be in any order.
#### Missing Units Commands
There is also method of telling the parser what to do with any datetime units you haven't specified. This uses '%' symbol followed by either a '0' or 'r'. So '%0' will set all missing values to '0' if an hour, minute or second, or '1' if it is a day, month or year. Using '%r' will set missing values to the refernce datetime setup at the start. For example, '27s32m5h%r' will set the time to 05:32 and 27 seconds on the first of January 1970 (as this is the preset reference date). '27s32m5h%0' on the other hand will set the date to the first of January in year 1.
### Getting the Time and Date
#### Formatting
The subroutines 'set_format_date' and 'set_format_time' sets how the date and time should be written when you need a datetime stamp. This follows a similar procedure as setting the current time and date, but now we omit the numbers and add more letters and formatting marks to show how it will be written.
