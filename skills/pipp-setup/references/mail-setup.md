# Receiving and custom-domain sending

## Receiving

Read `forwarding.get` for the chosen inbox. Copy its current forwarding destination and instructions; do not invent an address. Change mailbox forwarding only through a separately authorized provider tool for this human and task. Otherwise provide the exact provider step and Pipp destination without soliciting provider credentials.

For a pending Gmail confirmation, show the returned requester, receipt time, mismatch flag and Pipp forwarding-screen link. Highlight an account mismatch. The human must open Pipp with their normal session and perform the protected confirmation click. Never request or extract Gmail's confirmation code/URL, including through generic case reads or downloads, and never claim an agent has performed the Admin click. Re-read status afterward.

When a receiving test is wanted and authorized, `forwarding.probe` sends only to the inbox's configured support address, with a maximum of five per hour. It is not an arbitrary-recipient test endpoint. Inspect the operation receipt and `forwarding.get` probe result. Queued does not prove received. Do not rotate/stop a working route to diagnose a pending test without an instruction covering that change.

## Domain records and authority

Read `domains.list` before `domains.add`. One connection that is not fully `disconnected` occupies the workspace slot, including a draft, failed attempt or cleanup. Reuse it for the intended domain; replacing a domain requires authorized disconnect with the exact typed domain and completed cleanup first. Do not disconnect it merely to make room. Read `domains.get` using `input:{id:connectionId}`.

`domains.add` returns a stable connection ID and complete DNS records immediately. It starts no DNS checks or provisioning. Show each actual record's type, complete hostname, exact value and whether required, recommended or optional. Copy values from the response, never examples. Explain the provider's relative-hostname convention when necessary without duplicating the zone suffix.

Required records include connection-specific DKIM, SPF and valid direct or inherited DMARC. Rotation/return-path records are recommended; tracking is optional. Initial instructions have not inspected DNS and cannot establish whether existing records conflict.

Before any DNS mutation, use separately authorized provider tools to read existing SPF and direct/inherited DMARC and inspect collisions at all proposed record names. Preserve other mail senders, existing valid DMARC and unrelated records. Merge the Pipp requirement into one valid SPF policy; never add a second SPF TXT record. If SPF is complex or already invalid, stop at the specific conflict for human/provider resolution. Never change MX or mailbox hosting. Without a reliable DNS read, hand off exact records and the preservation rules rather than guess or write.

If no valid DMARC exists, explain that a new policy affects all mail from that domain, including other senders. Obtain the human's explicit agreement to the proposed domain-wide policy before adding it, including `p=quarantine`. Consent to connect Pipp alone is not agreement to that policy. Preserve a valid inherited policy instead of needlessly replacing it.

Tracking stays off unless the human opts in. Include its exact record before verification when requested. Even with opt-in, enabling tracking requires its record to be verified.

## Confirmation and lifecycle

Before `domains.confirm`, establish that the records have actually been applied through confirmed provider results or the human's explicit statement that they applied them. An elapsed wait, proposed patch, DNS instructions or "keep going" without that confirmation is insufficient. Ask for the missing confirmation while preserving the existing draft. Pass the returned `connectionId`; each write needs its own durable request key.

Confirmation authorizes the single 24-hour attempt and subsequent activation once proof passes. Read saved `domains.get` status and return its observed records/conflicts, `nextCheckAt`, `deadlineAt` and blockers. Status reads perform no checks. Repeating confirmation does not extend the deadline. Do not poll in a tight loop or invent a scheduled watcher outside the host's capabilities.

States are `draft`, `pending`, `provisioning`, `ready`, `attention`, `failed`, `disconnecting` and `disconnected`. Keep those distinctions. Preserve the normal outcome email to the confirming human Admin/Owner. Restart a failed attempt only after actual DNS changes or an explicit human request; never automatically restart to evade the deadline. Restores invalidate old proof and need fresh authorized confirmation.

A `ready` domain still needs `domains.sender.set` for the intended inbox and sending address. Several inboxes can use different addresses on this one domain. Read sender readiness afterward. Clearing or disconnecting the sender blocks replies. Saved readiness is not a live DNS observation; sending rechecks readiness at dispatch.

If tracking is requested later and its record was not verified, explain that a ready connection cannot start another verification attempt. Do not silently disconnect/reconnect or turn on operator monitoring. Report the specific limitation.

No standalone outbound test-send operation exists. An actual test reply needs an existing permitted conversation and normal send authorization, using the case-work skill. Pipp counts up to 100 actual To+Cc recipient deliveries per workspace per UTC day across connections, not 100 messages or unlimited recipients.
