# Guidelines on Code Submission and Maintenance

## Table of Content:
- [Using Github: A Recap](#using_github)
  - [Creating a new repository](#create_repo)
  - [The Basic Commands](#basic_commands)
  - [Resources on Collaborative Projects](#collab_projects)
  - [Further Resources](#furt_res_1)
- [Best Practices for Code Maintenance](#best_practices)
  - [Further Resources](#furt_res_2)

<a name="using_github"></a>
## USING GITHUB: A RECAP

<a name="create_repo"></a>
### Creating a new repository

(a) CREATING LOCAL GIT REPOSITORY

Note that steps 1 and 2 are only necessary if you are creating the repository from scratch. If the project already exists on your computer, skip these 2 steps.

1. Create folder on your computer to contain the project.
2. Write code as intended and save file(s) to the new folder.
3. Navigate to this folder via command prompt or git bash (cd C:\yourpath\yourfolder).
4. Type `git init`
5. Type `git add filename.py` to add the new file(s)
6. Type `git commit -m 'yourmessage'`

(b) CONNECTING THE LOCAL REPOSITORY TO GITHUB
1. Go to github
2. Log in to your account
3. Click the new repository button (top right)
4. Click 'create repository'
5. Choose 'push an existing repository'

Alternatively, use the following commands from the terminal to do (b): 
```
$ git remote add origin git@github.com:username/new_repo
$ git push -u origin master
```

<a name="basic_commands"></a>
### The basic commands

AFTER MAKING LOCAL CHANGES:

- `git add`: indicates what changes to save. After you’ve made some small modifications to your project locally and checked that they work, use ‘git add’ to indicate that they’re ready.

- `git commit -m 'yourmessage'`: use this command to add the modifications to the repository; add a brief commit message (40-60 characters) indicating what this update adds. If you want to amend your last commit message, type: `git commit –amend -m 'new commit message'`.

- `git commit -a`: use this command if you want to commit all of the modifications you’ve made without having to explicitly add each file. This allows you to skip the separate ‘add’ and ‘commit’ commands.

- `git push`: use this command to push committed changes to github.

SEEING WHAT YOU HAVE CHANGED:

- `git status`: lists files that have been changed, as well as new files that haven’t been formally added.

- `git diff`: shows you which lines have been added and which have been deleted.

- `git log`: shows a list of all the commits made to the repository. 

PULL CHANGES FROM COLLABORATORS:

- `git fetch`: pulls changes from a remote but does not merge with local (eg. if you're working in master origin/master will be updated where as master stays the same). Use `git diff master origin/master` to check differences between remote and local.  

- `git pull`: pulls changes from a remote and instantly merges with local.

<a name="collab_projects"></a>
### Resources on collaborative features:
- Creating branches and merging: Creating separate branches is useful to develop a feature without disturbing the master branch; once you are satisfied you can merge these changes into the master. [Here](https://kbroman.org/github_tutorial/pages/branching.html) you can find instructions on how to branch and merge.

- Merge conflicts: [Here](https://kbroman.org/github_tutorial/pages/fork.html) are instructions for handling merge conflicts.

- Forking: [Here](https://kbroman.org/github_tutorial/pages/fork.html) are instructions for contributing to someone's repository via forking.

- Formatting: [Here](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) are instructions on how to format, e.g. when writing your README file.

<a name="furt_res_1"></a>
### Further resources:
General tutorial which the instructions above are based on: [Click here](https://kbroman.org/github_tutorial/)

Annotated lecture slides on git & github: [Click here](https://kbroman.org/Tools4RR/assets/lectures/04_git_withnotes.pdf)

Overview of git commands: [Click here](https://github.com/kbroman/Tools4RR/blob/master/04_Git/GitCommands/git_notes.md)

<a name="best_practices"></a>
## BEST PRACTICES FOR CODE MAINTENANCE

- **Commit messages**: write good and concise commit messages that will also make sense at a later timepoint.

- **Create a README file for your project**: when you create a new repository, consider making a README that gives an overview of what the different scripts in your project do. You can find an example template [here](https://github.com/elsewhencode/project-guidelines/blob/master/README.sample.md).

- **Commenting**: Write comments as you code and don't leave this for later; make sure that they would still make sense months later, when you might be working on something else. 

- **Avoid hardcoded values**: if absolutely necessary, add a comment explaining where the value came from - is it from an equation, a paper, an experimental observation...?

- **Naming variables and functions**: ideally choose names that are self-explanatory to improve readability.

- **Default argument values**: consider specifying values for variables at the end of a parameter list; e.g. def function1(a, b=0), especially when you already know that there will be only rare occasions where you would need to override these default values. Be careful to not use mutable objects as default values in the function or method definition.

- **Make error messages as informative as possible**: write error messages that provide solutions or direct you to helpful resources. E.g. `Error: Translation failed because of an invalid codon ("IQT") in position 1 in sequence 41. Ensure that this is a valid DNA sequence and not a protein sequence."`

- **Write docstrings for your functions**: docstrings specify the type of arguments and outputs the function will return. Here is an example:

```
def add_binary(a, b):
    '''
    Returns the sum of two decimal numbers in binary digits.
            Parameters:
                    a (int): A decimal integer
                    b (int): Another decimal integer
            Returns:
                    binary_sum (str): Binary string of the sum of a and b
    '''
    binary_sum = bin(a+b)[2:]
    return binary_sum
print(add_binary.__doc__)
```

- **Consider using numba to speed up your code**: Numba is a just-in-time compiler for Python; it can be installed via Anaconda (`conda install numba`) or pip (`pip install numba`), or compiled from source. Numba will particularly give you speed benefits if your code is numerically oriented (does a lot of math), uses NumPy a lot, or has a lot of loops. Numba uses decorators (the fundamental one is `jit`), which you will have to import at the beginning of your script (`from numba import jit`), and then add just before the function you want to speed up. Here is an example from the numba documentation:

```
from numba import jit
import numpy as np
x = np.arange(100).reshape(10, 10)
@jit(nopython=True) # Set "nopython" mode for best performance, equivalent to @njit
def go_fast(a): # Function is compiled to machine code when called the first time
    trace = 0.0
    for i in range(a.shape[0]):   # Numba likes loops
        trace += np.tanh(a[i, i]) # Numba likes NumPy functions
    return a + trace              # Numba likes NumPy broadcasting
print(go_fast(x))
```
Instead of writing `@jit(nopython=True)`, you can use the `@njit` decorator, which is an alias for the former.

Useful extra options that you can specify in your decorator are:
`parallel = True` (enables the automatic parallelization of the function)
`fastmath = True` (enables fast-math behaviour for the function)

- **Consider using pylint to find bugs or style problems**: you can install pylint via pip (`pip install pylint`), or from the source distribution. Pylint is meant to be called from the command line (e.g. `pylint mymodule.py`).

<a name="furt_res_2"></a>
### Further resources:
Python style guide: [Click here](https://google.github.io/styleguide/pyguide.html)

Numba documentation: [Click here](https://numba.readthedocs.io/en/stable/user/index.html)

Pylint tutorial: [Click here](https://pylint.pycqa.org/en/latest/tutorial.html)

Paper on documenting scientific code: [Click here](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1006561)
