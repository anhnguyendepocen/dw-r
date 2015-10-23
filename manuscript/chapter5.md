# Dealing with Character Strings
Dealing with character strings is often under-emphasized in data analysis training.  The focus typically remains on numeric values; however, the growth in data collection is also resulting in greater bits of information embedded in character strings.  Consequently, handling, cleaning and processing character strings is becoming a prerequisite in daily data analysis.  This chapter is meant to give you the foundation of working with characters by covering some [basics](#character_basics) followed by learning how to [manipulate strings](#string_manipulation) using base R functions along with using the simplified `stringr` package.  Next you will learn the basics of identifying, extracting, and replacing patterns in character strings commonly referred to as [regular expressions](#regex). Lastly, I offer some [additional resources](#add_resources) to learn more about dealing with characters and expressions.

## Character string basics {#character_basics}
In this section you'll learn the basics of creating, converting and printing character strings followed by how to assess the number of elements and characters in a string.

### Creating Strings

The most basic way to create strings is to use quotation marks and assign a string to an object similar to creating number sequences.

{linenos=off}
```r
a <- "learning to create"    # create string a
b <- "character strings"     # create string b
```

The `paste()` function provides a versatile means for creating and building strings. It takes one or more R objects, converts them to "character", and then it concatenates (pastes) them to form one or several character strings.

{linenos=off}
```r
# paste together string a & b
paste(a, b)                      
## [1] "learning to create character strings"
```

{linenos=off}
```r
# paste character and number strings (converts numbers to character class)
paste("The life of", pi)           
## [1] "The life of 3.14159265358979"

# paste multiple strings
paste("I", "love", "R")            
## [1] "I love R"

# paste multiple strings with a separating character
paste("I", "love", "R", sep = "-")  
## [1] "I-love-R"

# use paste0() to paste without spaces btwn characters
paste0("I", "love", "R")            
## [1] "IloveR"

# paste objects with different lengths
paste("R", 1:5, sep = " v1.")       
## [1] "R v1.1" "R v1.2" "R v1.3" "R v1.4" "R v1.5"
```


### Converting to Strings
Test if strings are characters with `is.character()` and convert strings to character with `as.character()` or with `toString()`.

{linenos=off}
```r
a <- "The life of"    
b <- pi

is.character(a)
## [1] TRUE

is.character(b)
## [1] FALSE

c <- as.character(b)
is.character(c)
## [1] TRUE

toString(c("Aug", 24, 1980))
## [1] "Aug, 24, 1980"
```

### Printing Strings
The common printing methods include:

- [`print()`](#print): generic printing
- [`noquote()`](#noquote): print with no quotes
- [`cat()`](#cat): concatenate and print with no quotes
- [`sprintf()`](#sprintf): a wrapper for the C function `sprintf`, that returns a character vector containing a formatted combination of text and variable values

#### print() {#print}
The primary printing function in R is `print()`

{linenos=off}
```r
x <- "learning to print strings"    

# basic printing
print(x)                
## [1] "learning to print strings"

# print without quotes
print(x, quote = FALSE)  
## [1] learning to print strings
```


#### noquote() {#noquote}
An alternative to printing without quotes.

{linenos=off}
```r
noquote(x)
## [1] learning to print strings
```

#### cat() {#cat}
Another very useful function is `cat()` which allows us to concatenate objects and print them either on screen or to a file.  The output result is very similar to `noquote()`; however, `cat()` does not print the numeric line indicator.  As a result, `cat()` can be useful for printing nicely formated responses to users.

{linenos=off}
```r
# basic printing (similar to noquote)
cat(x)                   
## learning to print strings

# combining character strings
cat(x, "in R")           
## learning to print strings in R

# basic printing of alphabet
cat(letters)             
## a b c d e f g h i j k l m n o p q r s t u v w x y z

# specify a seperator between the combined characters
cat(letters, sep = "-")  
## a-b-c-d-e-f-g-h-i-j-k-l-m-n-o-p-q-r-s-t-u-v-w-x-y-z

# collapse the space between the combine characters
cat(letters, sep = "")   
## abcdefghijklmnopqrstuvwxyz
```

You can also format the line width for printing long strings using the `fill` argument:

{linenos=off}
```r
x <- "Today I am learning how to print strings."
y <- "Tomorrow I plan to learn about textual analysis."
z <- "The day after I will take a break and drink a beer."

cat(x, y, z, fill = 0)
## Today I am learning how to print strings. Tomorrow I plan to learn about textual analysis. The day after I will take a break and drink a beer.

cat(x, y, z, fill = 5)
## Today I am learning how to print strings. 
## Tomorrow I plan to learn about textual analysis. 
## The day after I will take a break and drink a beer.
```

#### sprintf() {#sprintf}
A wrapper for the C function `sprintf`, that returns a character vector containing a formatted combination of text and variable values.

To substitute in a string or string variable, use `%s`:

{linenos=off}
```r
x <- "print strings"

# substitute a single string/variable
sprintf("Learning to %s in R", x)    
## [1] "Learning to print strings in R"

# substitute multiple strings/variables
y <- "in R"
sprintf("Learning to %s %s", x, y)   
## [1] "Learning to print strings in R"
```

For integers, use `%d` or a variant:

{linenos=off}
```r
version <- 3

# substitute integer
sprintf("This is R version:%d", version)
## [1] "This is R version:3"

# print with leading spaces
sprintf("This is R version:%4d", version)   
## [1] "This is R version:   3"

# can also lead with zeros
sprintf("This is R version:%04d", version)   
## [1] "This is R version:0003"
```


For floating-point numbers, use `%f` for standard notation, and `%e` or `%E` for exponential notation:

{linenos=off}
```r
sprintf("%f", pi)         # '%f' indicates 'fixed point' decimal notation
## [1] "3.141593"

sprintf("%.3f", pi)       # decimal notation with 3 decimal digits
## [1] "3.142"

sprintf("%1.0f", pi)      # 1 integer and 0 decimal digits
## [1] "3"

sprintf("%5.1f", pi)      # decimal notation with 5 total decimal digits and 
## [1] "  3.1"            # only 1 to the right of the decimal point

sprintf("%05.1f", pi)     # same as above but fill empty digits with zeros
## [1] "003.1"

sprintf("%+f", pi)        # print with sign (positive)
## [1] "+3.141593"

sprintf("% f", pi)        # prefix a space
## [1] " 3.141593"

sprintf("%e", pi)         # exponential decimal notation 'e'
## [1] "3.141593e+00"

sprintf("%E", pi)         # exponential decimal notation 'E'
## [1] "3.141593E+00"
```


### Counting Elements
To count the number of elements in a string use `length()`:

{linenos=off}
```r
length("How many elements are in this string?")
## [1] 1

length(c("How", "many", "elements", "are", "in", "this", "string?"))
## [1] 7
```

### Counting Characters
To count the number of characters in a string use `nchar()`:

{linenos=off}
```r
nchar("How many characters are in this string?")
## [1] 39

nchar(c("How", "many", "characters", "are", "in", "this", "string?"))
## [1]  3  4 10  3  2  4  7
```


## String manipulation {#string_manipulation}
Basic string manipulation typically inludes case conversion, simple character and substring replacement, adding/removing whitespace, and performing set operations to compare similarities and differences between two character vectors.  These operations can all be performed with base R functions; however, some operations (or at least their syntax) are simplified with the `stringr` package.  This section illustrates these string manipulation capabilities.

### String manipulation with base R

#### Convert to Lower Case
To convert all upper case characters to lower case use `tolower()`:

{linenos=off}
```r
x <- "Learning To MANIPULATE strinGS in R"

tolower(x)
## [1] "learning to manipulate strings in r"
```

#### Convert to Upper Case
To convert all lower case characters to upper case use `toupper()`:

{linenos=off}
```r
toupper(x)
## [1] "LEARNING TO MANIPULATE STRINGS IN R"
```

#### Simple Character Replacement
To replace a character (or multiple characters) in a string you can use `chartr()`:

{linenos=off}
```r
# replace 'A' with 'a'
x <- "This is A string."
chartr(old = "A", new = "a", x)
## [1] "This is a string."

# multiple character replacements
# replace any 'd' with 't' and any 'z' with 'a'
y <- "Tomorrow I plzn do lezrn zbout dexduzl znzlysis."
chartr(old = "dz", new = "ta", y)
## [1] "Tomorrow I plan to learn about textual analysis."
```

Note that `chartr()` replaces every identified letter for replacement so the only time I use it is when I am certain that I want to change every possible occurence of a letter.

#### String Abbreviations
To abbreviate strings you can use `abbreviate()`:

{linenos=off}
```r
streets <- c("Main", "Elm", "Riverbend", "Mario", "Frederick")

# default abbreviations
abbreviate(streets)
##      Main       Elm Riverbend     Mario Frederick 
##    "Main"     "Elm"    "Rvrb"    "Mari"    "Frdr"

# set minimum length of abbreviation
abbreviate(streets, minlength = 2)
##      Main       Elm Riverbend     Mario Frederick 
##      "Mn"      "El"      "Rv"      "Mr"      "Fr"
```

Note that if you are working with U.S. states, R already has a pre-built vector with state names (`state.name`).  Also, there is a pre-built vector of abbreviated state names (`state.abb`).

#### Extract/Replace Substrings
To extract or replace substrings in a character vector use `substr()`, `substring()`, and `strsplit()`:

Extract and replace substrings with specified starting and stopping characters with `substr()`:

{linenos=off}
```r
alphabet <- paste(LETTERS, collapse = "")

# extract 18th character in string
substr(alphabet, start = 18, stop = 18)
## [1] "R"

# extract 18-24th characters in string
substr(alphabet, start = 18, stop = 24)
## [1] "RSTUVWX"

# replace 1st-17th characters with `R`
substr(alphabet, start = 19, stop = 24) <- "RRRRRR"
alphabet
## [1] "ABCDEFGHIJKLMNOPQRRRRRRRYZ"
```

Extract and replace substrings with only a specified starting point with `substr()`.  Also allows you to extract/replace in a recursive fashion:

{linenos=off}
```r
alphabet <- paste(LETTERS, collapse = "")

# extract 18th through last character
substring(alphabet, first = 18)
## [1] "RSTUVWXYZ"

# recursive extraction; specify start position only
substring(alphabet, first = 18:24)
## [1] "RSTUVWXYZ" "STUVWXYZ"  "TUVWXYZ"   "UVWXYZ"    "VWXYZ"     "WXYZ"     
## [7] "XYZ"

# recursive extraction; specify start and stop positions
substring(alphabet, first = 1:5, last = 3:7)
## [1] "ABC" "BCD" "CDE" "DEF" "EFG"
```

To split the elements of a character string use `strsplit()`:

{linenos=off}
```r
z <- "The day after I will take a break and drink a beer."
strsplit(z, split = " ")
## [[1]]
##  [1] "The"   "day"   "after" "I"     "will"  "take"  "a"     "break"
##  [9] "and"   "drink" "a"     "beer."

a <- "Alabama-Alaska-Arizona-Arkansas-California"
strsplit(a, split = "-")
## [[1]]
## [1] "Alabama"    "Alaska"     "Arizona"    "Arkansas"   "California"
```

Note that the output of `strsplit()` is a list.  To convert the output to a simple atomic vector simply wrap in `unlist()`:

{linenos=off}
```r
unlist(strsplit(a, split = "-"))
## [1] "Alabama"    "Alaska"     "Arizona"    "Arkansas"   "California"
```

### String manipulation with `stringr`
The [`stringr`](http://cran.r-project.org/web/packages/stringr/index.html) package was developed by Hadley Wickham to act as simple wrappers that make R's string functions more consistent, simpler, and easier to use.  To replicate the functions in this section you will need to install and load the `stringr` package:

{linenos=off}
```r
# install stringr package
install.packages("stringr")

# load package
library(stringr)
```
For more information on getting help with packages visit the [working with packages section](#packages).

#### Basic Operations
There are three string functions that are closely related to their base R equivalents, but with a few enhancements:

* Concatenate with [`str_c()`](#str_c)
* Number of characters with [`str_length()`](#str_length)
* Substring with [`str_sub()`](#str_sub)

{#str_c}
`str_c()` is equivalent to the `paste()` functions: 

{linenos=off}
```r
# same as paste0()
str_c("Learning", "to", "use", "the", "stringr", "package")
## [1] "Learningtousethestringrpackage"

# same as paste()
str_c("Learning", "to", "use", "the", "stringr", "package", sep = " ")
## [1] "Learning to use the stringr package"

# allows recycling 
str_c(letters, " is for", "...")
##  [1] "a is for..." "b is for..." "c is for..." "d is for..." "e is for..."
##  [6] "f is for..." "g is for..." "h is for..." "i is for..." "j is for..."
## [11] "k is for..." "l is for..." "m is for..." "n is for..." "o is for..."
## [16] "p is for..." "q is for..." "r is for..." "s is for..." "t is for..."
## [21] "u is for..." "v is for..." "w is for..." "x is for..." "y is for..."
## [26] "z is for..."
```

{#str_length}
`str_length()` is similiar to the `nchar()` function; however, `str_length()` behaves more appropriately with missing ('NA') values: 

{linenos=off}
```r
# some text with NA
text = c("Learning", "to", NA, "use", "the", NA, "stringr", "package")

# compare `str_length()` with `nchar()`
nchar(text)
## [1] 8 2 2 3 3 2 7 7

str_length(text)
## [1]  8  2 NA  3  3 NA  7  7
```

{#str_sub}
`str_sub()` is similar to `substr()`; however, it returns a zero length vector if any of its inputs are zero length, and otherwise expands each argument to match the longest. It also accepts negative positions, which are calculated from the left of the last character.

{linenos=off}
```r
x <- "Learning to use the stringr package"

# alternative indexing
str_sub(x, start = 1, end = 15)
## [1] "Learning to use"

str_sub(x, end = 15)
## [1] "Learning to use"

str_sub(x, start = 17)
## [1] "the stringr package"

str_sub(x, start = c(1, 17), end = c(15, 35))
## [1] "Learning to use"     "the stringr package"

# using negative indices for start/end points from end of string
str_sub(x, start = -1)
## [1] "e"

str_sub(x, start = -19)
## [1] "the stringr package"

str_sub(x, end = -21)
## [1] "Learning to use"

# Replacement
str_sub(x, end = 15) <- "I know how to use"
x
## [1] "I know how to use the stringr package"
```

#### Duplicate Characters within a String
A new functionality that stringr provides in which base R does not have a specific function for is character duplication:

{linenos=off}
```r
str_dup("beer", times = 3)
## [1] "beerbeerbeer"

str_dup("beer", times = 1:3)
## [1] "beer"         "beerbeer"     "beerbeerbeer"


# use with a vector of strings
states_i_luv <- state.name[c(6, 23, 34, 35)]
str_dup(states_i_luv, times = 2)
## [1] "ColoradoColorado"         "MinnesotaMinnesota"      
## [3] "North DakotaNorth Dakota" "OhioOhio"
```

#### Remove Leading and Trailing Whitespace
A common task of string processing is that of parsing text into individual words.  Often, this results in words having blank spaces (whitespaces) on either end of the word. The `str_trim()` can be used to remove these spaces:

{linenos=off}
```r
text <- c("Text ", "  with", " whitespace ", " on", "both ", " sides ")

# remove whitespaces on the left side
str_trim(text, side = "left")
## [1] "Text "       "with"        "whitespace " "on"          "both "      
## [6] "sides "

# remove whitespaces on the right side
str_trim(text, side = "right")
## [1] "Text"        "  with"      " whitespace" " on"         "both"       
## [6] " sides"

# remove whitespaces on both sides
str_trim(text, side = "both")
## [1] "Text"       "with"       "whitespace" "on"         "both"      
## [6] "sides"
```

#### Pad a String with Whitespace
To add whitespace, or to *pad* a string, use `str_pad()`:

{linenos=off}
```r
str_pad("beer", width = 10, side = "left")
## [1] "      beer"

str_pad("beer", width = 10, side = "both")
## [1] "   beer   "

str_pad("beer", width = 10, side = "right", pad = "!")
## [1] "beer!!!!!!"
```

### Set operatons
There are also base R functions that allows for assessing the set union, intersection, difference, equality, and membership on two vectors.  

#### Set Union
To obtain the elements of the union between two character vectors use `union()`:

{linenos=off}
```r
set_1 <- c("lagunitas", "bells", "dogfish", "summit", "odell")
set_2 <- c("sierra", "bells", "harpoon", "lagunitas", "founders")

union(set_1, set_2)
## [1] "lagunitas" "bells"     "dogfish"   "summit"    "odell"     "sierra"   
## [7] "harpoon"   "founders"
```

#### Set Intersection
To obtain the common elements of two character vectors use `intersect()`:

{linenos=off}
```r
intersect(set_1, set_2)
## [1] "lagunitas" "bells"
```

#### Identifying Different Elements
To obtain the non-common elements, or the difference, of two character vectors use `setdiff()`:

{linenos=off}
```r
# returns elements in set_1 not in set_2
setdiff(set_1, set_2)
## [1] "dogfish" "summit"  "odell"

# returns elements in set_2 not in set_1
setdiff(set_2, set_1)
## [1] "sierra"   "harpoon"  "founders"
```

#### Testing for Element Equality
To test if two vectors contain the same elements regardless of order use `setequal()`:

{linenos=off}
```r
set_3 <- c("woody", "buzz", "rex")
set_4 <- c("woody", "andy", "buzz")
set_5 <- c("andy", "buzz", "woody")

setequal(set_3, set_4)
## [1] FALSE

setequal(set_4, set_5)
## [1] TRUE
```

#### Testing for *Exact* Equality
To test if two character vectors are equal in content and order use `identical()`:

{linenos=off}
```r
set_6 <- c("woody", "andy", "buzz")
set_7 <- c("andy", "buzz", "woody")
set_8 <- c("woody", "andy", "buzz")

identical(set_6, set_7)
## [1] FALSE

identical(set_6, set_8)
## [1] TRUE
```

#### Identifying if Elements are Contained in a String
To test if an element is contained within a character vector use `is.element()` or `%in%`:

{linenos=off}
```r
good <- "andy"
bad <- "sid"

is.element(good, set_8)
## [1] TRUE

good %in% set_8
## [1] TRUE

bad %in% set_8
## [1] FALSE
```

#### Sorting a String
To sort a character vector use `sort()`:

{linenos=off}
```r
sort(set_8)
## [1] "andy"  "buzz"  "woody"

sort(set_8, decreasing = TRUE)
## [1] "woody" "buzz"  "andy"
```



## Regular expressions {#regex}
A regular expression (aka regex) is a sequence of characters that define a search pattern, mainly for use in pattern matching with text strings.  Typically, regex patterns consist of a combination of alphanumeric characters as well as special characters.  The pattern can also be as simple as a single character or it can be more complex and include several characters.  

To understand how to work with regular expressions in R, we need to consider two primary features of regular expressions.  One has to do with the *syntax*, or the way regex patterns are expressed in R.  The other has to do with the *functions* used for regex matching in R.  In this section, we will cover both of these aspects.  First, I cover the [syntax](#regex_syntax) that allow you to perform pattern matching functions with meta characters, character and POSIX classes, and quantifiers.  This will provide you with the basic understanding of the syntax required to establish the pattern to find.  Then I cover the  [functions](#regex_functions) you can apply to identify, extract, replace, and split parts of character strings based on the regex pattern specified.  

{#regex_syntax}
### Regex Syntax
At first glance (and second, third,...) the regex syntax can appear quite confusing.  This section will provide you with the basic foundation of regex syntax; however, realize that there is a plethora of [resources available](#regex_resources) that will give you far more detailed, and advanced, knowledge of regex syntax.  To read more about the specifications and technicalities of regex in R you can find help at `help(regex)` or `help(regexp)`.

#### Metacharacters
Metacharacters consist of non-alphanumeric symbols such as: 

C> . &nbsp;&nbsp; \\\ &nbsp;&nbsp; | &nbsp;&nbsp; ( &nbsp;&nbsp; ) &nbsp;&nbsp; [ &nbsp;&nbsp; { &nbsp;&nbsp; $ &nbsp;&nbsp; * &nbsp;&nbsp; + &nbsp;&nbsp;? 

To match metacharacters in R you need to escape them with a double backslash "\\\\".  The following displays the general escape syntax for the most common metacharacters:

![](images/metacharacter_escape.png)

The following provides examples to show how to use the escape syntax to find and replace metacharacters.  For information on the `sub` and `gsub` functions used in this example visit the [main regex functions page](#main_regex_functions).

{linenos=off}
```r
sub(pattern = "\\$", "\\!", "I love R$")
## [1] "I love R!"

sub(pattern = "\\^", "carrot", "My daughter has a ^ with almost every meal!")
## [1] "My daughter has a carrot with almost every meal!"

gsub(pattern = "\\\\", " ", "I\\need\\space")
## [1] "I need space"
```

#### Sequences
To match a sequence of characters we can apply short-hand notation which captures the fundamental types of sequences.  The following displays the general syntax for these common sequences:

![](images/anchor_sequence.png)
    
The following provides examples to show how to use the anchor syntax to find and replace sequences.  For information on the `gsub` function used in this example visit the [main regex functions page](#main_regex_functions).

{linenos=off}
```r
gsub(pattern = "\\d", "_", "I'm working in RStudio v.0.99.484")
## [1] "I'm working in RStudio v._.__.___"

gsub(pattern = "\\D", "_", "I'm working in RStudio v.0.99.484")
## [1] "_________________________0_99_484"

gsub(pattern = "\\s", "_", "I'm working in RStudio v.0.99.484")
## [1] "I'm_working_in_RStudio_v.0.99.484"

gsub(pattern = "\\w", "_", "I'm working in RStudio v.0.99.484")
## [1] "_'_ _______ __ _______ _._.__.___"
```
{#character_class}
#### Character classes
To match one of several characters in a specified set we can enclose the characters of concern with square brackets [ ].  In addition, to match any characters **not** in a specified character set we can include the caret ^ at the beginning of the set within the brackets.  The following displays the general syntax for common character classes but these can be altered easily as shown in the examples that follow:

![](images/character_class.png)

The following provides examples to show how to use the anchor syntax to match character classes.  For information on the `grep` function used in this example visit the [main regex functions page](#main_regex_functions).

{linenos=off}
```r
x <- c("RStudio", "v.0.99.484", "2015", "09-22-2015", "grep vs. grepl")

grep(pattern = "[0-9]", x, value = TRUE)
## [1] "v.0.99.484" "2015"       "09-22-2015"

grep(pattern = "[6-9]", x, value = TRUE)
## [1] "v.0.99.484" "09-22-2015"

grep(pattern = "[Rr]", x, value = TRUE)
## [1] "RStudio"        "grep vs. grepl"

grep(pattern = "[^0-9a-zA-Z]", x, value = TRUE)
## [1] "v.0.99.484"     "09-22-2015"     "grep vs. grepl"
```

#### POSIX character classes
Closely related to regex [character classes](#character_class) are POSIX character classes which are expressed in double brackets [[ ]].

![](images/posix.png)   

The following provides examples to show how to use the anchor syntax to match POSIX character classes. For information on the `grep` function used in this example visit the [main regex functions page](#main_regex_functions).

{linenos=off}
```r
x <- "I like beer! #beer, @wheres_my_beer, I like R (v3.2.2) #rrrrrrr2015"

gsub(pattern = "[[:blank:]]", replacement = "", x)
## [1] "Ilikebeer!#beer,@wheres_my_beer,IlikeR(v3.2.2)#rrrrrrr2015"

gsub(pattern = "[[:punct:]]", replacement = " ", x)
## [1] "I like beer   beer   wheres my beer  I like R  v3 2 2   rrrrrrr2015"

gsub(pattern = "[[:alnum:]]", replacement = "", x)
## [1] "  ! #, @__,    (..) #"
```

#### Quantifiers
When we want to match a **certain number** of characters that meet a certain criteria we can apply quantifiers to our pattern searches.  The quantifiers we can use are:

![](images/quantifier.png)    

The following provides examples to show how to use the quantifier syntax to match a **certain number** of characters patterns. For information on the `grep` function used in this example visit the [main regex functions page](#main_regex_functions).  Note that `state.name` is a built in dataset within R that contains all the U.S. state names.

{linenos=off}
```r
grep(pattern = "z+", state.name, value = TRUE)
## [1] "Arizona"

grep(pattern = "s{2}", state.name, value = TRUE)
## [1] "Massachusetts" "Mississippi"   "Missouri"      "Tennessee"

grep(pattern = "s{1,2}", state.name, value = TRUE)
##  [1] "Alaska"        "Arkansas"      "Illinois"      "Kansas"       
##  [5] "Louisiana"     "Massachusetts" "Minnesota"     "Mississippi"  
##  [9] "Missouri"      "Nebraska"      "New Hampshire" "New Jersey"   
## [13] "Pennsylvania"  "Rhode Island"  "Tennessee"     "Texas"        
## [17] "Washington"    "West Virginia" "Wisconsin"
```

{#regex_functions}
### Regex Functions
Now that I've illustrated how R handles some of the most common regular expression elements, it’s time to present the functions you can use for working with regular expression.  R contains a [set of functions](#main_regex_functions) in the base package that we can use to find pattern matches.  Alternatively, the R package stringr also provides [several functions](#stringr_regex_functions) for regex operations.  We will cover both these alternatives.

{#main_regex_functions}
#### Main regex functions in R
The primary base R regex functions serve three primary purposes:  [pattern matching](#pattern_matching), [pattern replacement](#pattern_replacement), and [character splitting](#character_splitting).

##### Pattern matching {#pattern_matching}
There are five functions that provide pattern matching capabilities.  The three functions that I provide examples for are ones that are most common.  The two other functions which I do not illustrate are `gregexpr()` and `regexec()` which provide similar capabilities as `regexpr()` but with the output in list form.

###### `grep()`:
To find a pattern in a character vector and to have the element values or indices as the output use `grep()`:

{linenos=off}
```r
# use the built in data set `state.division`
head(as.character(state.division))
## [1] "East South Central" "Pacific"            "Mountain"          
## [4] "West South Central" "Pacific"            "Mountain"


# find the elements which match the patter
grep("North", state.division)
##  [1] 13 14 15 16 22 23 25 27 34 35 41 49


# use 'value = TRUE' to show the element value
grep("North", state.division, value = TRUE)
##  [1] "East North Central" "East North Central" "West North Central"
##  [4] "West North Central" "East North Central" "West North Central"
##  [7] "West North Central" "West North Central" "West North Central"
## [10] "East North Central" "West North Central" "East North Central"


# can use the 'invert' argument to show the non-matching elements
grep("North | South", state.division, invert = TRUE)
##  [1]  2  3  5  6  7  8  9 10 11 12 19 20 21 26 28 29 30 31 32 33 37 38 39
## [24] 40 44 45 46 47 48 50
```

###### `grepl()`:
To find a pattern in a character vector and to have logical (TRUE/FALSE) outputs use `grep()`:

{linenos=off}
```r
grepl("North | South", state.division)
##  [1]  TRUE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE  TRUE
## [23]  TRUE  TRUE  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
## [34]  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE FALSE
## [45] FALSE FALSE FALSE FALSE  TRUE FALSE

# wrap in sum() to get the count of matches
sum(grepl("North | South", state.division))
## [1] 20
```

###### `regexpr()`:
To find exactly where the pattern exists in a string use `regexpr()`:

{linenos=off}
```r
x <- c("v.111", "0v.11", "00v.1", "000v.", "00000")

regexpr("v.", x)
## [1]  1  2  3  4 -1
## attr(,"match.length")
## [1]  2  2  2  2 -1
## attr(,"useBytes")
## [1] TRUE
```

The output of `regexpr()` can be interepreted as follows.  The first element provides the starting position of the match in each element.  Note that the value **-1** means there is no match.  The second element (attribute "match length") provides the length of the match.  The third element (attribute "useBytes") has a value TRUE meaning matching was done byte-by-byte rather than character-by-character.

###### Pattern Replacement Functions {#pattern_replacement}
In addition to finding patterns in character vectors, its also common to want to replace a pattern in a string with a new patter.  There are two options for this:

* Replace the first occurrence
* Replace all occurrences

###### `sub()`:
To replace the **first** matching occurrence of a pattern use `sub()`:

{linenos=off}
```r
new <- c("New York", "new new York", "New New New York")
new
## [1] "New York"         "new new York"     "New New New York"

# Default is case sensitive
sub("New", replacement = "Old", new)
## [1] "Old York"         "new new York"     "Old New New York"

# use 'ignore.case = TRUE' to perform the obvious
sub("New", replacement = "Old", new, ignore.case = TRUE)
## [1] "Old York"         "Old new York"     "Old New New York"
```

###### `gsub()`:
To replace **all** matching occurrences of a pattern use `gsub()`:

{linenos=off}
```r
# Default is case sensitive
gsub("New", replacement = "Old", new)
## [1] "Old York"         "new new York"     "Old Old Old York"

# use 'ignore.case = TRUE' to perform the obvious
gsub("New", replacement = "Old", new, ignore.case = TRUE)
## [1] "Old York"         "Old Old York"     "Old Old Old York"
```

##### Splitting Character Vectors
To split the elements of a character string use `strsplit()`:

{linenos=off}
```r
x <- paste(state.name[1:10], collapse = " ")

# output will be a list
strsplit(x, " ")
## [[1]]
##  [1] "Alabama"     "Alaska"      "Arizona"     "Arkansas"    "California" 
##  [6] "Colorado"    "Connecticut" "Delaware"    "Florida"     "Georgia"

# output as a vector rather than a list
unlist(strsplit(x, " "))
##  [1] "Alabama"     "Alaska"      "Arizona"     "Arkansas"    "California" 
##  [6] "Colorado"    "Connecticut" "Delaware"    "Florida"     "Georgia"
```


{#stringr_regex_functions}
#### Regex functions in `stringr`

{#regex_resources}
### Additional Resources


## Additional resources {#add_resources}