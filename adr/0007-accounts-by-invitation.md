# ADR-0007: Accounts are by invitation only

Status: **Accepted**

## Context

Maker-5 is for the people an organisation works with, not the public. The cloud
already refuses self sign-up at the user pool, but it had no way to bring
someone in: adding a member found an existing account by e-mail, and only an
operator could create one, outside the site.

## Decision

- There is **no sign-up**. A person comes in by invitation.
- An organisation's admins invite by adding an e-mail to the organisation or to one
  of its workspaces (Settings). An e-mail without an account gets one
  (Cognito `AdminCreateUser`), and Cognito sends the invitation with a
  temporary password. The person sets their own password at first sign-in.
- The e-mail is marked verified: whoever invites vouches for it, and the
  invitation proves it on first sign-in.
- An invitation that was never used can be **sent again**, by an admin of a
  organisation the person is in, or by an operator.
- Operators see **every account** and can **disable** or enable one. A
  disabled account cannot sign in; its memberships stay. Nobody disables
  their own account.
- An organisation's admins remove people from their own groups only.

## Consequences

- Membership and access are one step: the invitation already places the
  person in the right groups.
- Status is read from Cognito: *invited* (never signed in), *active*,
  *disabled*. Nothing is stored twice.
- Everything here requires two-step sign-in, as all organisation management does.
