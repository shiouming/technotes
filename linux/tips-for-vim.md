# Handy tips for using vim editor

Creation date: 2008-4-20

Keywords: *vim, vi, linux*

---


## Running vim editor

1. To *substitute* vi editor with vim

   Use which command to find out the full path of the vim editor
   ```
   which vim
   ```

   then use alias command to overwrite the vi usage
   ```
   alias vi="<path_to_vim>"
   ```

   Now typing vi <filename> will bring up vim editor instead.

   For removing this alias, just type
   ```
   unalias vi
   ```

1. To open file in read-only mode
   ```
   vim -R <filename>
   ```

1. To open file in binary mode
   ```
   vim -b <filename>
   ```

## Searching text within vim editor

1. To make text searching within vim editor case insensitive or the other way around
   ```
   :set ic
   :set noic
   ```

1. To search and replace all occurrences of a particular text pattern within the file, with or without confirmation
   ```
   :%s/<search_pattern>/<replacement_text>/g
   :%s/<search_pattern>/<replacement_text>/gc
   ```


## Display formatting

1. To show or hide invisible characters, such as SPACE and TAB
   ```
   :set list
   :set nolist
   ```


## Indentation

1. Set width of TAB (in number of whitespaces)
   ```
   :set tabstop=4
   ```

1. Set width of indentation shifting (in number of whitespaces)
   ```
   :set shiftwidth=4
   ```

1. To turn off/on smartindent feature (especially useful when pasting source code of certain languages, sometimes smartindent might messes up indentation)
   ```
   :set nosmartindent
   :set smartindent
   ```

1. Expand tab, entering TAB character produces multiple SPACE characters instead
   ```
   :set et
   :set noet
   ```

1. To shift indentation of current line forward/backward for one level, **regardless of cursor position on the line**
   ```
   # under INSERT mode
   <CTRL> t
   <CTRL> d

   # under COMMAND mode
   >>
   <<
   ```


## Navigation under COMMAND mode

1. Move cursor to beginning or end of the line
   ```
   0
   $
   ```

1. Move cursor to first or last line of the file
   ```
   gg
   <shift> g
   ```
