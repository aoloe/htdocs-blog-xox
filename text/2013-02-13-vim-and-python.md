----
    title:Python autompletion in Vim
    date:2013-02-13
----

Today I've been asked for ways to get interactive help while typing Python code in Vim.

I had to admit that I try to use Vim "as Vanilla as possible"... but I wanted to give it a try.

So, i guess that what you most want is to have some drop downs popping up that tell you which method are avaiable in a specific context.

As a first thing, you have to make sur that Vim is compiled with the +python option. The best way to know it, is to run:

    $vim --version

If it says (among other things) `-python` you're out of luck. If it says `-python` and you're on Debian, and you only have installed plain `vim` (or, even worse, `vim-tiny`) you should install `vim-nox`to get many more options compiled in. You'll also get `+python`.

Once you've got that, it's easy to get "[Omni completion](http://vim.wikia.com/wiki/Omni_completion) to work. You can start Vim, save a .py file and type:

    import string
    string.

We will now configure Vim in a way that when your cursor is after the last dot, a pop shows up and you get a documentation in a separate pane.

In vim, type the following commands:

    :filetype plugin on
    :set ofu=syntaxcomplete#Complete

Now, while the cursor is after the dot, use the  `ctrl-x ctrl-o` to get hints in a popup!

If you like how Omni completion works, you can add the following lines to your .vimrc file:

    filetype plugin on
    set ofu=syntaxcomplete#Complete
