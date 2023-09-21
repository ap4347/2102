---
output:
  pdf_document: default
  html_document: default
---
# Lecture 04 {-}

&nbsp;


## R Markdown Files {-}

`R Markdown` provides an easy way to produce a rich, fully-documented reproducible analysis. It combines your code, results, commentary, and all metadata needed to reproduce your analysis in R. It allows to include chunks of code in the file along with text. R Markdown supports many output formats such as `PDF`, `Word`, `slideshows`, `presentations` etc.

R Markdown files can be used in many ways. For instance, it can be utilized for communicating to decision makers, who wants to focus on the conclusions, not the code behind the analysis; for collaborating with other data analysts, who are interested in both your conclusions and how you reached them. In addition, it can be used as an environment in which we do analysis.

Even these lecture notes are written using R Markdown files. In this lecture we will learn how to create an R Markdown file and its basic functionalities. You will be using R Markdown files to create your homework assignments.


### Creating R Markdown Files {-}

R Markdown files have `.Rmd` extensions. To create R Markdown files, you need the **rmarkdown** package. But you don't need to explicitly install and load this package, as RStudio automatically does both when needed.

You can create a new R Markdown file  with _File > New File > R Markdown..._

As mentioned earlier, R Markdown can produce files of various formats (`PDF`, `Word`, `HTML` etc.). You will be asked to submit your homework assignments in the `PDF` format, so in this class we will focus on this output format. PDF output format requires installing `LaTex` distribution on your machine. There are several traditional options including _MikTeX_, _MacTeX_, and _TeX Live_, but it is recommended to use _TinyTeX_.

You can install _TinyTeX_ with the R package **tinytex**. Run the following code in your console:



```r
tinytex:: install_tinytex()

```


### R Markdown File Components {-}


#### YAML {-}

The top part of the R Markdown file is called the YAML header. It is surrounded by `--`s and it stores the metadata needed for the document (for example, file title, author, output format etc.)


#### Text Formatting with Markdown {-}

##### Fonts {-}

Markdown is designed to be easy to read, write, and learn. Below you can find the basic guide that will help you format texts in your homework assignments:

* If you need to type a plain text, do it like in a word document (just type it).

* Use the `**  **` operator to change the font type to bold. For instance, **Hello** was typed as `**Hello**`.
* Use the `_   _` operator to use _italics_ font. For example, _Hello_ was typed as `_Hello_`.
* Use the ` ~~ ~~` operator to cross the words out. For example, ~~Hello~~ was typed as `~~Hello~~`.


##### Headers {-}

You can create headers in your text using `#` operator and control their size by simply adding one or more `#` in front of the text you would like to denote the header. 


```r

# Top Level Header (For example, Question 1)

## Second Level Header (For example, Question 1 part a)

### Third Level Header

#### and so on

```


##### Block Quotes {-}

You can create a block quote using `>` operator. For example the line below


```r

> Your first block quote

```

will produce the following result:

> Your first block quote


##### Lists {-}

In R Markdown you can create both ordered and unordered lists. To create an unordered list, use `*` operator:



```r
* Bullet point 1

* Bullet point 2

  * bullet point 2a

  * bullet point 2b

```



* Bullet point 1
* Bullet point 2
  * bullet point 2 a
  * bullet point 2 b
  


To create an ordered lists, simply use numbers:


```r
1. Bullet point 1

2. Bullet point 2
   * bullet point 2a
   * bullet point 2b

```


1. Bullet point 1
2. Bullet point 2
   * bullet point 2a
   * bullet point 2b


##### Tables {-}

Use the following formatting style to create tables in R Markdown:



```r
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

```
  
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell


##### Links {-}

Use the following format to insert a link into your text:


```r
[Your first link](http://posit.co)
```


It produces a link to the desired webpage and can be included in your text: [Your first link](http://posit.co)


##### Inline Equations {-}

Use `$ $` operator to include equations in your text. For example, $A = \pi*r^{2}$ was typed as `$A = \pi*r^{2}$`.


##### Line Breaks {-}

In order to start a new paragraph or new line, you will need to be sure that white space exists between the two paragraphs/line:


```r
This is the first lines.


And here we have another line.
```


This is the first lines.


And here we have another line.


Another useful way to divide up different parts of your file is by including horizontal lines. We can achieve it by using `***` operator:


```r
***
```

It will produce the horizontal line below.

***

Finally, if you want start a paragraph or a line on a new page, use `\newpage` command:



```r
\newpage
```

#### R Chunks of Code {-}

In addition to a plain text, you can also run  _R codes_ inside the file and include them in it along with the plain text to show what code you used to produce the results. It will be an integral and important part of your homework assignments and you will **always** need to include them in the HW file unless otherwise instructed.

You can run and include the _R code_ by manually typing the chunk deliminiters ` ```{r}` and ` ``` `. It will display the code you used and the results that this code produced. For example, the following chunk will run the code specified inside of it and will produce corresponding output:


<pre><code>```{r}

print(2 + 2)

``` </code></pre>



```
#> [1] 4
```


##### Options {-}
Chunk can be customized with **options**, that is, arguments supplied to chunk header. There are nearly 60 options that you can use to customize your code, but in this class we will discuss the most important and commonly used options:


* `eval = FALSE` prevents code from being evaluated. As a results, the code will not run and no results will be produced. This is useful for displaying example code.


<pre><code>```{r, eval = FALSE}
print(2 + 2)
``` </code></pre>
* `include = FALSE` runs the code, but doesn't show the code or results in the final document.

<pre><code>```{r, include = FALSE}
print(2 + 2)
``` </code></pre>

* `echo = FALSE` prevents code, but not the results from appearing in the finished file.


<pre><code>```{r, echo = FALSE}
print(2 + 2)
``` </code></pre>
* `results = 'hide'` prevents results, but not the code from appearing in the finished file.


<pre><code>```{r, results = 'hide'}
print(2 + 2)
``` </code></pre>


### Generating R Markdown Files {-}

Once you are done with the text and code that you want to include in your file, you can generate R Markdown files by clicking `knit` button on the top of the editor pane. It has a drop-down menu and allows you to pick output formats. It is recommended to use `HTML` format while you are preparing the file and fixing error. It will display the file in the `Viewer` tab nicely and conveniently. When you finalize your code and text, you can generate your file by clicking `knit` button once again and selecting `PDF` output format.

***

### Additional Resources {-}

We discussed basic and crucial parts of R Markdown files such as creating, formatting, and generating files. Of course, R Markdown allows you to do much more. Unfortunately, this is beyond the scope of this class and we don't have enough time to cover all topics. In case you are interested in exploring this topic further, below you can find a list of useful links and resources.

* [R Markdown Webpage](https://rmarkdown.rstudio.com/)
* [R Markdown Reference Guide](https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf)
* [R Markdown Cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/)
* [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/)

