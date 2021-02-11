# Useful Notes for CTF
This area is intended to give a list of useful notes, tool, hints and tips that we have come across while learning about and doing Capture the Flag challenges.

These will be split into sections by type of challenge. These are organised into:
- [Table of contents by category](#contents.md)
- [Alphabetical index for searching](#index.md)

## Adding pages
- Add pages as you feel appropriate and put your file it in one of the subdirectories.
- If an appropriate subdirectory doesn't exist, create one
- To include in the index, use comments to tell the indexer about the article
  - Use a `brief:` comment to add a description
  - Use a `xref:` comment to say the article could belong in another category other than the directory the file is in
  - Use a `keywords:` comment to add keywords to aid searching the index
- You don't need to include all three. Any one included straight after a heading (at any level) will include that heading in the index. 
- For example
```
# Hello world
<!-- brief: Describes how to write "Hello World" programs -->
<!-- xref: pwn, re -->
<!-- keywords: programming, c, python
Article text...

## Don't index this subheading
Subsection text

## A sub heading I want indexed
<!-- brief: A subheading to include in the index -->

### Another un-indexed heading
```
- The content page will include all headings and subheadings to level 3 (three hashes).
- Once your page is created, run `create-indexes.py` in the top level to create the *index.md* and *content.md* in the top level of *Useful Notes*, and the *README.md* files in each of the sub-directories.
- Commit and push the resulting pages

