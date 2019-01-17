# ScrolledCheckbuttonListBox_Widget
[//]: # "Created 17 January, 2018"

[![Python](https://img.shields.io/badge/Python-3.x-green.svg)](https://www.python.org/)
[![Page](https://img.shields.io/badge/Page-4.19-green.svg)](https://sourceforge.net/projects/page/?source=directory)
[![Tkinter](https://img.shields.io/badge/Tkinter-%20-green.svg)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/mit)

## Introduction
I originally needed a scrolled checked list box widget for a database application I was writing and became very frustrated because Tkinter didn't support one.  Since I was using Page as the GUI designer, I thought that I could easily write one.  However, I ran into a brick wall when trying to create on that did not require the class to be put into the main GUI python file.  I needed one that could be completely loaded, setup and used from the _support.py file that Page provides.  I was in conversation with Don Rosenberg, who wrote and maintains Page, and told him of my problems.  He asked me to send him the project and very soon, he returned the project with a working copy.  That became the core of what you see here.

The requirements started out fairly simply.

- Have a class that would support being a widget in a Page design file.
- Be able to send in a list of text items that will populate the widget.
- Be able to get notification (as an event) when an item has been checked or unchecked.
- Be able to clear all checked items.
- Be able to set various items as checked upon startup.
- Eventually be able to send in a list coning a text & database key integer and get the
      text/key back for selected items.

Below, you can find a screenshot of the widget within a Page project.

![ScrolledCheckbuttonListBox in a Page Project](/Custom Widget Demo.png)

## How to use

Using Page, design your GUI form and use a standard frame widget as a holder for the custom widget, making it the height and width you wish. Select the Custom widget from the Toolbox and place it into the frame.  Using the Widget Tree, select the Custom widget, then right click on it and select the Widget option from the context menu (you need to hold the left mouse button to do the next step. Select the Fill container option.  Save the .tcl definition file and generate the two Python files.  All of your code edits should be done in the _support.py file.


## Writing the code to support the Scrolled Checked List Box Widget

When Page generates the _support.py file for you, it put in the following dummy line for you:
```python
Custom = Frame     # To be updated by user with name of custom widget.
```
You need to change it to support the real control widget.  Change it to:
```python
Custom = ScrolledCheckedListBox
```

Next you need to add an import statement to your file...
```python
from ScrolledCheckedListBox import ScrolledCheckedListBox
```

Now you need to create a function to act as the callback for the custom control.  Page usually does this for you when you use "regular" widgets, but since Page didn't know anything about your actual widget, it couldn't create anything.  Page is awesome, but it's not psychic.
```python
def on_click(s=None):
    # Your callback handler code here...
```
Next, you need to create a function that will handle the initialization of the custom widget.  In this case, all you have to do is set the callback handler.  Notice that we use w.Custom1 to reference the custom widget.  This references the code in the main gui.py file.  Also notice that we don't use on_click(), just on_click
```python
def initialize_custom_widget():
    w.Custom1.cback = on_click
```

In the init function provided by Page, you need to call the the initialize function you just created...
```python
    initialize_custom_widget()
```
Finally, you need to send some data to the widget.  This is done using the .load() function.  The widget expects the data to be pass in the form of one of two types of lists. It can be either a list of text items like...
```python
ListInfo = ['Appetizer', 'Snack', 'Barbecue', 'Cake', 'Candy']
```
or a list of lists where each "inner" list has two items.  It needs to be in the form of ['text item',keyvalue].  Something like this...
```python
ListInfo = [['Appetizer', 1], ['Snack', 2], ['Barbecue', 3], ['Cake', 4]]
```
Ultimately you can create this data as a result from a SQL query.

Once you have your list data, then you can simply pass it into the widget like so...
```python
w.Custom1.load(ListInfo)
```
When you need to know which items in the list have been selected, use the .get() function...
```python
selecteditems = w.Custom1.get()
```
This can be called from the callback function from the actual widget callback we set earlier or through some other code.

You can clear all of the checked items programmatically by calling the .clear() function of the widget.
```python
w.Custom1.clear()
```
You can also set various items as checked by using the .set() function.  It also expects a list...
```
w.Custom1.set(['Appetizer', 'Candy', 'Breads'])
```
This will check the items with the text 'Appetizer', 'Candy' and 'Breads' in the widget.  This would normally done to reflect the value of items that match a database record.
## Version Information
- Version 1.1 supports the mousewheel to scroll the list in addition to using the mouse drag on the scroll bar.  This has been tested under Linux, but should work under Windows and MacOS.

- Version 1.2 fixes issues under Windows where the click event was not updating the list properly.

## To contact me...
If you need to contact me, please feel free to email **thedesignatedgeek[at]gmail[dot]com**
or visit my main page at https://thedesignatedgeek.xyz and use the embedded email tool.  You can also submit issues in the issues tab at the top of this page.

*Greg*
