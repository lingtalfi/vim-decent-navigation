Decent vim navigation keyboard for mac terminal
=================================
2016-10-19


Add IDE like navigation to vim.

You're welcome.



Successful tests
--------------------
I was able:

- install the navigation shortcuts on my MacOSX and use them from the mac terminal
- install the navigation shortcuts on a (vagrant) ubuntu14 server and use them via ssh from my mac terminal




Features
-------------
- duplicate line(s) --> ctrl+d
- move line(s) up/down --> alt+(up/down)
- move to the next/previous word --> alt+(right/left)
- delete the next/prev word --> alt+(suppr/backspace)
- mouse can be used to position the cursor --> alt+click (default mac terminal)
- copy the selection to the system clipboard --> cmd+c (default mac terminal)
- go to the beginning of the line --> Home (native vim)
- go to the end of the line --> End (native vim)
- scroll up --> Page Up (native vim)
- scroll down --> Page Down (native vim)




How to
----------------

Three steps:
- paste the "vim basic nav" code below in your .vimrc
- fix the code to your needs
- check that everything worked



### Step 1: paste the "vim basic nav" code

Copy paste the following code in your .vimrc config file.
Note: it won't work yet, it's just the first step.


```vim


" general settings (optional)
syntax enable
set nocompatible
set tabstop=4

" do not use vim mouse 
set mouse=

" duplicate line(s) - ctrl+d 
nnoremap <C-d> :normal! mmyyp`m<CR>
inoremap <C-d> <ESC> :normal! mmyyp`m<CR>i
vnoremap <C-d> y'>p<CR>

" move to the next/prev words - alt+(right/left)
nnoremap <ESC>f w
inoremap <ESC>f <ESC> :normal! w<CR>i
nnoremap <ESC>b b
inoremap <ESC>b <ESC> :normal! b<CR>i

" move line(s) up/down - alt+(up/down) -  http://vim.wikia.com/wiki/Moving_lines_up_or_down
nnoremap <ESC>l :m .+1<CR>==
nnoremap <ESC>u :m .-2<CR>==
inoremap <ESC>l <Esc>:m .+1<CR>==gi
inoremap <ESC>u <Esc>:m .-2<CR>==gi
vnoremap <ESC>l :m '>+1<CR>gv=gv
vnoremap <ESC>u :m '<-2<CR>gv=gv

" remove next/prev word - alt+(suppr+backspace)
nnoremap <Esc>d dw
inoremap <Esc>d <ESC> :normal! dw<CR>i
nnoremap <ESC>^H dbh 
inoremap <ESC>^H <ESC> :normal! dbh<CR>i 


```

### Step 2: fix the code

This is a two part process

- check that your alt shortcuts match those in the config files
- fix the backspace mapping

#### Check that your alt shortcuts match those in the config files

Understand that this config works on my mac for a mac terminal.
However, my mac terminal may be different than yours.
For instance, I'm using this profile: https://github.com/lingtalfi/mac-terminal-shortcuts/blob/master/README.md

Anyway, you need to know what your arrow keys return when pressed with the alt modifier, and paste your values into the .vimrc.

First, test your alt arrows codes.

In a terminal, type cat, then enter.

```bash
cat

```

After pressing enter, cat will listen to your keyboard and tell you the code that it receives.
Type the following sequences and see what code you've got. 
Here are mines:

Sequence                |           code
---------------------- | --------------------
alt+up                  |  ^[u
alt+right                  |  ^[f
alt+down                  |  ^[l
alt+left                  |  ^[b
alt+suppr                  |  ^[d
alt+backspace                  |  ^[^H


If you've got the same values, you can go to the next step.

If you have different values, you might have to do some research, but continue to read first.

What I understood is that the "caret left bracket" **^[** translates to **&lt;ESC>** in the .vimrc.

So for instance if you look at the "vim basic nav" code, in the "move to the next/prev words..." section, the first line is this:

```vim
nnoremap <ESC>f w
```
The mapped sequence is **&lt;ESC>f** (and it is mapped to **w** in this case), and it corresponds to the alt+right shortcut.

Now that you understand that, the idea is that you adapt the mappings according to the output of your terminal (using cat as a helper tool).




#### Fix the backspace mapping

By now, I'll assume that alt mappings have been taken care of, except for the alt-backspace mapping.

If you recall from the last section, the alt-backspace code is the following:

```bash
^[^H
```

However, you cannot just paste it as is in your .vimrc, you need to recreate it. I don't know why, but it's related to invisible chars that might not be transferred during a normal copy-paste operation.

So, open your .vimrc with vim, and switch to insert mode,
then go to last section, where the lines are the following:

```vim
nnoremap <ESC>^H dbh 
inoremap <ESC>^H <ESC> :normal! dbh<CR>i 
```

Now, for each of those lines, delete the ^H part, and type ctrl-v, followed by ctrl-h. 

It should write the ^H sequence for you.

When you type ctrl-v, you won't see anything, that's normal (you need to finish the sequence in order to see something).

Note: if you can notice it, the color of a successful mapping is blue like the comments (and not black).


Save the .vimrc again.

So now, your shortcuts are ready, but are YOU?

Let's practice a bit.


### Step 3: check that everything worked

So now comes the fun part.
If everything worked as expected, you'll be able to use the shortcuts.
However, it will take 2 more minutes to absorb those new shortcuts.

Ready? 

Open a terminal and type 

```bash
vim crap
```

(switch to insert mode and) Paste the following code in it:
```bash
one
two
three
four
five ioij ozjf ozej oze porpzeo rpoezjproj zeporjpzej fozeklf
six
seven
eight
nine
ten
```

For each of the tests described below, you want to start with the same file content. This means you should quit vim without saving your changes: ***:q!***, then re-open the file (do this before each test).



#### test move up and down

##### moving one line
Press ESC to switch to normal mode.

Place your cursor on line five and move the line up multiple times until you reach the top of the document (alt+up).

Check that you haven't switched to another mode.

Do the same with move down (alt+down).

Repeat the above steps but this time being in insert mode.

##### moving multiple lines

Press ESC to switch to normal mode.

Place your cursor on line five.

Type V to switch to **visual line** mode (this will select the current line), release, then press down two times to select the five, six and seven lines.

Move the lines down and up (alt+down and alt+up) multiple times.



#### test move right and left
Press ESC to switch to normal mode.

Place your cursor on line five (the longest).
Press ****End** to go to the end of the line, and **Home** to go back to the
beginning of the line.

Then move forward by word (alt+right) to the end of the line, then backward again (alt+left) to the beginning of the line.

Check that you haven't switched to another mode.

Repeat the above steps but this time being in insert mode.


#### test duplicate line(s)

##### duplicating one line
Press ESC to switch to normal mode.

Place your cursor on line five (the longest).

Duplicate the line (ctrl+d).

Undo (u).

Check that you haven't switched to another mode.

Repeat the above steps (except for the undo) but this time being in insert mode.

##### duplicating multiple lines
Press ESC to switch to normal mode.

Place your cursor on line six.

Type V to switch to **visual line** mode (this will select the current line), release, then press down two times to select the six, seven and eight lines.

Duplicate the lines (ctrl+d).


#### test remove next/prev words
Press ESC to switch to normal mode.

Place your cursor on line five (the longest).
And place your cursor at the beginning of the second word (ioij).

Delete the two words after (alt+suppr 2 times)

Then move to the beginning of the word before the last (zeporjpzej).

Delete the two words before (alt+backspace 2 times).

Your line should look like this: five ozej oze zeporjpzej fozeklf.



#### test mouse skills

##### Positioning
Press ESC to switch to normal mode.

Position your mouse on the u of the four at the 4th line (alt+click the target).

##### Copy-pasting

Press ESC to switch to normal mode.
Select lines five and six with your mouse.

Copy them into your clipboard(cmd+c).
Open sublime text (or another thing to paste your code into) and paste the clipboard (cmd+v).


##### Selecting

Sadly, we won't be able to create a vim selection with those settings, at least with the mouse.

With the current settings, your mouse is dedicated to cross device copy-pasting (and believe me that's what you want).

BUT, fortunately, to create a selection in vim is a simple as pressing V (in normal mode), and then extending/reducing the selection by pressing the up and down arrows.

Once you've got a vim selection, you can move it (alt+up/down) or duplicate it (ctrl+d), as we've just seen in the tests.







Know your vim
---------------
I suggest you learn vim basics if you have time for it.

Here is a taste of its power.

- cc: change the current line (delete the current line and switch to insert mode)
- ciw: change the current word
- ci): change inside parenthesis
- ci": change inside double quotes
- yiw: copy the current word (p to paste)
- yi2w: copy the current word and the one after that
- yy: copy the whole line
- u: undo
- ctrl+r: redo 
- A: append (go to end of the line and switch to insert mode) 
- I: insert (go to beginning of the line and switch to insert mode) 
- M: go to the middle of the screen






Related
-------------
- [vim decent navigation](https://github.com/lingtalfi/vim-decent-navigation)
- [vim survivor kit](https://github.com/lingtalfi/vim-survivor-kit)
- [vim refresher notes](https://github.com/lingtalfi/vim-refresher)





Sources
-----------
http://vim.wikia.com/wiki/Moving_lines_up_or_down

