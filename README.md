## simp-tpm12-simulator

<!-- vim-markdown-toc GFM -->

* [Overview](#overview)
  * [This is a SIMP project](#this-is-a-simp-project)
* [Setup](#setup)
  * [Requirements](#requirements)
  * [Building the `simp-tpm12-simulator` RPM](#building-the-simp-tpm12-simulator-rpm)
  * [Beginning with simp-tpm12-simulator](#beginning-with-simp-tpm12-simulator)
* [Usage](#usage)
* [Development](#development)

<!-- vim-markdown-toc -->

## Overview

Rake and config files to build/package newer versions of the [IBM TPM 1.2
simulator][ibmswtpm12] as EL6 and EL7 RPMs.

### This is a SIMP project

These module is a component of the [System Integrity Management
Platform][simp] a compliance-management framework built on Puppet.

If you find any issues, please submit them to our [bug tracker][simp-jira].

## Setup

### Requirements

The TPM 2.0 simulator build process requires:

* An EL6 or EL7 host with:
  - `rpm`
  - `tar`
  - `curl` (for direct downloads)
  - `rpm-build`
* Ruby 2.1+ with RubyGems.
* [bundler][bundler] 1.14+


To build rpm files to install the TPM 1.2 simulator, install this package and
update the configuration files, namely `things_to_build.yaml` and
and `simp-tpm12-simulator.spec`, as necessary.  Specifically, uncomment the
lines in the `things_to_build.yaml` corresponding to the build platform, EL6 or
EL7. Then create a softlink named `simp-tpm12-simulator` pointing the
corresponding `simp-el6-tpm12-simulator` or `simp-el7-tpm12-simulator` 
directory.

### Building the `simp-tpm12-simulator` RPM
To build the `simp-tpm12-simulator` RPM:

```sh
# Use bundler to install all necessary gems (https://bundler.io)
bundle install

# change to the softlinked project directory:
cd simp-tpm12-simulator/

# Download source + build the RPM
bundle exec rake pkg:rpm

# The RPM will be in the dist/ directory
ls -l dist/*.rpm
```

### Beginning with simp-tpm12-simulator

The TPM 1.2 simulator relies upon a couple rpm packages which should be
installed on any target system intended to use the module. The packages are
gcc, the GNU Compiler Collection, and trousers, and implementation of the 
Trusted Computing Group's Software Stack(TSS) specification.  Additionally
tpm-tools, a group of tools to manage and utilize the Trusted Computing
Group's TPM hardware, is recommended.

## Usage

To install the rpm, copy the rpm file to the target system and install it
with the command:

```yaml
yum localinstall simp-tpm12-simulator-*.rpm
```

This will install the simulator programs, utilities, and servics necessary
to start and utilize the TPM 1.2 simulator.

To initialize and use the TPM simulator on EL7, issue the following commands:

```yaml
# systemctl start tpm12-simulator
# systemctl start tpm12-tpmbios
# systemctl restart tpm12-simulator
# systemctl start tpm12-tpmbios
# systemctl start tpm12-tpminit
# systemctl start tpm12-tcsd
```

To initialize and use the TPM simulator on EL6, issue the following commands:

```yaml
# service tpm12-simulator start
# service tpm12-tpmbios start
# service tpm12-simulator restart
# service tpm12-tpmbios start
# service tpm12-tpminit start
# service tpm12-tcsd start
```

## Development

Please read our [Contribution Guide](http://simp-doc.readthedocs.io/en/stable/contributors_guide/index.html).

[simp]: https://github.com/NationalSecurityAgency/SIMP/
[jira]: https://simp-project.atlassian.net/
[simp-jira]: https://simp-project.atlassian.net/
[ibmswtpm12]: https://sourceforge.net/projects/ibmswtpm/

