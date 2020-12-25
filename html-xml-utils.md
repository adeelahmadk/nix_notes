# Process HTML/XML Files (html-xml-utils)

Nix shell has a set powerful tools to process HTML & XML documents in CLI. These HTML and XML manipulation utilities are included in a package named `html-xml-utils`. You can install this package using your package manager:

```sh
sudo apt install html-xml-utils
```

`html-xml-utils` provides a number of simple utilities for manipulating and converting HTML & XML files in various ways. The suite consists of the following tools:

- `asc2xml`      -  convert from UTF-8 to &#nnn; entities

- `xml2asc`      -  convert from &#nnn; entities to UTF-8

- `hxaddid`      -  add IDs to selected elements

- `hxcite`       -  replace bibliographic references by hyperlinks

- `hxcite-mkbib` -  expand references and create bibliography

- `hxclean`      -  apply heuristics to correct an HTML file

- `hxcopy`       -  copy an HTML file while preserving relative links

- `hxcount`      -  count elements and attributes in HTML or XML files

- `hxextract`    -  extract selected elements

- `hxincl`       -  expand included HTML or XML files

- `hxindex`      -  create an alphabetically sorted index

- `hxmkbib`      -  create bibliography from a template

- `hxmultitoc`   -  create a table of contents for a set of HTML files

- `hxname2id`    -  move some ID= or NAME= from A elements to their parents

- `hxnormalize`  -  pretty-print an HTML file

- `hxnsxml`      -  convert output of hxxmlns back to normal XML

- `hxnum`        -  number section headings in an HTML file

- `hxpipe`       -  convert XML to a format easier to parse with Perl or AWK

- `hxprintlinks` -  number links & add table of URLs at end of an HTML file

- `hxprune`      -  remove marked elements from an HTML file

- `hxref`        -  generate cross-references

- `hxselect`     -  extract elements that match a (CSS) selector

- `hxtoc`        -  insert a table of contents in an HTML file

- `hxuncdata`    -  replace CDATA sections by character entities

- `hxunent`      -  replace HTML predefined character entities to UTF-8

- `hxunpipe`     -  convert output of pipe back to XML format

- `hxunxmlns`    -  replace "global names" by XML Namespace prefixes

- `hxwls`        -  list links in an HTML file

- `hxxmlns`      -  replace XML Namespace prefixes by "global names"

    

#### `hxnormalize`

- Enforce xml tag endings (</>) for single tags:

- Add ending tags to html/xml and print:

```sh
hxnormalize -e test.html
```

#### `hxextract`


#### `hxwls`

Extract URL links from an HTML File:

```sh
hxwls http://www.example.com
```

#### `hxtabletrans`

Transpose a table:

```sh
hxtabletrans table.html > table2.html
```

#### Examples:

```sh
curl -s https://www.dawn.com | hxnormalize -x | hxselect -c -s '\n\n' "div.mb-4 > article.box.story > h2 > a" | tr -s ' '  > topNews

curl -s https://www.expressvpn.com/support/troubleshooting/china-status/ | \
    hxnormalize -x | hxselect -i div#target-linux ul | \
    hxnormalize -e > html_file

curl -s https://www.expressvpn.com/support/troubleshooting/china-status/ | \
    hxnormalize -x | \
    hxselect -c "div#target-android > ol > li:nth-child(2) > a::attr(href)"

curl -s https://www.expressvpn.com/support/troubleshooting/china-status/ | \
    hxnormalize -x | \
    hxselect -c "div#target-android > ol > li:nth-child(2)" | hxwls 

cat html_file | hxwls
```


