# learnvim
***
## Basic
[Vim](http://www.vim.org/) (Vi IMproved) это клон популярного текстового редактора для Unix. Он разработан с целью повышения скорости и продуктивности и повсеместно используется в большинство Unix-подобных систем. В нем имеется множество клавиатурных сочетаний для быстрой навигации к определенным точкам в файле и быстрого редактирования.
***
## Basics of navigating Vim
```
    vim <filename>   # Открыть <filename> в vim
    :q               # Выйти из vim
    :w               # Сохранить текущий файл
    :wq              # Сохранить и выйти
    :q!              # Выйти из vim не сохраняя файл

    :x               # Сохранить файл и выйти из vim, короткая версия :wq

    u                # Отмена последней команды
    CTRL+R           # Отмена отмены

    h                # Переместить курсор на один символ влево
    j                # Переместить курсор на один символ вниз
    k                # Переместить курсор на один символ вверх
    l                # Переместить курсор на один символ вправо

    # Перемещение по строке

    0                # Переместить курсор к началу строки
    $                # Переместить курсор к концу строки
    ^                # Переместить курсор к первому непустому символу в строке

    # Поиск в тексте

    /<word>          # Подсветить все вхождения <word>  в тексте после курсора
    ?<word>          # Подсветить все вхождения <word>  в тексте до курсора
    n                # Передвигает курсор к следующему вхождения искомого слова
    N                # Передвигает курсор к предыдущему вхождения искомого слова

    :%s/foo/bar/g    # Меняет «foo» на «bar» во всем файле
    :s/foo/bar/g     # Меняет «foo» на «bar» на текущей строке

    # Переходы к символу

    f<character>     # Перенести курсор к <character>
    t<character>     # Перенести курсор вперед и остановиться прямо
                     # перед <character>

    # Например,
    f<               # Перести курсор и остановиться на <
    t<               # Перенсти курсор и остановиться прямо перед <

    # Перемещение по словам

    w                # Переместиться вперед на одно слово
    b                # Перенеститься назад на одно слово
    e                # Перейти к концу текущего слова

    # Другие команды для передвижения по тексту

    gg               # Перейти к началу файла
    G                # Перейти к концу файла
    :NUM             # Перейти к строке под номером NUM
                     # (NUM может быть любым числом)
    H                # Переместить курсор к верхнему краю экрана
    M                # Переместить курсор к середине экрана
    L                # Переместить курсор к нижнему краю экрана
```
***
## Help docs
Vim has built in help documentation that can accessed with `:help <topic>`. For example `:help navigation` will pull up documentation about how to navigate your workspace!
`:help` can also be used without an option. This will bring up a default help dialog that aims to make getting started with vim more approachable!
***
## Modes
Vim is based on the concept on **modes**.
- Normal Mode - vim starts up in this mode, used to navigate and write commands
- Insert Mode - used to make changes in your file
- Visual Mode - used to highlight text and do operations to them
- Ex Mode - used to drop down to the bottom with the ':' prompt to enter commands
```
    i                # Переводит vim в режим вставки перед позицией курсора
    a                # Переводит vim в режим вставки после позиции курсора
    v                # Переводит vim в визуальный режим
    :                # Переводит vim в режим командной строки
    <esc>            # Выходит из любого режима в котором вы находитесь
                     # в командный режим

    # Копирование и вставка текста

    y                # Скопировать выделенное
    yy               # Скопировать текущую строку
    d                # Удалить выделенное
    dd               # Удалить текущую строку
    p                # Вставить скопированный текст после текущей позиции курсора
    P                # Вставить скопированный текст перед текущей позицией курсора
    x                # Удалить символ под текущей позицией курсора
```
***
## The 'Grammar' of vim
Vim can be thought of as a set of commands in a 'Verb-Modifier-Noun' format, where:
- Verb - your action
- Modifier - how you're doing your action
- Noun - the object on which your action acts on
A few important examples of 'Verbs', 'Modifiers', and 'Nouns':
```
    # «Глаголы»

    d                # Удалить
    c                # Изменить
    y                # Скопировать
    v                # Визуально выделить

    # «Модификаторы»

    i                # Внутри
    a                # Снаружи
    NUM              # Число
    f                # Ищет что-то и останавливается на нем
    t                # Ищет что-то и останавливается перед ним
    /                # Ищет строку после курсора
    ?                # Ищет строку перед курсором

    # «Существительные»

    w                # Слово
    s                # Предложение
    p                # Параграф
    b                # Блок

    # Образцы «предложений» или команд

    d2w              # Удалить 2 слова
    cis              # Изменить объемлющее предложение
    yip              # Скопировать объемлющий параграф
    ct<              # Изменяет текст от курсора до следующей открывающей скобки
    d$               # Удалить все от положения курсора до конца строки
```
***
## Some shortcuts and tricks
```
    >                 # Indent selection by one block
    <                 # Dedent selection by one block
    :earlier 15m      # Reverts the document back to how it was 15 minutes ago
    :later 15m        # Reverse above command
    ddp               # Swap position of consecutive lines, dd then p
    .                 # Repeat previous action
    :w !sudo tee %    # Save the current file as root
    :set syntax=c     # Set syntax highlighting to 'c'
    :sort             # Sort all lines
    :sort!            # Sort all lines in reverse
    :sort u           # Sort all lines and remove duplicates
    ~                 # Toggle letter case of selected text
    u                 # Selected text to lower case
    U                 # Selected text to upper case
    J                 # Join the current line with the next line

    # Fold text
    zf                # Create fold from selected text
    zd                # Delete fold on the current line
    zD                # Recursively delete nested or visually selected folds
    zE                # Eliminate all folds in the window
    zo                # Open current fold
    zO                # Recursively open nested or visually selected folds
    zc                # Close current fold
    zC                # Recursively close nested or visually selected folds
    zR                # Open all folds
    zM                # Close all folds
    za                # Toggle open/close current fold
    zA                # Recursively toggle open/close nested fold
    [z                # Move to the start of the current fold
    ]z                # Move to the end of the current fold
    zj                # Move to the start of the next fold
    zk                # Move to the end of the previous fold
```
***
## Macros
Macros are basically recordable actions. When you start recording a macro, it records **every** action and command you use, until you stop recording. On invoking a macro, it applies the exact same sequence of actions and commands again on the text selection.
```
    qa                # Start recording a macro named 'a'
    q                 # Stop recording
    @a                # Play back the macro
```
### Configuring ~/.vimrc
The `.vimrc` file can be used to configure Vim on startup.
Here's a sample ~/.vimrc file:
```
" Example ~/.vimrc
" 2015.10

" Required for vim to be iMproved
set nocompatible

" Determines filetype from name to allow intelligent auto-indenting, etc.
filetype indent plugin on

" Enable syntax highlighting
syntax on

" Better command-line completion
set wildmenu

" Use case insensitive search except when using capital letters
set ignorecase
set smartcase

" When opening a new line and no file-specific indenting is enabled,
" keep same indent as the line you're currently on
set autoindent

" Display line numbers on the left
set number

" Indentation options, change according to personal preference

" Number of visual spaces per TAB
set tabstop=4

" Number of spaces in TAB when editing
set softtabstop=4

" Number of spaces indented when reindent operations (>> and <<) are used
set shiftwidth=4

" Convert TABs to spaces
set expandtab

" Enable intelligent tabbing and spacing for indentation and alignment
set smarttab
```
***
