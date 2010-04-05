---
layout: post
title: super finder in vim
published: true
---

There are many editors([Emacs][], [TextMate][], [UltraEdit][], [EditPlus][] etc)  available, and more are coming. I believe every long-live editor must have some features that suit some people well, so they keep catching people's eyes. I am a long-time(>5 years) vim user. Most of time, I am pretty happy with it. But I although keep an eye on other editors to see if it is possible to borrow some useful feature into vim, and build my vim configuration to be most productive for myself, a developer.

TextMate is becoming very hot these days, it's very neat and powerful, just like vim ;) 

One killer feature of TextMate, and borrowed by many other IDEs later, is very convenient finder tool for file, class etc. You can input part of the file's name, and TextMate will provide a list of possible candicates for you to choose. The key point here is that, you don't need to input the directory name, just part of the file name, and TextMate will collect all files starting with your input across all folders recursively under the project root directory. More details [here][TextMate Moving Between Files]

I found this feature is pretty useful and then started to think how to clone it within vim. After some experiments, I managed to do it! 

To make this feature happen in vim, you need following stuffs: 
  * [vim][] off course
  * [fuzzyfinder.vim][]
  * [Exuberant Ctags][]

Fuzzyfinder.vim is a very powerful vim plugin, in fact, if I only can select one vim plugin that is must-have for me, I will choose this one. There are many features in fuzzyfinder, but largely, it will match user's input against files, directors, buffers, tags in fuzzy way. 'Fuzzy' means if you input 'abc' in fuzzyfinder, fuzzyfinder will expand it to '*a*b*c*' and provide candicates list that match the fuzzy pattern '*a*b*c*'. 

But one problem of fuzzyfinder is: to find a file, for example, app/controllers/foo_controller.rb, you must input 'app', then 'controllers', before you get chance to find foo_controller.rb, that is not efficient compared to TextMate(in TextMate, you can input the file's name directly).

Fuzzyfinder provide several modes: buffers, files, directories, bookmarks, tags etc. Tags mode make the TextMate clone possible. After you install it, run :help fuf.txt will give you a very comprehensive documentation.

Ctags is a tool that will parse files based on regular expression, and generate function defination to file path and regex mapping into a plain text file(named tags or TAGS). You can open the generate file and have a look, it is much easier that you think.  Other edits can use the tags file to navigate from a place where a function is used, to funciton defination easily. 

My solution is: treat file name as kind of tag, generate a tags file that contains the mapping between file name and file path, and  be compatible with tags file's format, then we can use fuzzyfinder to find the file easily.

[Here][ftags.sh] is the bash script to generate tags file for files. 

To use it, you need to run this script under your project root directory like this:
  
    ftags.sh .

then a tag file named ftags will be generated under current directory. You need to tell vim about the new tags file, put the following line into your .vimrc:

    :set tags+=ftags 

When you enter the tags mode of fuzzyfinder(by default, the shortcut is <C-f><C-t>) , file's names show up together with other kind of tags, so you can search files, functions, classes etc in fuzzy way at the same time. That is pretty powerful, right ? 

I have used the super fuzzy finder mode for several months and pretty pretty happy with the effect, it makes navigation within vim become a joy. 

Now I am not jealousy at TextMate any more :) 

[Emacs]: http://www.gnu.org/software/emacs/
[TextMate]: http://macromates.com
[UltraEdit]: http://www.ultraedit.com
[EditPlus]: http://www.editplus.com
[TextMate Moving Between Files]: http://manual.macromates.com/en/working_with_multiple_files#moving_between_files_with_grace
[fuzzyfinder.vim]: http://www.vim.org/scripts/script.php?script_id=1984
[Exuberant Ctags]: http://ctags.sourceforge.net/ 
[ftags.sh]: http://github.com/linsong/dailyconfig/raw/master/mytools/ftags.sh
[vim]: http://www.vim.org
