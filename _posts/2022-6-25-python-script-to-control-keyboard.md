---
layout: post
title:  Python script to control keyboard
categories: [Python]
---

We will create a Python script to control the keyboard by stimulating key combinations and stimulating typing.

**Table of contents:**

1. Introduction to PyAutoGui
2. write() function
3. press() function
4. keyDown() and keyUp() function
5. hold() function
6. hotkey() function
7. Keyboard keys attribute
8. Simulate typing

## Introduction to PyAutoGui

PyAutoGui is a Python module for automation with the Graphical User Interface (GUI). It can be used to programmatically control the keyboard. To install just run the following command in the terminal.

```
pip install pyautogui
```

## write() function

The `pyautogui.write()` function will simulate the user typing a string of text on the keyboard. We can also specify the time delay after each character is type.

```python
import pyautogui
import time

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
# make the text editor window active
pyautogui.click(300,1060)
pyautogui.write("# Hello world!",0.25)
```

## press() function

Using the `pyautogui.press()` function, we can simulate the user pressing specific key on the keyboard. `pyautogui.press(['right', 'right'])` is equivalent to pressing the right arrow twice. `pyautogui.press('\t')` has the same result as press the Tab key once on the keyboard.

```python
import pyautogui
import time

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
# make the text editor window active
pyautogui.click(300,1060)
pyautogui.press(['right', 'right'])
pyautogui.press('\t')
```

## keyDown() and keyUp() function

The `pyautogui.keyDown('shift')` has the same effect as holding down the shift key on the keyboard and the `pyautogui.keyUp('shift')` function will released the shift key. The output on the text editor will be `@2` instead of `22`.

```python
import pyautogui
import time

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
# make the text editor window active
pyautogui.click(300,1060)
pyautogui.keyDown('shift')
pyautogui.press('2')
pyautogui.keyUp('shift')
pyautogui.press('2')
```

## hold() function

We can achieve the same result as above by using with `pyautogui.hold('shift')` statement.

```python
import pyautogui
import time

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
# make the text editor window active
pyautogui.click(300,1060)
with pyautogui.hold('shift'):
    pyautogui.press('2')
pyautogui.press('2')   
```

## hotkey() function

A common use of the `hotkey()` function is to simulate the copy (ctrl + c) action on the keyboard. Without using the `hotkey()` function, we need to make use of the `keyDown()`, `keyUp()` function written in example below.

```python
import pyautogui
import time

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
# make the text editor window active
pyautogui.click(300,1060)
pyautogui.keyDown('ctrl')
pyautogui.keyDown('c')
pyautogui.keyUp('c')
pyautogui.keyUp('ctrl')
```

By using the `hotkey()` function, the code can be simplified as the example below.

```python
import pyautogui
import time

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
# make the text editor window active
pyautogui.click(300,1060)
pyautogui.hotkey('ctrl', 'c')
```

## Keyboard keys attribute

The following key strings can be passed to the `press()`, `keyDown()`, `keyUp()` and `hotkey()` functions.

| Key strings | Explanation |
|:--------:|:--------:|
| `'0', '1', '2', ..., 'A', 'a', 'B', 'b', ..., '!', '@', '#', ...`   | numbers, alphabets, punctuation and symbol keys  |
| `'enter', '\n', 'return', '\r'`   | `Enter` Key  |
| `'\t'`   | `Tab` key  |
| `'esc'`   | `Esc` key  |
| `'shift'`, `'shiftleft'`, `''shiftright'`   | `Shift` key  |
| `'ctrl'`, `'ctrlleft'`, `'ctrlright'`   | `Ctrl` key  |
| `'alt'`, `'altleft'`, `'altright'`   | `Alt` key  |
| `'delete'`   | `Delete` key  |
| `'backspace'`   | `Back Space` key  |
| `'pageup'`, `'pagedown'`   | `Page Up`, `Page Down` key  |
| `'up'`, `'down'`, `'left'`, `'right'`   | `↑ ↓ ← →` key  |
| `'f1'`, ..., `'f12'`   | `F1` to `F12` key  |
| `'capslock'`, `'numlock'`, `'scrolllock'`   | `Caps lock`, `Num Lock`, `Scroll Lock` key  |

## Simulate typing

In the example below we simulate typing by using function provided by the `pyautogui` module. The `time` module is imported for our `time.sleep()` function. Lastly, the `pathlib` module provide the function to open and read the content of our text file.

```python
import pyautogui
import time
from pathlib import Path
```

The `time.sleep()` function provide a cooldown period to allow the user to terminate the python script.
```python
# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)
```

We use the `.read_text().splitlines()` function from the `pathlib` module to assign every new line of text into the `list` variable (`quotes`). Next the `pyautogui.click()` function is used to make the text editor window active.

```python
data_folder = Path("data/")
file_to_open = data_folder / "pangram.txt"
quotes = file_to_open.read_text().splitlines()

# make the text editor window active
pyautogui.click(300,1060)
```

In the below codes, we iterate through every items (type `strings`) in our `list` variable (`quotes`). We use `pyautogui.write()` to simulate typing of every item in our `list` variable followed by `pyautogui.press(\n)` to input a new line.

If it is an empty string (i.e. paragraph spacing),` pyautogui.write()` will have nothing to type and `pyautogui.press(\n)` will input a new line, simulating a new paragraph spacing.

```python
for item in quotes:
    pyautogui.write(item)
    pyautogui.press('\n')
```

### Here is the entire code block for reference
```python
import pyautogui
import time
from pathlib import Path

# 5 seconds cooldown to allow user to terminate the program by moving the mouse to one of the 4 corners
time.sleep(5)

data_folder = Path("data/")
file_to_open = data_folder / "pangram.txt"
quotes = file_to_open.read_text().splitlines()

# make the text editor window active
pyautogui.click(300,1060)
for item in quotes:
    pyautogui.write(item)
    pyautogui.press('\n')
```

Content of the `pangram.txt` file is listed below for reference, you may copy it to try out the above python script that simulate user typing on the keyboard.
```
Pangrams - a unique sentence in which every letter of the alphabet is used at least once. The name comes from the Greek root words pan, meaning all, and gram, meaning something written or recorded. Consider the following examples.

1. Waltz, bad nymph, for quick jigs vex. (28 letters)
2. Quick zephyrs blow, vexing daft Jim. (29 letters)
3. Sphinx of black quartz, judge my vow. (29 letters)
4. Two driven jocks help fax my big quiz. (30 letters)
5. Five quacking zephyrs jolt my wax bed. (31 letters)

6. The five boxing wizards jump quickly. (31 letters)
7. Pack my box with five dozen liquor jugs. (32 letters)
8. The quick brown fox jumps over the lazy dog. (35 letters)
9. Jinxed wizards pluck ivy from the big quilt. (36 letters)
10. Crazy Fredrick bought many very exquisite opal jewels. (46 letters)

11. We promptly judged antique ivory buckles for the next prize. (50 letters)
12. A mad boxer shot a quick, gloved jab to the jaw of his dizzy opponent. (54 letters)
13. Jaded zombies acted quaintly but kept driving their oxen forward. (55 letters)
14. The job requires extra pluck and zeal from every young wage earner. (55 letters)
```

This article was originally published at OpenGenus:

[link to my article at OpenGenus](https://iq.opengenus.org/python-script-to-control-keyboard/ "my article at OpenGenus")