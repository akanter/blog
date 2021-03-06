+++
author = "Aaron Kanter"
categories = ["tech", "security"]
tags = ["passwords"]
date = "2016-04-04T16:07:17-07:00"
description = "Thoughts about the seemingly only option for free, cross-platform password management"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Password Management for the [Somewhat] Common Man"
type = "post"
+++

### 123456, password, qwerty

In the offchance you've been living under a rock lacking internet access for the past several years, passwords and account information are leaked on a frighteningly regular basis. [Amazon](https://nakedsecurity.sophos.com/2015/11/25/amazon-data-breach-rumours-spread-as-passwords-are-reset-on-some-accounts/) forced some portion of its customers to reset their passwords around Black Friday 2015. Over 30 million [Ashley Madison](http://krebsonsecurity.com/2015/07/online-cheating-site-ashleymadison-hacked/) accounts' details were leaked last summer including a plethora of personal details in addition to passwords. Back in 2013, more than 130 million [Adobe](http://www.troyhunt.com/2013/11/adobe-credentials-and-serious.html) user names, emails, encrypted (but [unsalted](https://crackstation.net/hashing-security.htm)) passwords, and clear-text password hints (**do not _ever_ use real password hints**) were loosed upon the internet.

**Side note**: While I am sorry for the damage caused by the Adobe leak, one silver lining is that someone created a series of very clever crossword puzzles based on the password hints [here](http://zed0.co.uk/crossword/). I got 100% on the first puzzle!

While there are services such as [HaveIBeenPwned](https://haveibeenpwned.com/) or [BreachAlarm](https://breachalarm.com/) which let you know when accounts related to your email address have been publicly compromised, their importance is drastically reduced as long as you take some necessary precautions. Furthermore, the idea of submitting your email address to a third party service is [questionable](http://mashable.com/2014/09/12/should-you-trust-gmail-password/). For example, the site owner could log your email address against your IP, service provider, and user-agent to be used in a later exploit. *If there is a leak, you are always better off just changing your password anyway*.

### The Rise of the Password Managers

Now there are a lot of things we can do to help protect ourselves from these disasters, but one simple idea is to use a different strong password for every account you have. This is not a new idea, but most people I know do not take measures because they are a _pain in the ass_. How many long sequences of characters can you remember? What about random sequences of characters? Of my friends and family, most have around 5 passwords they memorize with varying levels of security and generally remain unchanged until they are forced to do so. Many use the same password for all related services ("well it's easier to remember which password to use if all of my banking passwords are the same!"). This really, really doesn't fly.

In response to these leaks, more and more people are now thankfully using password managers such as [Dashlane](https://www.dashlane.com), [LastPass](https://lastpass.com), and [1Password](https://1password.com/) with a variety of different features ([feature comparison here](http://www.pcmag.com/article2/0,2817,2407168,00.asp)). Most of these are paid or subscription based products, but many also come with a free tier with differing levels of hobbling. This is all well and good, but I figured there _must_ be some sort of open-source system that I could use that would provide me with the most crucial features:

1. strongly encrypted
2. cross platform (OSX, Windows, Linux, Android, iOS, browsers)
3. easily distributable

It turns out I was right, and it's called [KeePass](http://keepass.info/).

### Kicking Pass with Open Source Software
KeePass is based on a very simple idea: take a database with your passwords and metadata in it, encrypt the whole thing in AES (or other cipher with plugins), and store it on your filesystem. You can even specify the number of encryption rounds to make it harder to crack! This gets us 2 out of 3 of our critical features while adding a few others. Importing and exporting are both supported with a variety of formats so there is no worry about lock-in: there isn't a company to be worried about losing customers here! Each password can have a history of customizable length. Furthermore as long as there are still applications that support the .kdbx (or the legacy .kdb) standard you will have a password manager.  While the reference implementation is for Windows, since it is open source there are also a number of ports for other platforms.

The last missing piece here is distribution. Frankly, this is actually a lot better that it's not a part of the KeePass project - file synchronization is a tricky enough problem as it is and I'd rather be able to swap out the implementations as I want. Luckily for us, there are a variety of cross-platform file distribution services to choose from. Many of the KeePass clients integrate well with [Dropbox](https://www.dropbox.com/), but [OneDrive](https://onedrive.live.com/), [Google Drive](https://drive.google.com), [Tresorit](https://tresorit.com), or any others work almost as well (or better!). Update 2016-10-19: [Sync][Sync] is a relatively new option that I'm considering using, so it should probably be mentioned here as well. Update 2017-11-26: I highly recommend checking out a private cloud solution such as [Nextcloud][Nextcloud] or [ownCloud][ownCloud], which you can either host yourself or use one of a [variety][Nextcloud_providers] of [companies][ownCloud_providers] that will do it for you (often for free!). I went with Swiss Nextcloud provider [wölkli][woelkli].

### My Setup

As of the time of this post, my database uses 8340000 encryption rounds which is high enough to delay any simple brute-force attacks and low enough to not drastically degrade the user experience on my phone. You can be sure that whenever I upgrade my phone I will be increasing this number.
My default generated password is 20 characters including mixed-case alphanumerics and symbols. Whenever KeePass generates a password that includes unsupported symbols for a particular site/application (e.g., `{, }, <, >, ~, \`), I manually replace the problematic characters with randomly chosen supported ones.

* File synchronization:
~~I went with [Tresorit](https://tresorit.com) because it offers end-to-end encryption and file storage in Switzerland for the low, low price of free. Unfortunately their free plan only allows 3 devices to sync files and I've experienced some finnicky-ness in getting my files on Android to upload. This will probably be resolved in future releases though so I'm willing to stomach it for the time being (especially since my password database doesn't change _that_ often). **Update 2016-10-14:** I'm happy to say that the Android client for Tresorit has improved a lot and I no longer have problems getting my KeePass database to upload or refetch properly as long as I do so manually. The next wrinkle is to figure out how to automatically upload my changes in Android consistently (it works _sometimes_). <br/>
**Update 2016-10-19:** I received an email today telling me I should upgrade to Tresorit Solo, which is a paid service with a 15-day trial. [Upon closer investigation](https://support.tresorit.com/hc/en-us/articles/224348627-Does-Tresorit-have-a-free-plan-) it looks like they have actually discontinued the free Basic plan and replaced it with the [Reader](https://support.tresorit.com/hc/en-us/articles/227327447-What-is-the-Reader-plan-) plan which is much more limited and crucially offers no cross-device syncing. Since the old Basic plans are grandfathered in, I'm still OK for the time being. That being said, I also just signed up for [Sync][Sync], which also offers end-to-end encryption for an unlimited number of devices. It doesn't quite at feature parity yet (e.g., no Linux, no two-factor authentication) but it looks like it could be a promising alternative.~~
<br/>
**Update 2017-11-26:** I switched to [Nextcloud][Nextcloud] because the integration with Keepass2Android (or any other client that supports [WebDAV][WebDAV]) is much better than with Tresorit. This is also neat because you can choose whichever hosting company in whatever country you want. Research your data privacy laws and go with whichever country you're most comfortable with!
* Android: [Keepass2Android](https://keepass2android.codeplex.com/) is open source, comes with its own keyboard to enter in your credentials into forms for you (so you're not vulnerable to clipboard-sniffing attacks), and supports fingerprint unlocking. It seems to have specific support for Dropbox, Google Drive, OneDrive, but it doesn't have a problem retrieving files from Tresorit either. It may have direct uploading support with the other storage services, but I haven't tried.  **Update 2017-11-26:** The ownCloud/Nextcloud/WebDAV integration with KP2A is _sweet_. It gives you a handy button to synchronize the database directly to and from KP2A.
* OSX/Linux: I could have run the reference client under [Mono](www.mono-project.com/Mono:OSX/), but [KeePassXC](https://www.keepassxc.org/) has a nice interface, is actively maintained, and is cross platform. Of note is that it implements a [KeePassHttp][KeePassHttp] endpoint for integration with your browser (see below).
* Windows: The [KeePass](http://keepass.info/) reference client seems to work well enough for me but I don't use my Windows machine enough to investigate if there are better options for me. If that ever changes, I'll probably check out KeePassX.
* Chrome: I'm using [ChromeIPass](https://chrome.google.com/webstore/detail/ompiailgknfdndiefoaoiligalphfdae), but it requires a running [KeePassHttp][KeePassHttp] instance (essentially, a webserver attached to an instance of KeePass that can securely send password data, also open source). At the moment, KeePassHttp is only supported by the original KeePass and KeePassXC.
Aside from ChromeIPass there is [CKP](https://chrome.google.com/webstore/detail/ckp-keepass-integration-f/lnfepbjehgokldcaljagbmchhnaaogpc), which is read-only and doesn't automatically refetch your passwords from the local filesystem (though supposedly it can refetch if it's set up with Dropbox).
* FireFox: I've heard that [PassIFox](https://addons.mozilla.org/en-us/firefox/addon/passifox/) is the extension you want. I haven't used it myself, so your mileage may vary.
* iOS: I don't use any iOS devices but I've heard good things about [Kypass][Kypass] and it also has WebDAV integration.

I'll also give a shoutout to [KeeWeb](https://keeweb.info/) which is a promising looking cross-platform application built on [Electron](http://electron.atom.io/). It doesn't quite have feature parity with my current setup yet (though KeePassHttp is on the roadmap!), but I'm certainly very interested to see its progress over the next few months.

There are also ports listed on the [KeePass website](http://keepass.info/download.html) for many more platforms, including Windows Phone, BlackBerry (!), PocketPC(!!), and PalmOS(!!!).


### You young'ns and your internets...
Many of my relatives use good-old-fashioned pen & paper to store their passwords. While this means the passwords are probably safe from most hackers and I'm not particularly worried about theft, this is a much larger risk if and when they move from one dwelling to the next and items get lost in the shuffle. As you might have guessed by the title of this post, I am confident that even the most non-technologically savvy members of my family can stomach KeePass and even without the complications of synchronizing the database across devices, it is much better than a piece of paper. If there is no cloud synchronization in your system though, I would strongly recommend some sort of file backup, even if it's just OSX's built-in Time Machine (though again, please encrypt this!).

[KeePassHttp]: https://github.com/pfn/keepasshttp/
[Kypass]: https://www.kyuran.be/software/kypass/
[Sync]: https://www.sync.com
[Nextcloud]: https://nextcloud.com/
[Nextcloud_providers]: https://nextcloud.com/providers/
[ownCloud]: https://owncloud.org/
[ownCloud_providers]: https://owncloud.org/providers/
[WebDAV]: https://en.wikipedia.org/wiki/WebDAV
[woelkli]: https://woelkli.com/en
