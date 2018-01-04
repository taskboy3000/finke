# Finke

A prototyping tool for card games.

## USAGE

  ./finke [DIR]

## DESCRIPTION

Finke is a prototyping tool for card games.  One of the challenges in
creating card games is keeping track of all the cards you have in a
way that allows for changes to be made easily.

Finke is a simple command line Perl script that wraps around the very
powerful Template Toolkit system.  It works with a user supplied JSON
file the defines the content of each card and one template file that
defines the layout of a card.  There is even an optional global JSON
that may be use to define template variables found in the rules JSON
file.  Finke then generates an HTML page that can be printed through a
web browser (like Chrome).

Additionally, the finally HTML contains references to jQuery,
Bootstrap and Font Awesome, so feel free to use those facilities in
your card layout.

## INSTALLATION

You must have a modern Perl on your system.  Perl 5.8 should work, but
5.2x is perferred.

1. Clone this respository to a local sandbox

2. Install the Perl module dependencies listed in the cpanfile (I
recommend installing carton for this, but apt-get/yum will work too).

3. Test the installation with 'make test'.  You should get an HTML file.

## CREATE A NEW GAME

Finke works on a project directory that must contain the following files:

 * rules.json
 * card.html.tt

Optionally, it may contain a 'global.json' file, which is discussed later.

### Example Project

Let's walkthrough setting a new project called Battlefish.

First, create a directory for our project files:

  mkdir battlefish

Change into that directory.

Next, create a simple card layout template in a file called card.html.tt:

```html
  <div class="card">
    [% card.id %] : [% card.rule %]
  </div>

Every card layout should be enclosed an a block element called 'card'.
This template is included in a layout template called cards.html.tt
which is part of the Finke project.

This simple card template is going to print an ID and rule.  If you
have some skill with HTML and css, you can make more sophisticated
layouts.  There is no builtin limits to the number of properties card
can have, although space is limited.

All card properties are accessed through a card object in the template.

Let's define the content of our cards in a file called rules.json:

```javascript

  [
    { "id": "1", "rule": "Take another card" },
    { "id": "2", "rule": "Get another turn" },
    { "id": "3", "rule": "You win!" }
  ]

The rules structure is just a list of objects written in object
notation. Each object represents one card.  The is no requirement on
the names of object properties outside normal javascript syntax.  The
layout template never refers to the details of any of these
objects. It simple cards that there is a list (even an empty one) over
which to iterate.

Now the contents of the card.html.tt template should make more sense.
Every card object has two properties: id and rule.  The template
ensures that both properties appear on the cards.

Finally we can generate the HTML by invoking Finke:

/path/to/install/dir/finke /path/to/battlefish

### ADVANCED OPTIONS

There are a couple of optional files you may want to use in your prototype.

##### card.css

If there is a card.css file in your project directory, it is included
in your HTML output.  This css file is a convenient place to put the
CSS for you require for the HTML structures in card.html.tt, if you
like unobtrusive code.

#### global.json

This is another optional file located in your project directory. It is
simply an object whose properties map to text or HTML to that you wish
to replace in your card template.  For example, you might have suit
icons that have a font glyph associated with them.  Instead of writing
out that HTML directly, you might have a global called 'HEARTS' that
prints '<i class="fa fa-hearts"></i>'.

Other tricky thing you can do in the template file is to reprocess a
template string.  For example, you can have a property of a card object in rules.json that is a string with a global template variable in it:

```javascript
   [ { "suit": "[% HEARTS %]" } ]

In your templating code, you would need to process the suit property
again to get the HTML you want.

```html
   <div class="card">
      [% card.suit.process %]
   </div>

This will render the correct HTML.

## LICENSE

This software is released under the MIT license.  See LICENSE.txt for details.

## ABOUT

Finke is the name of the oldest known river on Earth.  If you follow a
river, you will always end up in an interesting place.

For comments and issues, please send email to Joe Johnston <jjohn@taskboy.com>.
