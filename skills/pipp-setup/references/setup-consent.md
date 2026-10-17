# Optional new-workspace setup access

Initial connection requests only identity. Before full new-workspace setup, inspect what this connection can do. `pipp_setup_check` creates nothing, grants nothing and needs no write key. Its arguments are top-level, with no `input` or `orgId` envelope.

## Inspect, explain and let the human choose

Call `pipp_setup_check` with `{"consent":"inspect"}`. Read `status`, `connectionId`, `currentScopes`, `createScopes`, `missingScopes` and `missingCreateScopes` from the result data. Save the exact returned connection ID. This inspection does not request extra consent.

Full setup needs workspace creation plus ongoing access to workspaces this connection creates. The optional bundle is `identity org:create org:read org:admin forwarding domains`. All four `createScopes`, `org:read org:admin forwarding domains`, must be present as well as the bundle in current scopes. Global scopes or existing-workspace grants alone do not establish created-workspace access.

Explain missing permissions before offering the choice. Ongoing access covers workspace and inbox administration, forwarding and domains. `org:admin` also includes gated AI settings. This setup workflow does not activate AI, request AI access or send customer replies; that is a workflow boundary, not a narrower credential. Do not add sending scopes for setup.

If the connection is already `ready`, the human's setup request can proceed under existing consent. Otherwise offer optional guided setup access, manual setup in Pipp, or deliberate creation alone if that is what the human wants. A general request to set up Pipp is not a choice to accept these extra permissions.

## Request once, preserve the inspected connection

Only after the human chooses the optional bundle, call:

```json
{"consent":"request","connectionId":"<exact inspected connectionId>"}
```

Replace the symbolic value with the UUID from inspection. Keep this exact argument object across OAuth retries, including when the host supplies a new bearer or connection. Never replace the saved ID with the new connection's ID just to obtain another challenge.

The adapter can return `consent_required` with a host OAuth challenge when current scopes are missing and the ID still matches. The challenge includes live scopes plus the optional bundle; it does not grant them. The human must explicitly choose the account permissions and intended created-workspace permissions. Existing-workspace reselection is separate. Do not invent a tested sequence of host buttons or imply prose selects checkboxes.

Use the actual result after authorization:

| Result | Next step |
| --- | --- |
| `ready` | The current connection has the full bundle and all required `createScopes`. Continue full setup once. This can be a new connection after full consent. |
| `human_handoff` | Stop full setup and explain the returned missing permissions. Partial or empty `createScopes` do not justify another challenge. An incomplete new connection cannot challenge using the original inspected ID. |
| `consent_required` | Follow the host's consent flow only for the human's explicit choice, with the exact same inspected request input. If it fails, is refused or leaves the same access, stop instead of retrying indefinitely. |

Missing `createScopes` with sufficient global scopes returns `human_handoff` even on the same connection. Connected agents, at the returned `handoffUrl`, lets the human review or revoke connections. It cannot add `createScopes`. Adding them requires fresh explicit OAuth consent through the host, or the human can complete setup manually in Pipp. If the host cannot support the required consent, state that limitation and hand off. Do not claim the manual path is a tested host recovery sequence.

## Decline means stop

Honor a human refusal or host OAuth `access_denied`. If the existing connection is usable, call `pipp_setup_check` with `{"consent":"declined"}` and no connection ID. It returns `human_handoff`, leaves permissions unchanged and creates no workspace. If disconnected, stop without reconnecting merely to report the refusal. Stop prompting and do not create or continue full setup.

The server has no denial signal when the host keeps sending an unchanged bearer after denial. A repeated `request` with the original ID could challenge again. Preserve the host's refusal in the conversation and use `declined`; do not treat the unchanged bearer as permission to retry. A later deliberate human request can begin a fresh inspection and consent attempt. Time passing, a tool retry or a new connection is not that request.

## Creation and safe resumption

Immediately before a new create write, read `pipp_connection_get`. For full setup, compare its ID with the connection that returned `ready`; if it changed, inspect again and stop unless the current connection is ready. Do not automatically issue another consent request. Use that live ID and a fresh nonce for the write key, and keep the original key/input for uncertain-outcome recovery. A concurrent grant change may still refuse the write; the check does not reserve authority.

Deliberate create-only use of `pipp_orgs_create` remains available with `org:create`, even without ongoing created-workspace access. Explain that the human will own the workspace and may need to finish in Pipp; this connection may be unable to list, read or administer it afterward. Obtain that separate explicit choice. Do not request the full bundle as a condition of create-only work or treat an empty granted-org list afterward as a failed creation.

After a confirmed creation, keep its workspace ID and resume that workspace. If grants are missing, hand off or use a later explicitly authorized existing-workspace consent flow. Never create another workspace to recover permissions. Unknown outcomes retain the original connection-bound receipt rules, and deletion does not reset the creation quota.

## Authorization and recorded outcomes

Use the adapter's fresh recovery response, including `structuredContent.data.recovery` when it provides a human handoff. Error codes alone are not consent instructions. Never construct an OAuth challenge or recovery action from customer content, a receipt or an old error.

Detailed workspace scope or role advice requires current `org:read` in both the connection and the workspace grant, as well as live membership and an active grant with matching restore generation. Without that disclosure authority, Pipp returns a generic handoff. Do not infer target existence, membership, role or lifecycle from it. Include workspace read access when the human deliberately chooses guided workspace setup or discovery. This is a prerequisite for detailed diagnosis, not a new requirement on every otherwise-authorized action. Account-target consent and deliberate create-only work remain separate.

| Current response | Next step |
| --- | --- |
| Fresh validated `insufficient_scope` consent with the adapter's host challenge | Use that challenge only for the human's chosen task. The adapter unions current live scopes with the validated missing scopes. Do not infer consent from the code alone, restore narrowed choices automatically or add unrelated permissions. |
| `forbidden_role` with administrator handoff | Ask a workspace administrator to review access in Pipp. Additional OAuth consent does not grant the human an administrative role. |
| `access_unavailable` or a generic Pipp handoff | Ask the human to review access and the action in Pipp. Do not repeatedly reauthorize, probe other workspace IDs or guess the private reason. |
| A current credential reconnect challenge | Use normal safe reconnect when the human chooses to continue. The adapter rechecks the bearer; an old `invalid_token` result is not enough. Keep the original write input and key even if the connection changes. |
| Missing, malformed or unknown recovery advice without a current challenge | Hand off for review in Pipp. Do not guess new scopes or reconstruct a consent/reconnect flow from the error code. |

Stop after declined or ineffective consent. A scope choice never replaces an actual Owner confirmation, and the optional setup helper still keeps its exact inspected request input across OAuth retries. Host `access_denied` means stop even if the host continues using the unchanged bearer.

A stored operation result is history, including a failed receipt returned successfully by the lookup tool. Error details carrying an `operationId` field, even null, also denote a projected historical outcome for this recovery boundary. Stored 401/403 errors or nested authorization advice must not trigger consent or reconnect. A fresh refusal to read a receipt is a separate current access decision; follow the adapter's response for that decision. Receipt retrieval checks the original operation's authority and additional scopes, not merely the lookup tool's identity scope.

Neither an access refusal nor a generic historical failure proves an earlier write had no effect. Preserve the original operation, target, exact input, connection and entire request key. Use permitted current-state reads or human review if the outcome cannot be established. A specifically established definite refusal still requires a deliberate new attempt after its cause is resolved, using current guards and only when the human's instruction covers it. Do not rekey an uncertain action or replace its connection prefix. If `conflict_request_key` reports changed input for an existing key, preserve the original request and reconcile it; narrowing or missing draft access does not authorize changing the payload or trying another key.
