# Regex-Tutorial: Matching HTML Tag

This Regex (Regular Expression) tutorial is created to help you understand and define the sequence of special characters to verify a search term. A Regex is a sequence of characters that defines a specific search pattern. When included in code or search algorithms, regular expressions can be used to find certain patterns of characters within a string, or to find and replace a character or sequence of characters within a string. They are also used frequently to validate input data.

# Summary

In this Tutorial I will be covering and breaking down the components of a regular expression used to match HTML Tag.

Example: `/^<([a-z]+)([^<]+)_(?:>(._)<\/\1>|\s+\/>)$/`

The below content will explain what each section of this code does and more.

# Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Back-references](#back-references)

# Regex Components

### Anchors

Anchors are used in a regular expression to check the first or last symbols of a string. There are two anchors you can use a Caret (^) which will look at the start of a string and a Dollar Sign ($) which will look at the end of a string.

For example if you want to check if something is fairy tale you might want to see if it starts with "Once Upon A Time".
```
/^Once Upon A Time/ => Checks to see if the string starts with "Once Upon A Time"
```
Or you could check if it ends with "Happily Ever After".
```
/Happily Ever After$/ => Checks to see if the string ends with "Happily Ever After"
```
There is a third way that these anchors can be used and that is together. When a regex starts with a ^ and ends with a $ then everything in string has to match the given criteria otherwise it will not pass. So for example the regex:
```
/^This is a Fairy Tale$/
```
Will match the string:
```
"This is a Fairy Tale"
```
But will not match the string:
```
"This is a scary Fairy Tale"
```
As the additional scary is not included in our criteria.

In our regex we use these anchors together to ensure that the whole HTML tag conforms to our criteria.

### Quantifiers

Quantifiers are used to check for repetition in our string and is done by one of three methods the Star (*), the Plus (+), or the Question Mark (?):
```
* matches zero or more repetitions of the preceding matcher.

+ matches one or more repetions of the preceding character.

? matches zero or one instance of the preceding character, making it optional
```
In our regex we use the first two of these the * and the +
```
([a-z]+) => looks for one or more letters from a to z

(.*) => looks for zero or more character of any type

\s+ => looks for one or more whitespace character

([^<]+)* => looks for zero or more of one or more of a character that is not a <
```
The last use in the above list chains two quantifiers together. Since quantifiers deal with the preceding character you can read this from right to left * (zero or more) of + (one or more).

The ? quantifier is what is know as a lazy operator as it will look for only one of the preceding character as opposed to * and + which are greedy operators and will match the preceding character to every other character that it can before stopping.

### OR Operator

The OR Operator (|) is used to look for either one thing or another. for example:
```
/who|whom/ => would match an instance of either who or whom, effectively looking for both.
```
This alternation can be given context as well using [Grouping and Capturing](#grouping-and-capturing) like we do in our regex:
```
(?:>(.*)<\/\1>|\s+\/>) => Looks for either a close to the first tag, body, and a closing tag (i.e. > body </div>) or at least one whitespace a / then a > like for a self closing HTML tag (i.e.  />)
```
### Character Classes

Character Classes can be used to filter characters by thier type. The character class we can use to filter with are:
```
\d matches a character that is a digit => i.e. a number between 0-9

\w matches any alphanumeric word character => i.e. a-z and 0-9

\s matches a whitespace character => a space, a tab, or a line break

. matches any character

\**character** look for a specifc character => i.e. \/ will look for a / character
```
In our regex we use three of these character classes the . in our grouping that looks for the html tags internal content and the \s to look for white space before a closure of a self closing tag, and a \\/ which will look for a / that is present in a closing tag or at the end of a self closing tag.
```
(.*) => checks for any characters that are in our HTMLs content

\s+\/> => checks for space followed by a / character and  then a > character

<\/ => checks for a < character followed by a / character
```
Side note: by capitalizing the more general classes you can match any thing that is NOT that character:
```
\D matches any character that is not a digit

\W matches any character that is not alphanumeric

\S matches any character that is not whitespace
```
### Flags

Flags (or modifiers) modify the output of a regular expression and change the context over which the whole regex is evaluated. Flags are appended to the outside of the regex most of which are written between / symbols (ex. /regex/). Some common types of flags are, i, which makes the regex case insensitive, g, which makes the regex global matching all instances, not just the first, and, m, which cause [anchor](#anchors) meta characters to work on each line.
```
/the/g => matches every "the" in the string not just the first

/the/i => matches the first "the" in the string regardless of it is "The", "the" or "tHE"

/^The/m => matches the first "The" that starts a line
```

### Grouping and Capturing

In Regex you can use () to create groups, which is a grouping of all the subpatterns written inside the parentheses. Groups can be useful when used in conjunction with other methods such as [Quantifiers](#quantifiers) or the [OR Operator](#or-operator) to expand or limit thier context.
<br>
<br>
There are two types of groups Capturing and Non-Capturing.
<br>
<br>
By Capturing something you are indicating that you're interested in not only matching, but also using what you have matched in specific parts of the matched string later on. This is done by default when a group is created using ().

For example in our regex:
```
([a-z]+) => creates are first capture group which will look for one or more characters from a-z, creating this grouping lets us refer back to this if we want to repeat this pattern later.

([^<]+)* => the () create a group which "captures" [^<]+ so that we can use it later with a * that will apply to the whole group.
```
Non-Capturing is a capture group that will match the characters but will not create a capture group and is denoted by including ?: just as the first thing inside of your () group.

For example in our regex:
```
(?:>(.*)<\/\1>|\s+\/>) => This Non-Capturing group is used to give context to our Or operator | so that it we know to use either >(.*)<\/\1> OR \s+\/>. The (?:) keeps the context for the OR to soley to the two subpatterns inside the group.
```

### Bracket Expressions

Bracket Expressions ([ ]) can be used to specify groups or ranges of characters for the regex to match:
```
/[abc]/ => can be used to match a or b or c
/[a-z]/ => can be used to match any lower case letter a through z
/[a-z0-9-_] => matches lower case a through z, digits 0 through 9, - character, or _ character
```
Additionally a Caret (^) can be used inside of the brackets to look for anything that is NOT defined in the bracket expression.
```
/[^-_]/ => matches anything that is NOT a - or a _ character

/[^a-z]/ => matches any character that is NOT a lowercase a through z
```
So in our regex:
```
[a-z]+ => matches one or more letter character a-z

[^<]+ => matches one or more character that is NOT a <
```

### Back-references

A back reference can be used to refer to a previous [capture group](#grouping-and-capturing). The syntax is \ followed by the number that corresponds to the group (i.e. \1 or \2). Groups are assigned a number based on what order they appear in the regex. So looking at our regex:
```
/^<([a-z]+)([^<]+)*(?:>(.*)<\/\1>|\s+\/>)$/

([a-z]+) would be group 1

([^<]+) would be group 2

(?:>(.*)<\/\1>|\s+\/>) is a Non-Capturing group so it is not assigned a number

(.*) would be group 3

So

<\/\1> => \1 refers to capture group 1 so it could be rewritten as <\/[a-z]+>
```


# Author

I am passionate software developer, currently enrolled in 
full stack web developement course with georgia tech. 
My Github:https://github.com/emma4jesus
My Gist:https://github.com/emma4jesus# Regex-Tutorial: Matching HTML Tag

This Regex (Regular Expression) tutorial is created to help you understand and define the sequence of special characters to verify a search term. A Regex is a sequence of characters that defines a specific search pattern. When included in code or search algorithms, regular expressions can be used to find certain patterns of characters within a string, or to find and replace a character or sequence of characters within a string. They are also used frequently to validate input data.

# Summary

In this Tutorial I will be covering and breaking down the components of a regular expression used to match HTML Tag.

Example: `/^<([a-z]+)([^<]+)_(?:>(._)<\/\1>|\s+\/>)$/`

The below content will explain what each section of this code does and more.

# Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Back-references](#back-references)


# Regex Components

### Anchors

Anchors are used in a regular expression to check the first or last symbols of a string. There are two anchors you can use a Caret (^) which will look at the start of a string and a Dollar Sign ($) which will look at the end of a string.

For example if you want to check if something is fairy tale you might want to see if it starts with "Once Upon A Time".
```
/^Once Upon A Time/ => Checks to see if the string starts with "Once Upon A Time"
```
Or you could check if it ends with "Happily Ever After".
```
/Happily Ever After$/ => Checks to see if the string ends with "Happily Ever After"
```
There is a third way that these anchors can be used and that is together. When a regex starts with a ^ and ends with a $ then everything in string has to match the given criteria otherwise it will not pass. So for example the regex:
```
/^This is a Fairy Tale$/
```
Will match the string:
```
"This is a Fairy Tale"
```
But will not match the string:
```
"This is a scary Fairy Tale"
```
As the additional scary is not included in our criteria.

In our regex we use these anchors together to ensure that the whole HTML tag conforms to our criteria.

### Quantifiers

Quantifiers are used to check for repetition in our string and is done by one of three methods the Star (*), the Plus (+), or the Question Mark (?):
```
* matches zero or more repetitions of the preceding matcher.

+ matches one or more repetions of the preceding character.

? matches zero or one instance of the preceding character, making it optional
```
In our regex we use the first two of these the * and the +
```
([a-z]+) => looks for one or more letters from a to z

(.*) => looks for zero or more character of any type

\s+ => looks for one or more whitespace character

([^<]+)* => looks for zero or more of one or more of a character that is not a <
```
The last use in the above list chains two quantifiers together. Since quantifiers deal with the preceding character you can read this from right to left * (zero or more) of + (one or more).

The ? quantifier is what is know as a lazy operator as it will look for only one of the preceding character as opposed to * and + which are greedy operators and will match the preceding character to every other character that it can before stopping.

### OR Operator

The OR Operator (|) is used to look for either one thing or another. for example:
```
/who|whom/ => would match an instance of either who or whom, effectively looking for both.
```
This alternation can be given context as well using [Grouping and Capturing](#grouping-and-capturing) like we do in our regex:
```
(?:>(.*)<\/\1>|\s+\/>) => Looks for either a close to the first tag, body, and a closing tag (i.e. > body </div>) or at least one whitespace a / then a > like for a self closing HTML tag (i.e.  />)
```
### Character Classes

Character Classes can be used to filter characters by thier type. The character class we can use to filter with are:
```
\d matches a character that is a digit => i.e. a number between 0-9

\w matches any alphanumeric word character => i.e. a-z and 0-9

\s matches a whitespace character => a space, a tab, or a line break

. matches any character

\**character** look for a specifc character => i.e. \/ will look for a / character
```
In our regex we use three of these character classes the . in our grouping that looks for the html tags internal content and the \s to look for white space before a closure of a self closing tag, and a \\/ which will look for a / that is present in a closing tag or at the end of a self closing tag.
```
(.*) => checks for any characters that are in our HTMLs content

\s+\/> => checks for space followed by a / character and  then a > character

<\/ => checks for a < character followed by a / character
```
Side note: by capitalizing the more general classes you can match any thing that is NOT that character:
```
\D matches any character that is not a digit

\W matches any character that is not alphanumeric

\S matches any character that is not whitespace
```
### Flags

Flags (or modifiers) modify the output of a regular expression and change the context over which the whole regex is evaluated. Flags are appended to the outside of the regex most of which are written between / symbols (ex. /regex/). Some common types of flags are, i, which makes the regex case insensitive, g, which makes the regex global matching all instances, not just the first, and, m, which cause [anchor](#anchors) meta characters to work on each line.
```
/the/g => matches every "the" in the string not just the first

/the/i => matches the first "the" in the string regardless of it is "The", "the" or "tHE"

/^The/m => matches the first "The" that starts a line
```

### Grouping and Capturing

In Regex you can use () to create groups, which is a grouping of all the subpatterns written inside the parentheses. Groups can be useful when used in conjunction with other methods such as [Quantifiers](#quantifiers) or the [OR Operator](#or-operator) to expand or limit thier context.
<br>
<br>
There are two types of groups Capturing and Non-Capturing.
<br>
<br>
By Capturing something you are indicating that you're interested in not only matching, but also using what you have matched in specific parts of the matched string later on. This is done by default when a group is created using ().

For example in our regex:
```
([a-z]+) => creates are first capture group which will look for one or more characters from a-z, creating this grouping lets us refer back to this if we want to repeat this pattern later.

([^<]+)* => the () create a group which "captures" [^<]+ so that we can use it later with a * that will apply to the whole group.
```
Non-Capturing is a capture group that will match the characters but will not create a capture group and is denoted by including ?: just as the first thing inside of your () group.

For example in our regex:
```
(?:>(.*)<\/\1>|\s+\/>) => This Non-Capturing group is used to give context to our Or operator | so that it we know to use either >(.*)<\/\1> OR \s+\/>. The (?:) keeps the context for the OR to soley to the two subpatterns inside the group.
```

### Bracket Expressions

Bracket Expressions ([ ]) can be used to specify groups or ranges of characters for the regex to match:
```
/[abc]/ => can be used to match a or b or c
/[a-z]/ => can be used to match any lower case letter a through z
/[a-z0-9-_] => matches lower case a through z, digits 0 through 9, - character, or _ character
```
Additionally a Caret (^) can be used inside of the brackets to look for anything that is NOT defined in the bracket expression.
```
/[^-_]/ => matches anything that is NOT a - or a _ character

/[^a-z]/ => matches any character that is NOT a lowercase a through z
```
So in our regex:
```
[a-z]+ => matches one or more letter character a-z

[^<]+ => matches one or more character that is NOT a <
```

### Back-references

A back reference can be used to refer to a previous [capture group](#grouping-and-capturing). The syntax is \ followed by the number that corresponds to the group (i.e. \1 or \2). Groups are assigned a number based on what order they appear in the regex. So looking at our regex:
```
/^<([a-z]+)([^<]+)*(?:>(.*)<\/\1>|\s+\/>)$/

([a-z]+) would be group 1

([^<]+) would be group 2

(?:>(.*)<\/\1>|\s+\/>) is a Non-Capturing group so it is not assigned a number

(.*) would be group 3

So

<\/\1> => \1 refers to capture group 1 so it could be rewritten as <\/[a-z]+>
```




# Author

### I am passionate software developer, currently enrolled in full stack web developement course with georgia tech. 
### My Github:https://github.com/emma4jesus
### My Gist:https://gist.github.com/emma4jesus
