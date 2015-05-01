---
layout: post
title: Install tkiz-uml Extention with MacTeX 2014
categories: tricks
comments: true
tags: tikz-uml
---

All you need is putting your downloaded file `tikz-uml.sty` into the directory `~/Library/texmf/tex/latex/tikz-uml`.

<!--more-->

Create the directory if it doesn't exist.

```sh
mkdir -p ~/Library/texmf/tex/latex/tikz-uml/
```

Have fun with UML in latex! Here is a simple example.

```tex
\documentclass{article}

\usepackage{tikz}
\usepackage{tikz-uml}

\begin{document}

\begin{tikzpicture}
\umlclass[x=1,y=0]{Movie}{\tt priceCode: int}{\tt ...}
\umlclass[x=6,y=0]{Rental}{\tt daysRented: int}{\tt ...} 
\umlclass[x=10.5,y=0]{Customer}{\tt ...}{\tt statement()}
\umluniassoc[mult1=*,mult2=1]{Rental}{Movie}
\umluniassoc[mult1=1,mult2=*]{Customer}{Rental}
\end{tikzpicture}

\end{document}
```

<img src="/img/tikz-uml-example.png" style="width:600px"/>

### References
1. [GitHub Gist: how to install tikz-uml](https://gist.github.com/tangyi/5791997)
2. [StackExchange: Using PGF/TikZ 3.0 on mac](http://tex.stackexchange.com/questions/163213/using-pgf-tikz-3-0-on-mac)
3. [stackoverflow: How to open an uml2 .tex?](http://stackoverflow.com/questions/29082933/how-to-open-an-uml2-tex)

