[Link](https://www.codewars.com/kata/51ba717bb08c1cd60f00002f) #Algorithms

A format for expressing an ordered list of integers is to use a comma separated list of either

-   individual integers
-   or a range of integers denoted by the starting integer separated from the end integer in the range by a dash, '-'. The range includes all integers in the interval including both endpoints. It is not considered a range unless it spans at least 3 numbers. For example "12,13,15-17"

Complete the solution so that it takes a list of integers in increasing order and returns a correctly formatted string in the range format.

**Example:**

```java
Solution.rangeExtraction(new int[] {-10, -9, -8, -6, -3, -2, -1, 0, 1, 3, 4, 5, 7, 8, 9, 10, 11, 14, 15, 17, 18, 19, 20})
# returns "-10--8,-6,-3-1,3-5,7-11,14,15,17-20"
```

_Courtesy of rosettacode.org_

***
# Solutions
#java
```java
import static java.lang.Math.abs;

class Solution {
    public static String rangeExtraction(int[] sequence) {
        int minValue = sequence[0];
        int maxValue = minValue + 1;
        StringBuffer result = new StringBuffer(sequence.length);

        for (int item: sequence) {
            if (abs(maxValue - item) == 1) {
                maxValue = item;
                continue;
            }
            result.append(getChainItem(minValue, maxValue)).append(",");
            minValue = item;
            maxValue = item;
        }
        return result.append(getChainItem(minValue, maxValue)).toString();
    }
    public static String getChainItem(int minValue, int maxValue) {
        return switch (abs(minValue - maxValue)) {
            case 0 -> minValue + "";
            case 1 -> minValue + "," + maxValue;
            default -> minValue + "-" + maxValue;
        };
    }
}
```