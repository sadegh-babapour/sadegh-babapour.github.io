# How To Print Your System's Specification in a nice well formatted HTML

## For Linux neebies and enthusiasts :)

<div class="img-container">

<img id="poster" style="margin: 100; max-width: 100%; text-align:right; " title="Stock Image" src="https://cdn.arstechnica.net/wp-content/uploads/2011/06/deep%20shot.jpeg" width="200" height="300">
</div>


the command line keyword to get the details of your computer
can be retrieved via the lshw command.

You can use the man command like this below to get the terminal style
of the system printed:
man lshw and then by just pressing q you can exit!

Another fact that will make learning linux commands a liitle
less sucky! is learning the idea behind general rules
and then putting the pieces together:

Whenever you need to save something into a file through your terminal
interface using the 
```bash
  >   
```

followed by the the new (destination) file name
like this > myPcSpecs.html or 
```bash
echo "helloWorld!" > firstfile.txt
```

Now back to the article topic, printing the system specifications
on the terminal will not do any one any good except yourself! and this 
data needs to be transferred in some way to other users or potential buyers!

Last week, I was putting an ad on Kijiji to sell my newly acquired yet old Lenovo ThinkPad
and  I realized that same as any Kijiji ad, potentail buyers will start
bombarding the nbox by questions like what are the specs?
What generation the Intel CPU is, is the RAM DDR3 or DDR4, Sata or SSD, and the list goes on and on and on ...!

I learned about the lshw command recently and so decided to actually apply linux command options to serve
some actual purpose other than showing off my Linux Superiority!

used the command:
```bash
 sudo -i
```

to get my writefully given Admin Privileges! and then:
```bash
 lshw -html > /home/myusername/Documents/somerandomDirectory/filename.html
```

and low and behold, there was a nicely formatted HTML file, with all the details
about my computer like its birth certificate and whatnot!

emailed it to my other inbox, and added the info via screenshots on my cellphone!

This one liner code might not seem very important, but it served me a lot of effort
and no more taking pictures of the monitor showing specs like the old times!

Hope you find this mini? micro linux tip helpful!