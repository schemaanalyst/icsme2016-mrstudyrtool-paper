# icsme2016-mrstudyrtool-paper

This repository contains the LaTeX source code and additional resources for a
research paper that was accepted for publication at the [32nd International
Conference on Software Maintenance and Evolution](http://icsme2016.github.io/)
(ICSME 2016). The source code of the paper uses the LaTeX style files provided
by the organizers of the conference and a wide variety of other packages that
are normally standard with a modern LaTeX distribution such a TeXLive 2015.

You are invited to use this repository as a means for learning more about
preparing papers in the field of automated software testing and as a way to
investigate the results and writing in our paper. If you find this repository
useful, could we trouble you to star this repository and then acknowledge it in
your own research efforts? If you would like to learn more about research in
the field of testing database applications, then you can check out
[gkapfham/research-bibliography](https://github.com/gkapfham/research-bibliography)
and
[schemaanalyst/schemaanalyst-bibliography](https://github.com/schemaanalyst/schemaanalyst-bibliography).
If you would like to download the tool used to conduct the experiments
presented in this paper, then please visit the
[schemaanalyst/schemaanalyst](https://github.com/schemaanalyst/schemaanalyst)
repository.

Here is a reference for the paper:

> mrstudyr: Retrospectively studying the effectiveness of mutant reduction techniques
> Colton J. McCurdy, Phil McMinn, and Gregory M. Kapfhammer
> 32nd International Conference on Software Maintenance and Evolution (ICSME 2016)

## Installation Instructions

You can type the following command if you want to clone this repository:

```shell
git clone https://github.com/schemaanalyst/icsme2016-mrstudyrtool-paper.git
```

## Compiling the Paper

If you want to compile the paper to a PDF, then you should type the following commands.

```shell
cd icsme2016-mrstudyrtool-paper
pdflatex icsme2016_tool_paper.tex
bibtex icsme2016_tool_paper.aux
pdflatex icsme2016_tool_paper.tex
pdflatex icsme2016_tool_paper.tex
```

Please note that this has been tested on an Ubuntu 16.04 workstation running a
recent version of LaTeX that was manually installed using the TeXLive
installer. It is also worth noting that you can also compile the paper using
other LaTeX development tools, such as `latexmk`. If you are unable to compile
the paper with your development tools and your execution environment, then
please open a new issue and we will attempt to resolve your concerns.
