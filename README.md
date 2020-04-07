# conv-format

Convert rolling content input between input and output formats. 

Specifically, the input follows some variety of the following format:

```
[optional preamble content]

<header>
content

<header>
content

...

<header>
content

[optional end-of-stream content]
```

The output then shapes as follows:

```
[Optional global header]

<transformed header>
content
<optional footer>>

<transformed header>
content
<optional footer>>

...

<transformed header>
content
<optional footer>>

[Optional global footer]]
```

## Usage:

```
$ ./conv-format -c <config> <input file>
```

Pass the input either as a trailing parameter or as STDIN.

The output directs to STDOUT.

### Examples:

Generate an RSS feed from an HTML page:

```
$ ./conv-format -c html2rss.cfg input.html > rss.xml
```

Convert a [jrnl2](https://github.com/vparnas/jrnl2) journal to a markdown format:

```
$ jrnl2 -j notebook --reverse | conv-format -c jrnl2md.cfg > journal.md
```

Create and append a new entry to a VCARD contact list:

```
echo 'Vitaly Parnas : 123-456-5789 : name@email.com' | conv-format -c vcard.cfg >> contacts.vcf
```

See the example configurations to adapt to individual needs/formats.

### Configuration file

`$INPUT_ITEM_HEADER_REGEX`: 

1. Set to a Perl-compatible regular expression indicating the header (start) of each input entry.
1. Use the Perl numbered or named capture groups to access the respective elements in the `$ITEM_HEADER` variable (below). This is the key in the transformation. See the example configuration files.

`$INPUT_END_REGEX`: Optional, marks the input file end of content. If omitted and additional content exists beyond the last input entry, it will echo in the  output.

`$HEADER`: Optional, the global header to prepend to the output.

`$FOOTER`: Optional, the global footer to append to the output.

`$ITEM_HEADER`: Prepended to each output entry. References the named/numbered capture groups defined in `$INPUT_ITEM_HEADER_REGEX` above. Make sure to escape the respective '$' variable specifiers. See the configuration file examples.

`$ITEM_FOOTER`: If provided, appended to each output entry.
