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

## Why Regression & not Classification

<div style="text-align: justify">
<p>
it's been quite a while since I have learned the concept of logistic regression;it is a modeling technique that regressively models phenomena that have limited countable or better put, discrete outcome. it's a supervised learning technique so we gather as much sensor data (read information) 
as we can to be able to answer questions like is it going to rain or not? will that be eligible to get a loan from the bank or not...you get the idea!
</p>



<div style="text-align: justify">
<p>
here is a random picture of logistic regression visualization before we move on!
<p>
</div>

<div class="img-container">

<img id="poster" style="margin: 150; max-width: 100%; text-align:center; " title="Logistic Regression source: Deep Learning By Example" src="https://static.packt-cdn.com/products/9781788399906/graphics/f7b79fda-3d03-45ba-94d2-83d989499cca.png" width="600" height="300">
</div>
<p>
</p>

<div style="text-align: justify">
<p>
So the first question that pops into the mind is that: hol' on! wait a minute! if there is no relationship or inter/extra-polation between the current outcome and the next, how on earth this still is considered a regression technique!! Although you can go to StackOverflow or similar Q&A forums to get a better scientific answer, the answer is that in a way we actually do regressively predict results probabilty, however, at the last step we use a special function called sigmoid as a gate to do the heavy lifting for us! the regression is for the probability factors, so if we predict the value of some outcome to be 79%,  for certain conditions, we can have an arbitrary probabilityvalue of 79.56789 for another prediction. It is just that after having all these probability numbers, we just pass them through a filter to name some predictions as true and some to be false based on a threshold or a cut-off! Our prediction is simply a probabilty value rather than what class the outcome belongs too!
</p>

<p>
that's why it is not a classification problem and it's a regression thing!back to the title! if you have some sort of interest in the media, you sure have heard of the Clinton–Lewinsky scandal! and it was a hot topic back in its time!whether he had s**ual relations with that woman or not (although we all know the truth, I guess) could be solved by a logistic regression approach!in regression problems, we gather historic information and based on certain independent factors values, we predict a certain value which could be changed by tweaking one or many of the independent values!same thing happens with logistic regression, if we after careful study and based on some criteria come up with a ceretain probability value for a result, again by tweaking one of some of the impacting factors, we can come up with the new/different value of how probable that outcome is! at the end of the day, to justify the prediction, we can say that based on such and such we believe it will rain or whether the said allegation happened or not but here is the twist! we are only certain about this prediction with a probability of X: simply put, we believe that the result is MoniCAN but we are only 75% percent sure, and we used a threshold of 70%, so any outcome with a probability of less than 70% is MoniCAN't!
</p>

</div>


<div class="img-container">

<img id="poster" style="margin: 150; max-width: 100%; text-align:center; " title="Clinton Speech" src="https://www.mcall.com/resizer/K5tCFPrr3xTq5KYWc840Xrh7w8Y=/800x638/top/arc-anglerfish-arc2-prod-tronc.s3.amazonaws.com/public/OESKO5JLSQNBZJLRU5BASJOZUQ.jpg" width="400" height="320">
</div>

<div style="text-align: justify">
<p>
</p>
<p>

</p>
<p>
At the end of the day, it all comes to this: Where do you draw the line! or how sure is a good enough answer! The real beauty of this and probably the actual application of this is that in IRL short for in real life, we are tryingto come up with a better answer for our threshold! by gathering more info we can say that we are either less likely lowering the standards or raising the bar! so when the scandal first came out, the probability of it happening was probabl around 40% and people liked Clinton, so the bar was higher! then his apology video comes up and obviously wasn't as sincere as people expected but then again who has ever seen a sincere apology from any political or public person, specially considering so much is as stake! back to earth, this both increased the predicted probability
of outcome as well as lowering the threshold that such blasphemy! indeed could happen behind the Pentagon doors just like small office in a city like Calgary. So, finally one can say that Monican with a good 85-90% certainty and anything scandalous with with about 80% conviction is a 100% in our books!!  
</p>

</div>
Thanks for reading!

