
## Filter
Filter is a program that transforms a byte stream,
on unix-like system, filters are commands that:
- read bytes from their standard input or specified files
- perform useful transformation on the stream
- write the transformed bytes to their standard output
- most filter work on txt, UTF-8 or just ASCII
- most filters are line-based, few are byte based or character-based

### Using filters
Shell I/O redirections can be used to specify filter source and destination file
```shell
filter < input.txt < output.txt
< input.txt filter > output.txt
< input.txt > output.txt fitler
```
![[Pasted image 20250212205302.png]]

Alternatively, most filters allow input files to be specified as arguments
```
filter input1.txt input2.txt input3.txt > output.txt
```
![[Pasted image 20250212205344.png]]

In isolation, filters are reasonably useful
In combination, they provide a very powerful problem-solving toolkit
Filters are normally used in combination via a pipeline.
`filter1 | filter2 | .... | filterN`

**Note**: similar style of problem-solving and function composition

Unix filters use common conventions for command line arguments
- input can be specified by a list of file names
- if no files are mentioned, the filter reads from standard input which may have been re-directed from a file
- the filename - corresponds to standard input
```
# read from the file data1 
filter data1
# or
filter < data1
# read from the files data1 data2 data3
filter < data1 data2 data3
# read from data1, then stdin, then data2
filter data1 - data2
```
If a filter doesn't cope with named sources, you can use `cat` at the start of the pipeline

## Filters: Option

Filters normally perform multiple variations on a task
Selection of the variation is accomplished via command-line options
- options are introduced by a - ("minus"/ "dash")
- options have a 'short' form, - followed by a single letter (e.g. -v)
- options have a 'long' form, -- followed by a word (e.g. --verbose)
- short form options can usually be combined (e.g. -av vs -a -v)
- --help (-h sometimes -?) often gives a list of all command-line options

most filters have many options for controlling their behaviour.
Unix manual entries describe how each option works, to find what filters are available: `apropos` keyword
`RTFM`

## cat
cat command copies its input to output unchanged (identity filter)
cat - given filenames, concatenates them onto `stdout`
cat - given no filenames, copies `stdin` to `stdout` unchanged
```c
$ cat hello.c
#include <stdio.h>

int main(void) {
	printf("Hello\n");
}

$ cat < hello.c
# include <stdio.h>

int main(void) {
	print("Hello\n");
}
```

**Useful `cat` options**

`-n` number output lines (starting from 1)
`-A` display non-printing characters - handy for debugging (*not available on mac*)
`-s` squeeze consecutive blank lines into single blank line

`tac` command - reverses the order of lines
`rev` command  reverses the order of characters in lines

### `cat` implemented in `C` - passing bytes from input to stdout
```C
// write bytes of stream to stdout
void process_stream(FILE *stream) {
	int byte;
	while ((byte = fgetc(stream)) != EOF) {
		if (fputc(byte, stdout) == EOF) {
			perror("cat:");
			exit(1);
		}
	}
}
```

### cat: implemented in C - where does input come from
```c
// process files given as arguments
// if no argument process stdin
int main(int argc, char *argv[]) {
	if (argc == 1) {
		process_strea(stdin);
	} else {
		for (int i = 1; i < argc; i++) {
			FILE *in = fopen(argv[i], "r");
			if (in == NULL) {
				fprintf(stderr, "%s: %s: ", argv[0], argv[i]);
				perror("");
				return 1;
			}
			process_stream(in);
			fclose(in);
		}
	}

	return 0;
}
```

### cat: implement in python
```python
def process_stream(stream):
	"""
	copy bytes of f to stdout
	"""
	for line in stream:
		print(line, end="")

def main():
	"""
	process files given as arguments, if no arguments process stdin
	"""
	if not sys.argv[1]:
		process_stream(sys.stdin)
	else:
		for pathname in sys.argv[1]:
			with open(pathname, "r") as f:
				process_stream(f)
```
Unlike C, line-based and handles only text (UTF-8)

## common task - filter particular lines
very commonly we need a filter which passes only particular lines
often we want line number added to line
useful if filename add (for when input is coming from multiple lines)
one common need: select lines containing string(s)

### selecting lines containing a string - C
```C
// print lines containing the specified substring
void process_stream(FILE *stream, char *name, char *substring) {
	char *line == NULL;
	size_t line_size = 0;
	int line_number = 1;
	while (getline(&line, &line_size, stream) > 0) {
		if (strstr(line, substring) != NULL) {
			printf("%s:%d:%s", name, line_number, line);		
		}
		line_number++;
	}
	free(line);
}

int main(int argc, char *argv[]) {
	if (argc == 2) {
		process_stream(stdin, "<stdin>", argv[1]);
	} else {
		for (int i = 2; i < argc; i++) {
			FILE *in = fopen(argv[i], "r");
			if (in == NULL) {
				fprintf(stderr, "%s: %s: ", argv[0], argv[i]);
				perror("");
				return 1;
			}
			process_stream(in, argv[i], argv[1]);
			fclose(in);
		}
	}
}
```

### selecting lines containing a string - Python
```python
def process_stream(f, name, substring):
	"""
	print lines containing substring
	"""
	for (line_number, line) in enumerate(f, start=1):
		if substring in line:
			print(f'{name}: {line_number}:{line}', end='')

def main():
	"""
	process files given as arguments, if no arguments process stdin
	"""
	if len(sys.argv) == 2:
		process_stream(sys.stdin, "<stdin>", sys.argv[1])
	elif len(sys.argv) > 2:
		for pathname in sys.argv[2:]:
			with open(pathname, 'r') as f:
				process_stream(f, pathname, sys.argv[1])
```

## Matching Any of A Set of String
Previous programs too limited for many uses, often need to select lines containing any set of string, set may be huge/infinite.
Regular expressions - concise powerful notation for sets of string

## Regular Expressions
Regular expression (regex) often though of as a pattern but think of it as defining a set of strings.
regex libraries available for most languages, many tools use regex for searching.
POSIX standard(s) for regular expressions

### Regular Expressions Basics
Unless a character has a special meaning it matches itself, e.g. `a` has no special meaning so it matches `a`
`p*` denotes zero or more repetitions of `p`, e.g. `b*` matches the empty string and; `b, bb, bbb, bbbb, ....`. Note this is an infinite set of strings.
`pattern1 | pattern2` denotes the union of `pattern1` and `pattern2`, e.g. `perl | python | ruby` matches any of: `perl, python` or `ruby`, `|` is sometimes called alternation.
Parentheses are used for groupings, e.g. `c(,c)*` matches: `c c,c c,c,c c,c,c,c .....`, and `(d | e) * (f | g)` matches `f,g df, ef, eg, ddf, deg, edf, edg, eef, ...`
backslash `\` removes any special meaning of the following character, e.g. `\*` matches an `*` instead of indicating repetition.
Any regular expression can be written using only `()*|\`, but many syntax features are present for convenience & clarity.

### Convenient Regular Expressions for matching Single Characters
`.` (dot) matches any single character.
`[ ]` Square brackets provide convenient matching of any *one* of a set of characters
`[ lisOfCharacters] ` matches any *single* character from `listOfNumbers`, e.g. `[ aeiouAEIOU ]` matches any english vowel
a shorthand is available for ranges of characters `[ first - last ]` 
square brackets matching can be inverted with an `^`
`[ ^listOfCharacters ]` matches any *single* character except those in `listOfCharacters`, e.g. `[ ^ a-e ]` matches any character except one of the first 5 lowercase letters
Other characters lose their special meaning inside bracket expressions, e.g. `[ ^ X] * X` matches any characters up to and including the first `X`

### Anchoring Matches
Regular expressions may be used to match against a whole string, e.g. `re.fullmatch` in `python`
regex is often used to match a substring
	e.g. `grep` prints lines containing a substring matching the regex
	`re.search` in python (`re.match` matches only at start of string)
when matching of a string you can limit matches to the start or end of a string (or both)
start of the string is denoted by `^` (uparrow)
	`^hello` matches a string starting with `hello`
	`^[abc]` matches a or b or c at the start of a string
	`[^abc]` matches any character except a or b or c (anywhere in the string)
the end of the string is denoted by `$`
	`cat$` matches `cat` at the end of a string
	`^cat.*dog$` matches any string starting `cat` and finishing `dog`


**More special characters denoting repetition**
`p*` denotes zero or more repetitions of `p`
`p+` denotes one or more repetitions of `p`
	e.g. `[0-9]+` matches any sequence of digits .i.e. matches integers)
	e.g. `[-'a-zA-Z]+` matches any sequence of letters/hyphens/apostrophes
		this pattern could be used to match words in a piece of english text, .e.g., it's John,....

`p?` denotes zero or one occurrence of `p`
`p{n}` denotes $n$ repetitions of $p$, e.g. `z[0-9]{7}` matches a UNSW zid
`p{n,m}` denotes $n$ to $m$ repetitions of $p$
`p{n,}` denotes $n$ or more repetitions of $p$
`p{,m}` denotes $m$ or less repetitions of $p$

## grep - select lines matching a pattern
`grep` copies stdout lines that match a specified regular expression, some regex chars also special meaning to Shell, when run from Shell regular expression often needs single quotes
grep stands for Globally search with Regular Expressions and Print

Grep options:
`-E` - extended regular expression syntax
`-i` ignore upper/lower-case difference in matching
`-v` only display lines that *do not* match the pattern
`-c` print a count of matching lines
`-w` only match pattern if it makes a complete word
`-x` only match pattern if it makes a complete line

`grep -F` match strings only (no regex) - faster, avoids bugs from regex syntax accidentally occurring your match string
`grep -G` or `grep` matches a subset of regex, e.g. no `+ ? | () {}`, faster than `-E`, but generally just use `-E`
`grep -E` (extended grep) matches full POSIX regular expressions, `-E` is what you want most of the time.
`grep -P` POSIX regex + perl extensions
- standard python regex include some but not all Perl extensions
- use if you need Perl/Python regex extensions
- PCRE library widely use (e.g. Apache)

## wc: word counter
`wc` summarises its input as a single line, often useful as last command in pipeline, also useful in shell scripts, other filters may have counting options, e.g. `grep -c`,

`wc` options:
`-c` print the number of characters
`-w` print the number of words (non-white space) only
`-l` print the number of lines only

by default, `wc` prints the number of line, words, characters, in its input, e.g.
```
$ wc /etc/passwd
   49    79  2793    /etc/passwd
```

### WC in C
```C
int n_lines = 0;
int n_words = 0;
int n_chars = 0;
int in_word = 0;
int c;

while ((c = fgetc(in)) != EOF) {
	n_chars++;
	if (c == '\n') {
		n_lines++;
	}
	if (isspace(c)) {
		in_word = 0;
	} elseif (!in_word) {
		in_word = 1;
		n_words++;
	}
}
printf("%d %d %d %s\n", n_lines, n_words, n_chars, name);
```

### WC in Python
```python
def process_stream(stream):
	"""
	count lines, words, chars in stream
	"""
	lines = 0
	words = 0
	chars = 0
	for line in stream:
		lines += line.endswith(os.linesep)
		words += len(line.split())
		chars += len(line)
	print(f"{lines:>6} {words:>6} {chars:>6}", end="")
```


## tr: transliterate characters
`tr` reads chars and writes characters, mapping (replacing) some chars with others, the mapping is specified as 2 args: `tr sourceChars destChars**`
each char in `sourceChars` is mapping to the corresponding char in `destChars`, e.g.
`tr 'abc' '123' < someText`
$sourceChars = 'abc', destChars = '123': a->1 b->2 c -> 3$
`tr` doesn't accept file names on the command line - uses `stdin` only, and it is not line-based it works with individual chars
most `tr` implementations do not support multi-char characters (UTF-8), they really work with bytes not characters


chars that are not in `sourceChars` are copied unchanged to output, if there is no corresponding char (i.e. `destChars` is shorter than `sourceChars`), then the last char in `destChars` is used.
Shorthands are available for specifying char lists:
e.g. 'a-z' is equivalent to 'abcdefghijklmnopqrstuvwxyz'
note: newlines can be modified if the mapping specification requires it.

`tr` options
`-c` - map all bytes not occurring in `sourceChars` (complement)
`-s` - squeeze adjacent repeated chars out (only copy the first)
`-d` - delete all chars in `sourceChars` (no `destChars`)

```
# map all upper-case letters to lower-case equivalents
tr 'A-Z' 'a-z' < text

# naive encrption (a->b, b->c, ...z->a)
tr 'a-zA-Z' 'b-zaB-ZA' < text

# remove all digits from input
tr -d '0-9' < text

# break text file into individual words/ one per line
tr -cs 'a-zA-Z0-9' '\n' <text
```

