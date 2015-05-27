---
title: Remotest smart light bulb in the UK!
layout: posts
categories: work news
imageSource: /assets/Knoydart/seal.jpg
summary: Short blog explaining how did we develop the first remotest light bulb in the mainland UK.
---

#Phillips Light bulb diary:

##How it all came together:
In August 2014 [Jamie Cross][Jamie] Co-investigator of [Off the Grid][offgird] asked [myself][Hadi] and [Margus Lind][Margus] to join another project that he was leading in collaboration with Knoydart Foundation, Local Energy Scotland and Community Energy Scotland.

After developing a few data visualisation to try and find a way to translate the data from this tracker
![Hydro turbine](/assets/Knoydart/tracker.jpg)

into something meaningful for the community.
Sitting in Knoydart bunkhouse the three of us joined by [Kyle Smith][Kyle] operations manager of [Knoydart Foundation][Knoydart Foundation] and [Benny Talbot][Benny] from Community Energy Scotland.
We were trying to find a way to engage with the community and tourists about how to have better awareness of energy consumption to avoid power cuts and develop ways to use the surplass energy.

After a couple of hours of writing down ideas and sketches, [Kyle][Kyle] mentioned if we have seen the new Philips smart light bulbs. Neither of us have heard about them before, couple of more hours of research and the next day [Margus][Margus] developed the first hack of the light bulb, in our last night in Knoydart we managed to install it on one of the computers in the library to control the light bulb and change color according to the community energy consumption.
And this is what we left there for 3 month.

![Hydro turbine](/assets/Knoydart/philips-library.jpg)



##First evaluation:

When we went back to Knoydart at the end of same year, we found out that not many people knew about our [Power of knoydart website][powerofknoydart] and also all the lights in the library have been removed.

Not knowing about our website was because of reasons as simple:
- some lost the link to the page
- some didn't know about it
- some found it unreliable that the page was often down

The lights being moved from the library was again because our script lost communication with data server and the light bulb.

##Iteration:

To solve the advertising problem [Jamie][Jamie] printed out 300 water proof stickers that could be put in different locations as a point of reference. We've got one on our meeting room at Design informatics, here at University of Edinburgh!

To solve the reliability problem, we asked [Chris Barker][chrisbarker] to join our team. the two of us developed a new and clean page for [Power of Knoydart][powerofknoydart], made it more resilient and we all advertised it on any medium that we could like twitter, facebook and so on.

The last problem was to light the light bulb. We needed to bring it to somewhere more public and in the view of both tourists and the community.
We made a new version of the code which is documented and accessible on here: [Power of Knoydart][githubknoydart]



##Open source code and documentation:
I will publish the Phillips python code on the [Power of Knoydart website][githubknoydart] in the next couple of days.
Here's how to do it yourlself:

###Here's how we hacked the Phillips light:
Get yourself a:
- Raspberry pie (Instead of using a big computer, use one of these to save power and easy installation)
- Phillips Hue
- Miniature wifi (You need this to connect to the internet)


First You need to install a couple of libraries on the Raspberry pie in order to talk to the Phillips Hue light bulb.

Here's the list of libraries:
- interp (You need this to convert numerical ranges, for example in our case if we're using 10kwh of electricity convert it to a range of green and if it's more that 140 - 180 convert it to a range of red)
- phue (This is the imporatant library that let's you speak with the smart bulb)

That's it. Install these by [googling][google] or [ducking][duckduckgo] their name and python together.

And here's the simple code:

```python
b = phue.Bridge('IP address of the Phillips Hue')#You can get it from their online console.
     b.connect()
     b.get_api()
     r=0
     sys.stdout.flush()
```

And then, changing light bulbs color according to your data, in my case it was a number between 0-180
```python
if num>150 and num<=180: #num has the number I mentioned, the data you want to visualise into light colors.
    hue=int(interp(r,[150,180],[5000,1])) #interp will interpolate the values to the range I want here is red
elif num>100 and num<=150:
    hue=int(interp(r,[100,150],[15000,10000]))#here is yellow
elif num>0 and num<=100:
    hue=int(interp(r,[0,100],[30000,25000]))#green
bri=255 #brightness of the light
sat=254 #saturation of the light
lights = b.lights #this will give me the list of light bulbs to iterate through
command = {#preparing a command to send to the Phillips Hue controller
        'transitiontime': 3,#how long to take to change to a new color.
        'on': True,
        'bri': 255,
        'hue': hue,
        'sat': 255
    }
for l in lights:#iterate through and change colors.
    print(l.name)
    b.set_light(l.name, command)
time.sleep(10)
```




##Reflections:






[Jamie]:http://www.sps.ed.ac.uk/staff/social_anthropology/cross_jamie
[Hadi]:http://hadi.link
[Margus]:https://github.com/modulo-
[offgrid]:http://lifeoffthegrid.net
[Kyle]:http://www.knoydart-foundation.com/about/about-the-foundation/knoydart-renewables/
[Knoydart Foundation]:http://www.knoydart-foundation.com/
[Benny]:http://www.communityenergyscotland.org.uk/our-team.asp?id=29
[powerofknoydart]:http://powerofknoydart.org/
[trackercode]:https://github.com/Mehrpouya/PowerOfKnoydart/tree/master/TrackerCode
[chrisbarker]:https://github.com/TerribleBugger
[githubknoydart]:https://github.com/Mehrpouya/PowerOfKnoydart
[duckduckgo]:http://duckduckgo.com
[google]:http://google.com
