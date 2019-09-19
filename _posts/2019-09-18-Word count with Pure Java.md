# Using Java Stream API to tackle the infamous word count challenge

<div style="text-align: justify">
<p>
The first time I encountered the word count problem was in the BigData course. Count word frequencies and report them by descending order, using Hadoop and later on with PySpark. A quick Google search will bring you all the results which are mostly copy pasta-ed from some source, I mean as the saying goes the (code) resemeblence was uncanny!
</p>
<p>
I love python and programming with python will definitely help you dodge banging your head on the wall or long stares on the screen like you are looking at some ancient scripture (C++)!; However, the run time even for a small text file, and even using the University's cluster comupter system, still was long. Now that I am more comfident with Java, I decided to do the same task using Java on my own personal laptop.
<p>
For this, we will need Java 8 or greater, your favorite IDE (IntelliJ for sure), and some familiarity with map reduce would be sufficient.
Also, having a comprehensive list of stopWords (words that are useless in a sense like "the") and since we are dealing with novels and literature texts, a list of roman numerals would be a good starting point.
</p>
</div>
### Enough rambling, let's dive into the code!!!
<p>
Nothing special here, except, that an array is not a good idea, since searching through an array has complexity of O(n). This might not seem significant, however, since we will check against this list over and over for every line, will add extra unnnecessary overhead.
</p>

```html
<link rel="stylesheet" href="/C:/Users/TheMightyLobster/Documents\Personal_Website/sadegh-babapour.github.io/_posts/code highlight/monokai-sublime.css">
<script src="/C:/Users/TheMightyLobster/Documents/Personal_Website/sadegh-babapour.github.io/_posts/code highlight/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

```html

<pre><code class="java">
Set<String> romanNumeralsSet = new HashSet<>(Arrays
.asList(romanNumerals));
        
Set<String> badwords = new HashSet<>(Arrays.asList(stopWords));


</code></pre>
```