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

<style>
        .markk {
           background-color: #34495E;
           color: #FDFEFE;
        }       

</style>

<style>
      .img-container {
        text-align: center;
      }
    </style>

# Using Java Stream API to count word frequencies: "Order is the magical word!"

<div style="text-align: justify">
<p>
The first time I encountered the word count problem was in a Big Data course I took. Counting word frequencies and reporting them by descending order, using Hadoop and later on with PySpark. A quick Google search will bring you all the results which are mostly copy pasta-ed from some source and the only difference is the variable names.
</p>
<p>
I love python and programming with python will definitely help you dodge banging your head on the wall or long stares on the screen like you are looking at some ancient scripture (C++)! but there is always the performance issue; The run time even for a small text file using hadoop, and spark, and even using the University's cluster computer system, still was not convincing and did not deliver the fast performance promises. Now that I am more confident with Java, I decided to do the same task using Java on my own personal laptop and details for comparison will be added at the end.
</p>
<p>
For this, we will need Java 8 or greater (Preferably Java 11), your favorite IDE or just run it on terminal, and some familiarity with map-reduce or production & consumption would be sufficient.
Also, having a comprehensive list of stopWords (words that are useless like "the") and since we are dealing with novels and literature texts, a list of roman numerals would be a good starting point.
</p>

</div>

<div style="text-align: justify">
<p>
Most of the novels use roman numerals to divide the chapters in books, so we need to have a list of roman numerals that we will filter or replace. There is nothing special here, except, that an array is not a good idea, we will compare our text against these lists for every line and since searching through an array has complexity of O(n) it will introduce unnecessary overhead.
</p>
</div>

<div class="img-container">

<img id="poster" style="margin: 100; max-width: 100%; text-align:right; " title="Anna Karenina - Russian Movie Poster" src="https://cdn.cinematerial.com/p/500x/gt5rligj/anna-karenina-russian-movie-poster.jpg?v=1456583537" width="200" height="300">
</div>

<p>
I will be using the book titled " Anna Karenina" by Tolstoy from Project Gutenberg. It is about 2MB and has ~15000 lines. This is not really a big file, so I bloated up to have around <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">10 Million</mark> lines and it is around 500 MB txt file. Still not really a big data , but it is good enough to do some big data techniques with personal computer.
</p>

### let's dive into the code!!!


Below is a sample list of roman numerals.For complete list (up to 100), refer to the end of the post.

```java
String[] romanNumerals = {"i", "ii", "iii", "iv", "v", "vi", "vii", "viii"};
```

<div style="text-align: justify">
Now, We don't want to count all the "The"-s, "a"-s and any other useless words. I have gathered a long list of English stop words from multiple sources such as Github, NLTK, etc. here is a small portion of the list:
</div>

```java
String[] stopWords = {"a", "about", "above", "after", "again", "against"};
```

##### Few notes before streaming....

<div style="text-align: justify">
With Java 8, lambdas are introduced in java and we could use <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px"> .forEach(element -> element *2)</mark> method in our list to multiply list elements by 2, instead of writing a for loop; Which is concise, elegant and some might say it is beautiful as well
</div>

:man_facepalming:

```Predicate```


<div style="text-align: justify">
<p>
Before jumping to Predicate, here is a brief note about method reference. When using map, reduce and filter in streaming, we could use anonymous functions aka lambda-s, however, if you already have a well defined class with proper methods we could could method referencing with  colons <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px"><strong>::</strong></mark> which makes code more readable and less error prone.
</p>
<p>If you have a class called Car with a method isFast, we could use <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">Car::isFast</mark> no more (), with our stream stuff. However, what if we are looking for slow cars? This would force us to give up on the method referencing and move back to lambdas <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">
filter( car -> !car.isFast() )
</mark>.

</p>

<p>
Another option would be to go back to class definition and write another method called isSlow(). What if we feel lazy, or in a more concrete scenario, maybe we are not the owner of the class and don't have such privileges? then what? Well, Java 11 introduces Predicate.not() or negate(), to reverse our boolean logic or Predicate.
</p>

</div>

Below is a snippet of Predicate for the "isEmpty()" method is String class. String class has a method to check if the string is empty or not. We could either use snippet below:

```java
Predicate<String> isEmpty = String::isEmpty;
Predicate<String> notEmpty = isEmpty.negate();

stream().filter(notEmpty)
```
or, something like this for the one-liner fans:

```java
Predicate.not(isEmpty)
```

### Starting a Stream and applying filters and maps

<div style="text-align: justify">
Now, We need to get a hold of the file path and feed it to the Files class. I will not dive into this, but we have two options: <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">Files.lines(...)</mark> or <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">Files.readAllLines(..)</mark>; I had better performance with readAllLines(), but feel free to use the other option.
</div>

```java
Files.readAllLines(Paths.get(filePath))
```

### streamSupport & spliterator
<div style="text-align: justify">
There are multiple ways to kick start the stream of data or generate streams, but personally I prefer streamSupport as it gives the parallelization switch in its stream method arguments. other options are stream(), parallelStream() or stream().parallel()
</div>

```java
StreamSupport.stream(Files.readAllLines(Paths.get(filePath)).spliterator(), true);
```
Now, 





### Sequential vs Parallel : an Analogy

<div style="text-align: justify">
<p>
let's say we have 10 athletes and one coach and these athletes will be completing tasks such as doing some jumping and crawling while moving from point A to pint B. In the sequential sense, the coach takes each athlete from A to B, comes back and does the same thing until everyone is at point B.
</p>
<p>
for small number of athletes this is fine: A simple for loop iterating through an array. But what if we have 10 million athletes now, with parallelization, we can think of this as all available coaches provided by the cpu let's say 10, and we partition and assign athletes to these coaches (500 atheletes per coach). each coach takes a team rather than individuals and guides them through the instructions. It become now clear, hopefully, that why parallelization makes processing large files faster.
</p>
</div>

<div class="img-container">
<img id="poster" title="Sequence vs Parallel" src="/assets/images/Runner.png">
</div>


#### Complete list of stop words:






















<!-- <div>
<img  src="https://media.giphy.com/media/vFKqnCdLPNOKc/giphy.gif" width="300" height="200" style="display: block;  margin-left: auto;  margin-right: auto; ">
</div> -->
