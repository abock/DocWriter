DocWriter
=========

Desktop Editor for the ECMA XML Documentation.

This editor works on a tree of ECMA XML formatted documentation.   It expects
the documentation to be structed as the default output of the MonoDoc tooling,
that is:

* TopLevel Directory
  * `en/` - This is a subdirectory with the English translation
     * `ns-Foo.xml`
     * `Foo/`
       * `SomeType.xml`

When selecting a directory to document, you have to select the toplevel directory, 
and the tool will probe for the existence of `en/` directory, and the various
XML files under that directory structure.

Building it
===========

There are two user interfaces, one for Mac, and one for Windows.

* Mac: build the DocWriter.sln solution
* Windows: build the DocWriter.Windows.sln solution

How to use it:
==============

Checkout your docs
------------------

Checkout your documentation from Git.

Launch the App
--------------

Start the application, you will be prompted to open a directory,
select the toplevel for your docs, for example
"/Users/miguel/git/ios-api-docs".

Then select on the left side the node that you want to write
documentation for.  Then click on the right side (or tab) to start
editing the documentation.

Make Edits
----------

Changes are automatically saved when you switch from one page to
another.  That said, for the paranoid among us, you can hit Command-S
to save.

### Editing

Both the Edit and the Format menu contains a few useful commands to
spice up your documentation.  From adding lists, tables, examples,
links to other docs and code samples, to turning words into language
words or parameters.

Support for images is included.

### Formatting

In the Format menu there are commands that act on the selection, and
they are typically used to flag a word as a language word, a word to
be a parameter reference or a type reference (for generic methods).

### Links

Use Command-R to insert a reference, this will insert a template of a
ECMA type reference, that you can then edit to point to the actual
element you want (remember, you need things like T:System.String,
M:System.Console.WriteLine and so on).


Commit
------

Once you are happy, quit the app, and do a git commit, and push/rebase
as necessary.  You should review the changes before committing, just
in case there was a bug in the tool (in particular the tool does not
do well with rich copy pasted text).

It is recommended that you restart the app after a push/rebase, or you
risk losing data.  Because the app does not reload files that are
modified/altered behind its back.

Done!
------

And you are done!

TODO: 
-----

Some features that I would like to add to DocWriter:

- [ ] Member Lookup UI (see below)
- [ ] Support for editing delegates 
- [ ] Implement back/forward history
- [ ] Search bar at the top
- [ ] Make members clickable, so you can navigate to the contents of that particular node
- [ ] Disable formatting on paste (from console for example, we get lots of colors).
- [ ] Need to test that editing of <CDATA> sections does not have the same problems with nested divs generated by content editable

Commands:
- [ ] Hot key to insert common elements that might be referenced from the current item.   On a class, those might be members for example, on a member, those could be parameters.
- [ ] Table: Add Column
- [ ] Table: Remove Column
- [ ] Insert number list
- [ ] Clean <spans> from pasted text (even plain text is difficult to paste from a web browser)
- [ ] Command to indent/unindent region of text (for source code updates)

Bugs
----

When navigating to a property
(monoTouch.AudioToolbox.AudioChannelBit), if you click on the linked
"Bitmap", it actually *alters* the value of the target node's
container (in this case, it changes the contents of
AuiodChannelLayout)

Inserting an Example after an Example nests the example. (Command-E
twice)

It is not possible to add text after an Example.  Perhaps we need to
insert a spare div to allow editing after an example.

Searching
---------

This could be slow, but would be nice to load all the docs, so we
could search over the text

Easy Duplicator
---------------

Find a way to duplicate the body of elements from one element to
another (for example, overloads), and then the user could just fine
tune the eleemnts.

Big Ideas
---------

Perhaps we could host more than one Web View, host couple of side web
views that would dynamically get the contents from Googling on
StackOverflow side-by-side.

Perhaps show the source code for the binding for a particular API.

Tests
-----

Currently I use the body of documentation from ios-api-docs as a test.    Use the
Convert solution to load, it will try to convert all the docs to HTML and then back
from HTML into ECMA XML.

Then it uses an XML diff that ignores whitespace to determine if the
roundtripping works.  The code asserts aggressively to detect cases
that it can not handle.


Member Lookup UI
----------------

This is the user interface that pops up when you hit a keystroke to
add a link to a member.  It should offer member completion as you
type, to ensure that we actually insert references to valid targets.

It should have the ability to complete both the members in the
currently edited document as well as members from the system installed
documents (using MonoDoc.dll to resolve that).

It currently supports filtering the namespaces (only), and provides
simple code completion when pressing TAB, but does not currently have
support for inserting the result or for going beyond the namespace.

While we extract the prefix, we are not currently using it.

Ideally it should detect that it has a complete namespace, and if it
does, and you press ".", then it shows types, and the process is
repeated, when it knows you have a full type and you press "." it
should display members.

Currently, there is no really support for inserting the proper prefix,
it is just hardcoded to "N:".

Wanted:
-------

- [X] Flag members that are auto-documented as such, to now waste documenters time on it.


Handle Clicks on Links
----------------------

We could navigate to that page, 


Search Bar
----------

Linked to Command-L, as you type, it finds the member on the right side?
