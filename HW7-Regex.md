---
title: "Homework 7: Regular Expressions"
---

***

*20 points possible. 4 questions, 5 points per question.*

***

In this exercise, you will practice composing regular expressions. Make sure this is your own work though you may share strategies with your peers.

### Objectives

* Understand what Regular Expressions are and how to use them.
* Practice using Regular Expressions to find and change textual strings.

### Resources

* [Visual Studio Code](https://code.visualstudio.com/) or
* [Regex 101 Regular Expression Tester](https://regex101.com)
* [Cheatography Regular Expressions Cheat Sheet](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)
* [Microsoft Regular Expression Quick Reference](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)
* [Blank Answer Sheet](HW7-Regex-Answersheet.txt){: download="HW7-Regex-Answersheet.txt"} text file into which you will insert your answers.

### Performing Regular Expression Searches

You may use [Visual Studio Code](https://code.visualstudio.com/) or [Regex 101](https://regex101.com) for this exercise.

To use **Visual Studio Code**:

1. Open or paste in the text to be searched.
2. Open the search bar by pressing `Ctrl-F` or selecting `Edit > Find`
3. Turn on regular expression search by selecting the `.*` button to the right of the search box.
4. For this exercise, make sure that Match Case (`Aa`) is turned on and Match Whole Word is turned off. (buttons between the search box and the Regular Expression button)
5. Enter your regular expression in the search box. The search occurs immediately.

To use **Regex 101**:

1. Paste your text to be searched into the **Test String** box.
2. For this exercise, set the **Flavor** to Python.
3. Click the **Set Regex Options** flags to the right of the search box and turn on **global** and **multiline**.
2. Enter your regular expression in the **Regular Expression** box.

### How to Complete and Submit the Homework
*Please follow these instructions*

1. Download the [answersheet](HW7-Regex-Answersheet.txt){: download="HW7-Regex-Answersheet.txt"} and load it into a text editor such as VS Code.
2. Compose a Regular Expression that satisfies each question.
3. Paste expressions into the answer sheet.
4. Submit your answer sheet to LearningSuite for grading.


## Scenario 1: Usernames

On your website, you have a page that allows a user to create a username. The username must start with a capital letter and be followed by only lowercase letters. It must end in exactly two numbers and the whole username must be at least 8 characters long. Write a regex pattern that will check this.

Hints:
* The caret `^` indicates that the match must occur at the beginning of a line. Dollar sign, `$` indicates that the match must occur at the end of a line. So `^Snoopy$` matches a line with only the word "Snoopy", nothing before or after.


**Test Set:**

```
These should match:
Aabcde90
Qqqqqq00
Rrrrrrrrrrrrrrr99

These should not match:
ZAabcde00
AAabcd00
ZZZZZZZZ54
aaaaaaaaaa78
Aaaaa10
aA23aa88
```

## Scenario 2: Recognizing Social Security Numbers

You have recently taken over a website for a government agency. The last webmaster was very irresponsible. Users were required to enter their social security numbers, and the last webmaster stored these in a file named info.txt in plain text. Write a regular expression to recognize social security numbers whether or not they include the dashes.

Hints:

* The `\d` expression indicates any digit.
* In a regular expression, parentheses are used to delineate a subexpression.
* The bar `|` is an _alternation_ operator meaning either one thing or another. It can be used with single characters or with subexpressions.
* A number in braces (e.g. `{4}`) indicates the preceding expression must occur exactly that many times. You can also put a range in braces (e.g. `{3-5}`).

**Test Set:**

```
These should match:
123-45-6789
123456789

These should not match:
1234567890
123-456789
123 45 6789
12345-6789
--123456789
```

## Scenario 3: Recognizing email addresses

You have a list of email addresses but many of them are invalid. Write a regular expression that will recognize a properly composed email address. The emails can end in conventional TLDs `.com`, `.org`, `.net` or `.edu` but also newer ones such as `.info` or `.edu.au`.

Compose a regular expression that matches all of the valid addresses and none of the others.

Hints:

* Domain names (after the `@`) are composed of two or more names separated by periods. Names are composed of letters, numbers, and dashes.
* Email prefixes (before the `@`) are composed of letters, numbers, underscores, periods, and dashes. An underscore, period, or dash must be followed by one or more letters or numbers)
* Like character classes, subexpressions (see hints to Scenario 2) may be followed by a quantifier such as `*` for zero or more, `+` for one or more, `?` for zero or one, or braces `{}` to indicate a range of repetitions.
* You can create a custom character class using characters within brackets. Inside brackets, use dashes to indicate character ranges. For example `[A-Za-z]` will recognize all ASCII letters (whether upper or lower case).
* Use a backslash to treat a Regex character as a literal. For example use `\.` to match a period.
* The caret `^` indicates that the match must occur at the beginning of a line. Dollar sign, `$` indicates that the match must occur at the end of a line. So `^Snoopy$` matches a line with only the word "Snoopy", nothing before or after.

**Test Set:**

```
These should match:
hackerman@byu.edu
MARK@test.com
student@student.byu.edu
john.smith12@byu.net
some-special_is.allowed@example.com
some-special_is.very-allowed@example.com
numbers_okay123@example.com
123starting_numbers_okay@example.com
CAPITALS_OKAY@example.com
many_sub_domains_okay@subdomin.sub.s.u.b.example.com
some_special_in_sub_okay@s-u-b.example.com
capitals_in_sub_okay@SUB.example.com
some_special_in_domain_okay@example-1234.com
some_special_in_domain_okay@12.com
capitals_in_domain_okay@EXAMPLE.com
tlds_okay@example.com
tlds_okay@example.org
tlds_okay@example.net
tlds_okay@example.int
tlds_okay@example.edu
tlds_okay@example.gov
tlds_okay@example.mil

These should not match:
.com@net.com
801-555-1234
7854#4126.5
itpro@com
.no_special_at_beginning@example.com
-no_special_at_beginning@example.com
_no_special_at_beginning@example.com
no_other#special@example.com
no_other[special@example.com
no_extra_at@@example.com
no_extra_at@example.com@example.com
bad_sub_domain@.example.com
no_other_special@sub_domain.example.com
no_other_special@sub`domain.example.com
bad_domain@sub..com
bad_domain@.com
no_other_special@example$domain.com
no_other_special@example_domain.com
bad_tld@example.
bad_tld@example.co.
```

## Scenario 4: Data Cleanup

You receive a set of names in a text file. Some are listed as `Firstname Lastname` and others are listed as `Lastname, Firstname` (note the comma). You need them all to be consistent. Write a regular expression and replacement string that will make all entries `Lastname, Firstname`. It should ignore bad data such as text with numbers or more than two names.

* In **Visual Studio Code** open the Replace bar by selecting `Edit > Replace` or pressing `Ctrl-H`.
* In **Regex 101** select `Substitution` under `Function`.

Hints:
* In the replacement string, a dollar sign followed by a digit means insert the corresponding subexpression from the search string. Remember, a subexpression is indicated by parentheses.

**Test Set:**

```
These should be updated:
Harrison Ford
Flash Gordon
Eddie Murphy
Buckaroo Banzai
Owen Wilson
Carrie Fisher
Zooey Deschanel
Sirius Black

These should be left alone (already in the right format) or ignored (bad data):
Crowe, Russel
Kenobi, ObiWan
Robert
McFly, Marty
Skywalker, Luke
Bad Data Line
Three 3
Granger, Hermione
One Two Three Four
Bourne, Jason
Bond, James

```

![image](https://user-images.githubusercontent.com/76703677/147514009-88c9e7cf-52c6-4888-bd3a-85c3a3b3e53a.png)
