---
id: windows-quickstart
title: Windows Quickstart
---

[TOC]

This page attempts to provide a quickstart guide to brand-new yaq/python
users on Windows. Please note that there are many ways to install yaq.
This guide presents one option that we believe is best for beginners.

installation
------------

On Windows, we prefer to manage Python via Anaconda. You may already
have Anaconda installed, in which case you can skip this step. Please
note that yaq requires Python 3.7 or newer. Download and install
[Miniconda3](https://docs.conda.io/en/latest/miniconda.html). You\'ll
want the 64-bit version.

Once installed, you will find that a new application \"Anaconda Prompt\"
has been added. This application allows you to interact with your new
conda environment. Launch Anaconda Prompt and enter the following
commands:


    > conda config --add channels conda-forge
    > conda install yaqd-core yaqc

This will install yaqc-python and yaqd-core-python into your conda
environment.

starting a test daemon
----------------------

The yaqd-core-python package (which you just installed) exposes the
following \"abstract\" daemons:

-   [yaqd-hardware](https://yaq.fyi/daemons/hardware/)
-   [yaqd-continuous-hardware](https://yaq.fyi/daemons/continuous-hardware/)

These are useful for testing purposes. We are going to be running a
yaqd-continuous-hardware daemon for testing purposes.

The quickest and easiest way to run daemons is via the console script
entry point. These allow us to run a specific python function straight
from the Anaconda Prompt. Try typing `yaqd-continuous-hardware` into the
Anaconda Prompt. You will see that our daemon tries to run, but there is
a `FileNotFoundError`. We need to create a config file for our daemon
(see [YEP-102](https://yeps.yaq.fyi/102/)).

The config file needs to have a very specific filepath, which is printed
by the `FileNotFoundError` that we saw before. You will need to create
some folders in the AppData folder, which is hidden by default. You can
create this file in any way you choose, but we recommend simply using
Anaconda Prompt to create the file.


    > cd AppData
    > cd Local
    > mkdir yaq
    > cd yaq
    > mkdir yaqd
    > cd yaqd
    > mkdir continuous-hardware
    > cd continuous-hardware
    > notepad config.toml

This will allow you to create a file named \"config.toml\" in the
correct folder. This file should contain exactly the following:


    [test]
    port = 38000

Now that your config file has been created, type
`yaqd-continuous-hardware` into Anaconda Prompt again. This time, rather
than raising an error and returning you to Anaconda Prompt, the daemon
will print some helpful INFO statements and continue to run. We should
leave this prompt open so that the daemon can run while we play with
clients.

communicating with your daemon
------------------------------

Now we will communicate with our daemon. Without closing your existing
daemon, open a second Anaconda Prompt. Enter into the Python REPL by
typing `python`. In the Python REPL, try the following:


    >>> import yaqc
    >>> client = yaqc.Client(38000)
    >>> client.id()
    {'name': 'test', 'kind': 'continuous-hardware', 'make': None, 'model': None,
    'serial': None}
    >>> client.get_traits()
    ['is-daemon', 'has-limits', 'has-position']
    >>> client.set_position(5)
    >>> client.get_position()
    5

Try experimenting by opening a second client (third Anaconda Prompt).

You have successfully installed yaq-python and interacted with some
basic abstract daemons. Now you have the skills you need to begin
developing daemons or clients of your own design.