# Buildtest

Welcome to buildtest home page. This page provides documentation on how to use buildtest and how to contribute to the project.

## What is buildtest?

buildtest is a HPC testing framework that provides a flexible way to test software stack on HPC systems. 
buildtest is designed to be a generic framework that can be used to test any software stack. Test are written
in YAML format called **buildspecs** that is used to describe test configuration. buildtest will translate the 
configuration into a valid test script that can be executed on the system. 

## Installation

Buildtest requires Python 3.8 or higher. You can [Install Python](https://www.python.org/downloads/) or use 
[Anaconda](https://www.anaconda.com/) to manage python installation.

Buildtest is simple to install, just clone the repo and source the setup script. We recommend you create a python
virtual environment. Shown below are the instructions assuming you have cloned the repo in your home directory.

```console
git clone https://github.com/buildtesters/buildtest.git
python3 -m venv $HOME/.pyenv/buildtest
source $HOME/.pyenv/buildtest/bin/activate
source $HOME/buildtest/setup.sh
```

## Features

Buildtest 

## Example Test

An example test configuration can be shown below, typically a test will start off with declaration of **buildspecs** followed
by name of test name ``systemd_default_target``. The ``executor`` field is used to specify the executor to use to run the test.
The ``type`` field is used to determine which schema type to use for validating schema. The ``description`` is used to provide
a description of the test. The ``run`` field is used to provide the test script to run. The ``tags`` field is used to classify 
so they can be run by a tagname.

```yaml
buildspecs:
  systemd_default_target:
    executor: generic.local.bash
    type: script
    tags: [system]
    description: check if default target is multi-user.target
    run: |
      if [ "multi-user.target" == `systemctl get-default` ]; then
        echo "multi-user is the default target";
        exit 0
      fi
      echo "multi-user is not the default target";
      exit 1
```      
## Running Test

The ``buildtest build`` command is used to build test scripts from buildspecs. Typically
one would specify path to file using ``-b`` option which can be used to specify a file or directory path.
The `-b` option can be appended multiple times to specify multiple buildspecs.

```console
buildtest build -b /path/to/buildspecs.yml
buildtest build -b /path/to/directory
buildtest build -b <dir1> -b <dir2> -b <file1>
```

## References

- [buildtest Documentation](https://buildtest.readthedocs.io/en/latest/)
- [buildtest GitHub](https://github.com/buildtesters/buildtest)
- [buildtest Slack](https://buildtesters.slack.com)
- [Join buildtest Slack](https://communityinviter.com/apps/hpcbuildtest/buildtest-slack-invitation)


