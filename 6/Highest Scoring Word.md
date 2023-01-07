[Link](https://www.codewars.com/kata/57eb8fcdf670e99d9b000272) #Fundamentals #Strings #Arrays

Given a string of words, you need to find the highest scoring word.

Each letter of a word scores points according to its position in the alphabet: `a = 1, b = 2, c = 3` etc.

For example, the score of `abad` is `8` (1 + 2 + 1 + 4).

You need to return the highest scoring word as a string.

If two words score the same, return the word that appears earliest in the original string.

All letters will be lowercase and all inputs will be valid.

***
# Solutions
#cs
Alternative: `s.Split(' ').OrderBy(a => a.Select(b => b - 96).Sum()).Last()`
```cs
using System.Linq;

public class Kata
{
    public static string High(string input)
    {
        string result = "";
        int maxWeight = 0;

        foreach (var word in input.Split(' '))
        {
            int tempWeight = word.Aggregate(0, (charWeight, charItem) => charWeight += charItem - 96);
            if (tempWeight <= maxWeight) continue;
            maxWeight = tempWeight;
            result = word;
        }

        return result;
    }
}
```

#js 
```js
function high(x) {
    let word = "";
    x.split(" ").reduce((result, item) => {
        let weight = item.split("").reduce((result, item) => (result += item.charCodeAt(0) - 96), 0);
        if (result >= weight) return result;
        word = item;
        return weight;
    }, 0);
    return word;
}
```

#java
Alternative: `Arrays.stream(s.split(" ")).max(Comparator.comparingInt(a -> a.chars().map(i -> i - 96).sum())).get();`

```java
import java.util.Arrays;
import java.util.Comparator;

public class Kata {

    public static String high(String input) {
        return Arrays.stream(input.split(" "))
                .sorted(new Comparator<String>() {
                    public int compare(String value1, String value2) {
                        return getWeight(value2) - getWeight(value1);
                    }
                    private int getWeight(String value) {
                        return Arrays.stream(value.split(""))
                                .mapToInt(item -> item.charAt(0))
                                .reduce(0, (weight, item) -> weight += item - 96);
                    }
                }).toList().get(0);
    }
}
```