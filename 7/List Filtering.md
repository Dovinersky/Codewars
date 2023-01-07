[Link](https://www.codewars.com/kata/53dbd5315a3c69eed20002dd) #Lists #Filtering #DataStructures #Fundamentals

In this kata you will create a function that takes a list of non-negative integers and strings and returns a new list with the strings filtered out.

### Example

```csharp
ListFilterer.GetIntegersFromList(new List<object>(){1, 2, "a", "b"}) => {1, 2}
ListFilterer.GetIntegersFromList(new List<object>(){1, 2, "a", "b", 0, 15}) => {1, 2, 0, 15}
ListFilterer.GetIntegersFromList(new List<object>(){1, 2, "a", "b", "aasf", "1", "123", 231}) => {1, 2, 231}
```

***
# Solutions
#cs 
Solution `listOfItems.OfType<int>()` is possible as well.
```cs
using System.Collections.Generic;
using System.Linq;

public class ListFilterer
{
    public static IEnumerable<int> GetIntegersFromList(List<object> listOfItems)
    {
        return listOfItems.Where(item => item is int).Select(item => (int)item);
    }
}
```

#js 

```js
function filter_list(l) {
    return l.filter(item => typeof item == "number");
}
```

#java 
Alternative solutions: 
- `list.stream().filter(x -> x instanceof Integer).toList()`
-  `list.stream().filter(Integer.class::isInstance).toList()`
```java
import java.util.List;
import java.util.stream.Collectors;

public class Kata {

    public static List<Object> filterList(final List<Object> list) {
        return list.stream().filter(item -> Integer.class.isInstance(item)).collect(Collectors.toList());
    }
}
```