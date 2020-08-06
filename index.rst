:tocdepth: 1

.. sectnum::

Abstract
========

This is part of a series of technotes evaluating options for identity management for the Rubin Science Platform.
This document sketches a design for using GitHub with minimal additional infrastructure as an interim identity management system while using the Interim Data Facility.
This provides a minimum-effort baseline against which to compare other identity management options.

See `SQR-044`_ for the full list of identity management requirements and `SQR-045`_ for an evaluation of COmanage.

.. _SQR-044: https://sqr-044.lsst.io/
.. _SQR-045: https://sqr-045.lsst.io/

Overall design
==============

Each user of the Rubin Science Platform would have to create a GitHub account.
All authentication would be done via OAuth 2.0 using GitHub as the identity provider.

Access to the Science Platform would be controlled via team membership in a GitHub organization dedicated to that purpose.
Team membership would be manually maintained by Rubin Observatory staff.
Requests for access would happen via email, Slack, or some similar mechanism.
Determination of eligibility for access would be done manually by the approver.

All user metadata such as full name and email address would be retrieved from GitHub via the GitHub API.
Users would use the normal GitHub interface to set that information as they wished.
The user's username in the Science Platform would be the same as their GitHub username.
The user's UID would be taken from the ``id`` attribute provided by GitHub.

All groups for the Science Platform would be teams within the corresponding GitHub organization.
Group (team) membership would be managed manually by Rubin Observatory staff, or could be delegated, in places where that made sense, to specific users.
GIDs for groups within the Science Platform would be taken from the ``id`` attribute of the corresponding GitHub team.

Tokens for API access to Science Platform services would be issued by `Gafaelfawr`_, a service written and maintained by Rubin Observatory.
Some additional implementation work would be required to add the necessary token management features.

.. _Gafaelfawr: https://gafaelfawr.lsst.io/

All users would receive the same quotas.
There would be no per-user or per-group quota support.

Freezing an account would be done by removing it from the GitHub organization.
All team memberships would need to be recreated if the account were later unfrozen.

User impersonation would not be possible, nor would administrators be able to fix email addresses, names, or authentication problems.
Users would be responsible for managing their own account and using GitHub's built-in account recovery procedures if they had any authentication issues.

Gaps
====

This design would not meet most of the identity management requirements given in `SQR-044`_.
It would, however, meet all of the core requirements.

Major drawbacks:

- No quota support, so the only quotas the Science Platform could implement would be generic ones that applied equally to everyone.
- All user management would be manual.
  There would be no automated onboarding flow and no use of federation metadata to determine eligibility.
  There would also be no technical support for account expiration, group membership expiration, or re-evaluation of eligibility.
  All such processes would have to be done manually.
- No support for federated identity.
  Users would have to create GitHub accounts.
- Very little ability for user support around authentication of account metadata.
- Some distant possibility that GitHub would be unhappy with us using their service in this way.
