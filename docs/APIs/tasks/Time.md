# Time
The C date and time functions are a group of functions in the standard library of the C programming language implementing date and time manipulation operations. They provide support for time acquisition, conversion between date formats, and formatted output to strings.

The mbed libraries will be preparing for the year 2038 problem and hope to cause as little disruption as possible. Some more information about the year 2038 problem from wikipedia:

> The Year 2038 problem is an issue for computing and data storage situations in which time values are stored or calculated as a signed 32-bit integer, and this number is interpreted as the number of seconds since 00:00:00 UTC on 1 January 1970 ("the epoch").[1] Such implementations cannot encode times after 03:14:07 UTC on 19 January 2038, a problem similar to but not entirely analogous to the "Y2K problem" (also known as the "Millennium Bug"), in which 2-digit values representing the number of years since 1900 could not encode the year 2000 or later. Most 32-bit Unix-like systems store and manipulate time in this "Unix time" format, so the year 2038 problem is sometimes referred to as the "Unix Millennium Bug" by association.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/time_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/time_HelloWorld/file/b3b93997a0a6/main.cpp) 

```
/** Implementation of the C time.h functions
 *
 * Provides mechanisms to set and read the current time, based
 * on the microcontroller Real-Time Clock (RTC), plus some
 * standard C manipulation and formating functions.
 *
 * Example:
 * @code
 * #include "mbed.h"
 *
 * int main() {
 *     set_time(1256729737);  // Set RTC time to Wed, 28 Oct 2009 11:35:37
 *
 *     while(1) {
 *         time_t seconds = time(NULL);
 *
 *         printf("Time as seconds since January 1, 1970 = %d\n", seconds);
 *
 *         printf("Time as a basic string = %s", ctime(&seconds;));
 *
 *         char buffer[32];
 *         strftime(buffer, 32, "%I:%M %p\n", localtime(&seconds;));
 *         printf("Time as a custom formatted string = %s", buffer);
 *
 *         wait(1);
 *     }
 * }
 * @endcode
 */
 
/** Set the current time
 *
 * Initialises and sets the time of the microcontroller Real-Time Clock (RTC)
 * to the time represented by the number of seconds since January 1, 1970
 * (the UNIX timestamp).
 *
 * @param t Number of seconds since January 1, 1970 (the UNIX timestamp)
 *
 * Example:
 * @code
 * #include "mbed.h"
 *
 * int main() {
 *     set_time(1256729737); // Set time to Wed, 28 Oct 2009 11:35:37
 * }
 * @endcode
 */
``` 
