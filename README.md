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

Buildtest commands with many features to help you build, run, and inspect tests. Below are some of the features:

- Build and run tests from buildspecs
- Support job submission to resource managers including Slurm, LSF, PBS and Cobalt 
- Integration with environment modules and Lmod including CrayPE
- Support for running tests in containers (Docker, Singularity, Podman)
- Publish test results to CDASH
- Query report file and filter or format output to find relevant information
- Support for running tests in parallel
- Multi test generation via compilers or executors

## Example Test

An example test configuration can be shown below, typically a test will start off with declaration of *buildspecs* followed
by name of test name `systemd_default_target`. The `executor` field is used to specify the executor to use to run the test.
The `type` field is used to determine which schema type to use for validating schema. The `description` is used to provide
a description of the test. The `run` field is used to provide the test script to run. The `tags` field is used to classify 
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

The ``buildtest build`` command is used to build test scripts from buildspecs. Typically
one would specify path to file using `-b` option which can be used to specify a file or directory path.
The `-b` option can be appended multiple times to specify multiple buildspecs.

```console
buildtest build -b /path/to/buildspecs.yml
buildtest build -b /path/to/directory
buildtest build -b <dir1> -b <dir2> -b <file1>
```

A typical build output for this kind of test would look like this:

```console
(buildtest) spack@adf5079df74b:~/buildtest$ buildtest build -b general_tests/configuration/systemd-default-target.yml 
╭─────────────────────────────────────────────────── buildtest summary ───────────────────────────────────────────────────╮                                                                                                             
│                                                                                                                         │                                                                                                             
│ User:               spack                                                                                               │                                                                                                             
│ Hostname:           adf5079df74b                                                                                        │                                                                                                             
│ Platform:           Linux                                                                                               │                                                                                                             
│ Current Time:       2024/02/29 20:07:35                                                                                 │                                                                                                             
│ buildtest path:     /home/spack/buildtest/bin/buildtest                                                                 │                                                                                                             
│ buildtest version:  1.8                                                                                                 │                                                                                                             
│ python path:        /home/spack/pyenv/buildtest/bin/python3                                                             │                                                                                                             
│ python version:     3.11.6                                                                                              │                                                                                                             
│ Configuration File: /home/spack/buildtest/buildtest/settings/spack_container.yml                                        │                                                                                                             
│ Test Directory:     /home/spack/runs                                                                                    │                                                                                                             
│ Report File:        /home/spack/buildtest/var/report.json                                                               │                                                                                                             
│ Command:            /home/spack/buildtest/bin/buildtest build -b general_tests/configuration/systemd-default-target.yml │                                                                                                             
│                                                                                                                         │                                                                                                             
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯                                                                                                             
───────────────────────────────────────────────────────────────────────────────────────────────────────  Discovering Buildspecs ────────────────────────────────────────────────────────────────────────────────────────────────────────
                             Discovered buildspecs                              
╔══════════════════════════════════════════════════════════════════════════════╗
║ buildspec                                                                    ║
╟──────────────────────────────────────────────────────────────────────────────╢
║ /home/spack/buildtest/general_tests/configuration/systemd-default-target.yml ║
╟──────────────────────────────────────────────────────────────────────────────╢
║ Total: 1                                                                     ║
╚══════════════════════════════════════════════════════════════════════════════╝


Total Discovered Buildspecs:  1
Total Excluded Buildspecs:  0
Detected Buildspecs after exclusion:  1
────────────────────────────────────────────────────────────────────────────────────────────────────────── Parsing Buildspecs ──────────────────────────────────────────────────────────────────────────────────────────────────────────
Valid Buildspecs: 1
Invalid Buildspecs: 0
/home/spack/buildtest/general_tests/configuration/systemd-default-target.yml: VALID
Total builder objects created: 1
                                                                                                 Builders by type=script                                                                                                  
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━┳━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ builder                         ┃ type   ┃ executor           ┃ compiler ┃ nodes ┃ procs ┃ description                                  ┃ buildspecs                                                                   ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━╇━━━━━━━╇━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩
│ systemd_default_target/1b413a0c │ script │ generic.local.bash │ None     │ None  │ None  │ check if default target is multi-user.target │ /home/spack/buildtest/general_tests/configuration/systemd-default-target.yml │
└─────────────────────────────────┴────────┴────────────────────┴──────────┴───────┴───────┴──────────────────────────────────────────────┴──────────────────────────────────────────────────────────────────────────────┘
──────────────────────────────────────────────────────────────────────────────────────────────────────────── Building Test ─────────────────────────────────────────────────────────────────────────────────────────────────────────────
systemd_default_target/1b413a0c: Creating Test Directory: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/1b413a0c
──────────────────────────────────────────────────────────────────────────────────────────────────────────── Running Tests ─────────────────────────────────────────────────────────────────────────────────────────────────────────────
Spawning 8 processes for processing builders
───────────────────────────────────────────────────────────────────────────────────────────────────────────── Iteration 1 ──────────────────────────────────────────────────────────────────────────────────────────────────────────────
systemd_default_target/1b413a0c does not have any dependencies adding test to queue
     Builders Eligible to Run      
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ Builder                         ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩
│ systemd_default_target/1b413a0c │
└─────────────────────────────────┘
systemd_default_target/1b413a0c: Current Working Directory : /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/1b413a0c/stage
systemd_default_target/1b413a0c: Running Test via command: bash systemd_default_target_build.sh
systemd_default_target/1b413a0c: failed to submit job with returncode: 1
────────────────────────────────────────────────────────────────────────────────────────── Error Message for systemd_default_target/1b413a0c ───────────────────────────────────────────────────────────────────────────────────────────

systemd_default_target/1b413a0c: Detected failure in running test, will attempt to retry test: 1 times
systemd_default_target/1b413a0c: Run - 1/1
systemd_default_target/1b413a0c: Running Test via command: bash systemd_default_target_build.sh
systemd_default_target/1b413a0c: failed to submit job with returncode: 1
systemd_default_target/1b413a0c: Test completed in 8.617189 seconds with returncode: 1
systemd_default_target/1b413a0c: Writing output file -  /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/1b413a0c/systemd_default_target.out
systemd_default_target/1b413a0c: Writing error file - /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/1b413a0c/systemd_default_target.err
                                      Test Summary                                      
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━┳━━━━━━━━━━━━┳━━━━━━━━━┓
┃ builder                         ┃ executor           ┃ status ┃ returncode ┃ runtime ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━╇━━━━━━━━━━━━╇━━━━━━━━━┩
│ systemd_default_target/1b413a0c │ generic.local.bash │ FAIL   │ 1          │ 8.617   │
└─────────────────────────────────┴────────────────────┴────────┴────────────┴─────────┘



Passed Tests: 0/1 Percentage: 0.000%
Failed Tests: 1/1 Percentage: 100.000%


Adding 1 test results to report file: /home/spack/buildtest/var/report.json
Writing Logfile to /home/spack/buildtest/var/logs/buildtest_a99wl1r2.log

```

Once test is complete you can view the output and error content using the `buildtest inspect query` command. This command will take an argument that is the name of the
test in this case `systemd_default_target` is the name of test that was specified in the buildspec. 

A typical output would look something like this:

```console
(buildtest) spack@adf5079df74b:~/buildtest$ buildtest inspect query -o -e systemd_default_target
───────────────────────────────────────────────────────────────────────────────────── systemd_default_target/3c14104c-6da6-4beb-afb0-f7c77494ee35 ──────────────────────────────────────────────────────────────────────────────────────
Executor: generic.local.bash
Description: check if default target is multi-user.target
State: FAIL
Returncode: 1
Runtime: 8.196687 sec
Starttime: 2024/02/29 20:09:09
Endtime: 2024/02/29 20:09:17
Command: bash systemd_default_target_build.sh
Test Script: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/3c14104c/systemd_default_target.sh
Build Script: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/3c14104c/systemd_default_target_build.sh
Output File: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/3c14104c/systemd_default_target.out
Error File: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/3c14104c/systemd_default_target.err
Log File: /home/spack/buildtest/var/logs/buildtest_e8hctigl.log
────────────────────────────────────────────────── Output File: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/3c14104c/systemd_default_target.out ──────────────────────────────────────────────────
==> Regenerating tcl module files                                                                                                                                                                                                       
multi-user is not the default target                                                                                                                                                                                                    
                                                                                                                                                                                                                                        
────────────────────────────────────────────────── Error File: /home/spack/runs/generic.local.bash/systemd-default-target/systemd_default_target/3c14104c/systemd_default_target.err ───────────────────────────────────────────────────                                                                                                                                                                                                                                                                                                                                                                                     
```

## Reporting Issues

Please report all issues related to buildtest at https://github.com/buildtesters/buildtest/issues. You may consider posting your question in slack to see 
if someone can help you.

## References

- [buildtest Documentation](https://buildtest.readthedocs.io/en/latest/)
- [buildtest GitHub](https://github.com/buildtesters/buildtest)
- [buildtest Slack](https://buildtesters.slack.com)
- [Join buildtest Slack](https://communityinviter.com/apps/hpcbuildtest/buildtest-slack-invitation)


