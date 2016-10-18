+++
featuredpath = ""
featuredalt = ""
categories = ["tech", "productivity"]
tags = []
date = "2016-10-16T22:38:05-04:00"
title = "The Lazy Person's Guide to Useful Everyday Software"
description = "Software to make your digital life more productive and less tedious"
linktitle = ""
featured = ""
author = "Aaron Kanter"
type = "post"
+++

[Several][lazy-lenssen] [people][lazy-wall] [have posited][lazy-atwood] that the best coders are lazy, citing that they will write DRY (**D**on't **R**epeat **Y**ourself) code to make maintenance and testing easier and that they will use the right tools to solve their problems with the least amount of required effort. Well, the same principle applies when it comes to general productivity when it comes to working with computers, whether you're a software developer, an Excel spelunker, or a human Markov chain email generator. The following is a list of software I find useful for my daily life and that I have found helps my friends and family. This is primarily focused on Apple OS X, but there are equivalent solutions for all platforms and really the concepts are the important part rather than the specific pieces of software.

### Reduce eye strain, sleep faster: [f.lux][f.lux]
If you've ever woken up in the middle of the night to answer an urgent email only to find that once you've finished your task you are wide awake, then f.lux is for you. f.lux progressively filters the blue light out of your screen according to the daily solar cycle (based on your location) or a schedule you set. The idea behind it is that blue light suppresses the production of [melatonin][melatonin] (a hormone partially responsible for sleep-wake cycles), so by avoiding exposure to blue light you can keep the sleep train chugging. What this means is that using your computer at night is far less deleterious to your sleep with f.lux than it would be otherwise. Of course, if you're playing an scary video game right before bed, you'll still probably be pretty amped but less exhilirating tasks can be done while letting your body wind down for rest. Furthermore, I find that my eyes are a lot less fatigued with the reduced color-temperature, similar to what the yellow-tinted, supposedly [ergonomic glasses][ergo-glasses] are supposed to achieve.

Of course, this idea applies to all platforms, perhaps being even more useful for mobile phones. I use [Twilight][Twilight] for Android (though f.lux is also available) and the latest versions of iOS have the feature built-in under the moniker of Night Shift. Android 7.0 (Nougat) also has a similar built-in feature called Night Mode but I don't have it on my phone yet.

### More efficient copy & paste: [ClipMenu][ClipMenu]
{{< figure src="/img/posts/clipmenu_sample.png" class="align-center" >}}

Clipboard managers are really awesome, especially when you've got to copy and paste a bunch of data around to different places. ClipMenu gives you a clipboard of variable length. Normal copy and paste works just fine (<kbd>⌘C</kbd>, <kbd>⌘V</kbd>), but when I activate the ClipMenu paste (<kbd>⌘⇧V</kbd>), it brings up a menu of my 20 most recent items from my clipboard. Then I can select the one I want by mouse (if it's pretty old), but normally I'll remember the item's relative age and can select it immediatley by a second keyboard shortcut (command + 3 for the third most recent item). This becomes really useful if I know I need to copy, say, 3 fields (SSH keys, long passwords, addresses, etc.) from one page in a browser to another. Instead of flipping back and forth I'll copy each field in order, then paste them in order by hitting <kbd><kbd>⌘⇧V</kbd>, <kbd>⌘3</kbd></kbd> 3 times.

There are a variety of other options, but I chose ClipMenu because it is free, lightweight, and has a bunch of other nice features (works with a variety of content types, backs up the clipboard, simple UI). It's not currently in active development but since I'm happy with the features so I don't see it as much of a problem.

### Window management: [SizeUp][SizeUp]
{{< figure src="/img/posts/sizeup_sample.png" class="align-center" >}}
Quite simply, using your mouse to drag the corners of your application windows around to resize them is a really crappy experience. Sure, you have a lot of control, but generally I'm more concerned with having my windows as large as possible while in the configuration I want with the rest of my open applications. SizeUp offers an alternative by offering keyboard shortcuts to handle most of the tasks I care about. This includes moving windows to different screen regions (top, bottom, left, or right halves; the four corners; center; full-screen), between different monitors, and across spaces. Not only is this a lot faster, but it's a lot more ergonomic since you're not using your mouse. It's worth every penny of its $13.

### Cross-platform document transfer: [DropBox][DropBox], [Tresorit][Tresorit]
Need to send your boarding pass or concert tickets to your phone? Print to PDF and save it in your DropBox folder and your phone can pick it up later. I wouldn't recommend this for large file transfers since you'll be sending data all the way up to the cloud and then back down to your other device, but for small files (or if time and data limits aren't issues) it's fantastic.

There are a variety of options here that are basically the same and are equally multiplatform so feel free to look at [Google Drive][GoogleDrive], [OneDrive][OneDrive], etc. [Tresorit][Tresorit] is a little different in that it is marketed specifically for the security spooks who want end-to-end encryption on their files. I personally use a combination of [DropBox][DropBox] and [Tresorit][Tresorit] for documents about whose security I care more about, such as my KeePass database.

### Password manager: [KeePassX][KeePassX], [1Password][1Password], [LastPass][LastPass]
I wrote a [whole blog post][keepass-blog] about this, but the short of it is that I have little interest in memorizing an increasingly large numbers of distinct secure passwords to keep myself safe should there be any security breaches. I chose [KeePass][KeePass] as the open source protocol, with [KeePassX][KeePassX] being the best client for OS X and [Keepass2Android][KP2A] ([Play Store][KP2A-Store] for Android. If you're interested in setting up KeePass, check out my [post][keepass-blog] for more details including how to get it set up auto-fill in your browser. [1Password][1Password] and [LastPass][LastPass] are some industry leaders if you want to pay for them instead.

### Remap special keys: [Seil][Seil] (+ [Karabiner][Karabiner])
When was the last time you actually used your caps lock key for anything other than "yelling" at someone? And can you really not just hold down shift instead?
With that in mind, I highly recommend everyone to remap their caps lock to either left control (for the emacs users among us) or to backspace (the normal placement for backspace is horribly remote for a key that is used so frequently). Seil and its companion Karabiner are designed to tweak the keycodes for each key on your board to allow such magic to happen. They are also useful if you have a keyboard with unusual keys that you'd like map to something else.

### Email client: [Inbox][Inbox]
{{< figure src="/img/posts/inbox_sample.png" class="align-center" >}}
Inbox has been around for almost two years now, but it's still not the default email client from Google. It automatically groups the emails in your inbox by type (finance, social, forums, updates, travel, purchases) and hides most of the contents of those groups except when you specifically drill into them or they are deemed especially important. You can then also delete or mark as done those groups. It may take a little bit of getting used to, but generally this addition of AI and the new user experiences it brings means that this is the best email experience I've ever had. If you're still using plain old [Gmail][Gmail], I _strongly_ recommend you give Inbox a whirl for a few days and you probably won't want to go back.

### Ad blocker: [uBlock Origin][uBlock]
Ads are not only annoying, but they also slow down your browser. The makers of uBlock Origin actually see themselves as a "wide-spectrum blocker" and ads are just one annoying aspect of the internet that they happen to be great at filtering. It doesn't use many resources (small memory footprint, requires few CPU cycles) and it works in most popular browsers. It even works for many video ads!

**NOTE:** For the sake of clarity uBlock _Origin_ is what you're looking for, not uBlock. Details over how and why uBlock Origin split from uBlock can be found [here][uBlockOriginStory].

On a more humorous note, [Adblock Plus][ABP], a previously reigning champion for ad blocking is now in the business of [selling ads][ABP-sells-ads]. This is in addition to the fact that their product now tends to slow down the browser (I think it used to be more lightweight).

What am I missing? Share your favorites in the comments below!

[lazy-lenssen]: http://blogoscoped.com/archive/2005-08-24-n14.html
[lazy-wall]: http://threevirtues.com/
[lazy-atwood]: https://blog.codinghorror.com/how-to-be-lazy-dumb-and-successful/
[f.lux]: https://justgetflux.com/
[melatonin]: http://www.webmd.com/sleep-disorders/tc/melatonin-overview
[Twilight]: https://play.google.com/store/apps/details?id=com.urbandroid.lux
[ergo-glasses]: https://www.amazon.com/Ergonomic-Advanced-Computer-Gaming-Glasses-Amber/dp/B00CJULBKC
[ClipMenu]: http://www.clipmenu.com/
[SizeUp]: http://www.irradiatedsoftware.com/sizeup/
[DropBox]: https://www.dropbox.com
[GoogleDrive]: https://drive.google.com/
[OneDrive]: https://onedrive.live.com
[Tresorit]: https://tresorit.com/
[KeePassX]: https://www.keepassx.org/
[LastPass]: https://lastpass.com/
[1Password]: https://1password.com/
[keepass-blog]: ../keepass
[KeePass]: http://keepass.info/
[KP2A]: http://keepass2android.codeplex.com/
[KP2A-Store]: https://play.google.com/store/apps/details?id=keepass2android.keepass2android
[Seil]: https://pqrs.org/osx/karabiner/seil.html.en
[Karabiner]: https://pqrs.org/osx/karabiner/index.html.en
[Inbox]: https://inbox.google.com
[Gmail]: https://mail.google.com
[uBlock]: https://github.com/gorhill/uBlock
[uBlockOriginStory]: http://tuxdiary.com/2015/06/14/ublock-origin/
[ABP]: https://adblockplus.org
[ABP-sells-ads]: http://www.theverge.com/2016/9/13/12890050/adblock-plus-now-sells-ads

