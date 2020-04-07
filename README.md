# conv-format

Convert rolling content input between input and output formats. 

Specifically, the input follows in some variety of the following format:

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

Pass the input either as a trailing parameter or as standard input.

The output directs to standard output.

### Examples:

Generate an RSS feed from an HTML page:

```
$ ./conv-format -c html2rss.cfg input.html > rss.xml
```

Convert a [jrnl2](https://github.com/vparnas/jrnl2) journal to a markdown journal:

```
$ jrnl2 -j notebook --reverse | conv-format -c jrnl2md.cfg > journal.md
```

Convert a [jrnl2](https://github.com/vparnas/jrnl2) journal to a markdown journal:

```
$ jrnl2 -j notebook --reverse | conv-format -c jrnl2md.cfg > journal.md
```

Create and append a new vcard contact entry to a list of contacts:

```
echo 'Vitaly Parnas : 123-456-5789 : name@email.com' | conv-format -c vcard.cfg >> contacts.vcf
```

See the example config files for detail and to adapt to individual input/output format.

### Configuration file

`$INPUT_ITEM_HEADER_REGEX`: 

1. Set to a Perl-compatible regular expression indicating the header (start) of each input entry.
1. Use the Perl numbered or named capture groups to access the respective elements in the `$ITEM_HEADER` variable (below). This is the key in the transformation. See the example configuration files for examples.

`$INPUT_END_REGEX`: Optional, marks the input file end of content. If additional content exists beyond the last input entry and this is omitted, it will echo in the last output entry.

`$HEADER`: Optional, the global header to prepend to the output.

`$FOOTER`: Optional, the global footer to append to the output.

`$ITEM_HEADER`: Prepended to each output entry. References the named/numbered capture groups defined in `$INPUT_ITEM_HEADER_REGEX` above. Make sure to escape the respective '$' characters. See the configuration file examples.

`$ITEM_FOOTER`: If provided, appended to each output entry.
