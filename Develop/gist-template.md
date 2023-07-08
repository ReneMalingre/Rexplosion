# Regex Tutorial - Email Address Validation

Email addresses are ubiquitous and vital to our modern life. As well as being used for communication from person to person or business to person, they are used for registering for services, logging into websites, two-factor authentication and resetting passwords, and more.

Being able to validate email addresses in data entry contexts is important, for both the user and the service provider. Users and service providers need to be assured that the email address the user enters is valid, so that services and communications can occur.

Using regular expressions (regex) to validate email addresses provides a quick and efficient means of checking whether the format of an email address adheres to a specified pattern. This enhances the user experience, helping to identify and correct common typos or formatting mistakes. Leveraging regex for client-side validation streamlines the process by decreasing server load and speeding validation. While regex is a powerful tool for pattern matching, it does not verify the actual existence or accessibility of an email account, and it cannot account for the complexity of the full email address standard.

This Gist will explain how to use regular expressions to validate standard email addresses.

## Table of Contents

- [Background](#background)
- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Background

### What is a Valid Email Address?

The international standard for an email address is set by the Internet Engineering Task Force (IETF) in RFC 5322.

An email address consists of three parts:

- Local Part: the local part identifies the individual mailbox, or user, of a particular mail server
- @ Symbol: this separates the Local Part from the Domain Part.

- Domain Part: the domain part identifies the mail server that hosts the user's mailbox, which could represent a website, an organisation, a company, a government department, an educational institute or an email service provider, for example

The Domain Part can be made up of two or more parts, separated by a dot (.), which are called labels.

- The first part is typically the Domain Name, representing the specific organisation, business, or institution, etc

- The second part is typically the Top Level Domain (TLD), which represents the type of organisation, business, or institution, etc

- The third part is typically the Country Code Top Level Domain (ccTLD), which represents the country where the organisation, business, or institution, etc is located.

For example, the email address `rene.malingre@gmail.com` consists of two parts plus the separator: the local part `rene.malingre` and the domain part `gmail.com`. The domain part consists of two parts: the domain name `gmail` and the top level domain `com`.

However, subdomains can also be used, such as `rene.malingre@bootcamp.example.com.au`, where bootcamp is a subdomain.

The IEFT RFC allows for a wide range of characters to be used in the local part. These can include letters, numbers, special characters and spaces. The domain part is more restricted, and can only contain letters, numbers and hyphens.

They allow quoted strings in the local part, comments enclosed in parentheses, domain literals, non-ASCII characters, and even nested comments. The local part of an email address is case sensitive, while the domain part is not.

This is a valid email address according to the IEFT RFC, although it should be encoded in UTF-8 for transmission:

``` bash
"(这是评论)"."Weird, but valid!é"(another comment)@bootcamp.[127.0.0.1].com.au
```

Because of the complexity of standards-compliant email addresses, it is difficult to validate them fully. Many email servers limit the characters that can be used in the local part, and some do not allow quoted strings, comments, or non-ASCII characters, and are case insensitive. This means that the majority of email addresses in use today conform to these server requirements and can be validated using a much simpler regular expression.

## Summary

This tutorial will explain how to use regular expressions to validate simple, typical email addresses, of the form `localpart@domainpart.tld.ccTLD`, where the local part consists of letters, numbers, special characters and spaces, and the domain part consists of letters, numbers and hyphens.

The regular expression used in this tutorial to validate email addresses is:

```regex
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/i
```

## Regex Components

Regular expressions, or regex, is a sequence of characters defining a search pattern within a string. They are used to find and match patterns in text. They are used in programming languages, text editors, databases and other software.

Common components of regular expressions include:

### Anchors

`^` and `$` are anchors, and match the start and end of a string, respectively.

### Quantifiers

Quantifiers specify how many instances of a character, group, or character class must be present in the input for a match to be found.

`+` matches one or more of the preceding token.
`*` matches zero or more of the preceding token.
`?` matches zero or one of the preceding token.
`{n}` matches exactly n of the preceding token.
`{n,}` matches n or more of the preceding token.
`{n,m}` matches at least n and at most m of the preceding token.

### OR Operator

`|` is the OR operator, and matches either the expression before or after the operator.

### Character Classes

`[]` is a character class, and matches any character within the brackets. For example, `[abc]` matches `a`, `b` or `c`.

### Flags

Flags alter the overall behaviour of the regular expression. for example, the `i` flag makes the regex case insensitive, `g` makes it global, and `m` makes it multiline.

### Grouping and Capturing

 `()` Parentheses () are used to group part of a regex together. This allows you to apply quantifiers to the entire group or to capture the group for backreferences.

### Bracket Expressions

A bracket expression is also known as a character class. It is used to match exactly one character from a set of characters specified within it. For example, `[a-z]` matches any lowercase letter from a to z, `[abc]` matches any character a, b or c, and `[0-9]` matches any digit from 0 to 9. If the first character in the bracket expression is a caret (^), it matches any character not in the set. For example, `[^a-z]` matches any character that is not a lowercase letter from a to z.

### Greedy and Lazy Match

"Greedy" and "lazy" are terms that refer to how much text a quantifier will try to match. The addition of `?` after a quantifier convert a quantifier from a greedy match to a lazy match. A greedy quantifier will try to match as much text as possible, while a lazy quantifier will try to match as little text as possible. For example, `q+` will match all of the q's in the string `qqqq`, while the regex `q+?` will match only the first q.

### Boundaries

A boundary refers to the position where one character meets another. For example, the space between two words is a boundary. `\b` matches a word boundary, and `\B` matches a non-word boundary. String boundaries are the caret `^` and the dollar sign `$`, representing the position before the first character in a string and the position after the last character in a string, respectively.

### Back-references

 A back-reference is a reference to a previously captured group surrounded by parentheses `()`. \n, where n is a positive integer, is a back-reference to a previously captured group. The groups are numbered from left to right, with the leftmost group being group 1

 They are used to find repeated words, or to replace parts of a string. For example, the regex `(\w+)\s+\1` matches any repeated word followed by a space.

### Look-ahead and Look-behind

A look-ahead matches a pattern only if it is followed by another pattern. A look-behind matches a pattern only if it is preceded by another pattern. It can be negative `(?=...)` or positive `(?!...)`; for example `X(?=Y)` matches X only if X is followed by Y.

A look-behind can be negative `(?<=...)` or positive `(?<!...)`; for example `(?<=Y)X` matches X only if X is preceded by Y.

## Author

The author of this tutorial is [Rene Malingre](https://github.com/ReneMalingre). Rene is a self-taught hobbyist coder since 1984.

René's first computer was a Tandy/Radio Shack Color Computer 1 with 4K of RAM, and he learned to program in BASIC. Skip forward to the 90's and he programmed in Visual Basic 3 to 6, and then VB.NET, and then on to C#.

He is a self-employed optometrist, and writes his own practice management and clinical records software, and integrations with Xero and other services. He is currently enrolled in University of Adelaide's Coding Bootcamp to learn more about modern coding paradigms and theory, and full-stack web development to modernise his technology stack. He's learned more in the last six months about programming than in the last 10 years.
