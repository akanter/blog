+++
categories = ["tech", "productivity"]
tags = ["development", "*nix", "MacOS"]
date = "2017-11-26"
title = "Eternal Terminal, tmux, iTerm2, and you"
description = "C-C-C-Combo!"
author = "Aaron Kanter"
type = "post"
+++

# Prelude: tmux
For the rest of this post to make sense, you ought to have some understanding of [**tmux**][tmux]. tmux is a **t**erminal **mu**ltiple**x**er and session manager, meaning that if you disconnect from your machine you can reattach to your previous session and the state of your terminal tabs (managed within tmux) will be restored as if you had never left. It is a _highly_ recommended tool for remote development. It is often considered a successor to [screen][screen].

# First there was mosh...

After I finally joined a team at my new job nearly two months past my February start date, my new team mates were quick to inform me that I should ditch my tried-and-true [ssh][ssh] in favor of [mosh][mosh]. What is mosh? Well, it is essentially a drop-in replacement for ssh that is designed for development with spotty connections (hence **mo**bile **sh**ell) where your local IP address might even change. It will essentially keep your SSH session alive regardless of your network status, meaning that even if you put your computer to sleep it will reconnect as if you'd never left: when your connection drops the shell will appear to hang, but when it automatically reconnects all recorded output from the remote server will be delivered to your local terminal and all queued input from your terminal will be sent to the server. Slick! This is especially important at my company where development typically takes place on a remote server which additionally requires [two-factor authentication (2fa)][2fa] to get access; with mosh you only have to 2fa when you make the first connection. Coupled with the session management of tmux, you will have a development environment automatically tolerant of any connectivity issues and system sleeping. The only time you need to do anything is when one of the machines is restarted (reinitiate the mosh session, attach/create the tmux session) which, if you're like me, is quite rare.

Now don't get me wrong, mosh is great, but it has some shortcomings that had me yearning for something better. With a little Google-fu, I discovered the drop-in-replacement tool I'd been looking for - [eternal terminal][et]


# ...and then there was et
The most important feature **et** has over mosh is that it supports the tmux control center flag, which you probably (hopefully!) know as **tmux -CC**. If you don't know about tmux control center, it allows you to run a tmux session using your local terminal emulator as a controller instead of having to use the shell controls (you'll never need to type that <kbd>ctrl+b</kbd> prefix again!). This means that you get a separate terminal window for your tmux session with tabs, mouse support, resizing, scroll history, searching, etc. With the connection tolerance of et added to the mix, it feels 99% identical to having a local terminal session! Awesome!! Superlative!!! One caveat: [iTerm2][iterm2] is ([roughly][tmuxcc-terminals]) the only terminal emulator compatible with tmux control center. Given all the awesome features iTerm2 has though (I love [instant-replay][instant-replay]), I can't see a reason to use a different terminal emulator anyway.

{{< figure src="/img/posts/et_5x.gif" class="align-center" >}}

# Shut up and take my money!
Getting et, tmux, and iTerm2 set up is pretty easy. Follow the [et guide][guide] and you'll be up and running in no time. If you enjoy these tools anywhere near as much as I have, please consider donating to these awesome developers (though only the [iTerm2][iterm2] guys readily accept them)! =)

[2fa]: https://en.wikipedia.org/wiki/Multi-factor_authentication
[et]: https://mistertea.github.io/EternalTCP/
[instant-replay]: https://www.iterm2.com/features.html
[mosh]: https://mosh.org/
[ssh]: http://linuxcommand.org/man_pages/ssh1.html
[screen]: https://www.gnu.org/software/screen/manual/screen.html
[tmux]: https://github.com/tmux/tmux/wiki
[iterm2]: https://www.iterm2.com/
[tmuxcc-terminals]: https://unix.stackexchange.com/questions/189805/what-terminal-emulators-support-tmux-control-mode
[guide]: https://mistertea.github.io/EternalTCP/usermanual/
