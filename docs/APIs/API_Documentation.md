#API Documentation

API documentation is a tells you what you need to know to use a library or work with a program. It details functions, classes, return types, and more.

In mbed, API documentation for programs and libraries is fully supported both within the mbed Online Compiler and in the code listings on the public site. 

## Browsing API documentation from within the mbed Compiler

<span class="images">![](images/docs_in_library_1.png)![](images/docs_in_library_2.png)</span>

Each documentation group contains only the documented definitions for that group:

* Classes: classes and methods.

* Structs: struct and union data types.

* Files: functions, variables, enums, defines, references to struct and unions, but no namespaces and classes.

* Groups: defined by the author; grouped documentation elements like files, namespaces, classes, functions, variables, enums, typedefs, defines and others for quick reference.

<span class="notes">**Note:** Classes, methods, functions etc that exist in the source code but aren't documented, won't appear in the documentation.</span>

## Viewing documentation

The documentation preview contains references presented as standard links that point to sub-sections of the document or to other documents, and which open inside the mbed Compiler when clicked:

<span class="images">![](images/docs_preview1.png)</span>

Clicking one of the "Definition at line X of file source.c" links would open the definition source file in the Editor (see below). The definition source file can also be opened with "Go to definition" option in the context menu from the navigation tree:

<span class="images">![](images/docs_preview2.png)</span>

<span class="notes">
**Note:** The documentation groups and individual documents **do not exist in your workspace**. They are meta navigation items that reflect the API documentation as present in the library's home page, and as such they cannot be moved, deleted, renamed and so on.
</span>

## New file from documentation example

Another notable feature of the API documentation in the mbed Online Compiler is the ability to create new files from documentation examples, making it easier to try them. The mbed Compiler would prompt for file name and may suggest ``main.cpp`` if that file doesn't already exist in the program root:

## How to add documentation to your own programs and libraries

The documentation is created out of comment blocks in your code, usually located above declarations and definitions. Let's take the following example:

```c++
class HelloWorld {
	public:
		HelloWorld();
		printIt(uint32_t delay = 0);
};
```

### Marking comments for documentation

The most important thing about code documentation is explicitly telling the system that a comment is intended for documenting (and isn't just an ordinary comment). To do that:

1. Put the comment block just above the definition.
1. Start the comment block with ``/**`` or ``/*!`` (as opposed to ``/*``, which is a normal comment).


```c++
/** My HelloWorld class.
*  Used for printing "Hello World" on USB serial.
*/
class HelloWorld {
	public:
		/** Create HelloWorld instance */
		HelloWorld();

		/** Print the text */
		printIt(uint32_t delay = 0);
};
```

You can also document single line comments, by starting them with ``///`` or ``//!``:

```c++
//! My HelloWorld class. Used for printing "Hello World" on USB serial.
class HelloWorld {
	public:
	/// Create HelloWorld instance
	HelloWorld();

	/// Print the text
	printIt(uint32_t delay = 0);
};
```

It requires almost no effort to translate scattered comments in code into well formatted documentation descriptions!

### Comment markup

Documentation has special markup to describe parameters, return values, notes, code example and so on.

Doxygen accepts reserved words prefixed with ``\`` or ``@``, where some of the commonly used are:

* @param <name> text

* @return text (synonym @returns)

* @note text

* @group <name>

* @code example @endcode

* @see ref [, ref2...] (synonym @sa)

Here is an example of advanced documentation:

```c++
/** My HelloWorld class.
*  Used for printing "Hello World" on USB serial.
*
* Example:
* @code
* #include "mbed.h"
* #include "HelloWorld.h"
*
* HelloWorld hw();
* 
* int main() {
*     hw.printIt(2);
* }
* @endcode
*/
class HelloWorld {
	public:
		/** Create HelloWorld instance
		*/
		HelloWorld();

		/** Print the text
		*
		* @param delay Print delay in milliseconds
		* @returns
		*   1 on success,
		*   0 on serial error
		*/
		printIt(uint32_t delay = 0);
};
```

To generate documentation for the example above, you have to click **Compile** > **Update Docs**:

<span class="images">![](images/docs_update2.png)</span>

Once the docs are generated, the navigation tree is refreshed and you can see the formatted documentation:

<span class="images">![](images/docs_example.png)</span>


## Extra Features

You can insert code and API documentation in to your wiki pages, notebook pages, questions, forum posts, etc.

## Example


[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/simon/code/Servo/docs/36b69a7ced07//classServo.html) 

These are done using the ``library``  macro and the URLs of the documentation you wish to pull in:

```
<<library http://mbed.org/users/simon/code/Servo/docs/36b69a7ced07/classServo.html>>
	
<<library http://mbed.org/users/simon/code/Servo/docs/tip/classServo.html>>
	
<<library http://mbed.org/users/simon/code/Servo/>>
```

Each public documentation page has a handy **Embed** code box that you can copy and paste into your wikipage.

Note the URL above with "tip" and the one with a mercurial revision hash. Linking to documentation with a tip URL means that the page will always show the latest documentation for your library.

See [Wiki syntax](http://mbed.org/cookbook/Wiki-Syntax) for more details.

## Additional notes

Last but not least, a few notes about Doxygen as being the core of the documentation generation in the mbed ecosystem:

* Doxygen is compatible with Javadoc and other documentation styles. Refer to the [Doxygen manual](http://www.stack.nl/~dimitri/doxygen/manual.html) for more information.

* Doxygen won't process the ``main.cpp`` file unless referenced in another file using ```/** @file main.cpp */``` markup. It's generally a good idea to split definitions and defines into library (libraries) instead; do not rely on ``main.cpp`` file documentation.

* Doxygen can process comments located right next to definitions and declarations, and also at other places. Refer to [documentation at other places](http://www.stack.nl/~dimitri/doxygen/docblocks.html#structuralcommands) in the Doxygen manual.
