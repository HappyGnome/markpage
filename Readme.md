# markpage

## Description
A LaTeX style package for annotating pdfs, either in-page or in an additional
margin.

## Installation
Add **markpage.sty** to your texmf directory and update the file list for your
tex distribution.

Requires packages:
  + pdfpages  
  + tikz
  + varwidth
  + environ  
  + keyval, and  
  + pifont*

\*see `noshortcuts` package option.
## Examples
See  example.tex and template.tex

## Usage reference
### Input files
When adding markpage to a latex file the prefix of the pdf file to annotate
must be given. If you just want to anotate single pdf this is the path to the
pdf without `.pdf`. For example, if `test.pdf` is in the same folder as your tex
file, you can pass `source=test` when importing markpage.

You can also colate multiple pdf files with names `<prefix>_<n>.pdf`. Where
`<n>=1,2,...`. For example to create one document with annotated pages of
`test_1.pdf` followed by annotated pages of `test_2.pdf` etc. pass `source=test`
when importing markpage. To annotate just `test_1.pdf` use instead
`source=test_1`. Note that if `test_1.pdf` is present then passing `source=test`
will also include `test_<n>.pdf` `<n>=1,2,...`

### Adding markpage
Use `\usepackage[source = <file>]{markpage}` where
 `<file>` is the prefix of a pdf or family of pdfs to annotate. See above.

#### Other package options:
+ `grid` : if present, a grid will be drawn over the pdf with row/column labels
so that you can position your annotations
+ `margin=<length>` : e.g. `margin=3cm` sets the default margin to add to the side of the page for your comments
+ `noshortcuts` : if present, shortcut macros will not be defined. Also removes the dependence on pifont.

### Changing default options
+ `\mpageDefaults <options>` :  sets default options for the mpage environment (these can be overridden at each instance). `<options>` should be exactly as passed to `mpage`. E.g.
`\mpageDefaults{num colour=orange}` corresponds to
`\begin{mpage}[num colour=orange]`

+ `\mnoteDefaults <options>` : sets default options for each mnote (these can be overridden at each instance)

+ `\mnothDefaults <options>` : sets default options for each mnoth (these can be overridden at each instance)

### Annotating a page (`mpage`)
The next available page from the source pdf file(s) can be annotated in the `mpage` environment. This is a `tikzpicture` environment overlaying the included pdf page
#### `mpage` options
+ `margin=<length>` : Override default margin option for this page. e.g. `margin=3cm`
+  `note width=<length>` : Override default line width for added notes option on this page. e.g. `note width=2cm`
+ `num show=<n><e><s><w>` : `<n>...<w>` are `1` or `0` to toggle row/column labels on that edge of the page
+ `num inset=<n><e><s><w>` : `<n>...<w>` are the number of grid squares inside the page margin to print labels at on each edge of the page
+ `num colour=<colour>` : Coulour of the grid labels `<colour>` should be in a format understood by tikz
+ `subgrid show=<toggle>` : Draw 0.5cm grid as well as the main 1cm grid. e.g. `subgrid show=0` hides the subgrid.

### Adding a note in the margin (`mnote`)
Use `\mnote <x><y><text>` to add `<text>` in the margin with an arrow to `(<x>,<y>)` on the page (corresponding to grid labels).

Case sensitive letters can be used instead of numbers (when there are fewer than 52 rows/columns). This increases the range of rows/columns that can be indicated with a single character - avoiding the need for braces `{...}`.
E.g. `\mnote eq{<text>}`adds note pointing to column `e`
row `q`.

When using letters, fractional row/column indices can still be indicated: `\mnote {e.5}{q.1}{<text>}` is equivalent to `\mnote {5.5}{17.1}{<text>}`

#### `mnote` options
+ `xoff=<x>` and `yoff=<y>` : Allow the note text to be offset from its default position alongside the mark it refers to. E.g. `\mnote[yoff=2]` moves the text 2cm up the page from the annotated point.
+ `width=<length>` : Set maximum width of note text paragraph. See 'note width' option for `mpage`.
+ `anchor=<pos>` : Anchor position for tikz node for note text. E.g. `anchor=north west`
+ `colour=<colour>` : Set colour of note line and text. `<colour>` should be in tikz compatible format
+ `line opts=<options>` : Other settings to expand and pass to tikz draw command. E.g. `line opts={ultra thick}`
+ `bend=<degrees>` : Use this to bend the annotation line left by a specified angle (or right for negative values). Here 'left' is meant in the sense of travelling from the annotated point to the margin text (with 'up' being out of the page).

### Adding a note on the page (`mnoth` -- '`mnote` here')
To add a note at a specified point on the page rather than in the margin, and without an annotation line, use `\mnoth <x><y><text>`. `<x>`, `<y>` and `<text>` are as for `mnote` E.g. `\mnoth{3.6}f{Hello}` adds the text 'Hello' at column 3.6, row 6.

#### `mnoth` options
+ `width` : As for `mnote`
+ `node opts` : Tikz options for the node drawing command E.g.
`node opts={ultra thick, shape=circle, draw}` to put a thick circle around the text.
+ `colour` : As for `mnote`

### Checking up (`mstatus`)
Use `\mstatus` at the end of your document (before `\end{document}`) to check whether you've missed some available pages.

There should be an error generated when you try to compile this line if you've added fewer `mpage`s than pages in the source files.

### Handy symbols and shortcuts
By default (unless the package option `noshortcuts` is used) the following macros will be available inside the `mpage` environment:
+ `\mtick` and `\mcross` : insert tick and cross wingdings from pifont
+ `\mtt<x><y>` : Equivalent to `\mnoth<x><y>\mtick`
+ `\mcc<x><y>` : Equivalent to `\mnoth<x><y>\mcross`

The following use a slightly unusual syntax to reduce keystrokes needed for marking:
+ `\mt<text> <x><y>/` : Add a tick followed by `<text>` at `(<x>,<y>)`. The space after `<text>` is necessary!  Note that `<text>` need only be enclosed in braces `{...}` if it contains spaces. E.g.
`\mt good fm/` adds a tick followed by 'good' at column 6 row 13. See the following for an example with spaces.
+ `\mc<text> <x><y>/` : Add a cross followed by `<text>`, similar to `\mt`. E.g. `\mc{not quite right!} f{e.5}/`
+ `\mq<score> <out_of> <x><y>/` : Add a large displaystyle fraction `\frac{<score>}{<out_of>}` in a thick circle at `(<x>,<y>)`. Note that the spaces are necessary! E.g.
`\mq 10 12 pq/`
