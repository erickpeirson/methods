Introduction to this Tutorial
=============================

In this tutorial different markups are used to distinguish between
different kinds of information. You will see the following two markups
when this tutorial talks about patterns and what the results for them
could be (don’t worry if you don’t understand these terms yet, they will
make more sense when you read on).

    Pattern: %\patternterm{this is the pattern}%
    Results:
    %\resultcontext{\highlight{this} is a result ... and \highlight{this} too}

In the result line, orange characters are characters that match the
given pattern. Grey characters around the highlights are text around the
found matches.

In general, patterns are printed in red with a typewriter font (). If a
pattern contains a blank (“ ”), this is intentional and part of the
pattern. Sequences of characters that match a given pattern are
highlighted in yellow ().

Text that might be helpful for troubleshooting or general tips about
regular expressions are boxed in.

This tutorial comes with a practice text. You are encouraged to try out
all the examples in this tutorial with this text.

General Introduction to Regular Expressions
===========================================

Regular expressions (regexes) are sequences of characters (so-called
“strings”) describing patterns in a text. You can use them to for
example tell the computer to find certain words or phrases in a text and
replace those with something else. You could for example tell the
computer to find all words starting with a capital letter or ending with
“ing.” Think of regular expressions as a way of telling your computer
what to look out for when going through a text.

Working with regexes is a two-step process. First, you need to define a
pattern the computer should look out for. Second, the computer takes
your pattern (your regular expression) and goes through your text trying
to find sequences of characters that match your pattern. For the first
step all you need is a way of writing down your pattern. This could be
some paper and a pen or a text editor on your computer. For the second
step, you need a piece of software that takes your pattern and the text
you want to find the pattern in and returns all the parts of your text
that match the pattern. Such a piece of software is called a regular
expression engine (or regex engine). Many text editors such as
TextWrangler (for Mac OS) and Notepad++ (for Windows) come with a regex
engine and you can use regular expressions when searching your document.
Most programming language include regular expression engines as well.

Usually the regex syntax differs slightly depending on the regex engine.
This tutorial focuses on the regex engines of TextWrangler and
Notepad++. If you are using a different engine and a regex pattern does
not return the expected results, you might have to adjust your pattern
to comply with the rules of your engine.

How to use TextWrangler’s/Notepad’s Regex Engine?
-------------------------------------------------

TextWrangler uses the same patterns as an application called “grep” (a
Unix command-line utility). This is the reason that in TextWrangler and
the TextWrangler manual you will often see the term “grep” instead of
“regular expressions.” Notepad++ uses “Perl Compatible Regular
Expressions” (PCRE) and in general uses the term “regular expressions.”

Detailed information on the two regex versions can be found using the
following links.

-   TextWranger Manual:\
    <span>http://ash.barebones.com/TextWrangler\_User\_Manual.pdf</span>

-   Description of regex used by Notepad$++$:\
    <span>http://www.boost.org/doc/libs/1\_49\_0/libs/regex/doc/html/boost\_regex/syntax/\
    perl\_syntax.html</span>

To search in TextWrangler or Notepadd++ using regular expressions select
“Search $>$ Find…” from the main menu (see and ).

![Open Find Dialog in TextWrangler<span
data-label="fig:menu"></span>](imgs/menu.png)

![Open Find Dialog in Notepad++<span
data-label="fig:npp-menu"></span>](imgs/npp-search-menu.png)

To turn regular expressions on when searching, make sure that the
checkbox labeled “grep” is checked in the find dialog (see and ). In
addition make sure your search is case sensitive (checkbox “Case
Sensitive”).

![Find Dialog in TextWrangler<span
data-label="fig:searchDlg"></span>](imgs/searchDlg.png)

![Find Dialog in Notepad++<span
data-label="fig:npp-searchDlg"></span>](imgs/npp-find-window.png)

What does a Regular Expression consists of? {#subsec:regex_consist}
-------------------------------------------

So, now that you know how to use a text editor as regular expression
engine, let’s look at what a regular expression actually is. The
simplest regex pattern is a sequence of characters. If you have ever
searched for a term in Microsoft Word, any text editor, or if you have
ever used any other search, you have already used such a regex pattern.
For example, if you search for the word you are searching for any word
that matches a pattern that consists of the characters t-a-b-l-e. The
results for that search will contain all the words containing the word
“table” such , , or .

In a regular expression, every character is a placeholder for a
character or a group of characters. In the simple example above (), is a
placeholder for the character “t”, is a placeholder for the character
“a”, and so on. Every character between “A” and “z” stands for itself.
Note that there is a difference between and . If you search for you will
get different results than when you search for .

What makes regular expression such a powerful tool is that in addition
to the characters A to z, there are characters that do not stand for
themselves. For example, the period character () is a placeholder for
any character whatsoever (except newlines). If your regex pattern is
your regex engine will find any two characters in which the first one is
“a” and the second something else; for example , , , or . Note that the
period character also matches a blank (\`\` \`\`). Therefore would also
find .

With only this one character you can already achieve a lot. Let’s assume
you want to find all the beginnings of quoted phrases. This is easily
done with the following pattern.

    Pattern: %\patternterm{ ".}%
    Results: 
    %\resultcontext{some\resultterm{ ``a}ntihumanist" ... as a\resultterm{ "s}ystem ... (Greek\resultterm{ ''t}heatron'',}%

Diving into Regular Expressions
===============================

In the last section you learned about simple patterns and the period
placeholder that can stand for any character. This section will dive
deeper into regular expressions and how you can build powerful patterns.

Special Characters {#subsec:specchar}
------------------

Let’s go into more detail on what kind of other placeholders there are.
Besides the period there are three more special character \^, \$, and
\\.

What does that mean? This means that if you want to find all the lines
in a text that begin with a certain pattern you start your pattern with
\^. Similar, if you want to find all lines that end with a certain
pattern you end your pattern with \$.

Let’s try for example to find all the lines in a document that start
with “The Humanities”.

    Pattern: %\patternterm{\textasciicircum The Humanities}%
    Results:
    %\resultcontext{\resultterm{The humanities} are academic disciplines ...}%
    %\resultcontext{\resultterm{The Humanities} Indicators ...}%
    %\resultcontext{\resultterm{The Humanities} Indicators, unveiled ...}%

Similarly, to find all the lines that end with “Law”:

    Pattern: %\patternterm{Law\$}%
    Results:
    %\resultcontext{1.4 \resultterm{Law}}%
    %\resultcontext{Main article: \resultterm{Law}}%

The backslash character (\\) has another role. It is not a placeholder
for another character but marks the beginning of a sequence in which the
“regular” characters do not stand for themselves anymore (so-called
“escape sequences”). For example, the escape sequence \\d stands for any
digit character (the numbers 0 to 9); the escape sequence \\s stands for
any whitespace character. A whitespace character is any character that
generals “white space” in the document: space, tab, carriage return,
line feed, and form feed. There are many different escape sequences and
they can differ slightly depending on what regex engine is used. To find
out what sequences there are you should consult the manual of your regex
engine.

The backslash has, however, also another function. You might have asked
yourself already how you would tell the regex engine that your pattern
includes an actual period character or a dollar sign. For that you use
the backslash character. Any character that does not stand for itself in
a regex pattern you can “escape” using the backslash. By escaping a
character it represents itself. For example to find all the words
standing at the end of a sentence and that end with “ing”, you could use
the pattern

    Pattern: %\patternterm{ing\textbackslash.}%
    Results:
    %... and listen\resultterm{ing.} ...%
    %... performance sett\resultterm{ing.} Dance ...%

You would use the same technique for example to include a dollar sign in
your pattern (). Since the backslash itself has a special meaning, you
need to escape it as well if you want to include a backslash in your
pattern ().

There are a few more reserved characters that you need to escape. You
will learn what they do in the following. For now what is important is
that if you want to include them in a pattern, you need to escape them.

Ranges of Characters
--------------------

In the last section, you have already seen escape sequences that
represent a range of characters; for example , which represents the
number 0 to 9. Such ranges of characters are also called “character
classes.” Besides character classes that are predefined by escape
sequences, you can define your own character classes. To create a
character classes you use square brackets. The square brackets contain
the characters that the character class should be made up of. For
example, if you include the character class in your pattern, it will
match the characters “a”, “c”, “e”, or “g”. However, it will only match
one of these characters. For example, the pattern will find and but not
.

You can use character classes in many different ways. You could for
example use it to find all the occurrences of the term “humanities”, no
matter if they start with an upper or lower case “h”.

    Pattern: %\patternterm{$[$Hh$]$umanities}%
    Results:
    %The \resultterm{humanities} are ...%
    %... The \resultterm{humanities} include ...%
    %4.1.1 The \resultterm{Humanities} Indicators ...%

The following list shows a selection of common character classes.

You can also specify a character class by defining which characters it
should not match. To do that you would use the \^character that you also
use to specify the beginning of a line. You place it right after the
opening square bracket: $[$\^…$]$. For example, would match all
characters that are not vowels such as , , , , or .

Let’s use the character classes that exclude characters to find all
words in a text that do not start with an alphabetic character (using
the Latin alphabet) or are quotes.

    Pattern: %\patternterm{ $[$\textasciicircum a-zA-Z''\textbackslash s\textbackslash($][$\textasciicircum\textbackslash s$]$}%
    Results:
    %\ldots during the\resultterm{ 20}th century \ldots%
    %\ldots (Greek "theatron",\resultterm{ $\theta\varepsilon$}$\alpha\tau\rho o\nu$) \ldots%
    %\ldots arts\resultterm{ 'k}ata' are \ldots%

Note that the regex engine also finds . To avoid this you need to
include in the first square brackets of the pattern.

Iterative Pattern Development
-----------------------------

As you can see from the last example, usually you won’t define your
pattern 100% correct. Pattern development is an iterative process with
the following steps:

1.  Develop a pattern.

2.  Run your pattern on the regex engine.

3.  Examine the results and find out what was found that shouldn’t have
    been found and what was not found that should have been found.

4.  Tweak your pattern according to the results in step 3) to improve
    the results.

5.  Start at 2) again until your results match your expectations.

This process can take a while depending on the complexity of your
pattern and it is likely that with a new text your pattern needs some
adjustments again.

Line Breaks
-----------

We’ve already talked a little bit about whitespace characters. Let’s go
into more detail here. You learnt in section [subsec:specchar] that the
escape sequence \\s represents any whitespace character. A whitespace
character is a character that simply creates white space on a page such
as a tab or a simple blank. You might wonder why “tab” is a character as
it is only generating the space of some blanks. Well, a tab is actually
represented by a special character, in plain text that is usually \\t.
Your text editor or word processor knows that this character should be
represented by some blank space.

The same thing is true for new lines. Let’s try the following:

    Text:
    %{\small Bust of Homer, a Greek poet}%
    %{\small The classics, in the Western academic tradition, \ldots}%
    Pattern: %\patternterm{poet The}%
    Results:
    %\resultterm{$[$none$]$}%

If you simply search for , your search will not return any results. The
reason is that a newline is represented by its own character. Or
actually by two characters: line feed and carriage return. A mechanic
device such as a printer has to execute two motions to create a new
line: it has to go down one line on the page (line feed) and then go
back to the beginning of the line (carriage return). In software
programs such as a text editor or a regex engine, these two motions
(represented by two characters) can be handled together and are often
represented by only one character. This character depends on the
operating system you are using. While Unix systems (such as Mac OSX)
usually use line feed, Windows systems usually use carriage return and
then line feed. To be concrete, to match a new line in TextWrangler you
can use either \\r or \\n. In Notepad$++$ you use both of them together:
\\r\\n.

Let’s try the last example again but this time with an encoded line
break.

    Text:
    %{\small Bust of Homer, a Greek poet}%
    %{\small The classics, in the Western academic tradition, \ldots}%
    Pattern (TextWrangler): %\patternterm{poet\textbackslash nThe}%
    Pattern (Nodepad++): %\patternterm{poet\textbackslash r\textbackslash nThe}%
    Results:
    %Bust of Homer, a Greek \resultterm{poet}%
    %\resultterm{The} classics, in the Western academic tradition, \ldots%

If you find it hard to remember that there are characters that you don’t
see but that you need to include in your patterns, you can simply make
your text editor display those characters as well.

In TextWrangler, go to “View -$>$ Text Display” and select “Show
Invisibles” (see ). In Notepad++ go to “View -$>$ Show Symbol -$>$ Show
All characters” (see ). Your editor will now show little gray symbols at
the end of each line, for tabs, and other non-printing (invisible)
characters.

![Show Invisibles in TextWrangler<span
data-label="fig:tw-show-invis"></span>](imgs/showinvis.png)

![Show Invisibles in Notepad++<span
data-label="fig:npp-show-invis"></span>](imgs/np-show-all.png)

Including Line Breaks for Period Matching
-----------------------------------------

In section [subsec:regex~c~onsist] , however, you don’t want to specify
explicitly where a new line might start. To change that behavior there
is an option called “DOTALL” for most regular expression engines. If you
ever use a programming language to run regular expression there will
probably be an option with that name or a similar name that you can turn
on to include newlines in the period.

You can tell TextWrangler to include newlines for the period by putting
in front of you pattern. For example if you want to find every
combination of in which can be anything, even a line break, your pattern
looks like the following.

    Pattern (TextWrangler): %\patternterm{(?s)s.F}%
    Results:
    %Humanitie\resultterm{s}%
    %\resultterm{F}rom Wikipedia, the free encyclopedia%
    %\ldots%

In Notepad++ it is even easier, you simply check the chechbox “. matches
newline”. However, remember that a new line in Notepad++ is matched by
\\r\\n, which means that you need to include two dots (..) to match a
newline.

    Pattern (Notepad++): %\patternterm{s..F}%
    Results:
    %Humanitie\resultterm{s}%
    %\resultterm{F}rom Wikipedia, the free encyclopedia%
    %\ldots%

Using the Or Operator
---------------------

Sometimes, you want to find parts of a text in which either one thing or
another things is written. For example, you might want to find all the
place where “is” or “are” appear. For that you can use the pipe
character ($|$).

    Pattern: %\patternterm{is$|$are}%
    Results:
    %The humanities \resultterm{are} academic \ldots%
    %\ldots academic d\resultterm{is}ciplines that \ldots%
    %\ldots as human\resultterm{is}ts.[2] \ldots%

When you use the pipe, the regex engine will check if either everything
to the left or everything to the right of the pipe matches somewhere in
the text. If you want not everything to the left and right of the pipe
to use in the or-statement, you need to use brackets. Place the brackets
around the part that should be used for the or-statement.

    Pattern: %\patternterm{th(o$|$e)se}%
    Results:
    %\ldots only to \resultterm{those} works \ldots%
    %\ldots in \resultterm{these} arts \ldots%
    %\ldots all of \resultterm{these} areas, \ldots%

Repeating Patterns or Sub-Patterns
----------------------------------

So far, you have learnt how to define a pattern character by character.
You can already do a lot with that, but often this is not enough. Often
you don’t know the exact length of the words or phrases you’re looking
for, or you want to find big chunks of texts with a given beginning or
ending. For that you need to be able to repeat patterns or sub-patterns
(a smaller part of the overall pattern). To define that a pattern is
repeating you need some of the other characters that you need to escape
in order to use them as regular characters (remember: \*, +, and all
kinds of brackets).

Let’s start simple. Assume you want to match all words that have a
structure like this aba or abba or abbba and so on. You could try to
find all the possible combinations one after the other but if you don’t
know what the longest “b” sequence is you’re out of luck. Regular
expressions give you a very simple way of expressing that a pattern is
there an unknown number of times: the plus sign (+) and the asterisk
character (\*).

In the above mentioned example, you could match all these words with one
pattern: . This pattern says that the sequence you are trying to find
starts with an “a” followed by a “b”. The “b” can appear multiple times
but at least once until there is another “a”. This pattern would match
words such as , , , or . However, it would not match or or . If you want
to include as well in your results, you can use \* instead of +: .

You can also define an exact number of times or a range of times by
using curly brackets. For example, would match only . would match , ,
and . To specify a range you first give the minimum number a pattern has
to occur and then the maximum in curly brackets:
`pattern{minimum, maximum}`. To specify that a pattern should occur at
least a given number of times, you can simply not give a maximum (but
still add the comma): `pattern{minimum, }`.

Let’s consider the following example: you want to find all the sentences
that start with “The humanities” (). First, you would start your pattern
with “The humanities”. Since it doesn’t matter what follows after (as
long as it is not a period), the next part of the pattern would be a
period (.). Because you don’t know how long a sentence is, there can be
any number of characters but at least one (let’s assume you don’t want
to find “The Humanities.”), so you append a plus sign (+). You end the
pattern with the escaped period sign that marks the end of a sentence
(\\.).

    Pattern: %\patternterm{The humanities.+\textbackslash.}%
    Results:
    %\resultterm{The humanities are academic disciplines that study human culture ... cultural }%
    %\resultterm{studies, law and linguistics.}%
    %\resultterm{Through the humanities we reflect \ldots and reason.}%

At this point it is important to remember that the period (.) does not
match a new line. So if you want to define a repeating pattern that
contains any character and might go over several lines, you explicitly
need to state that (by for example including \\r or \\n in your
pattern).

The box below shows some examples of how you could use the plus or
asterisk character in patterns.

If there is a part of pattern that you want to repeat (a sub-pattern)
then you enclose that part in brackets and append the repeat character:
(subpattern)+restpattern. The example below shows how you could find a
list of words in text concatenated with “and”.

    Pattern: %\patternterm{($[a-z]$+, )+and $[a-z]$+}%
    Results:
    %\ldots and modern \resultterm{languages, literature, philosophy, religion, and visual} and \ldots%
    %\ldots English literature, global \resultterm{studies, and art}.%
    %\ldots record of \resultterm{humans, societies, institutions, and any} topic \ldots%

Greedy and Non-Greedy Pattern Matching
--------------------------------------

A very important concept to understand when working with repeating
patterns is greedy and non-greedy repeats. By default your pattern is
greedy, which means that the regex engine will try to find the longest
possible match. For example, consider the text “I went down the street
with my dog and my dog barked.” If you pattern is , the regex engine
will find everything until the second “my dog” because that is the
longest possible match for your pattern: . If you want to find the
shortest possible match (in the example that would be “I went down the
street with my dog”) then you need to make the repeat character
non-greedy. You do that by appending a question mark (?) to the repeat
character: . This pattern would return only the text until the first “my
dog”: .

Search and Replace with Regex
=============================

In the last chapter, you have learnt how to write regular expressions to
find certain patterns in your text. Often you are looking for those
patterns because you would like to change your text. For example, in the
example text that comes with this tutorial, you might want to remove all
the square brackets that either contain “edit” or a reference. Most text
editors offer a search and replace functionality for that purpose (see
and ). The basic idea is that whatever you found in your text, you
replace it with something else.

If you have a static text that you want to use to replace the search
results that is still easy: you simply put the text in replace field and
hit “replace”.

![TextWrangler - Search and Replace<span
data-label="fig:tw-replace"></span>](imgs/replace.png)

![Notepad++ - Search and Replace<span
data-label="fig:npp-replace"></span>](imgs/npp-replace-all.png)

But what if the text you want to replace the search result with depends
on the search result? Let’s assume for example the case that you simply
want to append text to the search results; instead of simply having the
reference number (e.g. $[3]$) you want your references to look like
this: $[$3. reference$]$. This can be done using groups.

You can create a group by simply putting brackets around any part of
your regex pattern. For example, finds all your references (square
brackets with numbers in it). You can now create two groups: first, you
group the opening bracket with the following number: ; and then you the
closing bracket: . Your new pattern will now look like this: . In your
replace field you can now use the groups you specified in your regex
pattern. The text editor will take the text that was captured by a given
group and put it at the specified location. You reference a group in
your pattern by its number: the first group is `\1`, the second `\2`,
and so on. Let’s look at how such a replacement could look like.

    Pattern without groups: %\patternterm{\textbackslash$[$\textbackslash d+\textbackslash$]$}%
    Pattern with groups: %\patternterm{(\textbackslash$[$\textbackslash d+)(\textbackslash$]$)}%
    Replacement: %\patternterm{\textbackslash 1\textbackslash. reference\textbackslash 2}%
    Results:
        %Found: \ldots sciences.\resultterm{$[$1$]$} The \ldots %
        %Replaced: \ldots sciences.\resultterm{$[$1. reference$]$} The \ldots %

Note: Anything that is surrounded by brackets counts as a group. If you
use or-statements in your pattern with brackets, those brackets count as
groups too. For example, the pattern has two groups. The first group
contains the matched word, e.g. , while the second group contains “o” or
“e”, in the example .
