# okta-multi-app-provisioning-patterns

Sanitized provisioning and SSO integration patterns built across a real multi-app Okta deployment: NetSuite, monday.com, Adobe Creative Cloud, Kustomer, and Magnific.

## Problem

Every SaaS app has its own provisioning model: some support full SCIM, some only SAML with manual group mapping, some need a workflow-based connector. Documenting the pattern for each kind makes it reusable the next time a new app needs the same treatment, instead of re-deriving the approach from scratch.

## Approach

Each app below is documented as its own pattern: what provisioning method it supports, how access is modeled (group-based vs. role-based), and how the lifecycle (grant, update, revoke) is automated end to end.

## Patterns Included

### NetSuite — Token-Based Provisioning via Group Assignment
Access and roles are driven through Okta group assignments rather than individual grants (`giveAccess` set to true on the group push). Documents the token-based authentication setup and the rollback/retry approach used when provisioning pushes fail.

### monday.com — SCIM Role Sync
Full SCIM provisioning where the monday.com user role (admin, member, viewer, guest) is set directly from Okta group membership. One role group per role level, each mapped to the matching monday.com role, with new rollouts tested one user per group before wider deactivation changes.

### Adobe Creative Cloud — Okta Workflows Provisioning, Live for OIG Access Requests
Provisioning handled through an Okta Workflows connector (Adobe User Management) rather than Adobe's own User Sync Tool, chosen for tighter integration with the existing Okta-driven lifecycle. Built, tested, and now live in production: the workflow is wired directly into Okta Identity Governance, so a user requesting Adobe access through an OIG access request triggers the Workflows connector to provision the license and role automatically on approval, with no manual grant step. Offboarding removes the deactivated user from the Adobe org entirely and posts an automated reminder for the manager to claim the departing user's assets.

### Kustomer & Magnific — SAML SSO with Custom Username Mapping
SAML SSO setups covering both production and sandbox environments, including a custom Okta username expression to map between two internal email domains so SSO resolves correctly regardless of which domain a user's account originated under.

## Tech Stack

- Okta Workflows
- SCIM 2.0
- SAML 2.0
- Okta group-push and group-rule based access modeling

## Notes

All tenant IDs, group names, org names, and connector credentials have been replaced with placeholders. No real company or vendor account data is included.
