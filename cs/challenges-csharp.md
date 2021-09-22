# C# Challenges on a day

## Codewars
> reference: examples of this excercise are available from codewars.com

### Reverse string
#### Instruction
Write a function that takes in a string of one or more words, and returns the same string, but with all five or more letter words reversed (like the name of this kata).

Strings passed in will consist of only letters and spaces.
Spaces will be included only when more than one word is present.
Examples:
```
spinWords("Hey fellow warriors") => "Hey wollef sroirraw" 
spinWords("This is a test") => "This is a test" 
spinWords("This is another test") => "This is rehtona test"
```

#### A first working trial
```
using System.Collections.Generic;
using System.Linq;
using System;

public class Kata
{
  public static string SpinWords(string sentence)
  {
    string[] words = sentence.Split(' ');
    var index = 0;
    foreach(var word in words)
    {
      char[] letters = word.ToCharArray();
      if (letters.Length < 5) 
      {
        index++;
        continue;
      }
      
      var reverse = String.Empty;
      for (var i = letters.Length-1; i >= 0; i--)
      {
        reverse += letters[i];
      }
      words[index++] = reverse;
    }
    
    return string.Join(' ', words);
  }
}
```

#### Improvement
```
public class Kata
{
  public static string SpinWords(string sentence)
  {
    return String.Join(" ", sentence.Split(' ').Select(str => str.Length >= 5 ? new string(str.Reverse().ToArray()) : str));
  }
}
```

### Find Square number (perfect square)
#### Instruction
Given an integral number, determine if it's a square number:

In mathematics, a square number or perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself.

#### A first working trial
```
public static bool IsSquare(int n)
  {
    var nominator = 0;
    return n switch 
    {
        0 => true,
        var x when x<0 => false,
        _=> call(() => {
          while (n > nominator++)
          {
            if (n / nominator == nominator
               && n%nominator == 0)
            {
              return true;
            }
          }
          return false;
        })
    }
  }
```
OR
```
using System;

public class Kata
{
  public static bool IsSquare(int n)
  {
    var nominator = 0;   
    if (n==0) return true;
    if (n<0) return false;
    
    while (n > nominator++)
    {
      if (n / nominator == nominator
         && n%nominator == 0)
      {
        return true;
      }
    }
    return false;
  }
}
```
#### Improvement
> by Math function
```
using System;

public class Kata
{
  public static bool IsSquare(int n)
  {
    return Math.Sqrt(n) % 1 == 0;
  }
}
```
