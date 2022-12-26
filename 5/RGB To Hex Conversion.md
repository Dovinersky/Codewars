[Link](https://www.codewars.com/kata/513e08acc600c94f01000001) #Algorithms

The rgb function is incomplete. Complete it so that passing in RGB decimal values will result in a hexadecimal representation being returned. Valid decimal values for RGB are 0 - 255. Any values that fall out of that range must be rounded to the closest valid value.

Note: Your answer should always be 6 characters long, the shorthand with 3 will not work here.

The following are examples of expected output values:

```java
RgbToHex.rgb(255, 255, 255) // returns FFFFFF
RgbToHex.rgb(255, 255, 300) // returns FFFFFF
RgbToHex.rgb(0, 0, 0)       // returns 000000
RgbToHex.rgb(148, 0, 211)   // returns 9400D3
```

***
# Solutions
#java
```java
import java.util.HexFormat;

public class RgbToHex {
    public static String rgb(int red, int green, int blue) {
        if (red < 0) red = 0;
        if (red > 255) red = 255;
        if (green < 0) green = 0;
        if (green > 255) green = 255;
        if (blue < 0) blue = 0;
        if (blue > 255) blue = 255;

        return String.format("%s%s%s", toHex(red), toHex(green), toHex(blue)).toUpperCase();
    }
     private static String toHex(int value){
         return HexFormat.of().toHexDigits((byte)value);
     }
}
```