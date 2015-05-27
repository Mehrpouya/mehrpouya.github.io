---
title: Remotest smart light bulb in the UK!
layout: posts
categories: work news
imageSource: /assets/Knoydart/herald.png
summary: The Phillips Light Bulb project was recently featured in the Herald Scotland and Deadline News, and as part of the development team behind the work, and thought it would be helpful to document how the project was developed and the code behind the project.
---

#Phillips Light Bulb Diary:

The Phillips Light Bulb project was recently featured in the [Herald Scotland][herald] and [Deadline News][deadlinenews], and as part of the development team behind the work, and thought it would be helpful to document how the project was developed and the code behind the project.

## How it all came together:

In August 2014 [Jamie Cross][Jamie] Co-investigator of [Off the Grid][offgrid] asked [myself][Hadi] and [Margus Lind][Margus] to join another project that he was leading in collaboration with [Knoydart Foundation][knoydartfoundation], [Local Energy Scotland][localenergyscot] and [Community Energy Scotland][communityenergy].

After developing a few data visualisations to try and find a way to translate the data from this tracker, and fragility of this micro-hydro into something more meanngful for the community without creating panic.

![Tracker](/assets/Knoydart/tracker.jpg) ![Hydro](/assets/Knoydart/hydro.jpg)

Sitting in the [Knoydart bunkhouse][bunkhouse] the three of us joined by [Kyle Smith][Kyle] operations manager of [Knoydart Foundation][knoydartfoundation] and [Benny Talbot][Benny] from Community Energy Scotland.
We were trying to find a way to engage with the community and tourists about how to have better awareness of energy consumption to avoid power cuts and develop ways to use the surplass energy.

After a couple of hours of writing down ideas and sketches, [Kyle][Kyle] mentioned if we have seen the new [Phillips smart light bulbs][phillipshue]. None of us have heard about them before, however after a few more hours of research, the next day [Margus][Margus] developed the first hack of the light bulb. We managed on our last night in Knoydart to install it on one of the computers in the community library. The purpose was to change the color of the light bulb according to the community energy consumption.

And this is what we left there for the following 3 months.

![Phillips Hue in the library](/assets/Knoydart/philips-library.jpg)



##First evaluation:

We ran focus groups, interviewed people and made various versions of our visualisations. Finally the community were happy with three final designs.

![focus group](/assets/Knoydart/first-focusgroup.jpg)


1:Smart light bulb

2:The new [Power of knoydart website][powerofknoydart]

3:Data kettle (a widget for windows computers that shows the amount of energy consumption as a water level in a kettle).


When we went back to Knoydart at the end of same year, we found out that not many people knew about our [Power of knoydart website][powerofknoydart]. Furthermore all of the lights in the library have been removed, and sadly not many people installed our kettle on their computers.

Not knowing about our website or the kettle widget was because of reasons as simple as: some lost the link to the page, some didn't know about it, and some found it unreliable that the page was often down.

The lights being moved from the library was again because our script lost communication with data server and the light bulb.


##Iterations:

To solve the advertising problem [Jamie][Jamie] printed out 300 water proof stickers that could be put in different locations as a point of reference. We've got one on our meeting room at Design informatics, here at University of Edinburgh!

To solve the reliability problem, we asked [Chris Barker][chrisbarker] to join our team. The two of us developed a new and clean page for [Power of Knoydart][powerofknoydart], made it more resilient and made an easy installation for the [data kettle][datakettle] and we all advertised it on any medium that we could like twitter, facebook and so on.

The last problem was to light the light bulb. We needed to bring it to somewhere more public and in the view of both tourists and the community.
We made a new version of the code which is documented and accessible on here: [Power of Knoydart][githubknoydart]



##Open source code and documentation:
I will publish the Phillips python code on the [Power of Knoydart website][githubknoydart] in the next couple of days.
Here's how to do it yourself:

##Here's how we hacked the Phillips light:
Get yourself a:

**Raspberry pie** (Instead of using a big computer, use one of these to save power and easy installation)

**Phillips Hue**

**Miniature wifi** (You need this to connect to the internet)


First You need to install a couple of libraries on the Raspberry pie in order to talk to the Phillips Hue light bulb.

Here's the list of libraries:

**interp** (You need this to convert numerical ranges, for example in our case if we're using 10kwh of electricity convert it to a range of green and if it's more that 140 - 180 convert it to a range of red)


**phue** (This is the imporatant library that let's you speak with the smart bulb)

That's it. Install these by [googling][google] or [ducking][duckduckgo] their name and python together.

And here's the simple code:

```python
'''IP address of the Phillips Hue'''
b = phue.Bridge(0.0.0.0)
'''You can get it from their online console.'''
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



[Jamie]:http://www.sps.ed.ac.uk/staff/social_anthropology/cross_jamie
[Hadi]:http://hadi.link
[Margus]:https://github.com/modulo-
[offgrid]:http://lifeoffthegrid.net
[Kyle]:http://www.knoydart-foundation.com/about/about-the-foundation/knoydart-renewables/
[knoydartfoundation]:http://www.knoydart-foundation.com
[Benny]:http://www.communityenergyscotland.org.uk/our-team.asp?id=29
[communityenergy]:http://www.communityenergyscotland.org.uk
[powerofknoydart]:http://powerofknoydart.org/
[trackercode]:https://github.com/Mehrpouya/PowerOfKnoydart/tree/master/TrackerCode
[chrisbarker]:https://github.com/TerribleBugger
[githubknoydart]:https://github.com/Mehrpouya/PowerOfKnoydart
[duckduckgo]:http://duckduckgo.com
[google]:http://google.com
[localenergyscot]:http://www.localenergyscotland.org
[phillipshue]:http://www2.meethue.com/en-gb/
[bunkhouse]:http://www.knoydart-foundation.com/bunkhouse/about-the-bunkhouse/
[datakettle]:http://www.powerofknoydart.org/downloads.html
[herald]:http://www.heraldscotland.com/news/home-news/the-power-of-knoydart-lights-up-community.127060619
[deadlinenews]: http://www.deadlinenews.co.uk/2015/05/26/lightbulb-warns-remote-community-of-power-blackout/
