[Link](https://www.codewars.com/kata/52945ce49bb38560fe0001d9) #Arrays #Algorithms

Here you will create the classic [Pascal's triangle](https://en.wikipedia.org/wiki/Pascal%27s_triangle).  
Your function will be passed the depth of the triangle and your code has to return the corresponding Pascal's triangle up to that depth.

The triangle should be returned as a nested array. For example:

```
pascal(5) -> [ [1], [1,1], [1,2,1], [1,3,3,1], [1,4,6,4,1] ]
```

To build the triangle, start with a single `1` at the top, for each number in the next row you just take the two numbers above it and add them together, except for the edges, which are all `1`. e.g.:

```
      1
    1   1
  1   2   1
1   3   3   1
```

***
# Solutions
#cs
```cs
public class Kata
{
    public static long[][] Pascal(long depth)
    {
        long[][] result = new long[depth][];
        result[0] = new long[] { 1 };
        if (depth == 1) return result;
        result[1] = new long[] { 1, 1 };

        for (int i = 2; i < depth; i++)
            result[i] = getSequence(result[i - 1]);

        return result;
    }

    private static long[] getSequence(long[] prevSequence)
    {
        long[] result = new long[prevSequence.Length + 1];
        result[0] = 1;
        result[prevSequence.Length] = 1;
      
        for (int i = 1; i < prevSequence.Length; i++)
            result[i] = prevSequence[i] + prevSequence[i - 1];
      
        return result;
    }
}
```