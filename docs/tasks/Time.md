# Time

The C date and time functions are a group of functions in the standard library of the C programming language implementing date and time manipulation operations. They provide support for time acquisition, conversion between date formats and formatted output to strings.

## Hello world

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed_example/code/time_HelloWorld/)](https://developer.mbed.org/teams/mbed_example/code/time_HelloWorld/file/8593c9813840/main.cpp) 

## 2038

We will be preparing the mbed libraries for the year 2038 problem and hope to cause as little disruption as possible. Some more information about the year 2038 problem from [Wikipedia](https://en.wikipedia.org/wiki/Year_2038_problem):

> The Year 2038 problem is an issue for computing and data storage situations in which time values are stored or calculated as a signed 32-bit integer, and this number is interpreted as the number of seconds since 00:00:00 UTC on 1 January 1970 ("the epoch").[1] Such implementations cannot encode times after 03:14:07 UTC on 19 January 2038, a problem similar to but not entirely analogous to the "Y2K problem" (also known as the "Millennium Bug"), in which 2-digit values representing the number of years since 1900 could not encode the year 2000 or later. Most 32-bit Unix-like systems store and manipulate time in this "Unix time" format, so the year 2038 problem is sometimes referred to as the "Unix Millennium Bug" by association.
