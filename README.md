# Guide to JupyterLab - Debugging in JupyterLab

JupyterLab is a next-generation web-based user interface for Project Jupyter that enables the interactive development and organization of computing serivces across dozens of programming languages. When writing Python in JuypterLab, both new and experienced developers may run into issues. A debugger is a programming tool that can provide additional context to errors and exceptions that may occur when executing code. This guide will provide an introduction to debugging in JupyterLab. For more information on JupyerLab, check out the rest of the Guide to JupyterLab series.

## Outline
<!--ts-->
   * [Common Mistakes](#common-mistakes)
      * [Using the Proper Kernel](#using-the-proper-kernel)
      * [Library Management](#library-management)
      * [Deleting Code and Retain Results](#delete-code-and-keeping-changes)
   * [Debugging in JupyterLab](#debugging-in-jupyterlab)
      * [Magic Commands](#magic-commands)
      * [%debug](#debug)
      * [%pdb](#pdb)
   * [Review and Next Steps](#review-and-next-steps)
<!--te--> 

## Common Mistakes
Before exploring how to utilize a debugger for effective programming in JupyterLab, lets take a look at how to quickly solve three of the most frequent mistakes performed when using Jupyterlab, for more quick fixes check out Guide to JupyterLab - Common Mistakes.

### Using the Proper Kernel
A kernel provides programming language support in JupyterLab. Navigating the many development environments availble in JupyterLab can be tricking, especially in when utilizing cloud-managed notebook sevices. For instance, [Azure Notebooks](https://notebooks.azure.com/help/jupyter-notebooks/available-kernels) offers 5 different kernels in 3 programming languages. An error may be produced by running code in the wrong environment, such as the example below, but it is an easy fix.

|![gif2.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif2.GIF?raw=true) |  
|:--:| 
| *JupyterLab is a development frontend that enables many different programming language backends* |

### Library Management
Utilizing libraries is critical in extending the fuctionality of Python. When using managed JupyterLab instances, it can be hard to identify which library packages are available, as many come preinstalled. Lines of code beginning with an exclamation point `!` will be executed as shell commands in JupyterLab. This functionality can used to identify the packages already installed in an evironment by running `!pip list`. [pip](https://pypi.org/project/pip/) is a package installer fo Python, and can install missing libraries with `!pip install <missing package>` (note: often, a kernel will have to be restarted in order for the newly installed libraries to be available).

|![gif3.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif3.GIF?raw=true) |  
|:--:| 
| *Libraries must installed before they can be imported. This is commonly done with pip* |

### Deleting Code and Keeping Changes
In JupyterLab, code that is not needed can be deleted from a notebook by using the "cut cell" feature. However, the changes made to variables, objects, and files of deleted code will remain in the current kernel session. An example is illustrated in the gif below. If the value of x is intended to be 5 and not 1000, the variable will not revert back to its intial value just because the code that altered it was deleted, and the first cell of code must be rerun.


|![gif1.GIF](https://github.com/PubChimps/JupyterLab/blob/master/media/gif1.GIF?raw=true) |  
|:--:| 
| *The affects of code that was previously run do not disappear after a cell's deletion* |

## Debugging in JupyterLab
If solutions to common mistakes are not resolving the issues present in Python code, a debugger may be needed to provide additional information. This section will introduce two common methods of debugging in JupyterLab.

### Magic Commands 
Magic commands are special enhancements that can provide additional functionality beyond typical Python snytax and are called by using the percent sign `%`. For example, the same code provide to run a shell command in a previous section (`!pip list`) can be rewrititen with a magic command as follows:

```
%shell
pip list
```
Magic commands can also be used for debugging in JupyterLab.

### %debug
### %pdb

`%pdb` stands for "Python Debugger." Executing this magic command at the begging of a notebook will allow trigger JupyterLab it automatically enter debugging mode when it hits an error or exception. The gif below illustrates the previous example, this time using `%pdb`.

## Review and Next Steps
This guide provided some examples that can help when issues arise while coding in JupyterLab, both with and without debuggers. It showed how to run shell commands in a Python notebook with `!` and `%`. It also introduced magic commands, and illustrated how `%debug` and `%pdb` can be used to provide additional context and assistance when resolving Python errors and exceptions. The Guide to JupyterLab has many more examples of important concepts to understand to enable success in developing with JupyterLab, including how to check for and handle exceptions with Unit Tests and a Glossary of key terms.

