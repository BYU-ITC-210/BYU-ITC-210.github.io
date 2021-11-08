---
title: Regular Expressions Walkthrough
---

Known as "REs", "regexes", or "regex patterns", Regular Expressions are a standard language for matching text patterns. They are used in many programming languages including Python, JavaScript, PHP, Perl, C#, and more. They are also built into text editors like Visual Studio and into command-line utilities like [Grep](https://en.wikipedia.org/wiki/Grep), [Sed](https://en.wikipedia.org/wiki/Sed), and [Awk](https://en.wikipedia.org/wiki/AWK).

In this walkthrough, we will use Regular Expressions in [Visual Studio Code](https://code.visualstudio.com/) to find and update text.

## Resources

* [Visual Studio Code](https://code.visualstudio.com/)
* [Microsoft Regular Expression Quick Reference](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)
* [Regular Expressions Cheat Sheet](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)

## Getting Started

* Download [SampleText.txt](Regex-SampleText.txt){: download="SampleText.txt"}
* Open the file in [Visual Studio Code](https://code.visualstudio.com/)
* Open the search bar (`Ctrl+F` or `Edit > Find`)
* Turn on "Use Regular Expression" by clicking the `.*` button on the search bar or by pressing `Alt+R`
* Turn on "Match Case" by clicking the `Aa` button on the search bar or by pressing `Alt+C`

## Character Classes and Anchors

The following are basic character classes, anchors and such.

<table class="layout">
<tr><td><code>.</code></td><td>Any single character (except newline in some cases)</td></tr>
<tr><td><code>\w</code></td><td>Word character: any letter, digit, or underscore (\W is the opposite)</td></tr>
<tr><td><code>\b</code></td><td>Boundary between word and non-word</td></tr>
<tr><td><code>\d</code></td><td>Decimal digit</td></tr>
<tr><td><code>\s</code></td><td>Whitespace (space, tab)</td></tr>
<tr><td><code>\n</code></td><td>Newline</td></tr>
<tr><td><code>[aeiou]</code>&nbsp;</td><td>Custom character set. This example matches any lower-case English vowel.</td></tr>
<tr><td><code>[a-z]</code></td><td>Custom character range. This example matches any lower-case Roman letter.</td></tr>
<tr><td><code>[^q]</code></td><td>Negative character set. This example matches anything but the letter 'q'.</td></tr>
<tr><td><code>^</code></td><td>Beginning of a line</td></tr>
<tr><td><code>$</code></td><td>End of a line</td></tr>
<tr><td><code>\</code></td><td>Escape (treat a special character as a literal)</td></tr>
<tr><td><code>|</code></td><td>Or, may match one thing or another</td></tr>
<tr><td>&nbsp;</td><td></td></tr>
</table>

**Try the following expressions in the Visual Stdio Code search box:**

<table>
<tr><th>Expression</th><th>Matches</th></tr>
<tr><td>a</td><td>All occurrences of the letter 'a'.</td></tr>
<tr><td>ab</td><td>All occurrences of the letter 'a' followed by 'b'</td></tr>
<tr><td>^a</td><td>The letter 'a' at the beginning of a line.</td></tr>
<tr><td>b$</td><td>The letter 'b' at the end of a line.</td></tr>
<tr><td>[0-9ijk]</td><td>Any digit or the letters 'i', 'j', and 'k'.</td></tr>
</table>

## Repetition

The following patterns let you match a specific number of repetitions.

<table class="layout">
<tr><td><code>*</code></td><td>Zero or more occurrences of the preceding pattern.</td></tr>
<tr><td><code>+</code></td><td>One or more occurrences of the preceding pattern.</td></tr>
<tr><td><code>?</code></td><td>Zero or one occurrences of the preceding pattern.</td></tr>
<tr><td><code>{6}</code></td><td>Exactly six occurrences of the preceding pattern.</td></tr>
<tr><td><code>{2,5}</code>&nbsp;</td><td>Between 2 and 5 occurrences of the preceding pattern.</td></tr>
<tr><td><code>()</code></td><td>Parentheses encompass a pattern of more than one symbol.</td></tr>
<tr><td><code>?</code></td><td>Makes the preceding repetition non-greedy (see below).</td></tr>
<tr><td>&nbsp;</td><td></td></tr>
</table>

**Try the following expressions in the Visual Studio Code search box:**

<table>
<tr><th>Expression</th><th>Matches</th></tr>
<tr><td>a+</td><td>Any series of the letter 'a'</td></tr>
<tr><td>(ab)+</td><td>Any series of the letters 'ab' repeating at least once</td></tr>
<tr><td>r.*m</td><td>Any sequence of characters, on a single line, that starts with an 'r' and ends with an 'm'</td></tr>
</table>

By default, repetitions are "greedy". That is, they match as many repetitions as possible. Following the repetition with a `?` causes it to be non-greedy. That is, match as few repetitions as possible.

**Try this variation on the last pattern from the previous table:**

<table>
<tr><td>r.*?m</td><td>How are the matches different from before?</td></tr>
</table>

## Putting things together

**Try these patterns and make up your own:**

<table>
<tr><th>Expression</th><th>Matches</th></tr>
<tr><td>[\w.-]+@[\w.-]+</td><td>Email addresses (but may also match other stuff).</td></tr>
<tr><td>\d[A-Z]{3}\d{3}</td><td>California license plates (one digit, three letters, three digits)</td></tr>
<tr><td>^[A-Z][a-z]+ [A-Z][a-z]+$</td><td>Most two-word capitalized names.</td></tr>
<tr><td>^[A-Z][a-z]+ [A-Z]\. [A-Z][a-z]+$</td><td>Most general authority names.</td></tr>
<tr><td>(Fred)|(George)</td><td>Fred or George.</td></tr>
</table>

The general authority names pattern didn't match names that start with an initial and then the middle name. How can you combine two patterns with the `|` operator to match both name styles?

## Lookahead

Lookahead operators let you specify content that _must_ or _must not_ immediately follow a pattern.

<table class="layout">
<tr><td><code>(?=Joe)</code>&nbsp;</td><td>The word, 'Joe' must immediately follow the pattern but it's not included in the match.</td></tr>
<tr><td><code>(?!Joe)</code></td><td>The word, 'Joe' must NOT immediately follow the pattern.</td></tr>
<tr><td>&nbsp;</td><td></td></tr>
</table>

<table>
<tr><th>Expression</th><th>Matches</th></tr>
<tr><td>Isaac(?= Asimov)</td><td>Isaac if it is immediately followed by Asimov.</td></tr>
<tr><td>Isaac(?! Asimov)</td><td>Isaac if it is NOT immediately followed by Asimov (e.g. Isaac Newton)</td></tr>
</table>

## Replacement

Patterns in parentheses are a 'Group' which may be referenced in the replacement text. In the replacement text, a dollar sign followed by a digit indicates the value of a group should be substituted. For example, `$1` references the first group in the match.

In Visual Studio Code, press `Ctrl+H` or `File > Replace` to open the replace bar. Make sure regular expressions are turned on.

<table>
<tr><th>Expression</th><th>Replacement</th><th>Effect</th></tr>
<tr><td>^([A-Z][a-z]+) ([A-Z][a-z]+)$</td><td>$2 $1</td><td>Swaps first and last names.</td></tr>
<tr><td>([\w.-]+)@[\w.-]+</td><td>$1@gmail.com</td><td>Makes all email addresses be at gmail.com.</td></tr>
</table>
