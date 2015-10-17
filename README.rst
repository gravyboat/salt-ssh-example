salt-ssh-example
================

This is an example repo that quickly lets you create a virtualenv to spin up
salt-ssh locally while using your local keys.

Installation
------------

Clone the repo:

``git clone git@github.com:gravyboat/salt-ssh-example.git``

Configure the virtualenv:

``virtualenv salt-ssh-example/env``

.. note::
    Ensure that you DO NOT commit the env directory to source control

Activate the virtualenv:

``source salt-ssh-example/env/bin/activate``

CD to the directory and install the requirements:

``cd salt-ssh-example; pip install -r requirements.txt``

Add the log directory and log file:

``mkdir log; touch log/salt-ssh;``

You are now able to execute commands against all systems within the ``roster``
file.

.. note::
    The provided roster file has a single example system that does not actually
    work. You will need to populate this file with your systems. You can read
    more about roster files here:
    https://docs.saltstack.com/en/latest/topics/ssh/roster.html

Usage
-----

The example commands below assume that you are going to use this from a local
virtualenv so you'll need to be specific regarding which keys to use and so
forth.

Querying a system:

One with python 2.6 or greater:

``salt-ssh 'my-server.domain.com' --priv ../.ssh/my_key --roster-file roster --log-file=./log/salt-ssh -c ./config test.ping``

.. note::
    This command may take some time on minions that do not have salt in
    the tmp dir already, it is recommended to use ``-r`` when you simply want to
    user raw ssh commands. Please see the Options section below for more details
    on both the ``-r`` command, as well as removing the minion from the server
    with ``-W``.

One without using Python (using the ``-r`` option):

``salt-ssh 'my-server.domain.com' --priv ../.ssh/my_key --roster-file roster --log-file=./log/salt-ssh -c ./config -r ifconfig``

Querying every system:

``salt-ssh '*' --priv ../.ssh/my_key --roster-file roster --log-file=./log/salt-ssh -c ./config -W -max-procs 30 test.ping``

.. note::
    The above command will take a VERY long time to run, and will fail on
    systems which do not have Python 2.6. It is recommended instead to use the
    `-r` option, and omit the `-W` option like this:

``salt-ssh '*' --priv ../.ssh/my_key --roster-file roster --log-file=./log/salt-ssh -c ./config -r -max-procs 30 ls``

Options
-------

Use ``-W`` to ensure that the salt-minion is not left on the system.

For old boxes (or systems that do not have the minion in /tmp) use ``-r`` to
send raw ssh where Python is older than 2.6. This is also helpful when you
need to get things done quickly on new systems and know that the minion
will not exist in the /tmp dir.
