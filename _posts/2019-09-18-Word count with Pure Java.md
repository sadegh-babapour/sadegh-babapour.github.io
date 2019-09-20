<style>
        .center {
          margin: auto;
          width: 100%;
          font-size: 18PX;
          /* border: 3px solid #73AD21; */
          padding: 10px;
        }
        /* .center:hover{
          background-color: #34495E;
          color : #FDFEFE;
          } */

        </style>

# Using Java Stream API to count word frequnecies: "Order is the magick word!"

<div style="text-align: justify">
<p>
The first time I encountered the word count problem was in the Big Data course. Count word frequencies and report them by descending order, using Hadoop and later on with PySpark. A quick Google search will bring you all the results which are mostly copy pasta-ed from some source, I mean as the saying goes the (code) resemblance was uncanny!
</p>
<p>
I love python and programming with python will definitely help you dodge banging your head on the wall or long stares on the screen like you are looking at some ancient scripture (C++)!; However, the run time even for a small text file, and even using the University's cluster computer system, still was long. Now that I am more confident with Java, I decided to do the same task using Java on my own personal laptop.
</p>
<p>
For this, we will need Java 8 or greater, your favorite IDE (IntelliJ for sure), and some familiarity with map reduce would be sufficient.
Also, having a comprehensive list of stopWords (words that are useless in a sense like "the") and since we are dealing with novels and literature texts, a list of roman numerals would be a good starting point.
</p>

</div>

### let's dive into the code!!!

<div style="text-align: justify">
<p>
First we need to have a list of roman numerals that we will filter or replace.
Nothing special here, except, that an array is not a good idea, since searching through an array has complexity of O(n). This might not seem significant, however, since we will check against this list over and over for every line, will add unnecessary overhead. This will be revisited.

For complete list (up to 100), refer to the end of the post.
</p>
</div>

```java
String[] romanNumerals = {"i", "ii", "iii", "iv", "v", "vi", "vii", "viii"};
```
<div style="text-align: justify">
Now, We don't want to count all the "The"s, "a"s and any other useless words. I have gathered a long list of English stop words from multiple sources such as Github, nltk, etc. here is a small portion of the list:
</div>

```java
String[] stopWords = {"a", "about", "above", "after", "again", "against"};
```

`Predicate`
<div style="text-align: justify">
Before jumping to Predicate, here is a brief note about method reference. When using map, reduce and filter in streaming, we could use anonymous functions aka lambda-s, however, if you already have a well defined class with proper methods we could could method referencing with <mark style="background-color: lightgrey"><strong>::</strong></mark> which makes code more readable and less error prone.
</div>

<div style="text-align: justify">
<p>If you have a class called Car with a method isFast, we could use ```Car::isFast``` with our stream stuff. but what if we are looking for slow cars? This would force us to give up on the method referencing and move back to lambdas <mark style="background-color: lightgrey"><strong>filter(car -> !car.isFast())</strong></mark>.
</p>
<p> 
Another option would be to go back to class definition and write another method called isSlow. What if we feel lazy, or in a more concrete scenario, maybe we are not the owner of the class and don't have such privileges? then what? Well, Java 11 introduces Predicate.not() or negate(), to reverse our boolean logic or Predicate.
</p>
</div>

Below is a snippet of Predicate for empty method is String class.

```java
Predicate<String> isEmpty = String::isEmpty;
Predicate<String> notEmpty = isEmpty.negate();
```

### Starting a Stream and applying filters and maps
<div style="text-align: justify">
We need to get a hold of the file path and feed it to the Files class. I will not dive into this, but we have two options: ``Files.lines(...)`` or ``Files.readAllLines(..)``, I had better performance with `readAllLines()`, but feel free to use the other option.
</div>

```java
Files.readAllLines(Paths.get(filePath))
```

### streamSupport & spliterator
<div style="text-align: justify">
There are multiple ways to kick start the stream of data, but personally I prefer streamSupport as it gives the parallelization switch in its stream method arguments.
</div>

```java
StreamSupport.stream(Files.readAllLines(Paths.get(filePath)).spliterator(), true);
```

<script src="https://gist.github.com/sadegh-babapour/9868572630416be8f7efaf79a892f8d2.js"></script>














###### Complete list of stop words:

