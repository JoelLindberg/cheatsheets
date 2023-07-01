# vim

## Configuration file

My preferred base .vimrc config below. I use 2 tabstops to be able to work with both yaml and python. Another reason for 2 tabstops is also that [Google's style guide specifies 2 tabstops](https://google.github.io/styleguide/shellguide.html#s5.1-indentation) as preferred for bash scripts. While that shouldn't matter for me personally it might be a useful beacon since other coders also might adapt to that standard.

~~~
set number
syntax on
colorscheme ron
set tabstop=2
set autoindent
" Inform vim to insert 4 spaces instead of a tab character
set expandtab
" Inform vim to always use the same distance as the tabstop when moving your text
set shiftwidth=0
~~~

Check which vim config file that is being used. While in vim you can enter the command: `echo $MYVIMRC`


## Key shortcuts

Select a block of text = `shift + V` &rarr; move with your arrow keys to select

Cut = `x` \
Copy = `y` \
Paste = `p`