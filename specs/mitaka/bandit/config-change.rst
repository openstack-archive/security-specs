=============================
 Refactored Config for Bandit
=============================

Problem Description
===================
Bandit's config file has grown in complexity and size as more testing plugins
and options have been added to the project. It has been highlighted by adopters
that maintaining a Bandit config file in each projects repository is an
undesirable pain point.

Proposed Change
===============
This proposal aims to refactor the Bandit configuration system with the aim of
removing the need to include any per project configuration files if no major
deviation from preset defaults is required. To accomplish this goal, we first
looked at the existing configuration system. The configuration file breaks down
into three main areas, and each of these will be dealt with individually.
These areas are:

1) Tunable options, used to tweak miscellaneous bits in the Bandit runtime
2) Plugin options, used to control the operation of individual test plugin
3) Profiles, used to select the desired set of test plugins to run or not

Tunable Options
---------------
A number of tunable options are provided in the Bandit config. It was
originally foreseen that these would be useful in configuring Bandit's runtime
behavior. In all practical scenarios however, these options are redundant and
remain at the default setting.

Our proposed strategy is to remove these unnecessary config options and rely on
sensible values provided by the Bandit authors.

Plugin Options
--------------
This section represents the largest block of configuration data and thus the
largest pain point for adopters. It contains a lengthy set of configuration
options for all available test plugins, without these options many plugins will
malfunction or fail to run at all. The usefulness of many plugins is also
directly dependent upon the data provided in this section of the config (e.g.
the blacklist plugins) and as such this data should be considered part of the
plugins functionality.

Our proposed strategy is to modify the plugin system to provide sensible
defaults for all configurable test plugins, directly in the plugin code. This
data can then be overridden in an external configuration only when desirable.
This removes the need for any configuration data when default functioning is
sufficient, and this should be in the majority of cases.

To accomplish this, the test plugin system will be modified to use classes for
each plugin instead of the function that is currently used. Each class will
provide one or more testing methods, equivalent to the current testing
functions, and one method to return a default configuration shared by all
testing methods provided by that class. In the default case, this configuration
data will be sent back to each test method, or it may be overridden by an
external configuration if present.

In addition to removing the need for a configuration file this approach has two
advantages. Firstly, it allows external plugins from third parties to provide
configuration data along with their plugins without the need to edit a central
default config file shipped by the Bandit project. Secondly, it allows for a
config generation tool to auto-discover all available plugins and create a
default configuration file by simply invoking the config method on each
discovered plugin class, or on some desirable subset of these.

Profiles
--------
This section is the primary concern for gate adopters, it is here that they may
select the set of plugin tests that they wish to run on their project. It is
nearly always configured as part of the initial Bandit setup.

Our proposed strategy is to provide a number of new mechanisms to indicate the
desired set of tests to run, deprecating the current situation. Firstly, we
allow profiles to be configured in their own individual files that use a much
simpler layout than than the current single config file. Secondly, we will
permit the specification of test sets directly from the command line interface
of Bandit.

The first option will allow manual users of Bandit to easily create and re-use
various test profiles that they might need to use frequently.

The second option will permit gate adopters to list their desired test set as
part of the Bandit command invocation given within their existing tox.ini file.
This has precedent and matches closely to the way PEP8 tests are currently
configured.

To further improve upon the current state of affairs, again borrowing from
hacking/flake8/pep8, Bandit will support a canonical numbering system for test
plugins. These numeric names may be used anywhere that the usual descriptive,
but often unwieldy, full test names would be used.

An example canonical scheme may look like the following

    B1xx - blacklisted functions
    B2xx - blacklisted imports
    B3xx - injections
    ...

A Note On Blacklists
--------------------
As implied from the example scheme given above, each blacklisted module and
import now will also be assigned a unique identifier. The intention here is to
aid in configuration of these lengthy blacklists. So while blacklisted modules
and methods will share common logic, individual items will be distinguishable
as if they were separate tests when configuring Bandit.

Implementation
==============
See Proposed change, the indicated changes will be made to Bandit to enact the
new approach to configuration. Support for the old configuration file will
remain, but its use will be marked as deprecated and a message indicating this
will be presented to the user when utilizing the text formatted output and an
old style configuration.

Assignee(s)
-----------

The Bandit core team will lead the development of these changes.

Primary assignee:
  tkelsey
  tmcpeak

Milestones
----------

Target Milestone for completion:
  Mitaka 3

Work Items
----------

- Strip out miscellaneous options from Bandit config
- Add a separate text formatter that includes terminal output colors, removing
  the need for these to be configurable.
- Add support for external profile files.
- Convert plugins to classes which implement one or more tests and provide a
  config generation function.
- Assign canonical numbers to each plugin, including blacklist modules and
  imports.
- Add support for test selection via the CLI.
- Update the config generator to output default configs.
- Delete the config file bundled with Bandit.

Dependencies
============
N/A

References
==========
- https://blueprints.launchpad.net/bandit/+spec/config-change
