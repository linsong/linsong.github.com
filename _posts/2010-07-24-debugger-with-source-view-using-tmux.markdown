---
layout: post
title: debugger(rdb, gdb etc) with source view(vim) using tmux
published: true
---

I am a big CLI(Command Line Interface) fan, for example, I like [vim][] for editing, [mutt][] for email, [irssi][] for irc, [w3m][] for web surfing sometimes, [mpd][] and [ncmpcpp][] for music, [mplayer] for movie, [lftp][] for ftp access etc. I am pretty happy with CLI tools, not only because it is efficient(considering you can almost automating all functionalities), but also that is real cross-platform interface for me. I can use these tools anywhere, local or remote, without knowing any difference. That is a big win for anyone who may need to work on several hosts at the same time. To make things more easier, I put all my config files to a [github][] [repository][], so at the first hand, I can keep tweek all these configurations, on the other hand, whenever I start to work on a new environment, the first thing is checkout my config files, then I feel at home now(this is true for Linux, Mac OS X and even Windows by [cygwin][]). Hmm, I think I wrote too much about CLI here, maybe I should write another post to describe my point of view about CLI later :)

To be honest, sometimes I am jealous of some GUI tools. I was a C++, Python developer before and now a Ruby develoer. I use vim to editing codes and I think vim is the most powerful editor(note, I use editor here, so Emacser, please don't argue me since I don't think Emacs is a editor at all); but when I need to fix some bug in my codes, I will use [rdb][] that is a CLI ruby debugger, it inherits almost all the keyboard interface from [gdb][] that is one reason I prefer rdb over [pdb][], the python debugger. So far, so good. but when the debugging session is long, then rdb is sometimes less efficient compared to other IDE tools like Netbeans or Eclips , because in rdb to get current source context I have to press 'l'(the source code list key) to show codes around current running line, even so, the codes are not syntax highlighted as it is in vim and only a small section of codes are showed, that is tedious, right ? If the debugger can cooperate with vim, then that will be very useful. 

After I kept asking google for this kind of tool for a long time, I only found [clewn][] project that integrate gdb with vim, but rdb is not supported yet. I don't want to wait anymore, I want hack now. 

I like rdb's gdb like interface, that is efficient. So I will not implement some wrapper above rdb. I want to use rdb interface to drive the debugging session but using vim to view related source codes and with the current line marked or highlighted, that's all what I want. 

Thanks to tmux that makes my dream possible! below is a screenshot how it looks like: 

<img alt="rdb and vim in action" title="rdb plus vim for debugging" src="/screenshots/rdb_vim_in_action.png" width="800"/>

When the current line changes on the left window, the vim buffer will get updated. It turned out to be very simple to accomplish it: just 80 lines of ruby code! 
Here's how to do it: 

1. install required tools
   * [tmux][]
      tmux is very good replacement for [gnu screen][]. Before I switch to tmux, I had been a screen user for 5+ years. I had thought there would not be better similar tool than screen, but tmux proved I was wrong. try it, it definately deserved it. you need to install it after version 1.1
   * [vim][]
      need to enable "sign" feature, run "vim --version" to check it
   * ruby 
      in fact the small script mentioned later can be written in any language, but I happen to be a ruby developer now.

2. config tmux 
    create file .tmux.conf in your home directory, put line "bind C-d pipe-pane -o '~/.tmux/rdb'" into it. if you want a more advanced configuration, you can refer to [my tmux config file][].

3. download this [ruby script][] and save it as ~/.tmux/rdb

That's it.

Start a tmux session, run you programm, when in debugging session, you can press "Control"+"b", then "Control"+"d", then a vim window will show up, after that, whenever current code line changes, the vim window will be updated. If you press "Control"+"b", then "Control"+"d" again, the vim window will disappear. 

At last, this solution should support other debuggers(gdb, pdb etc) without any change(or just change regex on line 76 ) of the ruby script.

[vim]: http://www.vim.org
[mutt]: http://www.mutt.org
[w3m]: http://w3m.sourceforge.net
[mpd]: http://mpd.wikia.com/wiki/Music_Player_Daemon_Wiki
[ncmpcpp]: http://unkart.ovh.org/ncmpcpp
[mplayer]: http://www.mplayerhq.hu
[lftp]: http://lftp.yar.ru
[github]: http://github.com
[repository]: http://github.com/linsong/dailyconfig
[rdb]: http://bashdb.sourceforge.net/ruby-debug.html
[pdb]: http://docs.python.org/library/pdb.html
[gdb]: http://www.gnu.org/software/gdb/
[clewn]: http://clewn.sourceforge.net
[cygwin]: http://www.cygwin.com
[my tmux config file]: http://github.com/linsong/dailyconfig/blob/master/.tmux.conf
[ruby script]: http://github.com/linsong/dailyconfig/blob/master/.tmux/pipes/rdb
[tmux]: http://tmux.sourceforge.net
[gnu screen]: http://www.gnu.org/software/screen/
