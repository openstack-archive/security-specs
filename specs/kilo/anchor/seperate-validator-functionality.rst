..

This work is licensed under a Creative Commons Attribution 3.0 Unported
License.
http://creativecommons.org/licenses/by/3.0/legalcode

..

==========================================================
 Separate Anchor validation functionality to enable reuse
==========================================================

https://blueprints.launchpad.net/anchor/+spec/anchor-separate-package

Problem description
===================

Currently the Anchor CSR validation functionality is all located in the
Anchor package. This prevents the validation functionality from being
easily imported and used by other projects. Therefore, the validation
functionality should be split out into a module under anchor, so it can
be imported by other projects. This will provide a simple API to the
validation functionality, that can be used by both Anchor and other
projects.

This will enable re-use of the CSR validation functionality in other
projects, such as the proposed CA/RA Killick, or in Barbican.

Secondly, the code that performs validation of a CSR aborts when one of
the validators fails - this works ok for Anchor, but the validation
engine can be extended to provide added value for other services by
running all validators and returning a dictionary, giving the result
of each validator that was run.

Proposed change
===============

Break out the code that iterates over the validators from
certificate_ops.py into a new file 'validation.py'. Extend this to
return a dictionary giving the validator name and its return value, or
throw an exception if an error occured.

Modify certificate_ops.py to work with the new validation.py and
dictionary return, while catching any exceptions thrown.

Fix the tests to work with this new functionality.


Implementation
==============

doug-chivers

Milestones
----------

Target Milestone for completion:
  Kilo

Work Items
----------

1 - Break out validation functionality into new file
2 - Return the dictionary
3 - Fix testing to work with this, make sure that exceptions thrown by
validators are still caught by certificate_ops.py


Dependencies
============

Anchor

References
==========

N/A
