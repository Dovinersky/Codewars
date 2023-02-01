[Link](https://www.codewars.com/kata/525c7c5ab6aecef16e0001a5) #Parsing #Strings #Algorithms

In this kata we want to convert a string into an integer. The strings simply represent the numbers in words.

Examples:

-   "one" => 1
-   "twenty" => 20
-   "two hundred forty-six" => 246
-   "seven hundred eighty-three thousand nine hundred and nineteen" => 783919

Additional Notes:

-   The minimum number is "zero" (inclusively)
-   The maximum number, which must be supported is 1 million (inclusively)
-   The "and" in e.g. "one hundred and twenty-four" is optional, in some cases it's present and in others it's not
-   All tested numbers are valid, you don't need to validate them

***
# Solutions
#java 
```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;

public class Parser {
    private static final HashMap<String, String> dict = new HashMap<>();
    static {
        String[] digits = "one two three four five six seven eight nine".split(" ");
        String[] teens = "eleven twelve thirteen fourteen fifteen sixteen seventeen eighteen nineteen".split(" ");
        String[] dozens = "ten twenty thirty forty fifty sixty seventy eighty ninety".split(" ");
        dict.put("zero", "0");
        for (int i = 0; i < digits.length; i++) {
            dict.put(digits[i], (i + 1) + "");
            dict.put(teens[i], (i + 1 + 10) + "");
            dict.put(dozens[i], ((i + 1) * 10) + "");
        }
    }
    private static final List<String> modifiers = Arrays.asList("thousand million".split(" "));

    public static int parseInt(String input) {
        int result = 0;
        String[] tokens = input.replaceAll(" and ", " ").split(" ");

        for (int i = 0; i < tokens.length; i++) {
            String temp = dict.get(tokens[i]);
            if (temp == null) {
                String[] split = tokens[i].split("-");
                if (split.length > 1)
                    tokens[i] = (Integer.parseInt(dict.get(split[0])) + Integer.parseInt(dict.get(split[1]))) + "";
                else {
                    if (tokens[i].equals("hundred")) {
                        tokens[i - 1] = (Integer.parseInt(tokens[i - 1]) * 100) + "";
                        tokens[i] = "0";
                    }
                }
                continue;
            }
            tokens[i] = dict.get(tokens[i]);
        }

        int number = 0;
        for (int i = 0; i < tokens.length; i++) {
            try {
                number += Integer.parseInt(tokens[i]);
            }
            catch (Exception e) {
                result += number * Math.pow(1000, modifiers.indexOf(tokens[i]) + 1);
                number = 0;
            }
        }

        return result + number;
    }
}
```