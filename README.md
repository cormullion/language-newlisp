# newLISP language support in Atom

version of 2014-03-14 19:18:38

This package adds snippets and syntax highlighting for newLISP.

Contributions are greatly appreciated. Please fork this repository and open a
pull request to add snippets, make grammar tweaks, etc.

### The grammar

The file 'grammars/newlisp.cson' more or less defines newLISP syntax using regular expressions. These split the language up into *scopes*, which can then be styled separately by the editor, using the stylesheets.

There is much scope for improvement...

### The snippets

I'm not sure which approach to use for snippets. At the moment I'm thinking that I want to type a newLISP function only check the syntax/order of arguments, rather than to fill out a template. So you'll see every alternative syntax for a function, separated by '§' characters. So the snippet does not create working code:

(append list-1 [list-2 ... ]) § (append array-1 [array-2 ... ]) § (append str-1 [str-2 ... ]))

As of version 0.70, I'm currently fighting with the automatic parenthesis-closing (which I don't like, but which is part of the 'bracket matching' package, which I do like). At the moment, what happens is:

1  you type an opening parenthesis (and delete the automatically added closing parenthesis if necessary), followed by the name of a newLISP function

2  you press TAB

3  the syntax of that function (as in the newLISP manual) appears in the text.

4  the closing parenthesis is added

![animation](https://raw.github.com/cormullion/language-newlisp/master/animation1.gif)

But if you started typing inside an automatically-closed pair:

    ( )

then you'll have too many final parentheses when the expansion of the snippet has finished.

![animation](https://raw.github.com/cormullion/language-newlisp/master/animation.gif)

In other words, the snippet doesn't realise you've started with an opening parenthesis so isn't smart enough to not add an unwanted closing parenthesis.

An alternative approach is to use templates with slots, but I haven't done this yet.

### The stylesheets

Because all the syntax highlighing is using CSS (or something akin to CSS), it's easy - and extremely tempting - to specify the colors of different scopes over and above the coloring provided by the current Atom theme.

I've added a few of them to the file 'stylesheet/styles.less':

    .entity.paren.newlisp - color, opacity etc. of parentheses
    .invalid.deprecated.newlisp - files listed as 'deprecated' in grammars/newlisp.cson
    .string.quoted.braced.newlisp - {strings}
    .string.quoted.tagged.newlisp - [text]strings[/text]
    .string.quoted.double.newlisp - "strings"
    .punctuation.definition.string - the ", {, or [text] bit...

## Tips for working with newLISP in Atom

Currently, two packages are available for installing that will let you run newLISP scripts directly.

The *runner* package requires you to add the following to your ~/.atom/config.cson - you don't have to edit the package at all, just download it. This will do:

    'runner':
      'scopes':
        'coffee': 'coffee'
        'js': 'node'
        'ruby': 'ruby'
        'python': 'python'
        'go': 'go run'
        'newlisp': 'newlisp'
      'extensions':
        'spec.coffee': 'jasmine-node --coffee'
        'lsp': 'newlisp

Command-R (Mac) will run the file pointed to by the current window.

The *script* package has already been modified to recognise newLISP 'out of the box'. Command-I (Mac) will run the file pointed to by the current window.
