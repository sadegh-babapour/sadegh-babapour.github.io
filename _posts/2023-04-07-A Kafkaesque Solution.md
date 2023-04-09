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

## Streaming Realtime Vehicle Positions: City of Calgary

<div style="text-align: justify">
<p>

In this article, I am going to define a problem and propose a solution for it with the new technologies thst are the defacto buzzwords in the tech industry: Kafka, Cloud, Microservices, Docker, Python and more!

This will not only help myself to develop a solution based on my newly acquired skills, but also help my viewers learn by my learning my thought process.

Problem:
Recently after a long time since my last ride with the city of Calgary's transportation services, while I was waiting for my bus, I was trying to find out how I could get some info from their website about any arrival estimates. Of course one option was to text some number and get some info but I was not successful!

After some time I got home and decided to browse the City's website from my computer to crawl some data. alas, I found a page that updates a file every 30 seconds with new positions for all their fleet.

</p>
</div>








<div style="text-align: justify">
<p>
<p>
</div>
<br>
<br>
<div class="img-container">

<img id="poster" style="margin: 150; max-width: 100%; text-align:center; " title="Image Credit: Flat-icons-com" src="https://imgtr.ee/images/2023/04/09/n94mx.png" width="512" height="512">
</div>
<p>
</p>
<br>
<br>
<div style="text-align: justify">
<p>
The Solution:
In order to solve any problems, we need to::
 - first identify the problem
 - a big picture solution
 - bridging the gap
</p>


<p>
What We know: 
- We want to have a "file-downloader" that automatically get the data from the new file
- The file is in Google's Protobuf file format
- We are going to use Python, so we need solutions that can download a file periodically,
  and parse the data into readable format, insert these data into a database, have an ORM like SQLAlchemy to interact with the databse through python, Have a system to stream these data: This is Where Kafka comes into play!
  Have an automated system that with newer updates (inserts into the database), will automatically post these (and only these) data points into our streaming service (Debezium) and let Kafka take care of the rest.

  - finally, have a frontend to display these and showcase all these amazing techniques! 
</p>

</div>


<div class="img-container">
<br>
<br>

<img id="poster" style="margin: 150; max-width: 100%; text-align:center; " title="Clinton Speech" src="https://imgtr.ee/images/2023/04/09/n9Bgc.png" width="600" height="350">
</div>

<div style="text-align: justify">
<p>
</p>
<p>

</p>
<br>
<p>
A quick note

</p>

</div>

Hey Stranger! Thanks for visiting and viewing this article! I promise I will do my best to make this article as useful and as entertaining as possible. Now that the pleasantaries are exchanged let's get back to issue at hand. 

<br>

Thanks for reading!

