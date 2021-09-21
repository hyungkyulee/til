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

#### My solution
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

#### Best reference
```
public class Kata
{
  public static string SpinWords(string sentence)
  {
    return String.Join(" ", sentence.Split(' ').Select(str => str.Length >= 5 ? new string(str.Reverse().ToArray()) : str));
  }
}
```
