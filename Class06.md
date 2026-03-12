# Class 6: functions
Rebecca ( Pid 17228385)

## Background

All functions in R have at least 3 things:

-   A **name** that we use to call the function.
-   One or moore input **arguments**
-   The **body** the lines of R code that do the work

## Our first function

Let’s write a silly wee function called `add()` to add some numbers (
the input arguments)

``` r
add <-  function(x,y) {x+y}
```

Now we can use this function

``` r
add( x=10, y=10)
```

    [1] 20

``` r
add(100,1)
```

    [1] 101

``` r
add(c(100,1,100), 1)
```

    [1] 101   2 101

> Q. What if I give a multiple element vector to `x` and `y`?

``` r
add(c(100,1), y=c(100,1))
```

    [1] 200   2

> Q. What if I give three inputs?

``` r
#add x=c((100,1),y=1, z=1)
```

> Q. What if I give only one input to the add function?

``` r
addnew <-  function(x,y=1) {x+y}
```

``` r
addnew(x=100)
```

    [1] 101

``` r
addnew(c(100,1),100)
```

    [1] 200 101

If we write our function with input arguments having no default value
then the user will be required to set them when they use the function.
We can give our input arguments “default” values like setting them equal
to some sensible value - e.g y=1 in the `add()`

## A second function

Let’s try something more interesting: Make a sequence generating tool…

The `sample()` function can be a useful starting point here:

``` r
sample(1:10, size=4)
```

    [1] 5 9 8 7

> Q. Generate 9 random vectors taken from the input vector x=1:10?

``` r
sample(1:10, size=9)
```

    [1]  2  4  3  6  1  5  9 10  8

> Q. Generate 12 random vectors taken from the input vector x=1:10?

``` r
sample(1:10, size=12, replace = TRUE)
```

     [1]  7  9 10  8  7  3  3 10  2  9  4  6

> . Q. Write code for the `sample` function that generates nucleotide
> sequences of length 6?

``` r
sample(c("A","C","T","G"), size = 6, replace = TRUE)
```

    [1] "G" "G" "C" "T" "G" "G"

> Q. Write a first function `generate_dna()` that returns a *user
> specified* length DNA sequence:

``` r
generate_dna <- function(x=6){sample(c("A","C","T","G"), size = x, replace = TRUE)}
```

``` r
generate_dna(100)
```

      [1] "T" "G" "G" "C" "T" "A" "G" "A" "G" "G" "C" "G" "C" "C" "C" "G" "G" "T"
     [19] "C" "C" "G" "C" "C" "G" "T" "A" "G" "C" "T" "T" "C" "C" "G" "A" "A" "T"
     [37] "G" "G" "C" "C" "T" "C" "G" "T" "A" "T" "G" "G" "T" "T" "T" "C" "A" "A"
     [55] "C" "T" "T" "C" "C" "T" "C" "A" "G" "G" "T" "G" "A" "C" "T" "A" "C" "G"
     [73] "T" "G" "A" "T" "G" "T" "T" "A" "A" "G" "G" "G" "G" "T" "G" "C" "T" "A"
     [91] "G" "C" "G" "T" "G" "T" "G" "C" "C" "A"

> **Key Points**

Every function in R looks fundamentally the same in the terms of its
structure. Basically 3 things name, input and body

    name <- function(input) {body}

> Functions can have multiple inputs. These can be **required**
> arguments or **optional** arguments. With optional arguments having a
> set value.

> Q. Modify and improve our `generate_dna()` function to return it’s
> generated sequence in a more standard form like”AGTAGTA” rather than
> the vector “A”, “C”, “G”, “A”,

``` r
generate_dna <- function(x=6, fasta =TRUE){ ans <- sample(c("A","C","T","G"), 
                                     size = x, 
                                     replace = TRUE)
if(fasta){
  cat("Single-element vector output")
  ans <- paste(ans, collapse = "")
} else {
    cat("Multi-element vector output")
  }
return(ans)}

generate_dna(,FALSE)
```

    Multi-element vector output

    [1] "C" "A" "T" "C" "T" "C"

``` r
generate_dna(fasta=TRUE)
```

    Single-element vector output

    [1] "AGCCCA"

The `paste()` function it’s job is to join up or stick together(a.k.a
paste) input strings together

``` r
paste(c("alice","barry"), "loves R", sep ="****")
```

    [1] "alice****loves R" "barry****loves R"

Flow control means where the R brain goes in your code

``` r
good_mood <- TRUE

if(good_mood)  {cat("Great!")
} else {
    cat("Bummer!")}
```

    Great!

## A Protein generating fuction

> Q. Write a function that generates a user specified length protein
> sequence.

There are 20 natural amino-acids

``` r
aa <- c("A","R","N","D","C","E","Q","G","H","I","L","K","M","F","P","S","T","W","Y", "V")
```

``` r
generate_protein <- function(len) {
  # The amino-acids to sample from
  aa <- c("A","R","N","D","C","E","Q","G","H","I","L","K","M","F","P","S","T","W","Y", "V")
  # Draw  n=len amino acids to make our sequence
ans <- sample(aa, size = len, replace = T) 
ans <- paste(ans, collapse = "")
return(ans)
}
```

``` r
myseq <- generate_protein(42)
myseq
```

    [1] "KFCQCRRTQCRGMPGLMLAVWGEMELKASERDQNDMGQSHCN"

> Q. Use that function to generate random protein sequences between
> length 6 and 12

``` r
generate_protein(6)
```

    [1] "QERACL"

``` r
generate_protein(7)
```

    [1] "HPLEEHT"

``` r
generate_protein(8)
```

    [1] "HIPNHLFP"

``` r
generate_protein(9)
```

    [1] "GSRLRFYHR"

``` r
generate_protein(10)
```

    [1] "HGAGNYNPIW"

``` r
generate_protein(11)
```

    [1] "FKYHVVVDASE"

``` r
generate_protein(12)
```

    [1] "LASNSAYVPARN"

``` r
for(i in 6:12) {
  # FASTA ID line ">id"
  cat(">",i, sep = "", "\n")
  # Protein sequence line
  cat(generate_protein(i),"\n")}
```

    >6
    YDTRWH 
    >7
    SAPSIYP 
    >8
    DHQQYGLE 
    >9
    FCWFNCPPI 
    >10
    HANGMIFWLE 
    >11
    VDIKHNAEKEY 
    >12
    RFYVQGSRCPMD 

> Q. Are any of your sequencing unique i.e not found anywhere in nature?

The 6,7,8 are not unique but the 9,10,11,12 are unique
