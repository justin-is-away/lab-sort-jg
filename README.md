# lab: sorting runtimes

In this lab you will measure just how much faster the built-in `sorted` function is than the functions you implemented on the last homework assignment.
You will also learn about how to use git submodules to connect multiple git repos together.

You will need a partner for portions of this lab.
(And recall that you must do partner work either in class or the QCL.)

## Tasks

**Task 1: initialize the repo**

Do not clone this repository.
Instead:

1. Create a new empty repo.
    Ensure that the name of your repo is different than the name of your partner's repo (so that you can fork it later).

1. Copy the contents of this repo to your new repo with the following commands
    ```
    $ git clone https://github.com/mikeizbicki/lab-sorting-runtimes
    $ cd lab-sorting-runtimes
    $ git remote rm origin
    $ git remote add origin <URL>
    $ git push origin master
    ```

    > **NOTE:**
    > In the past I used `$URL` syntax to denote a "variable" that you need to substitute into a command.
    > Both the `$URL` and `<URL>` style syntaxes are commonly used.

    > **NOTE:**
    > At this point you should ensure you understand what the `git remote` commands are doing fully.
    > I will stop giving these commands to you explicitly in the future.

**Task 2: setup the submodule**

Run the `runtimes.py` file.
You should get a `ModuleNotFoundError` that looks something like
```
$ python3 runtimes.py
Traceback (most recent call last):
  File "runtimes.py", line 9, in <module>
    from sorting.sorting import merge_sorted, quick_sorted
ModuleNotFoundError: No module named 'sorting'
```
> **NOTE:**
> In the code block above, I put both the command and the entire output of the command.
> Before the code block, I put a short summary of my steps using English intermixed with inline-code.
> When reporting error messages, it is standard to have a short English language summary and a full output of the error message in a code block.
> (Even if the output is hundreds of lines long, people will still include the full output, since all of those lines provide important information.)
> If you report your errors in this same way (e.g. on github issues/stackoverflow), you will be more likely to get good help from people.

The problem is that the `runtimes.py` file is trying to load the functions that you wrote in the sorting homework,
but it can't find those files.
The most obvious way to get access to those files is to clone your sorting homework into the repository.
BUT DON'T DO THIS!
It is never a good idea to clone one git repository inside another repository directly.
Instead, we will use something called a [git submodule](https://git-scm.com/docs/gitsubmodules) which will allow the two git repositories to "talk" to each other when needed.

<!--
> **NOTE:**
> Git has a semi-official tutorial website at <https://git-scm.com>.
> It has a (very long) tutorial on [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) that discusses all of their features, and much more than you'll need to know for this class.
-->

You can initialize a git submodule by running the command:
```
$ git submodule add <URL>
```
where `<URL>` is the url to your sorting homework.

If this command works successfully, then the command
```
$ ls -a
```
should have a new `sorting` directory in its output and a file `.gitmodules`.
If you run
```
$ ls sorting
```
then you should see all of the contents of your sorting repo.
Running the command 
```
$ python3 runtimes.py
```
should now generate no errors.
If you run
```
$ git status
```
you should see that the `sorting` repo and the `.gitmodules` file have has been added into the staging area.
You should commit theses changes and push them to github now.

If you visit the github website for your lab repo,
you should see that the `sorting` folder is now visible in the repo,
and it will have a symbol next to it that looks like `@ f43eacf` where the hash number after the `@` is the hash of the commit of the sorting repo and will be different.
Clicking on the folder should take you to the url for your sorting repo and away from this repo.
There will be no indication on your sorting repo that it is a git submodule in another repo.

**Task 3: runtimes of sorting algorithms on random lists**

Run the command
```
$ python3 runtimes.py --max_x=5
```
This will use the `timeit` module on 5 different lists of increasing size to measure the runtimes of the built-in `sort` function (which uses timsort) versus the sorting functions that you implemented in your last homework.
The output of this command, however, is currently not nicely formatted.
At the end of the file is a line labeled `FIXME 1`.
Follow the instructions so that the `runtimes.py` file generates output in markdown table format.

> **NOTE:**
> It is very common to write python code that generates code in another programming language, especially markdown or html.

After you've completed the FIXME, run the following command
```
$ python3 runtimes.py --max_x=22
```
and copy/paste the resulting table into this README file below this line.

|    x    |      timsort      |   merge_sorted    |   quick_sorted    |
| ------- | ----------------- | ----------------- | ----------------- |
|    1    | 4.351e-06 seconds | 2.649e-06 seconds | 2.172e-06 seconds |
|    2    | 5.551e-06 seconds | 1.438e-05 seconds | 1.236e-05 seconds |
|    4    | 5.111e-06 seconds | 2.667e-05 seconds | 1.189e-05 seconds |
|    8    | 2.056e-06 seconds | 1.805e-05 seconds | 1.837e-05 seconds |
|   16    | 2.652e-06 seconds | 3.715e-05 seconds | 1.385e-04 seconds |
|   32    | 5.685e-06 seconds | 9.921e-05 seconds | 1.337e-04 seconds |
|   64    | 1.123e-05 seconds | 1.796e-04 seconds | 2.688e-04 seconds |
|   128   | 1.748e-05 seconds | 3.945e-04 seconds | 5.385e-04 seconds |
|   256   | 3.445e-05 seconds | 1.026e-03 seconds | 1.538e-03 seconds |
|   512   | 9.445e-05 seconds | 2.137e-03 seconds | 2.343e-03 seconds |
|  1024   | 1.689e-04 seconds | 4.231e-03 seconds | 1.030e-02 seconds |
|  2048   | 7.720e-04 seconds | 2.376e-02 seconds | 2.303e-02 seconds |
|  4096   | 1.258e-03 seconds | 4.210e-02 seconds | 4.415e-02 seconds |
|  8192   | 1.733e-03 seconds | 7.644e-02 seconds | 9.146e-02 seconds |
|  16384  | 6.764e-03 seconds | 1.690e-01 seconds | 1.844e-01 seconds |
|  32768  | 1.441e-02 seconds | 3.265e-01 seconds | 5.202e-01 seconds |
|  65536  | 3.451e-02 seconds | 8.048e-01 seconds | 1.045e+00 seconds |
| 131072  | 7.647e-02 seconds | 1.544e+00 seconds | 1.924e+00 seconds |
| 262144  | 1.454e-01 seconds | 3.718e+00 seconds | 5.039e+00 seconds |
| 524288  | 4.955e-01 seconds | 8.401e+00 seconds | 1.081e+01 seconds |
| 1048576 | 9.249e-01 seconds | 1.703e+01 seconds | 2.320e+01 seconds |
| 2097152 | 2.109e+00 seconds | 3.801e+01 seconds | 5.097e+01 seconds |

You should observe that python's built-in sort function is 10-100x faster than yours.
All functions have the same wort-case asymptotic complexity (i.e. $\Theta(n \log n)$),
but python's built-in sorting function uses lots of optimization tricks to achieve this extra speedup.
Native python code is not very good at performing these types of optimizations,
and so the built-in function is written in the C programming language.
(You can find the [source code of the built-in function on github](https://github.com/python/cpython/blob/c1b1f51cd1632f0b77dacd43092fb44ed5e053a9/Python/bltinmodule.c#L2356)).
Functions that must be very fast are generally written in C instead of Python.
One of the differences between a *computer sciencee* major and a *data science* major is that the CS major focuses on how to *write* these fast functions,
and the DS major focuses on how to *use* these fast functions to accomplish interesting tasks.

<!--
For fun, compare the runtimes of your sorting algorithms to your partner's to see who has the fastest implementation.
-->

Push your changes to github and verify that the table is being displayed correctly.

**Task 4: cloning**

Fork your partner's repo and clone your partner's repo to the lambda server.
In this forked repo, re-run the command
```
$ python3 runtimes.py --max_x=22
```
You should get a `ModuleNotFoundError` again.

The problem is that when you clone a repo with submodules,
the submodules are by default not cloned as well.

> **ASIDE:**
> [Facebook announced in 2014 that the size of the git repo for facebook.com was 54GB](https://news.ycombinator.com/item?id=7648237),
> and it is undoubtably much larger today.
> This 54GB only includes the code directly responsible for facebook.com, and not any of the libraries that facebook.com depends on.
> Facebook's releases many public libraries on github at <https://github.com/facebook>,
> and most of these are going to be included in the main facebook.com repo as submodules.
> Some popular examples include the [react](https://github.com/facebook/react) web framework and [pytorch](https://github.com/pytorch/pytorch) machine learning library.
> Since these are their own separate git repos (and therefore submodules of facebook.com),
> developers can work on these libraries without needing access to the main facebook.com repo.
> Or if they are working at Facebook and do have access,
> they don't need to waste their time downloading all of the 54GB of code for facebook.com just to work on react or pytorch.
>
> The main repo for facebook.com is not public, and not even hosted with github.
> Recall that one of the advantages of git is that it is a *distributed version control system* not tied to any hosting provider.
> Facebook only uses github for its public repos,
> and uses an internal git service for storing its proprietary repos like the one for facebook.com.

Now run the command
```
$ ls
```
and notice that the `sorting` folder does exist in your forked repo.
The problem is that it is empty,
and you can verify that with the command
```
$ ls sorting
```
We need to *populate* or *hydrate* this folder, which is currently *unpopulated* or *dehydrated*.
You can do that with the following sequence of commands:
```
$ git submodule init
$ git submodule update
```
Now, you should be able to run the original command successfully
```
$ python3 runtimes.py --max_x=22
```

**Task 5: runtime of sorting an already sorted list**

Remain in the forked repo for this task.

We will now measure the runtime of the sort functions on data that has already been sorted.
Do this by adding the `--input=sorted` argument to get the following
```
$ python3 runtimes.py --max_x=23 --input=sorted
Traceback (most recent call last):
  File "runtimes.py", line 38, in <module>
    xs = FIXME
NameError: name 'FIXME' is not defined
```
You'll notice that you get a `NameError` because the word `FIXME` is undefined.

> **NOTE:**
> It is common practice in python to put the word FIXME as a variable in a code path that you haven't implemented yet.
> If that code path accidentally gets run, then python will raise the `NameError` to let you know that you are running code that you shouldn't be running.
> From python's perspective, there's nothing special about the word `FIXME`:
> it's just a variable name that has not been defined.

Follow the instructions in the comments to provide a proper definition of `xs`,
then rerun the command above to generate a markdown table of runtimes.
Copy/paste the table into the README file below this line.

|    x    |      timsort      |   merge_sorted    |   quick_sorted    |
| ------- | ----------------- | ----------------- | ----------------- |
|    1    | 3.684e-06 seconds | 3.275e-06 seconds | 2.835e-06 seconds |
|    2    | 2.254e-06 seconds | 9.578e-06 seconds | 1.051e-05 seconds |
|    4    | 2.325e-06 seconds | 1.563e-05 seconds | 1.709e-05 seconds |
|    8    | 3.189e-06 seconds | 3.648e-05 seconds | 3.828e-05 seconds |
|   16    | 4.698e-06 seconds | 7.688e-05 seconds | 8.176e-05 seconds |
|   32    | 7.641e-06 seconds | 1.718e-04 seconds | 1.640e-04 seconds |
|   64    | 1.596e-05 seconds | 3.773e-04 seconds | 2.623e-03 seconds |
|   128   | 2.629e-05 seconds | 8.047e-04 seconds | 9.744e-04 seconds |
|   256   | 5.819e-05 seconds | 1.868e-03 seconds | 2.315e-03 seconds |
|   512   | 1.215e-04 seconds | 2.659e-03 seconds | 3.539e-03 seconds |
|  1024   | 2.482e-04 seconds | 7.576e-03 seconds | 1.047e-02 seconds |
|  2048   | 6.703e-04 seconds | 1.926e-02 seconds | 2.474e-02 seconds |
|  4096   | 1.470e-03 seconds | 4.018e-02 seconds | 3.349e-02 seconds |
|  8192   | 3.046e-03 seconds | 9.138e-02 seconds | 1.218e-01 seconds |
|  16384  | 6.552e-03 seconds | 1.966e-01 seconds | 2.537e-01 seconds |
|  32768  | 1.554e-02 seconds | 4.270e-01 seconds | 4.586e-01 seconds |
|  65536  | 3.945e-02 seconds | 8.346e-01 seconds | 1.113e+00 seconds |
| 131072  | 7.962e-02 seconds | 1.835e+00 seconds | 2.281e+00 seconds |
| 262144  | 1.932e-01 seconds | 4.025e+00 seconds | 4.821e+00 seconds |
| 524288  | 3.824e-01 seconds | 8.024e+00 seconds | 1.087e+01 seconds |
| 1048576 | 9.331e-01 seconds | 1.748e+01 seconds | 2.387e+01 seconds |
| 2097152 | 2.156e+00 seconds | 3.793e+01 seconds | 5.148e+01 seconds |

You should notice that the built-in `sorted` function ran much faster on this input,
but your `merge_sorted` and `quick_sorted` functions have essentially the same runtimes.
This is because TimSort is designed to not have to resort already sorted data,
and its best-case runtime is therefore $\Theta(n)$ instead of $\Theta(n\log n)$.
It turns out that in practice, data is often partially sorted,
an so TimSort is much faster than even highly optimized mergesort or quicksort implementations.

Add/commit your changes and push them to the forked github repo.
Issue a pull request to your partner with your changes,
and accept your partner's pull request when you receive it.

## Submission

Submit the link to both your and your partner's github repos to sakai.
