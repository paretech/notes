# Tkinter (Python)

Tkinter is the GUI framework distributed with Python.

## Resources

Apparently, documentation is a challenge for Python's Tkinter... Tkinter is based on "core" Tcl/Tk. Tkinter is reportedly more than a "thin wrapper". As such, you need to do some leg work...

Tcl/Tk version 8.6 has been in Python for some time, I don't have an exact Python version, but I believe at least since Python 3.8.

Tcl/Tk version 9.0.0 released September 26, 2024. First major Tcl/Tk release in 27 years!!!

As of Python 3.13, Windows is using Tcl/Tk v8.6.

- [Python Docs - Tkinter](https://docs.python.org/3/library/tkinter.html)
- [official Tk 8.6 command reference documentation at the main Tcl/Tk developer site](https://www.tcl-lang.org/man/tcl8.6/TkCmd/contents.htm)
- [The late John Shipman Tkinter Documentation (Tcl/Tk v 8.5 circa 2013)](https://tkdocs.com/shipman/index.html)

## Get Version

```
import tkinter as tk

print(f"Tcl/Tk version {tk.TclVersion}/{tk.TkVersion}")
```