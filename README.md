# Guide to JupyterLab - Debugging in JupyterLab

[*JupyterLab*](https://github.com/jupyterlab/jupyterlab) is a next-generation web-based user interface for *Project Jupyter* that enables the interactive development and organization of computing serivces across dozens of programming languages. When writing *Python* in JupyterLab, both new and experienced developers may run into issues. A *debugger* is a programming tool that can provide additional context to *errors* and *exceptions* that may occur when executing code. This guide will provide an introduction to debugging in JupyterLab. For more information on JupyerLab, check out the rest of the [Guide to JupyterLab series](https://github.com/PubChimps/JupyterLab/blob/master/README.md).

## Outline
<!--ts-->
   * [Common Mistakes](#common-mistakes)
      * [Using the Proper Kernel](#using-the-proper-kernel)
      * [Library Management](#library-management)
      * [Deleting Code and Keeping Changes](#deleting-code-and-keeping-changes)
   * [Debugging in JupyterLab](#debugging-in-jupyterlab)
      * [Magic Commands](#magic-commands)
      * [%debug](#debug)
      * [%pdb](#pdb)
   * [Review and Next Steps](#review-and-next-steps)
<!--te--> 

## Common Mistakes
Before exploring how to utilize a debugger for effective programming in JupyterLab, lets take a look at how to quickly solve three of the most frequent mistakes performed when using Jupyterlab, for more quick fixes check out [Guide to JupyterLab - Common Mistakes](https://github.com/PubChimps/JupyterLab/blob/master/README.md).

### Using the Proper Kernel
A *kernel* provides programming language support in JupyterLab. Navigating the many development environments available in JupyterLab can be tricky, especially when utilizing cloud-managed notebook sevices. For instance, [Azure Notebooks](https://notebooks.azure.com/help/jupyter-notebooks/available-kernels) offers 5 different kernels in 3 programming languages. An error may be produced by running code in the wrong environment, such as the example below, but it is an easy fix.

|![gif2.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif2.GIF?raw=true) |  
|:--:| 
| *JupyterLab is a development frontend that enables many different programming language backends* |

### Library Management
Utilizing libraries is critical in extending the fuctionality of Python. When using managed JupyterLab instances, it can be hard to identify which library packages are available, as many come preinstalled. A list of preinstalled libraries can be displayed using *shell commands*. Lines of code beginning with an exclamation point `!` will be executed as shell commands in JupyterLab. This functionality can used to identify the packages already installed in an environment by running `!pip list`. [*pip*](https://pypi.org/project/pip/) is a package installer for Python, and can install missing libraries with `!pip install <missing package>` (note: often, a kernel will have to be restarted in order for the newly installed libraries to be available).

|![gif3.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif3.GIF?raw=true) |  
|:--:| 
| *Libraries must installed before they can be imported. This is commonly done with pip* |

### Deleting Code and Keeping Changes
In JupyterLab, code that is not needed can be deleted from a notebook by using the "cut cell" feature. However, the changes made to variables, objects, and files of deleted code will remain in the current kernel session. An example is illustrated in the gif below. If the value of x is intended to be 5 and not 1000, the variable will not revert back to its intial value just because the code that altered it was deleted, and the first cell of code must be rerun.


|![gif1.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif1.GIF?raw=true) |  
|:--:| 
| *The effects of code that was previously run do not disappear after a cell's deletion* |

## Debugging in JupyterLab
If solutions to common mistakes are not resolving the issues present in Python code, a debugger may be needed to provide additional information. This section will introduce two common methods of debugging in JupyterLab.

### Magic Commands 
*Magic commands* are special enhancements that can provide additional functionality beyond typical Python syntax and are called by using the percent sign `%`. For example, the same code to run a shell command in a previous section (`!pip list`) can be rewrititen with a magic command as follows:

```
%shell
pip list
```
Magic commands can also be used for debugging in JupyterLab.

### %debug
The code below finds the intersection between two strings and will be used to showcase the `%debug` and `%pdb` features in JupyterLab.

```
def countdict(s):
    counter = dict()

    for i in s:
        if i in counter.keys():
            counter[i] += 1
        else:
            counter[i] = 1
    return counter

def intersect(s,t):
    s = s.replace(' ', '')
    t = t.replace(' ', '')
    result = ''

    count = countdict(s)

    for i in t:
        if count[i] != 0:
            result = result + i
            count[i] = count[i] - 1
  
    return result

def getintersect(s,t):
    print(intersect(s,t))
```

If copied into a notebook cell, this code can be run with `getintersect('Bloomberg','BQuant Advocate')` and will produce a KeyError as follows:


|![gif4.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif4.GIF?raw=true) |  
|:--:| 
| *KeyError* |

This KeyError can be investigated further be using `%debug`. Upon entering debugging mode, a list of available options can be presented by entering `h`. A *stack trace* is a hierarchical list of function calls that produce an exception. A full stack trace can be provided by entering `bt` and navigated by typing `up` or `down`. 


|![gif5.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif5.GIF?raw=true) |  
|:--:| 
| *h lists the available debugging commands* |

The current state of variables at the time of exception can also be displayed with `%debug`. The following gifs show how this is used to fix the KeyError.


|![gif6.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif6.GIF?raw=true) |  
|:--:| 
| *Finding variables that caused a KeyError* |


|![gif7.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif7.GIF?raw=true) |  
|:--:| 
| *Finding and fixing code thanks to `%debug`* |

`%debug` is an excellent option for debugging in JupyterLab as it is standard, lightweight, performant, and can cover a wide variety of debugging needs.

### %pdb

`%pdb` stands for "Python Debugger." Executing this magic command at the beginning of a notebook will JupyterLab to automatically enter debugging mode when it hits an error or exception. `pdb` is also a library and can be adding by placing `import pdb` at the beginning of a notebook. The addition of this library will allow the insertion of *breakpoints* to aid with debugging. A breakpoint is a location within a program where a debugger will temporarily pause execution so that a finer level of investigation can be achieved. Breakpoints can be added to a notebook with the line `pdb.set_trace()`. Here is the example similar to the one above with an included breakpoint.

```
%pdb
import pdb
def countdict(s):
    counter = dict()

    for i in s:
        if i in counter.keys():
            counter[i] += 1
        else:
            counter[i] = 1
    return counter

def intersect(s,t):
    s = s.replace(' ', '')

    count = countdict(s)

    t = t.replace(' ', '')
    result = ''

    for i in t:
        if count[i] != 0:
            result = result + i
            count[i] = count[i] - 1
  
    return result

def getintersect(s,t):
    pdb.set_trace()
    print(intersect(s,t))
```


|![gif8.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif8.GIF?raw=true) |  
|:--:| 
| *Inserting breakpoints can provide checks against troublesome code* |

From here, variables can be analzyed as before, and execution can continue to run by *stepping into* and *stepping over* subsequant code. Stepping into code brings the debugger to the next line of code in a sequence and into a function if it is called. This is achieved by entering `s` in the debugger. Stepping over code will skip over a called function and just return its results. This is useful for navigating passed functions that are known to operate as expected, and it is done by typing `n`.


|![gif9.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif2.GIF?raw=true) |  
|:--:| 
| *Stepping over the `countdict()` function with `n` as it is known to work* |

`%pdb` is a great option to use for debugging. With its easy inclusion of breakpoints, it can be especially useful when running other people's code.

## Review and Next Steps
This guide provided some examples that can help when issues arise while coding in JupyterLab, both with and without debuggers. It showed how to run shell commands in a Python notebook with `!` and `%`. It also introduced magic commands, and illustrated how `%debug` and `%pdb` can be used to provide additional context and assistance when resolving Python errors and exceptions. Some concepts are especially useful in debugging, such as analyzing a *stack trace*, inserting *breakpoints*, and *stepping into* as well as *stepping over* functions.

The [Guide to JupyterLab](https://github.com/PubChimps/JupyterLab/blob/master/README.md) has many more examples of important concepts to understand to enable success in developing with JupyterLab, including how to check for and handle exceptions with [Unit Tests](https://github.com/PubChimps/JupyterLab/blob/master/README.md) and a [Glossary](https://github.com/PubChimps/JupyterLab/blob/master/README.md) of key terms.

