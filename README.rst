======================================
OpenStack Security Specifications
======================================

This git repository is used to hold approved design specifications for
additions to the OpenStack Security Projects. Reviews of the specs
are done in gerrit, using a similar workflow to how we review and
merge changes to the security projects and supporting tools.

The layout of this repository is::

  specs/<release>/

You can find an example spec in `doc/source/specs/template.rst`.
Fill it in with the details of a new blueprint for documentation.

For security projects, blueprints are required for larger, coordinated projects
but not for small fixes. It's a judgement call for whether you need a
spec, so feel free to ask in the
#openstack-security IRC channel or on the openstack-security mailing list.

To propose a specification for a release-specific project, add a new file to
the `specs/<release>` directory and post it for review.  The implementation
status of a blueprint for a given release can be found by looking at the
blueprint in Launchpad (and the spec links to Launchpad).

Please realize that not all approved blueprints will get fully implemented.

Security blueprints are being introduced with the Liberty development cycle
using this repository.

Please note, Launchpad blueprints are still used for tracking the
current status of blueprints. For more information, see
https://wiki.openstack.org/wiki/Blueprints.

For more information about working with gerrit, see
http://docs.openstack.org/infra/manual/developers.html#development-workflow.

To validate that the specification is syntactically correct (i.e. get more
confidence in the Jenkins result), please execute the following command::

  $ tox

After running ``tox``, the documentation will be available for viewing in HTML
format in the ``doc/build/`` directory. Please do not check in the generated
HTML files as a part of your commit.

The files are published at http://specs.openstack.org/openstack/security-specs.

The git repository is located at
http://git.openstack.org/cgit/openstack/security-specs/.
