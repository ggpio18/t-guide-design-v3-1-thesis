# t-guide-design-v3-1-thesis
# Pandoc document template

## Description

This repository contains a simple template for building
[Pandoc](http://pandoc.org/) documents; Pandoc is a suite of tools to compile
markdown files into readable files (PDF, EPUB, HTML...).

## Usage

### Installing

In order to use this makefile you will need to make sure that the following
dependencies are installed on your system:
  - GNU make
  - Pandoc
  - LuaLaTeX
  - DejaVu Sans fonts

### Folder structure

Here's a folder structure for a Pandoc document:

```
my-document/     # Root directory.
|- build/        # Folder used to store builded (output) files.
|- src/          # Markdowns files; one for each chapter.
|- images/       # Images folder.
|- metadata.yml  # Metadata content (title, author...).
|- Makefile      # Makefile used for building our documents.
```

### Setup generic data

Edit the *metadata.yml* file to set configuration data:

```yml
---
title: My document title
author: Ralph Huwiler
rights:  Creative Commons Attribution 4.0 International
language: en-US
tags: [document, my-document, etc]
abstract: |
  Your summary text.
---
```

You can find the list of all available keys on [this
page](http://pandoc.org/MANUAL.html#extension-yaml_metadata_block).

### Creating chapters

Creating a new chapter is as simple as creating a new markdown file in the
*src/* folder; you'll end up with something like this:

```
src/01-introduction.md
src/02-installation.md
src/03-usage.md
src/04-references.md
```

Pandoc and Make will join them automatically ordered by name; that's why the
numeric prefixes are being used.

All you need to specify for each chapter at least one title:

```md
# Introduction

This is the first paragraph of the introduction chapter.

## First

This is the first subsection.

## Second

This is the second subsection.
```

Each title (*#*) will represent a chapter, while each subtitle (*##*) will
represent a chapter's section. You can use as many levels of sections as
markdown supports.

#### Links between chapters

Anchor links can be used to link chapters within the document:

```md
// src/01-introduction.md
# Introduction

For more information, check the [Usage] chapter.

// src/02-installation.md
# Usage

...
```

If you want to rename the reference, use this syntax:

```md
For more information, check [this](#usage) chapter.
```

Anchor names should be downcased, and spaces, colons, semicolons... should be
replaced with hyphens. Instead of `Chapter title: A new era`, you have:
`#chapter-title-a-new-era`.

#### Links between sections

It's the same as anchor links:

```md
# Introduction

## First

For more information, check the [Second] section.

## Second

...
```

Or, with al alternative name:

```md
For more information, check [this](#second) section.
```

### Inserting objects

Text. That's cool. What about images and tables?

#### Insert an image

Use Markdown syntax to insert an image with a caption:

```md
![A cool seagull.](images/seagull.png)
```

Pandoc will automatically convert the image into a figure (image + caption).

If you want to resize the image, you may use this syntax, available in Pandoc
1.16:

```md
![A cool seagull.](images/seagull.png){ width=50% height=50% }
```

Also, to reference an image, use LaTeX labels:

```md
Please, admire the gloriousnes of Figure \ref{seagull_image}.

![A cool seagull.\label{seagull_image}](images/seagull.png)
```

#### Insert a table

Use markdown table, and use the `Table: <Your table description>` syntax to add
a caption:

```md
| Index | Name |
| ----- | ---- |
| 0     | AAA  |
| 1     | BBB  |
| ...   | ...  |

Table: This is an example table.
```

If you want to reference a table, use LaTeX labels:

```md
Please, check Table /ref{example_table}.

| Index | Name |
| ----- | ---- |
| 0     | AAA  |
| 1     | BBB  |
| ...   | ...  |

Table: This is an example table.\label{example_table}
```

#### Insert an equation

Wrap a LaTeX math equation between `$` delimiters for inline (tiny) formulas:

```md
This, $\mu = \sum_{i=0}^{N} \frac{x_i}{N}$, the mean equation, ...
```

Pandoc will transform them automatically into images using online services.

If you want to center the equation instead of inlining it, use double `$$`
delimiters:

```md
$$\mu = \sum_{i=0}^{N} \frac{x_i}{N}$$
```

[Here](https://www.codecogs.com/latex/eqneditor.php)'s an online equation
editor.

### Output

This template uses *Makefile* to automatize the building process. Instead of
using the *pandoc cli util*, we're going to use some *make* commands.

#### Export to PDF

Use this command:

```sh
make pdf
```

The generated file will be placed in *build/pdf*.

Please, note that PDF file generation requires some extra dependencies (~ 800
MB):

```sh
sudo apt-get install texlive-latex-base texlive-fonts-recommended texlive-latex-extra 
```

#### Export to EPUB

Use this command:

```sh
make epub
```

The generated file will be placed in *build/epub*.

#### Export to HTML

Use this command:

```sh
make html
```

The generated file(s) will be placed in *build/html*.

## References

- [Pandoc](http://pandoc.org/)
- [Pandoc Manual](http://pandoc.org/MANUAL.html)
- [Wikipedia: Markdown](http://wikipedia.org/wiki/Markdown)
