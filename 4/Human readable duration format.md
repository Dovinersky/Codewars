[Link](https://www.codewars.com/kata/52742f58faf5485cae000b9a) #Strings #DateTime #Algorithms

Your task in order to complete this Kata is to write a function which formats a duration, given as a number of seconds, in a human-friendly way.

The function must accept a non-negative integer. If it is zero, it just returns `"now"`. Otherwise, the duration is expressed as a combination of `years`, `days`, `hours`, `minutes` and `seconds`.

It is much easier to understand with an example:

```text
* For seconds = 62, your function should return 
    "1 minute and 2 seconds"
* For seconds = 3662, your function should return
    "1 hour, 1 minute and 2 seconds"
```

**For the purpose of this Kata, a year is 365 days and a day is 24 hours.**

Note that spaces are important.

### Detailed rules

The resulting expression is made of components like `4 seconds`, `1 year`, etc. In general, a positive integer and one of the valid units of time, separated by a space. The unit of time is used in plural if the integer is greater than 1.

The components are separated by a comma and a space (`", "`). Except the last component, which is separated by `" and "`, just like it would be written in English.

A more significant units of time will occur before than a least significant one. Therefore, `1 second and 1 year` is not correct, but `1 year and 1 second` is.

Different components have different unit of times. So there is not repeated units like in `5 seconds and 1 second`.

A component will not appear at all if its value happens to be zero. Hence, `1 minute and 0 seconds` is not valid, but it should be just `1 minute`.

A unit of time must be used "as much as possible". It means that the function should not return `61 seconds`, but `1 minute and 1 second` instead. Formally, the duration specified by of a component must not be greater than any valid more significant unit of time.

***
# Solutions
#java
```java
import java.util.LinkedList;
import java.util.stream.Collectors;

public class TimeFormatter {
    // year =  31 536 000 seconds
    // month =  2 592 000 seconds
    // day =  86400 seconds
    // hour =  3600 seconds
    // minute = 60 seconds
    private static class DivisionResult {
        int integer = -1;
        int remainder = -1;
        public DivisionResult(int integer, int remainder) {
            this.integer = integer;
            this.remainder = remainder;
        }
    }

    public static String formatDuration(int secondsTotal) {
        if (secondsTotal == 0) return "now";

        DivisionResult result = getDivisionResult(secondsTotal, 31536000);
        int years = result.integer;

//        result = getDivisionResult(result.remainder, 2592000);
//        int months = result.integer;

        result = getDivisionResult(result.remainder, 86400);
        int days = result.integer;

        result = getDivisionResult(result.remainder, 3600);
        int hours = result.integer;

        result = getDivisionResult(result.remainder, 60);
        int minutes = result.integer;

        int seconds = result.remainder;

        return getFormattedString(years, 0, days, hours, minutes, seconds);
    }

    private static DivisionResult getDivisionResult(int value, int divider) {
        int integer = value / divider;
        if (integer == 0) return new DivisionResult(0, value);
        return new DivisionResult(integer, value % divider);
    }

    private static String getFormattedString(int years, int months, int days, int hours, int minutes, int seconds) {
        StringBuffer buffer = new StringBuffer();
        LinkedList<String> tokens = new LinkedList<>();

        tokens.add(getFormattedToken(years, "year"));
//        tokens.add(getFormattedToken(months, "month"));
        tokens.add(getFormattedToken(days, "day"));
        tokens.add(getFormattedToken(hours, "hour"));
        tokens.add(getFormattedToken(minutes, "minute"));
        tokens.add(getFormattedToken(seconds, "second"));

        tokens = tokens.stream().filter(item -> !item.equals("")).collect(Collectors.toCollection(LinkedList::new));

        if (tokens.size() == 1)
            return tokens.get(0);

        if (tokens.size() == 2)
            return buffer.append(tokens.get(0)).append(" and ").append(tokens.get(1)).toString();

        for (int i = 0; i < tokens.size() - 2; i++)
            buffer.append(tokens.get(i)).append(", ");

        buffer.append(tokens.get(tokens.size() - 2)).append(" and ").append(tokens.getLast());

        return buffer.toString();
    }

    private static String getFormattedToken(int time, String timeName) {
        StringBuffer buffer = new StringBuffer();
        if (time != 0) buffer.append(time).append(" ").append(timeName);
        if (time > 1) buffer.append("s");
        return buffer.toString();
    }
}
```