Slides Template Project
=======================

Michel Casabianca

casa@sweetohm.net

---
Slides Template Project
-----------------------

This is a template project to generate slides using [Remark JS](http://remarkjs.com). Customizable properties in the build file are the following:

- *TITLE* which if the title of the slides.
- *DESTINATION* which is the slides destination, used by *rsync* to publish the slides.

You can then edit the content of the slides in the *readme.md* file in [Markdown format](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

The makefile provides following targets:

- *slides* to generate slides in *build* directory.
- *clean* to delete files generated in *build* directory.
- *publish* to publish slides using *rsync*.

You can open generated slides in your favorite browser in *build* directory. You can view these slides in your browser [at this location](http://sweetohm.net/slides/slides).

---
Sample slides
=============

Pages of the slides are separated with three dashes, such as `---`. First page with title should start with first level header:

```md
Title of my slides
==================

subtitle

my email address

---
Second page title
-----------------

Content ...
```

Content is written with standard [Markdown syntax](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

---
Pictures
--------

To print a picture, put it in the *img* directory and integrate with following syntax:

```md
![The Author](img/casa.png)
```

Which will render as:

![The Author](img/casa.png)

---
Tables
------

You can render a table with following syntax:

```md
First column | Second column | Third column    | Fourth column
:----------- | :-----------: | --------------: | -------------
left aligned | centered      | right aligned   | default
second line  | second line   | second line     | second line
```

This will be rendered as:

First column | Second column | Third column    | Fourth column
:----------- | :-----------: | --------------: | -------------
left aligned | centered      | right aligned   | default
second line  | second line   | second line     | second line

---
Tips
====

- Your slides will display on the github page of the project (with horizontal separator between pages).
- To generate PDF, you can display the slides on Chrome and then print them as PDF.
- As all resources are embbeded in *res* directory, you don't need an internet connection to display your slides locally.

