[Link](https://www.codewars.com/kata/5848565e273af816fb000449) #Fundamentals #Strings #RegularExpressions #Arrays #Ciphers #Algorithms #Cryptography #Security

## Acknowledgments:

I thank [yvonne-liu](https://www.codewars.com/users/yvonne-liu) for the idea and for the example tests :)

## Description:

Encrypt this!

You want to create secret messages which can be deciphered by the [Decipher this!](https://www.codewars.com/kata/decipher-this) kata. Here are the conditions:

1.  Your message is a string containing space separated words.
2.  You need to encrypt each word in the message using the following rules:
    -   The first letter must be converted to its ASCII code.
    -   The second letter must be switched with the last letter
3.  Keepin' it simple: There are no special characters in the input.

## Examples:

```java
Kata.encryptThis("Hello") => "72olle"
Kata.encryptThis("good") => "103doo"
Kata.encryptThis("hello world") => "104olle 119drlo"
```

***
# Solutions
#java
```java
public class Kata {
    public static String encryptThis(String text) {
        if (text.length() == 0) return "";
        String[] tokens = text.split(" ");
        StringBuffer result = new StringBuffer();

        for (int i = 0; i < tokens.length; i++) {
            if (tokens[i].length() < 3) {
                tokens[i] = setASCII(tokens[i]);
                continue;
            }
            tokens[i] = setASCII(swap(tokens[i]));
        }
        
        for (String token: tokens) {
            result.append(' ').append(token);
        }
        return result.replace(0, 1, "").toString();
    }

    private static String setASCII(String value) {
        StringBuffer buffer = new StringBuffer(value);
        int ascii = value.charAt(0);
        buffer.replace(0, 1, ascii + "");
        return buffer.toString();
    }

    private static String swap(String value) {
        StringBuffer buffer = new StringBuffer(value);
        buffer.setCharAt(1, value.charAt(value.length() - 1));
        buffer.setCharAt(value.length() - 1, value.charAt(1));
        return buffer.toString();
    }
}
```