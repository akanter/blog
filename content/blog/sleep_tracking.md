+++
author = "Aaron Kanter"
categories = []
date = 2018-08-20T11:06:12-04:00
description = "Gluing all of the pieces together"
linktitle = ""
featured = ""
featuredpath = ""
featuredalt = ""
title = "Quantifiably sleeping better with Android"
type = "post"
+++

The most harrowing months of my work career were no doubt in the first half of 2018 in the run up to the legally required implementation date of the General Data Protection Regulation (GDPR) in the European Union. The product requirements changed if you sneezed at the wrong time, there was no room for error, and the launch date was immovable. There was a lot of work to do with too little time in which to accomplish it and many of our product testers were located in India, meaning that one of us frequently needed to be available at odd hours to help unblock them. I may write up a post later about some of my learnings about the project, but in this particular piece I want to talk about **sleep**.

Throughout those months, my sleep became very erratic. Two hours in the morning. An hour later on the bus. Sleep another two after an evening of more coding. Wake up and work for a few more hours. Repeat this whole process for weeks, including the weekends which were relieving only because there weren't any meetings or time spent commuting. It's no surprise that this wasn't doing great things for my performance at work, both for interpersonal relations and for creating software efficiently. Needless to say it also takes a psychological toll.

Based on that experience and me being a [life-long life-hacker][a_bit_different], I vowed that I would up my sleep efficiency game after GDPR was over. I already knew about [good sleep hygiene], which highlights (among other things) consistent schedules and having between five and nine hours every day. What I needed to discover was how long I actually needed so I could have that target! Thus I began searching for how to best collect that data.

## My system
What I settled on is a combination of my Android phone (at the time of writing, an original Google Pixel), a Xiaomi Mi Band 3 ($30 shipped), Sleep as Android ($4 to unlock "pro"), Notify & Fitness (also ~$4 for "pro"), and MacroDroid (I still have basic free version because it does all I need). So for roughly $40, you can have the best (imho) sleep tracking system on the market that will provide you with all the data (and graphs!) you want, is comfortable, only requires charging once every two weeks (actually though - that's what I get), and does not need to be manually enabled! Gluing it all together isn't the MOST straightforward, but that's why I'm here to help =)

### Notify & Fitness
Just install it. You'll need the pro version to enable Tasker (or in our case, MacroDroid) integration with your Mi Band. If you have this app installed, you do not need the Mi Fit app installed (though I do anyway). You'll need to keep this app installed but shouldn't need to interact with it unless you want to.

### Connect your Mi Band
Follow the instructions in Notify & Fitness and/or the Mi Fit App to set up your Mi Band. You'll probably need to upgrade the firmware.

### Sleep as Android
Install the app and upgrade to pro. In the settings of the app, navigate to Wearables (under Integrations) and tick both the "Use wearables" and the "Heart rate monitoring" boxes. Back in the main menu, navigate to Services (also under Integrations). Enable Tasker integration. This app is the brains of the whole operation. Once you've collected data for a while, you can start looking at the charts and see what suggestions it has for you.

### MacroDroid
This is the glue that holds it all together! You'll need to create 2 macros and enable them.

The first macro we want the trigger to be Intent Received: `com.mc.miband.tasker.fellAsleep`. Set the action to be "Locale/Tasker Plugin" and select Sleep, which should bring up a menu where you should select "Start sleep tracking".

The next macro will be very similar. Set the trigger to be Intent Received: `com.mc.miband.tasker.wokeUp`. Set the action to be "Locale/Tasker Plugin" and select Sleep, which should bring up a menu where you should select "Stop Sleep tracking".

After you enable these macros, you shouldn't need to touch this app again.

## Alternative systems

### Wrist device: FitBit Versa
I actually have a FitBit Versa that I tried out simulatneously. It is a very attractive watch that has many more features than the Mi Band 3 and is unquestionably the better fitness tracker and smart watch. The notification system on the Mi Band, for example, I've found to be nearly useless given the limited number of characters that can be displayed at any given time and lack of unicode support. The Versa has a few main drawbacks though that keep me from recommending it instead.

1. Price: The FitBit Versa is roughly $200. If you mostly care about sleep tracking (which is what this is all about) then this is a no brainer.
1. Sleep as Android support: I knew going into this that I really wanted all of the features of Sleep as Android. I've been using it as an alarm clock for years (smart alarm, bedtime reminders, and captcha have all been lifesavers) and the Fitbit app just isn't at feature parity yet. Alas, there is no easy integration with Sleep as Android for either data analysis purposes or live sleep tracking. Edit Jan 2019: Apparently Versa support for SAA is coming!
1. Ecosystem lock-in: You can't easily get your sleep tracking data out from Fitbit. That means if you ever want to switch to a different data analysis product you can't.
1. Comfort: The Mi Band 3 is comparitively tiny and quite comfortable
1. Battery life: This was also a huge factor for me. The Versa gives you roughly four days of battery life. The Mi Band 3 supposedly can give you **twenty** days.

### Phone
If you want to use iOS, I think the Versa is your best bet. Tasker doesn't really exist on iOS and Sleep as Android (ha!) sure doesn't.

### Tasker app
You can use whatever Tasker-like app you'd like. I just found Tasker to be overkill for what I needed and MacroDroid was the simplest (free) product that would respond to Android intents and fire sleep tracking actions via the Tasker protocol.

### Sleep tracking app
I know there are others out there but none seem to have the features, sleep focus, and user interface of Sleep as Android. Notify & Fitness actually does some amount of sleep data analysis as well, but I found it to be less accurate, have fewer processing capabilities, and generally a harder app to use.


[a_bit_different]: ../a_bit_different
[flaxseed]: http://sherylcanter.com/wordpress/2010/01/a-science-based-technique-for-seasoning-cast-iron/
[bodybuilding]: https://www.bodybuilding.com/content/collagen-health-hoax-or-superfood.html
[join_pain]: https://www.ncbi.nlm.nih.gov/m/pubmed/28177710/
[dandruff]: https://www.healthline.com/health/skin-disorders/dandruff-vs-dry-scalp
[layouts_1]: http://mkweb.bcgsc.ca/carpalx/?popular_alternatives
[layouts_2]: https://normanlayout.info/compare.html
[emacs_pinky]: http://wiki.c2.com/?EmacsPinky
[jean_freeze]: https://www.smithsonianmag.com/science-nature/the-myth-of-the-frozen-jeans-129092730/
[ergo_pro]: https://matias.ca/ergopro/pc/
[uhk]: https://ultimatehackingkeyboard.com/