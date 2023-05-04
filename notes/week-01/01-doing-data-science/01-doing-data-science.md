
## Doing Data Science

### What is Data Science?

Data Science is an interdisciplinary field that involves the use of
scientific methods, processes, algorithms, and systems to extract
knowledge and insights from structured and unstructured data. It
involves combining knowledge and techniques from statistics,
mathematics, computer science, and domain-specific knowledge to analyze
and understand complex phenomena and solve problems.

Data Science encompasses a wide range of activities, including data
collection, cleaning, integration, analysis, modeling, and
visualization, as well as the communication of findings and insights to
stakeholders. It involves the use of various tools, techniques, and
technologies, including statistical software, machine learning
algorithms, data visualization tools, and big data platforms.

Data Science has numerous applications across industries, including
finance, healthcare, marketing, social media, and many others. The
insights and predictions derived from data analysis can be used to drive
business decisions, inform policy, and improve the lives of individuals
and communities.

### How to do Data Science?

Learning Data Science is like learning to cook. You start with a set of
ingredients and follow a recipe to combine them and transform them into
a delicious dish.

![cooking](https://images.unsplash.com/photo-1507048331197-7d4ac70811cf?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1674&q=80)

While there are several workflows to do data science, the one that has
resonated the most with me, and what I have used over time is the one
recommended by [Hadley Wickham](https://hadley.nz/).

![](https://imgur.com/jiBlLaM.png)

The first step almost always is to **import** data. Data comes in
different forms and shapes: files, databases, APIs etc. Before you can
start analyzing data, you need to bring it into the environment you are
working with.

Raw data is almost always NEVER clean. Hence, the next step in a typical
data science workflow is to **tidy** it. Tidying data often involves
cleaning

Now that you have clean data, the third step is to **transform** it. In
this step, you often manipulate individual variables, add new variables,
select observations of interest, aggregate values across specific groups
etc.

The real fun starts after you have transformed your data. The next step
in the journey is to **visualize** it. Visualizations let you explore
the data and gain insights.

An equally important part of the workflow is to **model** the data. This
involves analyzing hypotheses on the data as well as building
predictions.

The **transform** \> **visualize** \> **model** cycle is usually a
highly iterative one and one will find themselves tweaking each step
repeatedly till you find the optimal solution.

The last step in the workflow is to **communicate** the data. This is
done by turning your analysis into reports, dashboards, web apps and
other artifacts that can be shared across a broad range of stakeholders.

Our focus in this course will largely be on the **transform** and
**visualize** steps of the workflow. Shown below is a slightly more
detailed roadmap of the key topics we will cover in this course.

![data-science-workflow-detailed](https://i.imgur.com/dAuyiLz.png)

### Why R & Tidyverse?

<!-- Why R? -->

One of the biggest strengths of R is its extensive ecosystem of
**packages**. An R package is a suite of functions along with data and
documentation, that extends the functionality of base R.

<!-- Why Tidyverse -->

The Tidyverse is a highly opinionated set of packages that share a
common underlying grammar and philosophy and are designed to work
together to execute the data science workflow.

Rather than talk about the beauty of the `tidyverse`, I want you to
experience it for yourself by working through the next lesson that uses
the `tidyverse` to turn a dataset of US baby names into a beautiful
animated bar chart.

## R in a Nutshell

### Everything that exists is an object \[John Chambers\]

The first idea to grasp is that everything in R is an object. The most
fundamental type of object in R is a **vector**. You can think about a
vector as a collection of elements. Shown below are three different
vectors, each of which has been assigned a name. The first object holds
characters, the second one holds logicals, the third consists of
integers, while the fourth consists of numbers. Overall, R supports SIX
different types of vectors: `integer`, `numeric`, `logical`,
`character`, `complex`, and `raw`. In this course, we will only worry
about the first FOUR!

``` r
baby_names <- c("John", "Marie", "Elizabeth")
baby_is_male <- c(TRUE, FALSE, FALSE)
baby_nb_births <- c(120L, 30L, 60L)
baby_pct_births <- c(0.04, 0.01, 0.02)
```

Some of you might be wondering if a vector is a collection of elements,
what would one call a single element of a vector? A scalar perhaps?
Well, the answer might surprise you. There are NO scalars in R. A single
element of a vector is also a vector, albeit of length 1.

``` r
baby_name <- "John" 
```

Three comments are in order.

1.  First you might have noticed the strange-looking symbol, `<-`. It is
    the assignment operator in R and is used to assign an object to a
    name. It consists of two symbols, a less-than-or-equal symbol
    followed by a hyphen, but might appear as a single symbol when you
    use fonts like Fira Code, which have special ligatures for
    combinations of symbols.

2.  Second, you might have observed that we used an `L` as a suffix for
    some numbers. The reason for this is that R needs a way to tell
    apart the integer 120 from the floating-point number 120, and adding
    the suffix provides that precise disambiguation.

3.  Third, what about that `c()` used to define these vectors. Is it an
    object, too? Nope! `c()` is a function that concatenates multiple
    elements into a single vector. You will learn more about functions
    in the next section.

### Everything that happens is the result of a function call \[John Chambers\]

The second core idea in R is that every action is the result of a
function call. You already encountered the `c()` function that took in
multiple elements and concatenated them into a single vector. The base R
language ships with a large range of functions that can perform simple
to complex actions. For example, let us suppose you want to generate
1000 random normal variables and plot a histogram. In most other
languages, you will need to write multiple lines of code to accomplish
this task. However, in base R, it boils down to two lines of code.

``` r
x <- rnorm(n = 1000)
hist(x)
```

![](img/function-call-1.png)<!-- -->

The first line calls the function `rnorm()` to generate a numeric vector
of 1000 random normal variables. The second line plots a **histogram**
of these numbers. You can even combine the two lines into a single line!

``` r
hist(rnorm(n = 1000))
```

![](img/histogram-1.png)<!-- -->

### Embrace the Pipe

![pipe](https://static.wixstatic.com/media/8bdd38_24ca8c3b77e84e89ab218bab1d04ecf6~mv2.jpeg/v1/fill/w_640,h_446,al_c,lg_1,q_80,enc_auto/8bdd38_24ca8c3b77e84e89ab218bab1d04ecf6~mv2.jpeg)

Now suppose you are given a vector of US state names, `state.name`, and
you want to create a bar plot that shows a frequency distribution of the
first letter. How would you go about it? Let us write down the steps
first.

1.  Extract the first letter from each state.
2.  Count the frequency of each letter.
3.  Create a bar plot of the frequency counts.

We are almost there! Now we need to figure out how to execute each of
these steps. The beauty of R is that it was designed by statisticians to
precisely solve data analysis problems and so it ships with functions
that can handle (1) - (3). We will use the `substr()` function to
extract the first letter, the `table()` function to tabulate the
frequency counts, and the `barplot()` function to create the bar plot.

``` r
# 1. Extract the first letter from each name.
# Note the first 1 means the substring starts at the first letter
# and the second 1 means the substring stops at the first letter.
first_letters <- substr(state.name, 1, 1)

# 2. Count the frequency of each letter
first_letters_counts <- table(first_letters)

# 3. Create a bar plot of the frequency counts
barplot(first_letters_counts)
```

![](img/sequential-1.png)<!-- -->

Running this code will create a nice bar plot. Note how we just had to
know the right functions to use to get the job done! The multiple lines
of code mirror the exact order of the steps. However, they add a lot of
clutter in terms of intermediate objects that have to be named
appropriately. As the number of steps increase, you will have to spend
more time to understand the code.

Alternatively, we could have also written this code as a one-liner!

``` r
barplot(table(substr(state.name, 1, 1)))
```

![](img/oneline-1.png)<!-- -->

It skips the step of having to create intermediate variables and naming
them appropriately. However, if you were to read the code carefully, you
will notice that the steps read in the opposite direction to our recipe.

1.  Create a bar plot (of)
2.  A frequency table (of)
3.  First letters of state names

Similar to the multi-line code, as you add more steps to the recipe,
this can obfuscate the intent of the code and make it really cumbersome
to read. Is there a solution that combines the best of both approaches?
The answer is yes

Well, fortunately, R has our backs covered once again. In R 4.1, the
language formally introduced the pipe operator `|>` that lets us rewrite
the same code in a more intuitive fashion.

``` r
state.name |>     # Take baby_names, then
  substr(1, 1) |> # Extract the first letter, then
  table() |>      # Tabulate the frequencies, then
  barplot()       # Create a bar plot.
```

![](img/pipe-1.png)<!-- -->

Notice how the steps are now ordered exactly the same way we wrote our
recipe, and there is no clutter of intermediate objects. In fact, if the
functions were named as verbs, the code would read just like the English
language.

### Embrace Data Structures

While learning any new programming language, it is important to
understand the data structures it supports, and how to create, read,
update, and delete its elements. R supports a rich set of data
structures.

**Homogeneous** data structures hold the same type of data (numeric,
character, logical etc.). Vectors are the simplest of this kind and are
1-dimensional in nature. Matrices are 2-dimensional and Arrays are
multi-dimensional. We won’t be working with matrices and arrays in this
course.

``` r
v <- c(1, 2, 3)
M <- matrix(1:9, nrow = 3, ncol = 3)
A <- array(1:24, dim = c(2, 3, 4))
```

**Heterogeneous** data structures can hold different types of data
(numeric, character, logical etc.). Lists are the 1-dimensional
equivalent of vectors, and can be used to store a mix of data types. For
example, the two lists shown below store the first name, last name and
age of a person. While both lists hold the same information, the second
list names its constituent elements, which can come very handy when we
try to access them.

``` r
l1 <- list('Jane', 'Austen', 42)
l2 <- list(first_name = 'Jane', last_name = 'Austen', age = 42L)
```

Lists are extremely flexible and can be nested to hold other lists too.

``` r
l1 <- list('Jane', 'Austen', 42)
l2 <- list('Jane', 'Eyre', 52)
l <- list(l1, l2)
```

A **Data Frame** is a special type of list where each element of the
list has the same length. Visually, this resembles a rectangular table,
the most ubiquitous data structure found everywhere from excel tables to
sql databases.

``` r
D <- data.frame(
  name = c("Mary", "John"),
  sex = c("Female", "Male"),
  year = c(1980, 1980),
  births = c(5490, 9102)
)
D
```

    ##   name    sex year births
    ## 1 Mary Female 1980   5490
    ## 2 John   Male 1980   9102

We will mostly work with data frames in this course.

This is a whirlwind introduction to R and summarizes the key ideas you
will need for this course. For a more thorough introduction to R, I
would strongly recommend the book [Hands On Programming with
R](https://rstudio-education.github.io/hopr/index.html). The online
version of the book is free to use. Familiarity with R beyond what is
covered in this course will be essential for those who hope to use the
language in data science-focused careers.
