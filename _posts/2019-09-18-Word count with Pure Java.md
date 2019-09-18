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

### Enough rambling, let's dive into the code!!!

<div style="background: #272822; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><table><tr><td><pre style="margin: 0; line-height: 125%">1
2
3
4</pre></td><td><pre style="margin: 0; line-height: 125%"><span style="color: #f8f8f2">String</span><span style="color: #f92672">[]</span> <span style="color: #f8f8f2">romanNumerals</span> <span style="color: #f92672">=</span> <span style="color: #f92672">{</span><span style="color: #e6db74">&quot;i&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;ii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;iii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;iv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;v&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;vi&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;vii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;viii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;ix&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;x&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xi&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xiii&quot;</span><span style="color: #f92672">,</span>
                <span style="color: #e6db74">&quot;xiv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xvi&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xvii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xviii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xix&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xx&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxi&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxiii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxiv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxvi&quot;</span><span style="color: #f92672">,</span>
                <span style="color: #e6db74">&quot;xxvii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxviii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxix&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxx&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxi&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxiii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxiv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxv&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxvi&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxvii&quot;</span><span style="color: #f92672">,</span>
                <span style="color: #e6db74">&quot;xxxviii&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xxxix&quot;</span><span style="color: #f92672">,</span> <span style="color: #e6db74">&quot;xl&quot;</span><span style="color: #f92672">};</span>
</pre></td></tr></table></div>








</div>