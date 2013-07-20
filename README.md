Static Python Page Generator (pypage)
=====================================
**pypage** is a really simple static page generator for Python. Ever wanted to programmatically generate parts of your HTML page without resorting to server-side solutions? (Perhaps you can only host static pages?) Well, that's where *pypage* comes in. It's a drop-dead simple solution for generating snippets or the whole of your static page using Python.

Tutorial
--------
Here's an example of what pypage does. For the following document:

```html
<html>
    <head>
        <title><py>write("Primary Colors")</py></title>
    </head>
    <body>
        List of primary colors:
        <ul>        
            <python>
            def li(item):
                return "<li>" + item + "</li>"

            def write_items(*items):
                write( "\n".join( li(item) for item in items ) )
            
            write_items('Red', 'Blue', 'Green')
            </python>
        </ul>
    </body>
</html>
```

Running `pypage`, turns it into:

```html
<html>
    <head>
        <title>Primary Colors</title>
    </head>
    <body>
        List of primary colors:
        <ul>        
            <li>Red</li>
            <li>Blue</li>
            <li>Green</li>
        </ul>
    </body>
</html>
```
**pypage** replaces the content enclosed by `<python>` and `<py>` in your static page, with the content (passed as strings) to a series of `write()` function calls.

There are two types of Python code delimiters in *pypage*:

* Multi-line delimiters: The `<python>` tag is used when you have more than one line of Python code. If the opening `<python>` tag is indented by 8 spaces, *pypage* will remove the initial 8 characters from every line of code following the opening delimiter. _Note:_ A caveat of multi-line delimiters is that both the opening and closing tags have to be on their own lines with no other non-space characters on them.

* In-line delimiters: The `<py>` tag is used for single lines of Python code. Both the opening and closing tags have to be on the same line.

You can override these default delimiters, as will be shown in the next section.

Usage
-----
Passing the `-h` option to **pypage** will produce the following help message explaning how to use it:

    usage: pypage.py [-h] [-o OUTPUT_FILE] [-v] [-p]
                     [-a MULTILINE_DELIM MULTILINE_DELIM]
                     [-g INLINE_DELIM INLINE_DELIM]
                     input_file

    Generates static HTML pages by executing the code within <python> and <py>
    tags, and replacing replacing them with the content passed to write() calls.

    positional arguments:
      input_file            HTML input file.

    optional arguments:
      -h, --help            show this help message and exit
      -o OUTPUT_FILE, --output_file OUTPUT_FILE
                            Write output to output_file. Default: stdout
      -v, --verbose         print a short message before preprocessing
      -p, --prettify        prettify the resulting HTML using BeautifulSoup --
                            requires BeautifulSoup4
      -a MULTILINE_DELIM MULTILINE_DELIM, --multiline_delim MULTILINE_DELIM MULTILINE_DELIM
                            override the default multi-line delimiters (<python>
                            and </python>). Specify the opening and closing multi-
                            line delimiters, in sequence.
      -g INLINE_DELIM INLINE_DELIM, --inline_delim INLINE_DELIM INLINE_DELIM
                            override the default in-line delimiters (<py> and
                            </py>). Specify the opening and closing in-line
                            delimiters, in sequence.

### Using pypage as a library
**pypage** exports two functions that enable it to be used as a library. The functions are:
* `pypage(input_text, verbose=False, prettify=False, ...)` — this function takes as argument a string representing the input page and returns the resulting generated page as a string. The options arguments do exactly what their corresponding command-line options shown in the message above do. This funtion also takes 4 other keyword argument, viz:  `multiline_delimiter_open = '<python>'`, `multiline_delimiter_close = '</python>'`, `inline_delimiter_open = '<py>'`, `inline_delimiter_close = '</py>'`. They allow you to override the default delimiters.
* `pypage_files(*files, prepend_path='', verbose=False, prettify=False, **delimiter_override)` — this functions takes as arguments the names of one or more files to be processed. The keyword argument `prepend_path` will be prepended before each file's name along with a trailing `\` character. `pypage_multi` returns a dictionary mapping the file names to strings representing their corresponding generated output pages. The optional arguments for overriding the default delimiters in the previous function can also be used here.

Installation
------------
There is no automatic installation mechanism or package for *pypage* at the moment. In the future I might release a Debian/Ubuntu package on [Launchpad](https://launchpad.net/). On POSIX systems, an easy way to use *pypage* either as a library or as a command-line tool would be to symlink to `pypage.py` on the `/usr/bin` directory (for use as a command-line tool) or on the `/usr/lib/python3/dist-packages` directory (for use as a Python3 library).

### Compatibility and Updates
*pypage* has only been tested with Python 3 and most probably will not work under Python 2.x. To get updates automatically, you could clone this git repo and setup a cron job to `git pull` every now and then. Once a Debian/Ubuntu package is released, this will no longer be necessary.

Ideas
-----
If you have any ideas on how to improve *pypage*, please let me know. I am willing to accept good pull requests. Thanks for any ideas/improvements in advance!

### Todos
* `pypage-service`: Allows users to set up directories to be monitored via [pyinotify](https://github.com/seb-m/pyinotify/wiki). Files ending with `*.pypage` in these directories are automatically mapped to their corresponding pypage-processed counterparts; and the mappings are kept up-to-update thanks to [intoify](https://en.wikipedia.org/wiki/Inotify).
* Integrate [markup.py](http://markup.sourceforge.net/) into pypage -- i.e. always import markup (implicity) and make its faculties available to the programmer.

License
-------
The [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
