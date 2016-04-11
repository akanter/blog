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

In the offchance you've been living under a rock lacking internet access for the past several years, passwords and account information are leaked on a frighteningly regular basis. [Amazon](https://nakedsecurity.sophos.com/2015/11/25/amazon-data-breach-rumours-spread-as-passwords-are-reset-on-some-accounts/) forced some portion of its customers to reset their passwords around Black Friday 2015. Over 30 million [Ashley Madison](http://krebsonsecurity.com/2015/07/online-cheating-site-ashleymadison-hacked/) accounts' details were leaked last summer including a plethora of personal details in addition to passwords. Back in 2013, more than 130 million [Adobe](http://www.troyhunt.com/2013/11/adobe-credentials-and-serious.html) user names, emails, encrypted (but [unsalted](https://crackstation.net/hashing-security.htm)) passwords, and clear-text password hints (**do not _ever_ use password hints**) were loosed upon the internet.

**Side note**: While I am sorry for the damage caused by the Adobe leak, one silver lining is that someone created a series of very clever crossword puzzles based on the password hints [here](http://zed0.co.uk/crossword/). I got 100% on the first puzzle!

While there are services such as [HaveIBeenPwned](https://haveibeenpwned.com/) or [BreachAlarm](https://breachalarm.com/) which let you know when accounts related to your email address have been publicly compromised, their importance is drastically reduced as long as you take some necessary precautions. Furthermore, the idea of submitting your email address to a third party service is [questionable](http://mashable.com/2014/09/12/should-you-trust-gmail-password/). For example, the site owner could log your email address against your IP, service provider, and user-agent to be used in a later exploit. *If there is a leak, you are always better off just changing your password anyway*.

### The Rise of the Password Managers

Now there are a lot of things we can do to help protect ourselves from these disasters, but one simple idea is to use a different strong password for every account you have. This is not a new idea, but most people I know do not take measures because they are a _pain in the ass_. How many long sequences of characters can you remember? What about random sequences of characters? Of my friends and family, most have around 5 passwords they memorize with varying levels of security and generally remain unchanged until they are forced to do so. Many use the same password for all related services ("well it's easier to remember which password to use if all of my banking passwords are the same!"). This really, really doesn't fly.

In response to these leaks, more and more people are now thankfully using password managers such as [Dashlane](https://www.dashlane.com), [LastPass](https://laspass.com), and [1Password](https://1password.com/) with a variety of different features ([feature comparison here](http://www.pcmag.com/article2/0,2817,2407168,00.asp)). Most of these are paid or subscription based products, but many also come with a free tier with differing levels of hobbling. This is all well and good, but I figured there _must_ be some sort of open-source system that I could use that would provide me with the most crucial features:

1. strongly encrypted
2. cross platform (OSX, Windows, Linux, Android, iOS, browsers)
3. easily distributable

It turns out I was right, and it's called [KeePass](http://keepass.info/).

### Kicking Pass with Open Source Software
KeePass is based on a very simple idea: take a database with your passwords and metadata in it, encrypt the whole thing in AES (or other cipher with plugins), and store it on your filesystem. You can even specify the number of encryption rounds to make it harder to crack! This gets us 2 out of 3 of our critical features while adding a few others. Importing and exporting are both supported with a variety of formats so there is no worry about lock-in: there isn't a company to be worried about losing customers here! Each password can have a history of customizable length. Furthermore as long as there are still applications that support the .kdbx (or the legacy .kdb) standard you will have a password manager.  While the reference implementation is for Windows, since it is open source there are also a number of ports for other platforms.

The last missing piece here is distribution. Frankly, this is actually a lot better that it's not a part of the KeePass project - file synchronization is a tricky enough problem as it is and I'd rather be able to swap out the implementations as I want. Luckily for us, there are a variety of cross-platform file distribution services to choose from. Many of the KeePass clients integrate well with [Dropbox](https://www.dropbox.com/), but [OneDrive](https://onedrive.live.com/), [Google Drive](https://drive.google.com), [Tresorit](https://tresorit.com), or any others work almost as well.

### My Setup

As of the time of this post, my database uses 8340000 encryption rounds which is high enough to delay any simple brute-force attacks and low enough to not drastically degrade the user experience on my phone. You can be sure that whenever I upgrade my phone I will be increasing this number.
My default generated password is 20 characters including mixed-case alphanumerics and symbols. Whenever KeePass generates a password that includes unsupported symbols for a particular site/application (e.g., `{, }, <, >, ~, \`), I manually replace the problematic characters with randomly chosen supported ones.

* File synchronization: I went with [Tresorit](https://tresorit.com) because it offers end-to-end encryption and file storage in Switzerland for the low, low price of free. Unfortunately their free plan only allows 3 devices to sync files and I've experienced some finnicky-ness in getting my files on Android to upload. This will probably be resolved in future releases though so I'm willing to stomach it for the time being (especially since my password database doesn't change _that_ often).
* Android: [Keepass2Android](https://keepass2android.codeplex.com/) is open source, comes with its own keyboard to enter in your credentials into forms for you (so you're not vulnerable to clipboard-sniffing attacks), and supports fingerprint unlocking. It seems to have specific support for Dropbox, Google Drive, OneDrive, but it doesn't have a problem retrieving files from Tresorit either. It may have direct uploading support with the other storage services, but I haven't tried.
* OSX/Linux: I could have run the reference client under [Mono](www.mono-project.com/Mono:OSX/), but [KeePassX](https://www.keepassx.org/) has a nice interface, is actively maintained, and is cross platform. 
been _too_ irritated by manually uploading my updated database.
* Windows: The [KeePass](http://keepass.info/) reference client seems to work well enough for me but I don't use my Windows machine enough to investigate if there are better options for me. If that ever changes, I'll probably check out KeePassX.
* Chrome: I'm using [ChromeIPass](https://chrome.google.com/webstore/detail/ompiailgknfdndiefoaoiligalphfdae), but it requires a running [KeePassHttp](https://github.com/pfn/keepasshttp/) instance (essentially, a webserver attached to an instance of KeePass that can securely send password data, also open source). At the moment, KeePassHttp is only supported by the original KeePass but has been [unofficially ported to KeePassX](https://github.com/keepassx/keepassx/pull/111) and available on OSX via the author's personal homebrew-pfa (`brew install eugenesan/homebrew-pfa/keepassx`). The big issue here is that we are now relying on the author to keep his branch up-to-date. =/
Otherwise, there is [CKP](https://chrome.google.com/webstore/detail/ckp-keepass-integration-f/lnfepbjehgokldcaljagbmchhnaaogpc), which is read-only and doesn't automatically refetch your passwords from the local filesystem (though supposedly it can refetch if it's set up with Dropbox).

I'll also give a shoutout to [KeeWeb](https://keeweb.info/) which is a promising looking cross-platform application built on [Electron](http://electron.atom.io/). It doesn't quite have feature parity with my current setup yet (though KeePassHttp is on the roadmap!), but I'm certainly very interested to see its progress over the next few months.

I don't use iOS or Firefox these days but if that ever changes or I receive strong recommendations from anyone I will update this post. There are also ports listed on the [KeePass website](http://keepass.info/download.html) for many more platforms, including Windows Phone, BlackBerry (!), PocketPC(!!), and PalmOS(!!!).


### You young'ns and your internets...
Many of my relatives use good-old-fashioned pen & paper to store their passwords. While this means the passwords are probably safe from most hackers and I'm not particularly worried about theft, this is a much larger risk if and when they move from one dwelling to the next and items get lost in the shuffle. As you might have guessed by the title of this post, I am confident that even the most non-technologically savvy members of my family can stomach KeePass and even without the complications of synchronizing the database across devices, it is much better than a piece of paper. If there is no cloud synchronization in your system though, I would strongly recommend some sort of file backup, even if it's just OSX's built-in Time Machine (though again, please encrypt this!).