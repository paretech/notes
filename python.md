# Python

## Slicing
- Why Python uses ["Half Open Intervals" for array slicing](https://web.archive.org/web/20190321101606/https://plus.google.com/115212051037621986145/posts/YTUxbXYZyfi), in the words of Guido van Rossum.

## Error Handling

- [Raising Hell, Catching Errors (article, David Beazley)](https://www.usenix.org/system/files/login/articles/login_apr15_10_beazley.pdf)


## Strings

- [Format Specification Mini-Language (docs)](https://docs.python.org/3/library/string.html#formatspec)
- [Format String Syntax (docs)](https://docs.python.org/3/library/string.html#format-string-syntax)
- [No formatted byte literal](https://stackoverflow.com/questions/45360480/is-there-a-formatted-byte-string-literal-in-python-3-6)

## Logging

- [Log message include function name, file name, line number (stackoverflow)](https://stackoverflow.com/questions/10973362/python-logging-function-name-file-name-line-number-using-a-single-file)
- [Community thoughts on custom logger (stackoverflow)](https://stackoverflow.com/questions/1319615/proper-way-to-declare-custom-exceptions-in-modern-python)

### Add Logging to Library

```python
import logging
log = logging.getLogger(__name__)
log.addHandler(logging.NullHandler())
```

## OpenGL
- Get into Game Development with Python and OpenGL ([YouTube Playlist](https://www.youtube.com/playlist?list=PLn3eTxaOtL2PDnEVNwOgZFm5xYPr4dUoR), [GitHub Source](https://github.com/amengede/getIntoGameDev/tree/main/pyopengl))
- [Python & OpenGL for Scientific Visualization (book, author of Glumpy)](https://www.labri.fr/perso/nrougier/python-opengl/)

## Numpy

### Structured Arrays
- [NumPy Structured Arrays](https://numpy.org/doc/stable/user/basics.rec.html) (documentation, NumPy)
- [Python Data Science Handbook - Structured Data](https://jakevdp.github.io/PythonDataScienceHandbook/02.09-structured-data-numpy.html) (book, Jake VanderPlas)

## Productivity

### Start a new Python CLI Project
- cookiecutter https://github.com/nvie/cookiecutter-python-cli.git

## Web Scraping and Testing

### Use Selenium to Setup Requests Session
Sometimes when scraping sites or testing you need to authenticate with a target site using methods like SSO before making additional requests. Libraries like Selenium are really not that great at introspecting raw responses so using the Requests module is usually desired. The following resource demonstrates minimum concept of how to setup such sessions. This method confirmed.

https://stackoverflow.com/questions/58170965/how-to-use-requests-library-with-selenium-in-python

Unconfirmed, but see also https://github.com/cryzed/Selenium-Requests

## Pandas

### Appending rows
Aww, you think you can easily append rows.


## Making Tests Importable
https://docs.python-guide.org/writing/structure/

## Decorators
- https://stackoverflow.com/questions/3931627/how-to-build-a-decorator-with-optional-parameters/3931654#3931654

### Tracer

This pair of decorators can be handy to log function and method calls on a class. 

```python
def debug(func):
    """Function decorator to log call"""
    log = logging.getLogger(func.__module__)
    msg = func.__qualname__
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        log.debug(msg)
        return func(*args, **kwargs)
    return wrapper

def debugmethods(cls):
    """Class decorator that recursively logs callables"""
    for base_cls in cls.__mro__:
        if base_cls is object:
            continue
        for name, val in vars(base_cls).items():
            if isinstance(val, type):
                setattr(cls, name, debugmethods(val))
            elif callable(val):
                setattr(cls, name, debug(val))
    return cls
```

## Catching Signals
- https://anonbadger.wordpress.com/2018/12/15/python-signal-handlers-and-exceptions/

## Blogs
- https://bm424.github.io/
- https://norvig.com/
- https://wumb0.in

## Handling Signals and Blocking Socket Calls

The origins of this demo came from a problem where TCP socket accept() calls would block indefinitely. One might encounter this situation when using sockets directly or through other modules that use sockets (like multiprocessing.connection.listener). In such situations, registering signal handlers for SIGINT are not enough as .accept() blocks indefinitely. The following example instead closes the listener which then causes the blocking accept to end with OSError. This example was inspired by David Beazley article ["raise SystemExit(0)"](https://www.usenix.org/system/files/login/articles/login_winter17_12_beazley.pdf).

```python
import atexit
import datetime
import multiprocessing.connection
import signal
import threading
import time

def exit_handler():
    print("Exit Handler")

def signal_handler(signum, frame):
    signame = signal.Signals(signum).name
    print(f'Signal handler called: {signame} ({signum})')
    raise SystemExit('Goodbye cruel world')

def listen(listener):
    # Can't use sentinel from parent thread as listener.accept blocks indefinitely
    while True:
        print(f'Listening on {listener.address}')

        try:
            print("Listening for incoming connections...")
            # Blocks forever, can't even catch sigint... 
            # Parent thread must close listener to unblock.
            conn = listener.accept()
        except OSError as e:
            print(f'Listening socket likely closed. Breaking listen loop.')
            break
        except Exception as e:
            print(f'Caught Unhandled Exception in Listener: {e}')
            raise
        else:
            # Do something with the connection...
            print("Connected!")

        try:
            read = conn.recv()
            print(f'Read: "{read}"')
        except EOFError as e:
            print(f'Did not receive client data. Ignoring...')

    # Close the listener when we're done
    listener.close()
    print('listener closed')

if __name__ == '__main__':
    # Set up signal handler for SIGINT
    signal.signal(signal.SIGINT, signal_handler)
    atexit.register(exit_handler)

    listener = multiprocessing.connection.Listener(address:=('127.0.0.1', 2524))

    threading.Thread(target=listen, args=(listener, )).start()

    try:
        while True:
            print(f'{datetime.datetime.now()}: Heartbeat')
            time.sleep(1)
    except SystemExit as e:
        print(f'Caught Exception in Main: {e}')
    finally:
        listener.close()
```


## Import Errors

Occasionally I run into import errors that at first are confusing but after investigation makes sense. Most of the import errors I run into are associated with compiled imports like OpenCV. Such imports often appear as a .pyd file (e.g. cv2.pyd). Here are some tips for troubleshooting. Note that there are some differences in the process if on Linux versus Windows. 

*Check if the .pyd file is in the the intended directory.*


*Check that the directory is on the Python search path.*

The easiest way to do this in my opinion is to use the Python debugger that allows you to inspect the import error. From there `import sys` and inspect `sys.path`. If the location of you .pyd file is not on the search path defined by sys.path, this is likely the reason.


*Check if the .pyd file is compatible with your Python version*

This sounds simple but it is easy to make mistakes here... Errors of this nature are often reported as `ImportError: DLL load failed while importing cv2`.

Using similar technique as earlier step, use the Python debugger that allows you to inspect the import error. From there `import sys` and inspect `sys.version`. Write down this value. 

You may be thinking, why not just run `python --version` from command prompt and be done. You could, but there are often other layers involved like Virtual Environments and custom build environments used by large code bases. Use the Python debugger where the actual issue was observed is the most concerete way and will let you see the error quicker than other methods. 

Now that the Python version is known, it's time to see if the .pyd file was build for that version. 

On Windows, I thought you could run `python -m pydfileinfo <filename>.pyd` but I'm not seeing that anywhere... Must be a LLVM hallucination... Instead you can use [lucasg Dependencies](https://github.com/lucasg/Dependencies) `Dependencies.exe <filename>.pyd -imports | Select-String Python`.

On Linux, `ldd <filename>.pyd | grep -i python`.

```python
import importlib
spec = importlib.util.find_spec('cv2')
mod = importlib.util.module_from_spec(spec)
```

## Find Serial Ports Matching Criteria

```python
import serial.tools.list_ports

def find_port(criteria):
    results = []

    for port in serial.tools.list_ports.comports():
        port_details = vars(port)

        criteria_of_interest = {
            key: value for key, value in port_details.items() 
            if key in criteria
        }

        if criteria_of_interest == criteria:
            results.append(port_details['name'])
        
    return results

criteria = {'manufacturer': 'Microsoft', 'PID': 1316}

matches = find_port(criteria)

# Get First Result or None even if an empty list
next(iter(find_port(criteria)), None)
```

## Low level bit and byte packing (often for hardware interfaces)

- "Python Cookbook" 3rd Edition, Chapter 6.12 "Reading Nested and Variable-Sized Binary
Structures"
- https://www.dabeaz.com/py3meta/Py3Meta.pdf
- https://wumb0.in/a-better-way-to-work-with-raw-data-types-in-python.html
- https://www.colmryan.org/posts/bitfield-nicieties-in-python/
- https://docs.python.org/3.5/library/ctypes.html#structures-and-unions

## Working with Nested and Variable-Sized Binary Structures

Here is a lightly modified example from David Beazley's Python Cookbook. This is handy when working with binary data strucutres. Can be used as a starting point for creating and reading messages.


```python
import struct

class StructField:
    """Descriptor representing a simple structure field"""
    
    def __init__(self, format, offset):
        self.format = format
        self.offset = offset
            
    def __get__(self, instance, cls):
        if instance is None:
            return self

        r = struct.unpack_from(self.format, instance._buffer, self.offset)
        return r[0] if len(r) == 1 else r

    def __set__(self, instance, value):
        struct.pack_into(self.format, instance._buffer, self.offset, value)


class NestedStruct:
    """Descriptor representing a nested structure"""

    def __init__(self, name, struct_type, offset):
        self.name = name
        self.struct_type = struct_type
        self.offset = offset

    def __get__(self, instance, cls):
        if instance is None:
            return self

        data = instance._buffer[self.offset:self.offset+self.struct_type.struct_size]
        result = self.struct_type(data)
        # Save resulting structure back on instance to avoid
        # further recomputation of this step
        setattr(instance, self.name, result)
        return result

    def __set__(self, instance, value):
        pass


class StructureMeta(type):
    """Metaclass that automatically creates StructField descriptors"""

    def __init__(self, clsname, bases, clsdict):
        fields = getattr(self, '_fields_', [])
        byte_order = ''
        offset = 0

        for format, fieldname in fields:
            if isinstance(format, StructureMeta):
                setattr(self, fieldname, NestedStruct(fieldname, format, offset))
                offset += format.struct_size
                continue

            if format.startswith(('<','>','!','@')):
                byte_order = format[0]
                format = format[1:]

            format = byte_order + format
            setattr(self, fieldname, StructField(format, offset))
            offset += struct.calcsize(format)

        setattr(self, 'struct_size', offset)


class Structure(metaclass=StructureMeta):
    def __init__(self, bytedata):
        self._buffer = memoryview(bytearray(bytedata))

    def __bytes__(self):
        return self._buffer.tobytes()

    @classmethod
    def from_file(cls, f):
        return cls(f.read(cls.struct_size))

    @classmethod
    def from_new(cls):
        return cls(memoryview(bytearray(cls.struct_size)))


class Point(Structure):
    _fields_ = [
        ('<d', 'x'),
        ('d', 'y')
    ]


class PolyHeader(Structure):
    _fields_ = [
        ('<i', 'file_code'),
        (Point, 'min'),
        (Point, 'max'),
        ('i', 'num_polys')
    ]
```

## Reading and Writing Objects to Disk

```python
import pathlib
import pickle

def save_object(filepath, data):
    filepath = pathlib.Path(filepath).expanduser().resolve()
    with filepath.open('wb') as f:
        pickle.dump(data, f)

    return str(filepath)

def load_object(filepath):
    filepath = pathlib.Path(filepath).expanduser().resolve()

    with filepath.open('rb') as f:
        return pickle.load(f)
```

## Wheelhouse (i.e. precompiled offline package install)
https://pip.pypa.io/en/latest/topics/repeatable-installs/#using-a-wheelhouse-aka-installation-bundles

On development machine:

```python
cd $project_directory
venv\scripts\activate
python -m pip wheel -r requirements.txt --wheel-dir=$wheel_directory
```

On Target Machine

```python
cd $target_directory
& $path_to_python -m venv venv
venv\scripts\activate
python -m pip install -force-reinstall -no-index -no-deps -find-links $path_to_wheelhouse -r requirements.txt
```

## Nested or Multiple (stack) of with/context managers
Pretty handy! For example if working with a vendor third-party API that has several APIs that need initialized but also need torn down if anything goes wrong. 

https://docs.python.org/3/library/contextlib.html#contextlib.ExitStack

Can use ExitStack itself as a context manager (CM) then exit_stack.pop_all() if get to bottom so that it can be destroyed with class or registered at exit.

See also https://stackoverflow.com/questions/3024925/create-a-with-block-on-several-context-managers

## Retry

Give https://tenacity.readthedocs.io/en/latest/ a try!

https://github.com/jd/tenacity


## Prefer

Prefer `ast.literal_eval` over `eval`. `ast.literal_eval` will allow you to evaluate Python datatypes and will raise an exception rather than executing random code. 

## Naming Things
- https://www.youtube.com/watch?v=hZ7hgYKKnF0


## Consume until done

```
while thumbs:
    thumbsrow, thumbs = thumbs[:cols], thumbs[cols:]
```

## Named Tuples and "Simple Points"

```
class Point(namedtuple('Point', ['x', 'y'])):
    def __mul__(self, scalar):
        return Point(self.x * scalar, self.y * scalar)
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
        
    def __sub__(self, other):
        return Point(self.x - other.x, self.y - other.y)
    
    def __truediv__(self, scalar):
        return Point(self.x / scalar, self.y / scalar)
        
    def __str__(self):
        return f"({self.x:.2f}, {self.y:.2f})"
```

## Generating Google Slides via API

https://towardsdatascience.com/creating-charts-in-google-slides-with-python-896758e9bc49

## Uninstall all PIP Installed Packages

```general
pip uninstall -y -r <(pip freeze)
```

```powershell
pip freeze | ForEach-Object { pip uninstall $_ }
```

## Singleton

```python
class SingletonMixin:
    """Mixin class to make singleton"""
    _instance = None

    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

```python
def singleton(cls):
    """Singleton decorator

    Example
    =======

    ```
    @singleton
    class SingletonMyClass(MyClass):
        pass
    ```
    """
    instances = {}
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance
```

## Style
- https://google.github.io/styleguide/pyguide.html

## Defining a Custom Statement (dont do this? :))

- https://stackoverflow.com/questions/214881/can-you-add-new-statements-to-pythons-syntax
- https://eli.thegreenplace.net/2010/06/30/python-internals-adding-a-new-statement-to-python/

## Windows Long File Paths

https://stackoverflow.com/questions/21194530/what-does-mean-when-prepended-to-a-file-path

## Generate Stubs (.pyi) from .NET CLR

I've successfully done this before using [McNeel's PythonStubs package](https://github.com/mcneel/pythonstubs).

```sh
Get-ChildItem -Path "<path_to_vendor_dll_library>" -Include *.dll -File -Recurse -ErrorAction SilentlyContinue | ForEach-Object {.\PyStubbler --dest="<output_dir>"" --search="<path_to_vendor_dll_library>" (Resolve-Path $_).path}
```


## Type Hinting

- https://docs.python.org/3/library/typing.html
- https://www.reddit.com/r/Python/comments/10zdidm/why_type_hinting_sucks/?rdt=64359

## Secure Shell (SSH)

- [Paramiko](https://docs.paramiko.org/en/stable/index.html) (low-level)
- [Fabric](https://github.com/fabric/fabric) (high-level)

  ## Options for dictionary like interfaces for classes
  https://stackoverflow.com/questions/35282222/in-python-how-do-i-cast-a-class-object-to-a-dict
- [Parallel-SSH](https://github.com/ParallelSSH/parallel-ssh)
