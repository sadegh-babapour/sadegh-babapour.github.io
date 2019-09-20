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

<div style="text-align: justify">
<p>
I will be using the book titled " Anna Karenina" by Tolstoy from Project Gutenberg. It is about 2MB and has ~15000 lines. This is not really a big file, so I bloated up to have around <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">10 Million</mark> lines and it is around 500 MB txt file. Still not really a big data , but it is good enough to do some big data techniques with personal computer.
</p>
</div>

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

### Few notes before streaming....

<div style="text-align: justify">
With Java 8, lambdas are introduced in java and we could use <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px"> .forEach(element -> element *2)</mark> method in our list to multiply list elements by 2, instead of writing a for loop; Which is concise, elegant and some might say it is beautiful as well
</div>

:man_facepalming:


#### Predicate

<div style="text-align: justify">
<p>
Before jumping to Predicate, here is a brief note about method reference. When using map, reduce and filter in streaming, we could use anonymous functions aka lambda-s, however, if you already have a well defined class with proper methods we could could method referencing with  colons <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px"><strong>::</strong></mark> which makes code more readable and less error prone.
</p>
<p>
If you have a class called Car with a method isFast, we could use <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">Car::isFast</mark> no more (), with our stream stuff. However, what if we are looking for slow cars? This would force us to give up on the method referencing and move back to lambdas <mark style="background:#34495E; color: #FDFEFE; font-weight:bold; font-size:16px">filter( car -> !car.isFast())</mark>.
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

For any computation over a sequence of data, we need an iterator. Spliterator is the object for traversal and partitioning of source elements in stream related operations. A Spliterator may traverse elements individually or **`sequentially in bulk`**.


## Let's build that (Data) pipeline :steam_locomotive:

We are streaming lines of strings, we need to lowercase the words, and replace useless keywords with whitespace

1- The first step is making everything lowercase:
The **`Punct`** is regex stuff for removing punctuation, with a quick StackOverFlow, you can get more info on that:

```java
.map(aLine -> aLine.toLowerCase()
    .replaceAll("\\p{Punct}", " ")
    .split("\\s"))
```

2- in this step, we will use the flatMap, for processing array of strings created by the split funktion: The reason for using flatMap instead of map is that we need to work with the individual string (words) now and using map will carry on with the array of strings.

```java
.flatMap(Arrays::stream)
```

3- In this part, we need to do some filtering: remove chapters, roman numerals and the stop words:

```java
.filter(x -> !x.startsWith("chapter"))
```

Unlike Python :snake:,There is no **`in`** keyword in Java :coffee:, but we need to filter our entries if they exist **`in`** a certain list:

```python
children_list = ["Chrystal", "Umar", "Charity", "Kyle", "Justin"]
naughty_list = ["Chrystal", "Umar"]

for child in children_list:
    if child in naughty_list:
        print(child,"--> No gifts for you!")
    else:
        print(child, ": you'll get an X-Box")
```

Instead, we will use a lambda function and filter them with the NoneMatch method, which returns true if none of the stream elements match the given predicate: (equals) in this case:

```java
.filter(entry -> Arrays.stream(romanNumerals).noneMatch(entry::equals))
.filter(entry -> Arrays.stream(stopWords).noneMatch(entry::equals))
.filter(notEmpty)
```

4- Now, it is time to put these outputs into a dictionary or a Map:
We will assign count 1, to all the words coming out of the previous process and map them into (Key:Value) format like "Anna":1.

```java
.map(word -> new AbstractMap.SimpleEntry<>(word, 1))
```
5- It's collecting time:
for all the entries in the map from previous step, we need to get a hold of its key, and sum the values if the kays are equal and map them to a new dictionary (Key: finalCount):

```java
.collect(toMap(AbstractMap.SimpleEntry::getKey, AbstractMap.SimpleEntry::getValue, Integer::sum))
```

6- We will stream this dictionary's entries, and sort them based on the entry's value. We can take advantage of the stream sorted method. The comparingByValue of the Dictionary(Map) item's value, as name suggests compares the values rather than keys, and the reverseOrder, returns the opposite of the comPareByValue comparator and everything gets sorted at the end. Next, we do another collect here, to another Dictionary, a different type of dictionary.

Dictionaries or Maps are not ordered collections, LinkedHashMaps in Java, however, are list like dictionaries in which there is order. we get the entries from previous and create new entries based on the value from the (key,value) pair.

```java
.sorted(Collections.reverseOrder(Map.Entry.comparingByValue()))
    .collect(
        toMap(Map.Entry::getKey,
            Map.Entry::getValue,
            e1, e2) -> e2, LinkedHashMap::new))
```

7- (Almost) Final step: print the elements of the new ordered dictionary

```java
.forEach((k, v) -> System.out.println((k + "," + v)));
```

# Order is the magic word!
Days after, finishing the project, there was some kind of an "Aha" moment. before brushing it off as something not so significant, I changed only 2 lines of code, increased the file size and voila, it was indeed something worth doing.

A- As mentioned before, we had two *`list`*s, But we just need them to filter data, we don't care about if the container is an ordered one (hence a list) or not.
So, I changed the lists into sets:

```java
Set<String> romanNumeralsSet = new HashSet<>(Arrays.asList(romanNumerals));
Set<String> badwords = new HashSet<>(Arrays.asList(stopWords));
```  

And therefore, the following changes followed through:

```java
.filter(Predicate.not(romanNumeralsSet::contains))
.filter(Predicate.not(badwords::contains))
```

for the small size files, there was still a huge increase in speed, but with the final file having almost 10 million lines, the first one took so long that it had to be put down :ambulance:.

### Sequential vs Parallel : an Analogy

<div style="text-align: justify">
<p>
let's say we have 10 athletes and one coach and these athletes will be completing tasks such as doing some jumping and crawling while moving from point A to pint B. In the sequential sense, the coach takes each athlete from A to B, comes back and does the same thing until everyone is at point B.
</p>
<p>
for small number of athletes this is fine: A simple for loop iterating through an array. But what if we have 10 million athletes now, with parallelization, we can think of this as all available coaches provided by the cpu let's say 10, and we partition and assign athletes to these coaches (500 athletes per coach). each coach takes a team rather than individuals and guides them through the instructions. It become now clear, hopefully, that why parallelization makes processing large files faster.
</p>
</div>

<div class="img-container">
<img id="poster" title="Sequence vs Parallel" src="/assets/images/Runner.png">
</div>


and finally table below is the top 5 frequent words:
if you are have way through the book like me, we can expect to read more about levin, who is the underdog in the story so far!

<table>  
<thead><tr><th>Word</th><th>Frequency</th></tr></thead>  <tbody>  <tr>
<td>levin</td><td>405972</td></tr>  <tr>
<td>vronsky</td><td>215460</td></tr>  <tr>
<td>anna</td><td>206136</td></tr>
<td>kitty</td><td>169092</td></tr>  <tr>
<td>alexey</td><td>158760</td></tr>
  </tbody> 
   </table>

The real numbers are 1611, 855, ... which means the files was copied over 252 times.

### Full code here abbreviated the long lists:

```java
public class Application3
{


    public static void main(String[] args)
    {
        long zer0 = System.nanoTime();
        String filePath = "C:/Users/TheMightyLobster/IdeaProjects/myKalk2/src/com/babapour" +
                "/AnnaKrennina.txt";

        String[] romanNumerals = {"i", "ii", "iii", "iv", "v", "vi", "vii"};


        String[] stopWords = {"a", "about", "above", "after", "again", "against",
                "am", "an", "and", "any", "are", "aren", "aren't", "as", "at", "be", "because"};

        Set<String> romanNumeralsSet = new HashSet<>(Arrays.asList(romanNumerals));
        Set<String> badWords = new HashSet<>(Arrays.asList(stopWords));

        Predicate<String> isEmpty = String::isEmpty;
        Predicate<String> notEmpty = isEmpty.negate();

        System.out.println("map after sorting by values in descending order: \n");

        try
        {
            StreamSupport.stream(
                    Files.readAllLines(
                            Paths.get(filePath))
                            .spliterator(), true)
                    .map(aLine -> aLine.toLowerCase()
                            .replaceAll("\\p{Punct}", " ")
                            .split("\\s"))
                    .flatMap(Arrays::stream)
                    .filter(x -> !x.startsWith("chapter"))
                    .filter(Predicate.not(romanNumeralsSet::contains))
                    .filter(Predicate.not(badWords::contains))
                    .filter(Predicate.not(isEmpty))
                    .map(word -> new AbstractMap.SimpleEntry<>(word, 1))
                    .collect(toMap(AbstractMap.SimpleEntry::getKey,
                            AbstractMap.SimpleEntry::getValue, Integer::sum))
                    .entrySet()
                    .stream()
                    .sorted(Collections.reverseOrder(Map.Entry.comparingByValue()))
                    .collect(toMap(Map.Entry::getKey, Map.Entry::getValue,
                            (e1, e2) -> e2, LinkedHashMap::new))
                    .forEach((k, v) -> System.out.println((k + "," + v)));
        } catch (IOException e)
        {
            e.printStackTrace();
        }
        long end = System.nanoTime();
        System.out.println((end - zer0));
    }
}
```




#### link to full source code:
https://github.com/sadegh-babapour/LargeFileProcessing/blob/master/src/com/babapour/Application3.java
























<!-- <div>
<img  src="https://media.giphy.com/media/vFKqnCdLPNOKc/giphy.gif" width="300" height="200" style="display: block;  margin-left: auto;  margin-right: auto; ">
</div> -->
