# Reference
## Auth
<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_jwks</a>() -> JwksResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the platform's JWT signing public key in RFC 7517 JWKS format. Use it with any JWKS-aware JWT library to verify `inv_*` invite tokens, `st_*` scoped tokens, and other platform-signed tokens locally, without calling back to the API for each verification. The response carries a one-hour `Cache-Control`, so cache it and re-fetch roughly hourly rather than on every verification. The `kid` value changes when the key rotates; re-fetch this document whenever you encounter a token signed with an unknown `kid`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_jwks()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_access_log</a>(...) -> ReadAccessLogPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a page of per-subject PHI read-access rows: who read which subject's PHI, when, against which record, and whether any sensitive value was actually revealed in plaintext. Metadata only — never the PHI itself. This is the disclosure-accounting surface from which a covered entity derives its HIPAA §164.528 accounting of disclosures. Provide at least one query axis: a subject (`subjectType` + `subjectId`) within a `contextId` for the primary accounting query; `resourceId` within a `contextId` for 'who read this record'; `callerKeyId` for 'what did this credential read' (account-wide forensic); or `contextId` alone to enumerate a whole context. `from`/`to` bound the time window. Results are scoped to your account, derived from your token — never from input. Requires the `access-log:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_access_log(
    subject_type="user",
    subject_id="user_abc123",
    context_id="ctx_intake",
    action="read",
    caller_key_id="key_abc123",
    resource_type="intake_form",
    resource_id="rec_456",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**subject_type:** `typing.Optional[str]` — Kind of subject to account for: `user`, or any ownership namespace — the built-in `org` and `client`, or one you registered such as `team`. A subject kind is your data, not a fixed list.
    
</dd>
</dl>

<dl>
<dd>

**subject_id:** `typing.Optional[str]` — Identifier of the data subject whose read history to return — the primary accounting-of-disclosures axis. Must be supplied together with `subjectType` (and a `contextId`).
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Restrict results to a single app context (the data-partition axis). Required for a subject or record query.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Start of the time window (ISO-8601 UTC, inclusive).
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — End of the time window (ISO-8601 UTC, exclusive; defaults to now).
    
</dd>
</dl>

<dl>
<dd>

**action:** `typing.Optional[str]` — Restrict to a single action: `read`, `list`, `lookup`, `search`, or `rag`.
    
</dd>
</dl>

<dl>
<dd>

**caller_key_id:** `typing.Optional[str]` — Forensic axis — restrict to reads performed by a single credential (the API key / scoped token key id that made the call).
    
</dd>
</dl>

<dl>
<dd>

**resource_type:** `typing.Optional[str]` — Restrict to a single accessed resource type: a record schema type name, or one of `document`, `search`, or `rag`.
    
</dd>
</dl>

<dl>
<dd>

**resource_id:** `typing.Optional[str]` — Forensic axis — restrict to reads of a single accessed record/document id.
    
</dd>
</dl>

<dl>
<dd>

**revealed_sensitive:** `typing.Optional[bool]` — If true, return only reads that actually revealed a sensitive value in plaintext.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Opaque cursor from a previous page's `nextCursor`; pass it back to fetch the next page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of rows to return per page (1–500; defaults to 200).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">list_scoped_keys</a>() -> ScopedKeyPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists your scoped API keys (`ssk_*`) in your credential's own environment — a live key lists live keys, a test key lists test keys. Revoked keys are excluded. Requires the `keys:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.list_scoped_keys()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">create_scoped_key</a>(...) -> ScopedKeyResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a scoped API key (an `ssk_*` secret) that inherits its permissions from an existing access profile in your account. The call is idempotent on the combination of tenant, context, user, and key name: re-issuing the same request returns the existing key WITHOUT re-disclosing its raw secret. The raw key is returned ONLY in this response — store it securely, as it cannot be retrieved again. Requires the `keys:c` scope. If you use a scoped credential, `keys:c` alone is not sufficient: because the minted key is durably bound to the profile you name, the profile's effective scopes may not exceed your own, and you may only mint against a profile whose `identityOverrides` values your own identity holds. Minting a key bound to your OWN principal needs nothing further; minting one bound to a DIFFERENT principal additionally requires the `delegate-mint` capability (`granted_capabilities`) on your credential — without it the request is refused. A root API key (`sk_`) is exempt from all three bounds.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.create_scoped_key(
    key_name="research-bot production",
    tenant_id="ten_live_xxx",
    context_id="myapp",
    user_id="alice-001",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**key_name:** `str` — A human-readable name for the key, used to identify and disambiguate multiple keys for the same user. Maximum 100 characters.
    
</dd>
</dl>

<dl>
<dd>

**tenant_id:** `str` — The tenant the key will operate on. Must be your live tenant ID or your test tenant ID; an ID outside your account returns a uniform `404 not found` (no distinction is made between a missing and an out-of-account tenant).
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `str` — The app context within the tenant. Must reference an app context that already exists (create one via `POST /v1/app-contexts`).
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `str` — The user the key binds to. May be a `HUMAN` or a `SERVICE` user (a service user is the typical agent or bot case). An access profile must already exist for this context and user — the key references that profile; it does not create one. Naming your own principal needs nothing further; naming any OTHER principal additionally requires the `delegate-mint` capability on your credential, or a root API key.
    
</dd>
</dl>

<dl>
<dd>

**label:** `typing.Optional[str]` — Optional display label surfaced through `/v1/ping` as `principalLabel`. The MCP `current_identity` tool uses it to show "signed in as ..." hints to end users (e.g. "Claude Desktop — clinical-notes RO"). Distinct from `keyName`, which is your own identifier for the key. Maximum 80 characters; if present, it must not have leading or trailing whitespace.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_scoped_key</a>(...) -> ScopedKeyResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the metadata for a single scoped API key. The raw secret is NOT included — it is only ever returned once, when the key is first created. Requires the `keys:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_scoped_key(
    key_id="keyId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**key_id:** `str` — The key's `keyId`, as returned when the key was created.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">revoke_scoped_key</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Revokes a scoped API key. Its status changes to `revoked` and it stops working within about 5 minutes, the maximum time authorization is cached. Revocation is permanent. Requires the `keys:d` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.revoke_scoped_key(
    key_id="keyId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**key_id:** `str` — The key's `keyId`, as returned when the key was created.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_admin_logs</a>(...) -> AdminLogsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns recent API call logs for your account. Each entry represents one API request; request and response bodies are never logged. `startTime` and `endTime` must be ISO-8601 UTC (e.g. `2025-01-15T09:00:00Z`); `endTime` defaults to now. Filter by resource, method, key id, or context id, or set `errorsOnly` to see only failures. Results are scoped to your account, derived from your token — never from input. Requires the `logs:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_admin_logs(
    start_time="startTime",
    resource="documents",
    method="POST",
    key_id="key_abc123",
    context_id="ctx_intake",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**start_time:** `str` — Start of the time window (ISO-8601 UTC).
    
</dd>
</dl>

<dl>
<dd>

**end_time:** `typing.Optional[str]` — End of the time window (ISO-8601 UTC; defaults to now).
    
</dd>
</dl>

<dl>
<dd>

**resource:** `typing.Optional[str]` — Filter by resource type. One of `documents`, `records`, `search`, `schemas`, `folders`, `entities`, `namespaces`, `users`, `usage`, `auth`, `models`, `ping`, `issuers`, `rag`, `chat`, `ask`, `erasure-requests`, or `export`. `clients` and `orgs` are not accepted — `/v1/orgs` and `/v1/clients` were retired onto `/v1/entities/{namespace}` and no log row was ever written under those resource names.
    
</dd>
</dl>

<dl>
<dd>

**method:** `typing.Optional[str]` — Filter by HTTP method (`GET`, `POST`, `PUT`, or `DELETE`).
    
</dd>
</dl>

<dl>
<dd>

**key_id:** `typing.Optional[str]` — Filter by API key id to inspect a specific credential's traffic.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Filter by app context id to inspect a specific context's traffic.
    
</dd>
</dl>

<dl>
<dd>

**errors_only:** `typing.Optional[bool]` — If true, return only requests with a status of 400 or higher.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of log entries to return per page (1–500; defaults to 200).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">list_access_profiles</a>(...) -> AccessProfilePage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the access profiles assigned within the given app context — in effect, who has access to this context and with what scopes. Each profile binds a principal to either a set of inline scopes or a referenced role. Results are paginated. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.list_access_profiles(
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` — Your identifier for the app context.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value returned by the previous page to fetch the next page; omit it to start from the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of results to return per page. Must be between 1 and 100; defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">create_access_profile</a>(...) -> AccessProfileResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new access profile under the given app context. This call is idempotent by `principalId`: if a profile with the same `principalId` already exists, the existing profile is returned (with status 200) instead of creating a duplicate. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing profile was returned) tells the two apart. To overwrite an existing profile's `scopes`/`roleId`, `identityOverrides`, and `status` instead of returning it unchanged, set `?upsert=true` (this also requires the `profiles:u` scope, and applies the same `identityOverrides` bounds the update endpoint documents — a scoped credential may not repoint or clear an identity value it does not itself hold). The `principalId` must name a principal that already exists: a `usr_` principal must be a live user in your tenant, so create the user before granting it a profile. A `usr_` id that names no such user is rejected, and no profile is created. `key_` principals are not checked this way. You must provide exactly one of `scopes` (an inline list of scopes) or `roleId` (a reference to a role); supplying both, or neither, is rejected. `identityOverrides` is keyed by ownership namespace in `scope:<namespace>` form — `scope:org` and `scope:client` for the reserved namespaces, or any namespace you have registered — and may name at most two; any other key (including the account identifier or `userId`) is rejected. If you use a scoped credential, the profile's effective scopes may not exceed your own; a root API key (`sk_`) is exempt. Requires the `profiles:c` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.create_access_profile(
    context_id="contextId",
    principal_id="usr_6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `AccessProfileRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a profile with the same `principalId` already exists its grant source (`scopes` or `roleId`), `identityOverrides`, and `status` are updated to the submitted values instead of being returned unchanged. Defaults to `false`. Requires the `profiles:u` scope in addition to `profiles:c`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">list_app_contexts</a>(...) -> AppContextPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of the app contexts in your account. Each app context is a namespace that groups the access profiles and roles for one of your applications. Requires the `app-contexts:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.list_app_contexts()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value returned by the previous page to fetch the next page; omit it to start from the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of results to return per page. Must be between 1 and 100; defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">create_app_context</a>(...) -> AppContextResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new app context. This call is idempotent by `contextId`: if an app context with the same `contextId` already exists, the existing app context is returned (with status 200) instead of creating a duplicate. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing context was returned) tells the two apart. To overwrite an existing context's `name`/`description` instead of returning it unchanged, set `?upsert=true` (this also requires the `app-contexts:u` scope). The reserved `contextId` value `vectros-admin` cannot be created through this endpoint; it is provisioned automatically for your account. Requires the `app-contexts:c` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.create_app_context(
    context_id="myapp",
    name="My Internal App",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `AppContextRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if an app context with the same `contextId` already exists its `name` and `description` are updated to the submitted values instead of being returned unchanged. Defaults to `false`. Requires the `app-contexts:u` scope in addition to `app-contexts:c`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">list_roles</a>(...) -> RolePage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the roles defined under the given app context. A role is a reusable, named bundle of scopes that access profiles can reference instead of listing scopes inline. Results are paginated. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.list_roles(
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` — Your identifier for the app context.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value returned by the previous page to fetch the next page; omit it to start from the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of results to return per page. Must be between 1 and 100; defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">create_role</a>(...) -> RoleResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new role under the given app context. This call is idempotent by `roleId`: if a role with the same `roleId` already exists, the existing role is returned (with status 200) instead of creating a duplicate. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing role was returned) tells the two apart. To overwrite an existing role's `name`/`description`/`scopes` instead of returning it unchanged, set `?upsert=true` (this also requires the `profiles:u` scope). If you use a scoped credential, the role's scopes may not exceed your own; a root API key (`sk_`) is exempt. Requires the `profiles:c` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, ScopeClause

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.create_role(
    context_id="contextId",
    role_id="engineering-member",
    name="Engineering Team Member",
    scopes=[
        ScopeClause(
            allowed_actions=[
                "records:cru",
                "search:r"
            ],
        )
    ],
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `RoleRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a role with the same `roleId` already exists its `name`, `description`, and `scopes` are updated to the submitted values instead of being returned unchanged. Defaults to `false`. Requires the `profiles:u` scope in addition to `profiles:c`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_access_profile</a>(...) -> AccessProfileResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single access profile by its `principalId` within the given app context. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_access_profile(
    context_id="contextId",
    principal_id="principalId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**principal_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">update_access_profile</a>(...) -> AccessProfileResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates an access profile. This is a partial update: any field you omit (or send as null) keeps its existing value. A profile must reference either inline `scopes` or a `roleId`, never both — so setting `scopes` clears any `roleId`, and setting `roleId` clears any inline `scopes`. The `contextId` and `principalId` are immutable. Status changes (for example active to suspended) take effect within about five minutes. If you use a scoped credential, the profile's effective scopes may not exceed your own, and its `identityOverrides` are bounded twice: you may only set a value your own identity holds, and you may only change or clear a value the profile already holds if that value is yours as well. Repointing or clearing another principal's established identity therefore returns 403. A root API key (`sk_`) is exempt. If you set `roleId`, it must reference a role that already exists in this context. Requires the `profiles:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.update_access_profile(
    context_id="contextId",
    principal_id_="principalId",
    principal_id="usr_6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**principal_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `AccessProfileRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">delete_access_profile</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes an access profile. Within about five minutes (the access-profile cache lifetime), token minting for this principal in this context will be denied. If you use a scoped credential and the profile carries `identityOverrides`, you may only delete it when you hold those values yourself — deleting a profile removes its identity, so the same bound applies as when clearing it. A profile with no `identityOverrides` is unaffected, and a root API key (`sk_`) is exempt. Requires the `profiles:d` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.delete_access_profile(
    context_id="contextId",
    principal_id="principalId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**principal_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_app_context</a>(...) -> AppContextResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single app context by its `contextId`. Requires the `app-contexts:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_app_context(
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` — Your identifier for the app context.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">update_app_context</a>(...) -> AppContextResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates the name and/or description of an app context. This is a partial update: any field you omit (or send as null) keeps its existing value. The `contextId` is immutable and is taken from the URL path, so any `contextId` in the request body is ignored. Requires the `app-contexts:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.update_app_context(
    context_id_="contextId",
    context_id="myapp",
    name="My Internal App",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `AppContextRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">delete_app_context</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes an app context and everything in it — every record, document, folder, schema, role, and access profile belonging to the context. This is irreversible. The deletion runs asynchronously: the call returns 202 immediately and the context's data drains in the background. Poll the context's `status` field to observe when the teardown completes (`purging` while draining, then `deleted`). To guard against accidental deletion, you must echo the contextId back in the `confirm` query parameter (`?confirm={contextId}`). The reserved `default` and `vectros-admin` contexts cannot be deleted. This operation requires a root API key (one beginning with `sk_`): no scoped credential, not even one with full wildcard (`*`) scope, can trigger this teardown.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.delete_app_context(
    context_id="contextId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**confirm:** `typing.Optional[str]` — Confirmation token. Must exactly equal the contextId being deleted; the request is rejected with 400 otherwise. This guards against accidental, irreversible deletion.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_role</a>(...) -> RoleResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single role by its `roleId` within the given app context. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_role(
    context_id="contextId",
    role_id="roleId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**role_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">update_role</a>(...) -> RoleResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates a role. This is a partial update: any field you omit (or send as null) keeps its existing value. The `roleId` and `contextId` are immutable. Scope changes take effect for access profiles that reference this role within about five minutes. If you use a scoped credential, the role's scopes may not exceed your own; a root API key (`sk_`) is exempt. Requires the `profiles:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, ScopeClause

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.update_role(
    context_id="contextId",
    role_id_="roleId",
    role_id="engineering-member",
    name="Engineering Team Member",
    scopes=[
        ScopeClause(
            allowed_actions=[
                "records:cru",
                "search:r"
            ],
        )
    ],
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**role_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `RoleRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">delete_role</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a role. A role that is still referenced by one or more access profiles cannot be deleted: the request is rejected with 409. Reassign or delete those profiles first, then retry. Requires the `profiles:d` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.delete_role(
    context_id="contextId",
    role_id="roleId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**role_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_access_profile_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes (create, update, and delete events) for an access profile, most recent first. Version history is always recorded for every access profile; there is no setting to turn it off. Results are paginated. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_access_profile_versions(
    context_id="contextId",
    principal_id="principalId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**principal_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value returned by the previous page to fetch the next page; omit it to start from the most recent versions.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_role_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes (create, update, and delete events) for a role, newest first. Version history is always recorded for every role; there is no setting to turn it off. Results are paginated. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_role_versions(
    context_id="myapp",
    role_id="roleId",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**context_id:** `str` — Your identifier for the app context.
    
</dd>
</dl>

<dl>
<dd>

**role_id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value returned by the previous page to fetch the next page; omit it to start from the most recent versions.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_usage</a>(...) -> UsageReportResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns full usage detail for the requested calendar month, broken down by category (search, documents, and records) with per-category credit estimates and a split between your live and test environments. Defaults to the current month when `year` and `month` are omitted. Requires the `billing:r` scope on scoped tokens; API keys always have access. **A token confined to a single app context sees only that context's usage**: totals, the environment split, and the `contexts` breakdown narrow to it, and the environment your context is not bound to is omitted (`null`), not zeroed. Only a token with cross-context reach sees your full account-wide totals. Two exceptions to the narrowing, since they have no per-context breakdown to narrow to: `reads.calls.used`/`reads.dataOut.bytes` (metered per account, not per context) read as `0` for a confined token rather than a narrowed figure — the corresponding overage-credit charge fields narrow correctly; and `credits.limit` stays your whole plan's ceiling, so `credits.remaining` may overstate the account's true remaining room.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_usage(
    context_id="default",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**year:** `typing.Optional[int]` — Calendar year (for example, 2026). Defaults to the current year.
    
</dd>
</dl>

<dl>
<dd>

**month:** `typing.Optional[int]` — Calendar month, 1-12 (for example, 5 for May). Defaults to the current month.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — App context id. For a token with cross-context reach, this only restricts the `contexts` breakdown to that single app context — your account-wide totals are unaffected. For a token confined to one app context, your totals are *already* narrowed to it (see the operation description) regardless of this parameter; supplying it must name your own context or the request is refused.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">get_issuer</a>(...) -> IssuerResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single registered issuer by issuerId. Requires a root API key or the bootstrap's provisioning capability. A credential confined to one app context sees only an issuer registered in that context; naming one registered in another context returns 404, identically to a nonexistent issuerId. A root API key sees every context.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.get_issuer(
    issuer_id="auth0-prod",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**issuer_id:** `str` — The issuer's slug.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">delete_issuer</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deregisters a trusted third-party IdP issuer. Requires a root API key or the bootstrap's provisioning capability. A credential confined to one app context may only deregister an issuer registered in that context; naming one registered in another context returns 404, identically to a nonexistent issuerId. A root API key may deregister any issuer. Refused if any user account was ever created or matched via this issuer (by a prior self-signup or accepted invite, through `POST /v1/auth/token/exchange`) — that access cannot be silently orphaned. Deactivate the affected users first if you intend to cut off their access, or register a replacement issuer before removing this one. An issuer that has never been used for an exchange (no bound users yet) can always be deregistered.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.delete_issuer(
    issuer_id="auth0-prod",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**issuer_id:** `str` — The issuer's slug.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">list_issuers</a>(...) -> IssuerPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the issuers registered in your tenant. Requires a root API key or the bootstrap's provisioning capability. A credential confined to one app context sees only the issuers registered in that context; a root API key sees every context. Returns a `{data, nextCursor}` envelope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.list_issuers()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor from a previous page's `nextCursor`.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum registrations per page (1-100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">register_issuer</a>(...) -> IssuerResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Registers a trusted third-party IdP issuer that BYO-IdP token exchange (`POST /v1/auth/token/exchange`) may accept a `subject_token` from. Requires a root API key or the CLI bootstrap's provisioning capability — never an ordinary partner-grantable scope. A credential authorized only via the provisioning capability may register only against the app context it is bound to; naming a different one returns 403. A root API key is unaffected and may register against any of its contexts. Idempotent by `issuerId` within your tenant; the `(issuer, audience)` pair must not already be registered by a different issuerId/tenant. If `issuerId` collides with a registration owned by a different app context than the one you're confined to, the request fails with 400 rather than returning that context's configuration. An app context may have at most one active issuer — deregister the existing one first if you need to replace it. One issuer MAY serve several contexts today, each via its own registration row with a distinct `audience`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.register_issuer(
    issuer_id="auth0-prod",
    issuer="https://your-tenant.us.auth0.com/",
    jwks_uri="https://your-tenant.us.auth0.com/.well-known/jwks.json",
    audience="https://api.your-app.example.com",
    context_id="casework",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**issuer_id:** `str` — Short slug identifying this issuer within your tenant: 3-31 characters, a lowercase letter first, then lowercase letters, digits, or hyphens. Immutable once registered.
    
</dd>
</dl>

<dl>
<dd>

**issuer:** `str` — The IdP's `iss` claim value, exactly as it appears in tokens it issues.
    
</dd>
</dl>

<dl>
<dd>

**jwks_uri:** `str` — The IdP's remote JWKS endpoint, used to verify presented tokens' signatures.
    
</dd>
</dl>

<dl>
<dd>

**audience:** `str` — The `aud` claim value this contract requires a presented subject_token to carry. Must be globally unique in combination with `issuer` — use a distinct audience per environment/context sharing one IdP account (most OIDC providers support this as an ordinary per-API/application default).
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `str` — Which of your app contexts an exchanged token targets. Must be an existing app context (create it first via `POST /v1/app-contexts`). A credential authorized via the CLI bootstrap's provisioning capability may only name the app context it is itself bound to; naming another one is refused. A root API key may name any of its contexts.
    
</dd>
</dl>

<dl>
<dd>

**sub_claim:** `typing.Optional[str]` — The claim in the IdP's token that carries the subject identifier. Defaults to `sub` if omitted.
    
</dd>
</dl>

<dl>
<dd>

**email_claim:** `typing.Optional[str]` — The claim in the IdP's token that carries the subject's email, used for first-login invite matching. Defaults to `email` if omitted.
    
</dd>
</dl>

<dl>
<dd>

**self_signup_policies:** `typing.Optional[typing.List[SelfSignupPolicy]]` — Opt-in self-service signup: a list of {signup_type, role_id} pairs. When a first-time exchange caller presents no invite token but names a signup_type matching one of these (or omits signup_type and exactly one entry exists), a brand-new user is created and bound to that entry's role — no invite required. Every entry must, by construction, be something you're willing to grant to ANY caller who can present a token from this issuer: no entry may target a role carrying elevated (provisioning or wildcard) scope — rejected. Omit entirely to leave self-signup disabled (the default).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">ping</a>() -> PingResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the identity bound to your credential — your account, principal type, key id, and scope details — so you can confirm who you are authenticated as and that the credential is valid. MCP clients use this to render "signed in as ..." in a chat UI without a separate identity endpoint.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.ping()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">list_profiles_for_principal</a>(...) -> AccessProfilePage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the access profiles for the given principal. Looking up your OWN principal — or holding the `context-directory-read` capability — returns the profiles across ALL of your contexts, letting you answer questions like "which apps does this user have access to?". A context-bound credential looking up a DIFFERENT principal instead sees only that principal's profile in your credential's own context (at most one result), never across contexts it has no authority over. Results are always confined to your account. Requires the `profiles:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.list_profiles_for_principal(
    principal_id="usr_6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**principal_id:** `str` — Identifier of the principal to look up — a user (`usr_<userId>`) or an API key (`key_<keyId>`).
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of access profiles to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">mint_token</a>(...) -> MintTokenResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a short-lived JWT bearer token restricted to specific actions and, optionally, to a particular user or identity entity (in any namespace). Use this to hand a narrowly-scoped credential to a browser or downstream service so it never sees your root API key. Only callable with a root API key (`sk_*`).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, ScopeRequest

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.mint_token(
    user_id="550e8400-e29b-41d4-a716-446655440000",
    scope=ScopeRequest(
        allowed_actions=[
            "records:crud",
            "schemas:r"
        ],
        identity={
            "userId": "550e8400-e29b-41d4-a716-446655440000"
        },
        data_scope={
            "scope:org": [
                "6ba7b810-9dad-11d1-80b4-00c04fd430c8"
            ]
        },
    ),
    expires_in_seconds=3600,
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**scope:** `ScopeRequest` 
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — The Vectros user ID of the end user this token is minted for. This is the UUID returned when you created the user via `POST /v1/users`. Optional — omit it for service-to-service tokens that have no specific user context. If provided, it must reference a real user in your account. Use `GET /v1/users?externalId={yourId}` to resolve your own system's user ID to the Vectros user ID.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — The app context to mint the token into. Optional — omit it to inherit your own credential's context (a root API key defaults to `default`). Must reference an app context that already exists in your tenant (create one via `POST /v1/app-contexts`); an unrecognized value returns a uniform `404 not found`. Only meaningful for root API key callers — this endpoint is root-key-only, so there is no confined credential this could let reach a context it doesn't hold.
    
</dd>
</dl>

<dl>
<dd>

**expires_in_seconds:** `typing.Optional[int]` — How long the token remains valid, in seconds. Maximum 3600 (1 hour), which is also the default.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">create_invite</a>(...) -> CreateInviteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Invite a new member to one of your app contexts by email. Creates a pending user with a pre-resolved access profile (their permissions on accept) and signs an invitation token. This call is idempotent on the combination of context and email: re-inviting the same email in the same context rotates the token and resends the invitation rather than creating a duplicate — this requires the `users:r` and `users:u` scopes in addition to `users:c`, because resending rotates a credential on an existing invitation and invalidates any link already sent. Without them the collision returns 409 instead, with no invitation details and no change to the outstanding invitation. Returns HTTP 201 on a new invite or a successful resend. Returns 409 if that email already belongs to an active or suspended member of the app context, or already has an identity elsewhere in your account (an email can currently belong to only one tenant per account, i.e. your test and live environments cannot share an email). When `sendEmail` is false, the response includes the raw token and a ready-to-use accept link so you can deliver the invitation through your own email provider. Requires the `users:c` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, AccessProfileSpec

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.create_invite(
    email="bob@example.com",
    context_id="myapp",
    access_profile=AccessProfileSpec(),
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `CreateInviteRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">resend_invite</a>(...) -> CreateInviteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Resend an outstanding invitation, identified by its email and app context. Rotates the invitation token and extends its expiry, then (when `sendEmail` is true) re-delivers the email. Rotating the token invalidates any previously issued link for this invitation, so only the newest link works. The invitee's pending permissions are left unchanged. Because this rotates a credential on an existing invitation, it requires the `users:c`, `users:r` and `users:u` scopes.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, AccessProfileSpec

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.resend_invite(
    email="bob@example.com",
    context_id="myapp",
    access_profile=AccessProfileSpec(),
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `CreateInviteRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.auth.<a href="src/vectros/auth/client.py">exchange_token</a>(...) -> TokenExchangeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

RFC 8693 OAuth 2.0 Token Exchange. Trades a JWT issued by a third-party identity provider you've registered (`POST /v1/auth/issuers`) for a Vectros `st_*` scoped bearer token — no Vectros credential required to call this endpoint. The exchanged token's scope is resolved entirely server-side from the matched user's access profile; this endpoint accepts no caller-supplied scope, resource, or audience parameter (RFC 8693 §2.1's `resource`/`audience`/`scope` are not used in v1 — the registered `(issuer, audience)` pair alone pins the target tenant and app context). On a first-time login (no existing Vectros identity for this subject), two opt-in binding paths exist: `invite_token` (a `PENDING` sub-user invitation), and — if the registration declares one or more self-signup policies — `signup_type` (a brand-new user is created and bound to the policy's configured role). If `invite_token` is present at all, it is the ONLY path tried — a failed invite never falls through to self-signup. Neither field is required for a subject with an existing identity. If your issuer is registered against more than one app context (each via its own audience), `context_id` selects which one to target; omit it when your token's `aud` claim matches only one registered context — the common case, unaffected by this field. Uses the OAuth-standard error envelope (`{"error":..., "error_description":...}`, RFC 6749 §5.2), NOT this API's usual `{"message":...}` shape — its client is generic OAuth tooling, not the Vectros SDK.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.auth.exchange_token(
    grant_type="urn:ietf:params:oauth:grant-type:token-exchange",
    subject_token="subject_token",
    subject_token_type="urn:ietf:params:oauth:token-type:jwt",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**grant_type:** `str` — Must be exactly `urn:ietf:params:oauth:grant-type:token-exchange`.
    
</dd>
</dl>

<dl>
<dd>

**subject_token:** `str` — The IdP-issued JWT to exchange for a Vectros-scoped token.
    
</dd>
</dl>

<dl>
<dd>

**subject_token_type:** `str` — The type of `subject_token`. Accepted: `urn:ietf:params:oauth:token-type:jwt` and `urn:ietf:params:oauth:token-type:id_token`.
    
</dd>
</dl>

<dl>
<dd>

**requested_token_type:** `typing.Optional[str]` — Accepted-and-ignored if present (this contract mints exactly one token shape). Optional.
    
</dd>
</dl>

<dl>
<dd>

**invite_token:** `typing.Optional[str]` — The `inv_*` invitation token from a sub-user invite email, when this exchange is a first-time login for a subject with no existing Vectros identity yet (TOKEN-EXCHANGE-CONTRACT.md §6). Not part of RFC 8693 — a Vectros-specific extension field, additive to the standard grant. Omit for a subject that already has an active Vectros identity; required to complete first login for one that doesn't. Delivered to the end user out-of-band (the same invite-email link flow as today), never generated by this endpoint.
    
</dd>
</dl>

<dl>
<dd>

**signup_type:** `typing.Optional[str]` 

Selects which self-service signup policy to apply for a first-time login with NO invite token, when the registered issuer declares one or more `selfSignupPolicies` (`POST /v1/auth/issuers`). A plain client-supplied selector, not a value your identity provider needs to assert. Omit when the issuer has exactly one policy entry (the unambiguous default); required to pick among multiple. Ignored entirely if the caller already has an existing Vectros identity, presented an `invite_token`, or the issuer offers no self-signup policies at all.

This does NOT reopen the caller-supplied-scope concern named above: `signup_type` never selects a privilege level, only WHICH pre-authored, already-open policy entry to bind to. Every `selfSignupPolicies` entry is, by construction, something you already decided ANY caller who can present a token from this issuer may have — self-service, no invite, is exactly that decision, so there is no privilege differential between entries for a caller to escalate into by naming a different one than your frontend intended. The platform independently enforces that this is actually true (no entry may ever resolve to an elevated role) regardless of what this field's value is. Because "any caller who can present a token from this issuer" is the real trust boundary, self-signup is only as narrow as your issuer's own audience — it is not a substitute for restricting who can obtain a token from your identity provider in the first place.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Selects which app context to target, for an issuer registered against more than one (each via its own `POST /v1/auth/issuers` row and a distinct `audience`). Not part of RFC 8693 — a Vectros-specific extension field, additive to the standard grant. Omit when your `subject_token`'s `aud` claim matches only one registered context (the common case, and unaffected by this field's addition — behavior is unchanged from before this field existed). When your token's `aud` claims could match MORE than one of your registered contexts, name the one you want; a mismatch (naming a context this issuer is not registered against) is refused identically to an unrecognized issuer — the response does not distinguish the two.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Documents
<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">list_documents</a>(...) -> DocumentPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of your documents, optionally filtered by folder (`folderId`) and/or owner (`userId` or `scope`). The response is a `{data, nextCursor}` envelope; pass `nextCursor` back as `startFrom` to fetch the next page. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.list_documents(
    user_id="550e8400-e29b-41d4-a716-446655440000",
    scope="group:eng-team",
    folder_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Filter by owning user — the Vectros-assigned UUID of a user. To resolve from your own identifier, call GET /v1/users?externalId=.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Filter to documents carrying this scope value, in `namespace:value` form (a value is 1-128 chars: a letter or digit first, then letters, digits, `_` or `-`) — for example `group:eng-team`, `org:<id>`, or `client:<id>`. Resolve an entity's UUID from your own identifier via `GET /v1/entities/{namespace}?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**folder_id:** `typing.Optional[str]` — List only documents in this folder (the Vectros folder ID). Can be combined with the owner filters.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of documents to return per page (1-100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">ingest_document</a>(...) -> DocumentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a document from a raw text string and queues it for asynchronous indexing so it becomes searchable. Optionally supply an `externalId` to make the create idempotent — if a document with the same `externalId` already exists in your context, that existing document is returned unchanged instead of a duplicate being created. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing document was returned) tells the two apart. To overwrite an existing document's content instead of returning it unchanged, set `?upsert=true` (this also requires the `documents:u` scope). Requires the `documents:c` scope to create. Being returned the existing document on a collision is a read of that document's data and additionally requires the `documents:r` scope — a credential holding `documents:c` alone receives a `400` ("already exists") on collision instead of the document.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.ingest_document(
    title="Patient Intake Form — Jane Doe",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `DocumentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a document with the same `externalId` already exists its content is overwritten (the submitted `payload`, `title`, and — when supplied — `text` are applied and the version is bumped) instead of being returned unchanged; the immutable `externalId`, `schemaId`, `indexMode`, and ownership are never changed. Defaults to `false`. Requires the `documents:u` scope in addition to `documents:c`.
    
</dd>
</dl>

<dl>
<dd>

**allow_clear:** `typing.Optional[bool]` — Only relevant with `?upsert=true`, which overwrites an existing document as a full replacement. If the submitted request omits (or sends as null) a stored field that a list or lookup response returns only as an indexed projection (a large document whose payload is stored externally), the overwrite is rejected unless you set `allowClear=true` to confirm that clearing those fields is intended. Use PATCH to update without clearing omitted fields. Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">get_document</a>(...) -> DocumentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single document by its ID, including its full structured payload. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.get_document(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">update_document</a>(...) -> DocumentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Replaces the mutable fields of a document. This is a full replacement of the payload — to merge fields instead, use PATCH. If you supply new `text`, the document body is re-ingested and re-queued for indexing. Requires the `documents:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.update_document(
    id="id",
    title="Patient Intake Form — Jane Doe",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `DocumentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**allow_clear:** `typing.Optional[bool]` — A `PUT` is a full replacement: if the submitted request omits (or sends as null) a stored field that a list or lookup response returns only as an indexed projection (a large document whose payload is stored externally), the update is rejected unless you set `allowClear=true` to confirm that clearing those fields is intended. Use PATCH to update without clearing omitted fields. Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">delete_document</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes the document and removes it from the search index. This cannot be undone. Requires the `documents:d` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.delete_document(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">patch_document</a>(...) -> DocumentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Partially updates a document using an RFC 7386 JSON Merge Patch. The `payload` object is deep-merged: keys you send overwrite existing values (recursing into nested objects), a key set to `null` is deleted, and keys you omit are preserved — unlike PUT, which replaces the whole payload. Top-level fields (`title`, `folderId`, `schemaId`, ownership) are set when present and left unchanged when omitted; sending a top-level field as `null` is rejected. Supplying `text` re-ingests the document body (same as PUT). `indexMode`, `externalId`, and `storeText` (text retention is fixed at ingest) are immutable and rejected if present. The merged result is validated against the bound schema. Pass `expectedVersion` for optimistic concurrency (409 on conflict). Requires the `documents:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.patch_document(
    id="id",
    title="Patient Intake Form — Jane Doe",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `DocumentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">lookup_documents</a>(...) -> DocumentLookupPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Finds documents of a given type by field value. Supported fields: `externalId` (the document's first-class external identifier — no schema declaration required) and any field declared as a lookup field on the bound schema. A lookup on a sensitive field is rejected here because the value would appear in the URL query string; use POST /v1/documents/lookup (the request-body variant) for a sensitive field instead. `type`'s schema resolves with basedOn-aware shadowing: your own `userId`- or `scope`-owned variant if you have one, otherwise the shared base — for a scoped credential the owner is always your own token identity; `userId`/`scope` here only apply as an explicit owner selector for a root API key. Results are paginated: set `limit` for the page size and feed the returned `nextCursor` back as `startFrom` to fetch the next page. The response is a `{data, nextCursor}` envelope. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.lookup_documents(
    type="invoice",
    field="po_number",
    value="PO-1001",
    prefix="PO-2024",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
    user_id="550e8400-e29b-41d4-a716-446655440000",
    scope="org:6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**type:** `str` — The document type to look up (the `type` of the bound schema).
    
</dd>
</dl>

<dl>
<dd>

**field:** `str` — The field to look up by. Use `externalId` to look up by the document's external identifier, or the name of any lookup field declared on the bound schema.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match. Mutually exclusive with `from`/`to` and `prefix`. Rejected for a sensitive field — use POST /v1/documents/lookup so the value isn't exposed in the URL.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup (requires `to`; range-enabled, non-sensitive fields only). Mutually exclusive with `value` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup (requires `from`).
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Prefix to match for a prefix lookup (range-enabled string fields only). Mutually exclusive with `value` and `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of documents to return per page (1-100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[LookupDocumentsRequestOrder]` — Sort direction for the returned documents: `asc` (the default) or `desc`.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Root API key ONLY: resolve `type`'s schema as this user would (basedOn-aware shadowing) instead of the shared base — mirrors `GET /v1/schemas?recordType=`'s `userId` selector. Ignored for a scoped credential.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Root API key ONLY: resolve `type`'s schema as this scope would, as a single `namespace:value` entry — mirrors `GET /v1/schemas?recordType=`'s `scope` selector. Ignored for a scoped credential.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">lookup_documents_by_body</a>(...) -> DocumentLookupPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Request-body equivalent of GET /v1/documents/lookup. Use this when looking up by a sensitive field: the value travels in the request body (and is blind-indexed server-side) instead of the URL query string, so it never lands in access, CDN, or proxy logs. The GET variant rejects `value` for a sensitive field and directs you here. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.lookup_documents_by_body(
    type="invoice",
    field="mrn",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**type:** `str` — Document type to look up (the type defined by the bound schema).
    
</dd>
</dl>

<dl>
<dd>

**field:** `str` — The field to look up by. Use `externalId` to look up by the document's external identifier (no schema declaration required), or the name of any lookup field declared on the bound schema. For a sensitive field, this body variant is required (the GET variant rejects sensitive-field lookups).
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match (equality mode). Mutually exclusive with `from`/`to` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup (requires `to`; non-sensitive, range-enabled fields only). Mutually exclusive with `value` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup (requires `from`; mutually exclusive with `value` and `prefix`).
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Prefix to match for a prefix lookup (range-enabled string fields only). Mutually exclusive with `value` and `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of documents to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[DocumentLookupRequestOrder]` — Sort direction for the returned results: `asc` (default) or `desc`.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Root API key ONLY: resolve `type`'s schema as this user would (basedOn-aware shadowing) instead of the shared base — mirrors `GET /v1/schemas?recordType=`'s `userId` selector. Ignored for a scoped credential, which always resolves via its own token identity.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Root API key ONLY: resolve `type`'s schema as this scope would (basedOn-aware shadowing), as a single `namespace:value` entry — mirrors `GET /v1/schemas?recordType=`'s `scope` selector. Ignored for a scoped credential.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">get_document_download_url</a>(...) -> DocumentDownloadResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a short-lived presigned S3 GET URL for the original uploaded file. Only available for file-backed documents (created via POST /v1/documents/upload); text-only documents return 400. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.get_document_download_url(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">get_document_text</a>(...) -> DocumentTextResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the document's full text body when it is retained: always available for text-ingested documents, and for file-uploaded documents unless they were uploaded with `storeText=false` (which discards the extracted text once indexing completes — the original file remains available via `GET /{id}/download`). Returns 404 when the document does not exist, its text was not retained, or extraction has not yet completed. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.get_document_text(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">get_document_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes (CREATE, UPDATE, DELETE) for a document. History is recorded only for documents bound to a schema that has audit history enabled (the default for typed documents); untyped documents have no version history. The response is a `{data, nextCursor}` envelope. Requires the `documents:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.get_document_versions(
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The ID of the document whose history you want.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor — pass the `nextCursor` returned by the previous page.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">upload_document</a>(...) -> FileUploadResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Starts a file-based document by returning a short-lived presigned S3 PUT URL. Upload the file bytes directly to `uploadUrl`; the document is then automatically queued for text extraction and asynchronous indexing. Supplying an `externalId` makes this idempotent — re-initiating an upload with the same `externalId` re-issues a fresh presigned URL to the SAME existing document/object (so a re-upload inherently replaces the file body) rather than creating a duplicate. The response's `created` field (and the HTTP status — 201 when a new document was minted, 200 when an existing one was matched) tells the two apart. With `?upsert=true`, the submitted `payload`/`title` are also applied to the matched document (file-body divergence cannot be diffed at upload-init — the bytes have not arrived yet — so the re-upload itself replaces the body). Creating a NEW document requires the `documents:c` scope. Re-uploading over an EXISTING document overwrites (and re-indexes) its body, so it is an update: it requires the `documents:u` scope (as does `?upsert=true` for the metadata).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.documents.upload_document(
    file_name="patient_intake_2024_01_15.pdf",
    file_type="application/pdf",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**file_name:** `str` — Original file name, including its extension. Used as the document title when no separate title is provided.
    
</dd>
</dl>

<dl>
<dd>

**file_type:** `str` — MIME type of the file being uploaded. Used to set the correct Content-Type on the stored object.
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true` and a document with the same `externalId` already exists, apply the submitted `payload`/`title` to that existing document (a metadata upsert) before re-issuing the presigned URL. The file body is replaced inherently by the re-upload; it cannot be diffed at upload-init. Defaults to `false`. Requires the `documents:u` scope in addition to `documents:c`.
    
</dd>
</dl>

<dl>
<dd>

**index_mode:** `typing.Optional[FileUploadRequestIndexMode]` — Indexing strategy applied after the file is processed and its text is extracted. `HYBRID` runs both BM25 keyword and dense-vector semantic indexing (recommended). `SEMANTIC` indexes only as dense vectors. `TEXT` indexes only with BM25. `NONE` is store-only (archival): the file is still uploaded and its text extracted, but it is not search-indexed — retrievable by id/download and structured-field lookup only. Optional: omit to inherit the bound schema's default index mode. If neither this field nor the schema specifies one, the request is rejected. When both are set, this per-file value wins.
    
</dd>
</dl>

<dl>
<dd>

**store_text:** `typing.Optional[bool]` — Whether the text extracted from this file is retained after indexing. Defaults to true: the extracted text stays retrievable via `GET /v1/documents/{id}/text` and usable by `POST /v1/documents/{id}/ask`. Set false to discard the extracted text once indexing completes — search results and the original file download are unaffected, but `/text` returns 404 and `/ask` returns 409 for the document. Fixed at ingest time: it cannot be changed later, and a re-upload to the same document keeps the original choice.
    
</dd>
</dl>

<dl>
<dd>

**folder_id:** `typing.Optional[str]` — ID of the folder in which to place this document. Omit to use your account's default root folder.
    
</dd>
</dl>

<dl>
<dd>

**payload:** `typing.Optional[typing.Dict[str, typing.Any]]` — The document's structured data, as a flat key/value object. When `schemaId` is set, declared fields are validated against the schema and its lookup fields become directly queryable (matching record and text-ingest behavior); undeclared keys pass through as free-form and are searchable via the `filters` parameter on `POST /v1/search`.
    
</dd>
</dl>

<dl>
<dd>

**schema_id:** `typing.Optional[str]` — Optional ID of a record schema to bind this document to. When set, the document's `payload` is validated against the schema and its lookup fields become directly queryable (matching record and text-ingest behavior).
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Owning user ID — the Vectros-assigned UUID of a user in your account. Optional. With an API key, sets the document's owner explicitly. With a scoped token the owning user is attributed by the server from your credential and cannot be set to a different user; supplying one that conflicts is rejected.
    
</dd>
</dl>

<dl>
<dd>

**scopes:** `typing.Optional[typing.List[str]]` — The document's scope ownership, as `namespace:value` entries (at most 2 namespaces) — for example `["org:6ba7b810-9dad-11d1-80b4-00c04fd430c8", "group:eng-team"]`. `org` and `client` are reserved namespace names, registered like any other; others are namespaces you registered yourself (lowercase, 2-32 chars). A `value` is 1-128 characters: a letter or digit first, then letters, digits, `_` or `-`. Resolve a namespace's UUID from your own identifier with `GET /v1/entities/{namespace}?externalId=`. When supplied, this is the document's COMPLETE scope declaration: each entry must fall inside the `data_scope` of a single clause of your credential that also grants this write — your identity supplies the DEFAULT value when you state none, it does not limit which value you may state. An empty array creates a document owned by the calling user alone (the private tier), and requires a credential whose identity carries a user. Omit the field to inherit the token's full identity — the default. Filter lists by these values with `?scope=`.
    
</dd>
</dl>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Stable, caller-supplied identifier for this document. Optional. Immutable after create. Unique within your account and context: initiating an upload again with the same `externalId` returns the same document plus a fresh presigned URL (idempotent — no duplicate), and it is the key other records use to reference this one. Max 256 characters.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Identity
<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">list_entities</a>(...) -> EntityPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of entities in a namespace. Filter by `userId` (entities owned by a user), by `externalId` (exact lookup by your own identifier), or by `scope` (`scope=<namespace>:<value>` — entities that have that value as a parent, e.g. `scope=org:6ba7...`). Naming this namespace's own name in `scope` resolves the entity itself (`scope=team:6ba7...` on `/v1/entities/team` returns that team), since an entity is always in its own scope. `userId` and `scope` can be combined to narrow on both dimensions at once; `externalId` identifies a single entity and cannot be combined with either. Requires the `entities:r:<namespace>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.list_entities(
    namespace="team",
    context_id="myapp",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context to read from. **Required when the namespace is context-placed** and rejected otherwise: a tenant-placed namespace's entities are shared by every context, so there is nothing to name. A context-placed namespace's entities belong to exactly one context and are invisible from the others — the same `externalId` may name a different entity in each. A context-confined credential may only name its own context.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Return only entities owned by this user (Vectros user ID).
    
</dd>
</dl>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Look up an entity by your own identifier. Returns a list with the single match, or empty.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Filter by parent edge as `<namespace>:<value>` (e.g. `org:6ba7...`). Matches any of the entity's parents, not just its first. Naming this route's own namespace resolves the entity itself.
    
</dd>
</dl>

<dl>
<dd>

**type:** `typing.Optional[str]` — Schema record type whose lookup fields you want to query. Supply with `field` and one lookup mode (`value`, `from`/`to`, or `prefix`).
    
</dd>
</dl>

<dl>
<dd>

**field:** `typing.Optional[str]` — The schema-declared lookup field to filter on. Supply with `type` and one lookup mode.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match for `field` (equality). Not allowed for a sensitive field — use `POST /v1/entities/{namespace}/lookup`.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup (requires `to`; range-enabled fields).
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup (requires `from`).
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Match all values of `field` starting with this prefix (range-enabled string fields).
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[ListEntitiesRequestOrder]` — Sort direction by the field's value for a `type`/`field` lookup: `asc` (ascending, the default) or `desc`. Lookup mode only — listing by namespace, `userId`, `scope`, or `externalId` does not take a sort direction and rejects this parameter.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum entities per page (1-100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">create_entity</a>(...) -> EntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new entity in the given namespace. This call is idempotent on `externalId` within the namespace: if an entity with the same `externalId` already exists, the existing record is returned instead of creating a duplicate (`created: false`, HTTP 200). To overwrite an existing entity's content instead of returning it unchanged, set `?upsert=true` (also requires the `entities:u:<namespace>` scope). The namespace must be entity-backed (`org`/`client`, or registered via `POST /v1/namespaces`). Requires the `entities:c:<namespace>` scope to create. Being returned the existing entity on a collision is a read of that entity's data and additionally requires the `entities:r:<namespace>` scope — a credential holding `entities:c:<namespace>` alone receives a `400` ("already in use") on collision instead of the entity.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.create_entity(
    namespace="team",
    context_id="myapp",
    external_id="team_eng_platform",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**request:** `EntityRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, overwrite an existing entity's mutable fields instead of returning it unchanged. Requires the `entities:u:<namespace>` scope in addition to `entities:c:<namespace>`.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context owns the new entity. **Required when the namespace is context-placed** and omitted otherwise: a tenant-placed namespace's entities are shared by every context, so supplying one is rejected. A context-confined credential may only name its own context. An entity's context is fixed at creation and cannot be changed afterwards.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_entity</a>(...) -> EntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single entity by its namespace and Vectros-assigned ID. Requires the `entities:r:<namespace>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.get_entity(
    namespace="team",
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**id:** `str` — The Vectros-assigned ID (UUID) of the entity.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context to read from. **Required when the namespace is context-placed** and rejected otherwise: a tenant-placed namespace's entities are shared by every context, so there is nothing to name. A context-placed namespace's entities belong to exactly one context and are invisible from the others — the same `externalId` may name a different entity in each. A context-confined credential may only name its own context.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">update_entity</a>(...) -> EntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates the mutable fields of an entity. Omitted fields are preserved (a null value does not clear a field), and the `payload` object is replaced in full when supplied. Providing `scopes` replaces the entity's parent edges. Requires the `entities:u:<namespace>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.update_entity(
    namespace="team",
    id="id",
    context_id="myapp",
    external_id="team_eng_platform",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `EntityRequest` 
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context to read from. **Required when the namespace is context-placed** and rejected otherwise: a tenant-placed namespace's entities are shared by every context, so there is nothing to name. A context-placed namespace's entities belong to exactly one context and are invisible from the others — the same `externalId` may name a different entity in each. A context-confined credential may only name its own context.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">delete_entity</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes an entity. This action cannot be undone. Requires the `entities:d:<namespace>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.delete_entity(
    namespace="team",
    id="id",
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context to read from. **Required when the namespace is context-placed** and rejected otherwise: a tenant-placed namespace's entities are shared by every context, so there is nothing to name. A context-placed namespace's entities belong to exactly one context and are invisible from the others — the same `externalId` may name a different entity in each. A context-confined credential may only name its own context.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">lookup_entities</a>(...) -> EntityPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Looks up entities in a namespace by a schema-declared field value, with the criteria in the request body instead of the URL. Use this for a sensitive field: the value travels in the body and never appears in the URL. Body equivalent of the `type`/`field`/`value` lookup on `GET /v1/entities/{namespace}`, which rejects sensitive-field values and directs you here. Requires the `entities:r:<namespace>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.lookup_entities(
    namespace="team",
    context_id="myapp",
    type="person_v1",
    field="ssn",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**request:** `IdentityLookupRequest` 
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context to read from. **Required when the namespace is context-placed** and rejected otherwise: a tenant-placed namespace's entities are shared by every context, so there is nothing to name. A context-placed namespace's entities belong to exactly one context and are invisible from the others — the same `externalId` may name a different entity in each. A context-confined credential may only name its own context.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_entity_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes made to an entity, newest first. Sensitive field values are redacted in the history. Requires the `entities:r:<namespace>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.get_entity_versions(
    namespace="team",
    id="id",
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The entity namespace.
    
</dd>
</dl>

<dl>
<dd>

**id:** `str` — The Vectros-assigned ID (UUID) of the entity.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — Which app context to read from. **Required when the namespace is context-placed** and rejected otherwise: a tenant-placed namespace's entities are shared by every context, so there is nothing to name. A context-placed namespace's entities belong to exactly one context and are invisible from the others — the same `externalId` may name a different entity in each. A context-confined credential may only name its own context.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor from a previous page's `nextCursor`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_namespace</a>(...) -> NamespaceResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single scope-namespace registration by name.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.get_namespace(
    namespace="team",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The namespace name.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — An app context to resolve this namespace for — its own registration if it has one, else the tenant-wide one. Omit for the tenant-wide registration only.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">update_namespace</a>(...) -> NamespaceResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates the mutable fields (`entityBacked`, `defaultSchemaId`, `specificityRank`) of a registered namespace. The namespace name and its `contextId` (which row is selected) are both immutable. Requires a root API key. `org` and `client` are updatable like any other namespace — there is no reserved-built-in exception.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.update_namespace(
    namespace_="team",
    namespace="team",
    specificity_rank=1500,
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The namespace name.
    
</dd>
</dl>

<dl>
<dd>

**request:** `NamespaceRequest` 
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — The app context that owns this registration. Omit for the tenant-wide registration.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">delete_namespace</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a registered scope namespace. Requires a root API key. `org` and `client` are deletable like any other namespace — there is no reserved-built-in exception. A namespace that still has entities cannot be deleted (409) — delete its entities first; this keeps them reachable by the account-teardown and erasure sweeps.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.delete_namespace(
    namespace="team",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespace:** `str` — The namespace name.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — The app context that owns this registration. Omit for the tenant-wide registration.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">list_namespaces</a>(...) -> NamespacePage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the scope namespaces registered in your account. Returns a `{data, nextCursor}` envelope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.list_namespaces()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor from a previous page's `nextCursor`.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum registrations per page (1-100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — List one app context's OWN registrations instead of the tenant-wide ones. Omit for the tenant-wide registrations only — a context's own registrations are never mixed into the unfiltered listing.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">register_namespace</a>(...) -> NamespaceResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Registers a new scope namespace and declares whether its values resolve to identity entities (`entityBacked`). Also requires `specificityRank`, an explicit, account-unique position in the specificity order used to break recordType schema-resolution ties. Requires a root API key or the CLI bootstrap's provisioning capability — never an ordinary partner-grantable scope. `org` and `client` are reserved names, registered the same way as any other namespace.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.register_namespace(
    namespace="team",
    specificity_rank=1500,
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `NamespaceRequest` 
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — The app context to own this registration. Omit for a TENANT-WIDE registration visible to every context.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">list_users</a>(...) -> UserPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of the users in your account. Pass `externalId` to look up a single user by your own identifier. To filter on schema-declared lookup fields, supply `type` and `field` together with one lookup mode: `value` (exact match), `from`+`to` (range), or `prefix`. Requires the `users:r` scope. A context-confined credential only sees users who hold an access profile in the credential's own app context — others are silently absent from the page, not an error.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.list_users(
    external_id="usr_12345",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
    type="person_v1",
    field="team",
    value="engineering",
    prefix="eng",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Look up a single user by your own `externalId`. Returns a one-element list, or an empty list if no match. Cannot be combined with the `type`/`field`/`value` lookup parameters.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of users to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**type:** `typing.Optional[str]` — The schema record type whose lookup fields you are querying (the schema's declared record type, not the user's `HUMAN`/`SERVICE` kind). Must be supplied together with `field` and exactly one lookup mode (`value`, `from`+`to`, or `prefix`).
    
</dd>
</dl>

<dl>
<dd>

**field:** `typing.Optional[str]` — The lookup field to filter on. Must be declared as a lookup field on the schema named by `type`. Supplied together with `type` and one lookup mode.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match for `field`. Cannot be combined with `from`/`to` or `prefix`. Not allowed for a sensitive (blind-indexed) field on this endpoint; use POST /v1/users/lookup instead, which keeps the value out of the URL.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup. Requires `to`, and is only valid on non-sensitive fields declared with range support. Cannot be combined with `value` or `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup. Requires `from`.
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Prefix to match. Only valid on string fields declared with range support. Cannot be combined with `value` or `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[ListUsersRequestOrder]` — Sort direction for the matched users: `asc` (ascending, the default) or `desc` (descending).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">create_user</a>(...) -> UserResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a user identity in your account. The operation is idempotent on `externalId`: if a user with the same `externalId` already exists, the existing record is returned instead of creating a duplicate. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing user was returned) tells the two apart. To overwrite an existing user's mutable fields (email, status, payload, schema binding) instead of returning it unchanged, set `?upsert=true` (this also requires the `users:u` scope). Requires the `users:c` scope to create. Being returned the existing user on a collision is a read of that user's data and additionally requires the `users:r` scope — a credential holding `users:c` alone receives a `400` ("already exists") on collision instead of the user.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.create_user(
    external_id="usr_12345",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `UserRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a user with the same `externalId` already exists its mutable fields (email, status, payload, schemaId) are updated to the submitted values instead of being returned unchanged; the immutable `externalId` and `type` are never changed, and `email` cannot be changed while an invitation to that user is still outstanding. Defaults to `false`. Requires the `users:u` scope in addition to `users:c`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_user</a>(...) -> UserResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single user by its Vectros-assigned ID. Requires the `users:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.get_user(
    id="550e8400-e29b-41d4-a716-446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the user.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">update_user</a>(...) -> UserResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates mutable fields on an existing user (such as email, status, payload, or schema binding). The `type` field is immutable after creation, and `email` cannot be changed while an invitation to that user is still outstanding — revoke the invitation, or invite the new address instead. This endpoint also activates an invited user: a PUT that moves a PENDING user to ACTIVE and carries `inviteToken`, `externalSubject`, and `emailVerifiedAttestation=true` completes the invitation. Requires the `users:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.update_user(
    id="550e8400-e29b-41d4-a716-446655440000",
    external_id="usr_12345",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the user.
    
</dd>
</dl>

<dl>
<dd>

**request:** `UserRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">delete_user</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes a user identity. This cannot be undone. If the user is a pending invitation, the associated access profile created for that invitation is also removed. Requires the `users:d` scope. Deleting your account's last OWNER is refused (409) — an account must always retain at least one owner. Called by a context-confined credential, deletion is also refused (409) when the user holds access in an app context other than the caller's own — remove the user from the caller's own context first, or use a credential with cross-context reach.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.delete_user(
    id="550e8400-e29b-41d4-a716-446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the user.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">lookup_users</a>(...) -> UserPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Looks up users by a schema lookup field, with the query criteria carried in the request body rather than the URL. Use this when looking up by a sensitive (blind-indexed) field: the value is blind-indexed server-side and never appears in the URL, request logs, or proxies. The query semantics are identical to the GET /v1/users lookup, which rejects sensitive-field values and directs you here. Returns a page in the `{data, nextCursor}` envelope. Requires the `users:r` scope. A context-confined credential only sees users who hold an access profile in the credential's own app context — others are silently absent from the page, not an error.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.lookup_users(
    type="person_v1",
    field="ssn",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `IdentityLookupRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">user_exists_by_email</a>(...) -> UserExistsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Answers "does a user with this email hold an ACTIVE access profile in this app context" — a narrow existence check, not a general lookup. `exists` is false for a member whose access to this context has been suspended, not only for a member who was never granted it. The answer is scoped to the `contextId` you supply: it does not reveal whether the email exists elsewhere in your tenant or account, only whether it belongs to an active member of the named context. Returns `{exists, userId, status}` — never the full user record — so a caller asking "does X exist" cannot receive that user's payload/schema binding/etc. as a side effect. Useful for resolving an email you were handed (for example, by an org-admin adding an existing member to another org) to a `userId`, without paging through the full context membership. Requires the `users:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.user_exists_by_email(
    email="user@example.com",
    context_id="myapp",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**email:** `typing.Optional[str]` — The email address to check. Matched case-insensitively.
    
</dd>
</dl>

<dl>
<dd>

**context_id:** `typing.Optional[str]` — The app context to check membership in. A context-confined credential (a scoped token or key bound to one context) may only name its own bound context — naming any other context is rejected with a uniform 403, before the existence check runs.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_user_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes to a user, most recent first. Identity history is always recorded and always available. Sensitive field values are redacted in every historical version. Returns a page in the `{data, nextCursor}` envelope. Requires the `users:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.identity.get_user_versions(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the user.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the ID of the last version from the previous page to fetch the next page.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Compliance
<details><summary><code>client.compliance.<a href="src/vectros/compliance/client.py">create_erasure_request</a>(...) -> ErasureRequestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Submits a right-to-erasure request for a single end-subject (a user, or an identity entity in any namespace). Erasure removes exactly the data the subject solely owns across the declared contexts, plus the subject's identity and lookup rows. It never touches another account's data and never cascades into another subject's data. The request is asynchronous: it returns 202 with a `requestId`; poll `GET /v1/erasure-requests/{id}` until the job completes to obtain the completion certificate. Requires a root API key — a scoped credential is rejected with 403.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.compliance.create_erasure_request(
    subject_type="user",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**subject_type:** `str` — The kind of end-subject to erase: `user` for a person, or an ownership namespace such as `org` or `client` — including a custom namespace you have defined.
    
</dd>
</dl>

<dl>
<dd>

**subject_id:** `typing.Optional[str]` — The platform id of the subject to erase (the Vectros-assigned id returned when the subject was created). Provide exactly one of `subjectId` or `externalId`.
    
</dd>
</dl>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Your own externalId for the subject, as an alternative to `subjectId`. Provide exactly one of `subjectId` or `externalId`.
    
</dd>
</dl>

<dl>
<dd>

**context_scope:** `typing.Optional[typing.List[str]]` — The contexts in which to erase the subject. As the data controller, you declare the blast radius: each listed context is erased independently and reported separately on the certificate. Omit or leave empty to erase every context the subject has data in; those contexts are then enumerated and erased one at a time, never as a single account-wide sweep. Any context you do not list is left untouched, and managing that residual data is your responsibility.
    
</dd>
</dl>

<dl>
<dd>

**audit_disposition:** `typing.Optional[ErasureRequestAuditDisposition]` — How to handle the subject's audit and version-history trail. This governs only the audit data — the subject's records and documents are always erased regardless. `retain-redacted` (the default) keeps the compliance audit trail with sensitive data redacted; `purge` additionally hard-removes the audit history itself (subject to your legal obligations).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.compliance.<a href="src/vectros/compliance/client.py">get_erasure_request</a>(...) -> ErasureRequestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Polls an erasure request by id. While the job is still running this returns its status only; once it completes, the response also includes the verifiable completion certificate (which contexts were swept, per-context deletion counts, and reports of dangling references and shared rows that were left intact). Requires a root API key.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.compliance.get_erasure_request(
    id="er_550e8400e29b41d4a716446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The id of the erasure request to poll, as returned when the request was submitted.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.compliance.<a href="src/vectros/compliance/client.py">create_export</a>(...) -> ExportRequestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Submits an asynchronous job to export your account's data, or a single end-subject's data, across the requested contexts. The request returns 202 with an `exportJobId`; poll `GET /v1/admin/export/{id}` until the job completes to obtain a short-lived presigned download URL and a manifest describing the payload. Requires a root API key — a scoped credential is rejected with 403.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.compliance.create_export()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**scope:** `typing.Optional[ExportRequestScope]` — What to export. `tenant` (the default) exports all of your account's data across the requested contexts; `subject` exports a single end-subject's data (for example, to satisfy a one-person data-portability request). Additional scopes may be added in the future.
    
</dd>
</dl>

<dl>
<dd>

**subject_type:** `typing.Optional[str]` — The kind of subject to export: `user` for a person, or an ownership namespace such as `org` or `client` — including a custom namespace you have defined. Required only when `scope` is `subject`.
    
</dd>
</dl>

<dl>
<dd>

**subject_id:** `typing.Optional[str]` — The platform id of the subject to export. Used only when `scope` is `subject`. Provide exactly one of `subjectId` or `externalId`.
    
</dd>
</dl>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Your own externalId for the subject, as an alternative to `subjectId` (only when `scope` is `subject`). Provide exactly one of `subjectId` or `externalId`.
    
</dd>
</dl>

<dl>
<dd>

**context_scope:** `typing.Optional[typing.List[str]]` — The contexts to export. Omit or leave empty to export all of your contexts. Each listed context is exported and reported separately in the manifest.
    
</dd>
</dl>

<dl>
<dd>

**format:** `typing.Optional[ExportRequestFormat]` — Serialization format of the export payload. NDJSON (newline-delimited JSON, one row per line) is currently the only supported format; more may be added in the future. The manifest carries a `formatVersion` so you can parse the payload in a forward-compatible way.
    
</dd>
</dl>

<dl>
<dd>

**include_audit_history:** `typing.Optional[bool]` — Whether to also include the audit and version-history trail of the exported data, not just the current live rows. Defaults to false (live data only). Set true to also export the retained version history. This governs only the audit data — the live records are always included.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.compliance.<a href="src/vectros/compliance/client.py">get_export</a>(...) -> ExportRequestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Polls an export job by id. While the job is still running this returns its status only; once it completes, the response also includes a short-lived presigned download URL and a manifest (format version and per-context counts). Requires a root API key.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.compliance.get_export(
    id="exp_550e8400e29b41d4a716446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The id of the export job to poll, as returned when the job was submitted.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Folders
<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">list_folders</a>(...) -> FolderPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of your folders. Pass `parentFolderId` to list the direct children of a specific folder (tree navigation); omit it for a flat list across your account. You can also filter by owner using `userId` or `scope`. Results are returned as a `{data, nextCursor}` envelope — pass `nextCursor` as `startFrom` to fetch the next page. Requires the `folders:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.list_folders(
    parent_folder_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    user_id="550e8400-e29b-41d4-a716-446655440000",
    scope="group:eng-team",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**parent_folder_id:** `typing.Optional[str]` — List only the direct children of this folder — the Vectros-assigned UUID of a folder — for tree navigation. Omit for a flat list across your account.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Filter to folders owned by this user — the Vectros-assigned UUID of a user. Use `GET /v1/users?externalId=` to resolve your own external ID to this UUID.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Filter to folders carrying this scope value, in `namespace:value` form (a value is 1-128 chars: a letter or digit first, then letters, digits, `_` or `-`) — for example `group:eng-team`, `org:<id>`, or `client:<id>`. Resolve an entity's UUID from your own identifier via `GET /v1/entities/{namespace}?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of folders to return per page. Must be between 1 and 100; defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">create_folder</a>(...) -> FolderResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a folder to organize your documents and records. If `parentFolderId` is omitted, the folder is created under your context's default root folder. Folder creation is idempotent by (slug + parent): if a folder with the same slug already exists under the same parent, that existing folder is returned unchanged instead of a duplicate being created. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing folder was returned) tells the two apart. To overwrite an existing folder's mutable fields instead of returning it unchanged, set `?upsert=true` (this also requires the `folders:u` scope). Requires the `folders:c` scope to create. Being returned the existing folder on a collision is a read of that folder's data and additionally requires the `folders:r` scope — a credential holding `folders:c` alone receives a `400` ("already exists") on collision instead of the folder.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.create_folder(
    name="Patient Records 2024",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `FolderRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a folder with the same slug already exists under the same parent its mutable fields (`name`, `description`, and ownership) are overwritten from the request and the version is bumped, instead of the existing folder being returned unchanged; the immutable slug and parent are never changed. A re-applied upsert whose content matches is a no-op (no version bump). Defaults to `false`. Requires the `folders:u` scope in addition to `folders:c`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">get_folder</a>(...) -> FolderResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single folder by its ID. Requires the `folders:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.get_folder(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">update_folder</a>(...) -> FolderResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Replaces a folder's name and description. Omitted fields are preserved (a null does not clear a field). The slug and parent folder are immutable and cannot be changed here. Requires the `folders:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.update_folder(
    id="id",
    name="Patient Records 2024",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `FolderRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">delete_folder</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes a folder. The folder must be empty (contain no documents or sub-folders) and must not be protected. Your context's root folder is protected and cannot be deleted. Requires the `folders:d` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.delete_folder(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">patch_folder</a>(...) -> FolderResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Partially updates a folder using an RFC 7386 JSON Merge Patch. The `name`, `description`, and ownership fields (`userId`, `scopes`) are applied when present and left unchanged when omitted; sending any of these as null is rejected, because clearing a field is not supported in this release (omit it instead). `slug` and `parentFolderId` are immutable — a folder cannot be re-slugged or moved via the API — and the request is rejected if either is present. Pass `expectedVersion` for optimistic concurrency (you get a 409 if the folder changed since you last read it). Requires the `folders:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.patch_folder(
    id="id",
    name="Patient Records 2024",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request:** `FolderRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.folders.<a href="src/vectros/folders/client.py">get_folder_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes (create, update, and delete events) for a folder, newest first. Folder version history is always recorded; there is no setting to turn it off. Results are returned as a `{data, nextCursor}` envelope — pass `nextCursor` as `startFrom` to fetch the next page. Requires the `folders:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.folders.get_folder_versions(
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The ID of the folder whose history you want.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value from the previous page to fetch the next page; omit it for the first page.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Inference
<details><summary><code>client.inference.<a href="src/vectros/inference/client.py">list_inference_models</a>() -> ModelsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns every inference model available to you, including each model's context window, the plan tiers it's available on, and the exact credit rates charged per 1K input and output tokens. Use this to populate model pickers and to validate a request before calling `/v1/chat`, `/v1/rag`, or `/v1/documents/{id}/ask`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.inference.list_inference_models()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.inference.<a href="src/vectros/inference/client.py">chat_inference</a>(...) -> typing.Iterator[bytes]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Streams a model response as Server-Sent Events (SSE). Send the full conversation history in the `messages` array; a message with role `system` is extracted and used as the system prompt. Token cost is debited from your pre-paid inference balance (in cents), and a small per-call flat fee is debited from your monthly platform credit allowance. Requires the `inference:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, ChatMessage

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.inference.chat_inference(
    messages=[
        ChatMessage(
            role="system",
            content="content",
        )
    ],
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**messages:** `typing.List[ChatMessage]` — The conversation history, in order. A message with role `system` is extracted and used as the system prompt; `user` and `assistant` messages are sent to the model as conversation turns.
    
</dd>
</dl>

<dl>
<dd>

**model:** `typing.Optional[str]` — Model alias to use, from the list returned by `GET /v1/models`. Defaults to `claude-haiku-4-5`.
    
</dd>
</dl>

<dl>
<dd>

**max_tokens:** `typing.Optional[int]` — Maximum number of tokens to generate. Defaults to 2048; the maximum is 8192.
    
</dd>
</dl>

<dl>
<dd>

**temperature:** `typing.Optional[float]` — Sampling temperature, between 0.0 and 1.0. Higher values produce more varied output. Defaults to 0.7.
    
</dd>
</dl>

<dl>
<dd>

**top_p:** `typing.Optional[float]` — Top-p (nucleus) sampling. Ignored when `temperature` is greater than 0.
    
</dd>
</dl>

<dl>
<dd>

**allow_global_region:** `typing.Optional[bool]` — Opt this request into global (non-US) region serving for lower cost. Requires a signed global-processing waiver on your account that permits per-request override; otherwise the request is rejected with 403. When omitted, the request follows your account's default residency setting. Configure data residency under Data Residency and Region settings in the developer portal.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.inference.<a href="src/vectros/inference/client.py">document_ask</a>(...) -> typing.Iterator[bytes]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Loads a single document's extracted text, supplies it as context, and streams a model answer about it. The document must be fully indexed. If the document exceeds the 32K-token cap, the call returns 413 with no credits charged — use `POST /v1/rag` instead to answer over larger or multi-document collections. Requires the `inference:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.inference.document_ask(
    id="id",
    prompt="prompt",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**prompt:** `str` — The question or instruction to answer about the document.
    
</dd>
</dl>

<dl>
<dd>

**instructions:** `typing.Optional[str]` — Optional system prompt that overrides the default. Defaults to a generic instruction to act as a helpful document analyst.
    
</dd>
</dl>

<dl>
<dd>

**model:** `typing.Optional[str]` — Model alias to use, from the list returned by `GET /v1/models`. Defaults to `claude-haiku-4-5`.
    
</dd>
</dl>

<dl>
<dd>

**max_tokens:** `typing.Optional[int]` — Maximum number of tokens to generate. Defaults to 2048; the maximum is 8192.
    
</dd>
</dl>

<dl>
<dd>

**temperature:** `typing.Optional[float]` — Sampling temperature. Defaults to 0.2.
    
</dd>
</dl>

<dl>
<dd>

**allow_global_region:** `typing.Optional[bool]` — Opt this request into global (non-US) region serving for lower cost. Requires a signed global-processing waiver on your account that permits per-request override; otherwise the request is rejected with 403. When omitted, the request follows your account's default residency setting.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.inference.<a href="src/vectros/inference/client.py">rag_inference</a>(...) -> typing.Iterator[bytes]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs hybrid search over your indexed content, then streams a model answer grounded in the top results. The SSE stream emits a `search_results` event first (carrying the matched results and their metadata), an optional `truncation_warning` if lower-scoring results were dropped to fit the model's context window, then `content_delta` chunks, and finally a terminal `done` event. Requires the `inference:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.inference.rag_inference(
    query="query",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The natural-language question to answer over your indexed content.
    
</dd>
</dl>

<dl>
<dd>

**instructions:** `typing.Optional[str]` — Optional system prompt that overrides the default. Defaults to a generic instruction to answer using only the provided context.
    
</dd>
</dl>

<dl>
<dd>

**model:** `typing.Optional[str]` — Model alias to use, from the list returned by `GET /v1/models`. Defaults to `claude-haiku-4-5`.
    
</dd>
</dl>

<dl>
<dd>

**search:** `typing.Optional[RagSearch]` 
    
</dd>
</dl>

<dl>
<dd>

**max_tokens:** `typing.Optional[int]` — Maximum number of tokens to generate. Defaults to 1024; the maximum is 4096.
    
</dd>
</dl>

<dl>
<dd>

**temperature:** `typing.Optional[float]` — Sampling temperature. Defaults to 0.3, favoring more deterministic, retrieval-grounded answers.
    
</dd>
</dl>

<dl>
<dd>

**allow_global_region:** `typing.Optional[bool]` — Opt this request into global (non-US) region serving for lower cost. Requires a signed global-processing waiver on your account that permits per-request override; otherwise the request is rejected with 403. When omitted, the request follows your account's default residency setting.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Records
<details><summary><code>client.records.<a href="src/vectros/records/client.py">batch_get_records</a>(...) -> BatchGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Reserved endpoint for fetching multiple records by ID in one call. When available, the response will contain only the records you can see; any IDs that do not exist or are outside your scope are silently omitted (there is no per-ID existence signal), matching the not-found behavior of the single-record GET. It currently returns 501 (not implemented). The documented 200 response schema is the stable shape this endpoint will use once available. Requires the `records:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.batch_get_records()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**ids:** `typing.Optional[typing.List[str]]` — The record ids to fetch (maximum 100). Any id you cannot access — nonexistent or outside your account or token scope — is silently omitted from the response, so a missing id never reveals whether that record exists.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">batch_lookup_records</a>(...) -> BatchLookupResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Reserved endpoint for batch lookup and reference resolution. The published response shape correlates results to each input and carries a per-item status envelope. It currently returns 501 (not implemented). The documented 200 response schema is the stable shape this endpoint will use once available. Requires the `records:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.batch_lookup_records()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requests:** `typing.Optional[typing.List[BatchLookupInput]]` — The lookups to resolve (the `requests` array). Each carries a `ref` you supply, echoed back on its result group so you can correlate results to inputs.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">batch_write_records</a>(...) -> BatchWriteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Reserved endpoint for bulk record writes. The published response shape includes a per-item partial-failure envelope and an atomicity flag. It currently returns 501 (not implemented). The documented 200 response schema is the stable shape this endpoint will use once available, published now so SDK integrations against it will not break when it ships. Requires the `records:c` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.batch_write_records()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**atomicity:** `typing.Optional[BatchWriteRequestAtomicity]` — Controls how the batch commits. `all_or_nothing` commits every item or none (transactional, but allows a smaller maximum batch size); `best_effort` commits each item independently and reports a per-item outcome. Defaults to `best_effort`.
    
</dd>
</dl>

<dl>
<dd>

**items:** `typing.Optional[typing.List[RecordRequest]]` — The records to write. Each item has the same shape as the body of a single create-record request (`POST /v1/records`).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">list_records</a>(...) -> RecordPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of records in your account as a `{data, nextCursor}` page. Supply exactly one of `type`, `folderId`, or `recent=true` to choose the mode: `type` lists all records of a single type; `folderId` lists all records in a folder (any type); and `recent=true` returns the account-wide recently-updated feed across all types, newest first. You may combine `type` with `folderId` to list a single type within a folder. The owner filters (`userId`, `scope`) further narrow the type and folder modes; the `recent` feed is standalone and ignores all filters. Each token only sees the record types it is scoped to read. Requires the `records:r` scope. By default the response returns the indexed projection of each record; set `includePayload=true` to include full payloads.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.list_records(
    type="intake_form",
    folder_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    user_id="550e8400-e29b-41d4-a716-446655440000",
    scope="group:eng-team",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**type:** `typing.Optional[str]` — The record type to list, e.g. `intake_form`. Required unless `folderId` or `recent=true` is given.
    
</dd>
</dl>

<dl>
<dd>

**folder_id:** `typing.Optional[str]` — List all records in this folder, regardless of type. The value is a Vectros-assigned folder UUID. Required unless `type` or `recent=true` is given; may be combined with `type` to list a single type within the folder.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Filter to records owned by this user. The value is the Vectros-assigned UUID of a user; resolve one from your own ID via `GET /v1/users?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Filter to records carrying this scope value, in `namespace:value` form (a value is 1-128 chars: a letter or digit first, then letters, digits, `_` or `-`) — for example `group:eng-team`, `org:<id>`, or `client:<id>`. Resolve an entity's UUID from your own identifier via `GET /v1/entities/{namespace}?externalId=`. Combine with `type` or `folderId`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of records to return per page. Allowed range 1–100; defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**include_payload:** `typing.Optional[str]` — Set to `true` to include each record's full payload in this response, rehydrating any externally stored payloads (hydration is capped per page). Defaults to `false`, which returns only the indexed projection; fetch a full payload via `GET /v1/records/{id}`.
    
</dd>
</dl>

<dl>
<dd>

**recent:** `typing.Optional[str]` — Set to `true` to return the account-wide recently-updated feed across all record types, newest first — a type-agnostic activity view. This mode is standalone: it ignores `type`, `folderId`, and the owner filters. Per-record scope still applies — you see only the record types your token may read.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">create_record</a>(...) -> RecordResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new record of a given type. The `payload` is validated against that type's schema before the record is stored. Identify the type by sending `typeName`, `schemaId`, or both (they must agree); if you send only `schemaId`, the type is taken from that schema. Optionally supply an `externalId` to make the create idempotent — if a record with the same `externalId` already exists in your context, that existing record is returned unchanged instead of a duplicate being created. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing record was returned) tells the two apart. To overwrite an existing record's content instead of returning it unchanged, set `?upsert=true` (this also requires the `records:u:<type>` scope). Requires the `records:c:<type>` scope to create. Being returned the existing record on a collision is a read of that record's data and additionally requires the `records:r:<type>` scope — a credential holding `records:c:<type>` alone receives a `400` ("already exists") on collision instead of the record.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.create_record(
    type_name="intake_form",
    schema_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    payload={
        "first_name": "Jane",
        "email": "jane@example.com"
    },
    folder_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `RecordRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a record with the same `externalId` already exists its content is overwritten (the submitted `payload` and mutable fields are applied and the version is bumped) instead of being returned unchanged; the immutable `externalId`, `schemaId`/`typeName`, and ownership are never changed. A re-applied upsert whose content matches is a no-op (no version bump). Defaults to `false`. Requires the `records:u:<type>` scope in addition to `records:c:<type>`.
    
</dd>
</dl>

<dl>
<dd>

**allow_clear:** `typing.Optional[bool]` — Only relevant with `?upsert=true`, which overwrites an existing record as a full replacement. If the submitted `payload` omits (or sends as null) a stored field that a list or lookup response returns only as an indexed projection (a large record whose payload is stored externally), the overwrite is rejected unless you set `allowClear=true` to confirm that clearing those fields is intended. Use PATCH to update without clearing omitted fields. Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">get_record</a>(...) -> RecordResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single record by its Vectros-assigned ID, including its full payload (payloads that were externalized to object storage are rehydrated for this response). Sensitive fields are masked according to the record's schema. Requires the `records:r:<type>` scope. A record outside your account or scope returns 404 (not found) rather than revealing its existence.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.get_record(
    id="550e8400-e29b-41d4-a716-446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the record to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">update_record</a>(...) -> RecordResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Replaces a record's payload and mutable fields. This is a full replacement: the `payload` you send overwrites the existing payload entirely, so include every field you want to keep (use the PATCH endpoint to change only specific fields). `typeName` and `schemaId` are immutable and cannot be changed. The new payload is validated against the record's schema. Pass `expectedVersion` to make the update conditional on the record not having changed since you last read it (optimistic concurrency). Requires the `records:u:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.update_record(
    id="550e8400-e29b-41d4-a716-446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the record to update.
    
</dd>
</dl>

<dl>
<dd>

**request:** `RecordRequest` 
    
</dd>
</dl>

<dl>
<dd>

**allow_clear:** `typing.Optional[bool]` — A `PUT` is a full replacement: if the submitted `payload` omits (or sends as null) a stored field that a list or lookup response returns only as an indexed projection (a large record whose payload is stored externally), the update is rejected unless you set `allowClear=true` to confirm that clearing those fields is intended. Use PATCH to update without clearing omitted fields. Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">delete_record</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes a record. This is a hard delete: the record is removed and a tombstone plus an audit-trail entry are recorded (you can later retrieve the tombstone via `GET /v1/records/{id}/tombstone`). Requires the `records:d:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.delete_record(
    id="550e8400-e29b-41d4-a716-446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the record to delete.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">patch_record</a>(...) -> RecordResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Partially updates a record using an RFC 7386 JSON Merge Patch. The `payload` object is deep-merged into the existing payload: keys you send overwrite (recursing into nested objects), a key set to null is deleted, and keys you omit are left unchanged — so you can change a single field without re-sending the rest (unlike the full-replacement PUT). Top-level fields (`status`, `folderId`, `userId`, `scopes`) are set when present and left unchanged when omitted; sending a top-level field as null is rejected (clearing a top-level field is not supported in this release — omit it instead). `typeName`, `schemaId`, `externalId`, and `indexMode` are immutable and rejected if present. The merged result is validated against the schema. Pass `expectedVersion` to make the patch conditional (optimistic concurrency, 409 on conflict). Requires the `records:u:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.patch_record(
    id="550e8400-e29b-41d4-a716-446655440000",
    payload={
        "status": "done"
    },
    expected_version=5,
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the record to patch.
    
</dd>
</dl>

<dl>
<dd>

**request:** `RecordRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">lookup_records</a>(...) -> RecordLookupPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Finds records by the value of a lookup field declared on the type's schema. Provide exactly one lookup mode: `value` (exact match), `from`+`to` (inclusive range, ascending by value), or `prefix` (string fields only, ascending). Range and prefix lookups are not supported on a sensitive field, because its value is stored as a blind index and has no sortable order. An exact-`value` lookup on a sensitive field is also rejected on this GET endpoint — the value must not appear in the URL — so use the `POST /v1/records/lookup` body variant for sensitive fields.

A `value` lookup can additionally be narrowed to a window of the lookup field's **sort key** using `sortFrom` and/or `sortTo` (inclusive) — for example, one session's records created since a timestamp. The sort key is whatever the schema declares as that lookup's `sortBy` (`createdAt` by default, `lastUpdated`, or another field), and bounds are given in that field's own units — epoch milliseconds for the two timestamp options. **Records that have no value for the sorted field are never included in a bounded window** — they are ordered ahead of every record that does have one, and a `sortFrom`/`sortTo` window only ever selects from records carrying a value. The sorted field does not have to be `required`.

Narrowing is available on any lookup field your schema declares for fast equality lookup, and on `externalId` — which is always ordered by creation time, so its bounds are epoch milliseconds whatever the schema says. It is rejected (`400`) for a field declared with `rangeEnabled`, for a field declared beyond the schema's fast-lookup budget, for a lookup whose `sortBy` names a sensitive field (a sensitive value is stored as a blind index and has no order), and for a window whose start is after its end. Ownership fields are not lookup fields on this endpoint at all — see the `field` parameter.

While paging a narrowed lookup, keep every other parameter identical. The cursor is valid only for the exact query that returned it — changing `order`, `sortFrom`, `sortTo`, or dropping them altogether, is rejected rather than silently resumed at a position that means something different in the new query.

Results are paginated: set `limit` for the page size and pass the returned `nextCursor` back as `startFrom` for the next page. **Keep paging until `nextCursor` is null** — a page can come back empty or shorter than `limit` while more results remain, so an empty page is not the end of the results. Requires the `records:r:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.lookup_records(
    type="intake_form",
    field="email",
    value="jane@example.com",
    values=[
        "open"
    ],
    prefix="jane",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**type:** `str` — The record type to look up, e.g. `intake_form`.
    
</dd>
</dl>

<dl>
<dd>

**field:** `str` — The name of the lookup field to match on, as declared on the type's schema.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match. Mutually exclusive with `from`/`to`, `prefix` and `values`. Rejected for a sensitive field — use `POST /v1/records/lookup` instead so the value is not exposed in the URL.
    
</dd>
</dl>

<dl>
<dd>

**values:** `typing.Optional[typing.Union[str, typing.Sequence[str]]]` 

Exact values to match, for a lookup declared over several fields — one value per field, in the order the lookup declares them, given as a repeated parameter (`?field=status,area&values=open&values=billing`). Mutually exclusive with `value`; a single `values` is identical to `value`.

You may supply fewer values than the lookup declares, as long as they are a leading run of its fields; doing so returns the records grouped by the fields you left unspecified. Narrowing with `sortFrom`/`sortTo` then requires a value for every field, because the ordering is only continuous within one fully specified combination.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound of a range lookup (requires `to`; non-sensitive fields only). Mutually exclusive with `value` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound of a range lookup (requires `from`).
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Prefix to match (string, non-sensitive fields only). Mutually exclusive with `value` and `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**sort_from:** `typing.Optional[str]` — Inclusive lower bound on the lookup field's sort key, narrowing a `value` match to records at or after this point. Use with `value`; combine with `sortTo` to bound both ends. Give the bound in the same form as the sorted field's own values — epoch milliseconds when the lookup sorts by `createdAt` or `lastUpdated`. Records with no value for the sorted field are never included in a bounded window.
    
</dd>
</dl>

<dl>
<dd>

**sort_to:** `typing.Optional[str]` — Inclusive upper bound on the lookup field's sort key, narrowing a `value` match to records at or before this point. Use with `value`; combine with `sortFrom`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of records to return per page. Allowed range 1–100; defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**include_payload:** `typing.Optional[str]` — Set to `true` to include each record's full payload, rehydrating any payloads stored externally (capped per page). Defaults to `false`, which returns only the indexed projection.
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[LookupRecordsRequestOrder]` — Sort direction by the field's value: `asc` (ascending, the default) or `desc` (descending).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">lookup_records_by_body</a>(...) -> RecordLookupPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Body-based equivalent of `GET /v1/records/lookup`. Use this when looking up by a sensitive field: the value travels in the request body (and is blind-indexed server-side) instead of in the URL query string, so it never lands in access, CDN, or proxy logs. The GET variant rejects an exact-value lookup on a sensitive field and directs you here. Non-sensitive exact-value, range (`from`+`to`), and prefix lookups also work here, as does narrowing a `value` lookup by the field's sort key with `sortFrom`/`sortTo` — see the GET variant for what the sort key is and when narrowing by it is available. Returns the same `{data, nextCursor}` envelope and uses the same pagination as the GET variant; keep paging until `nextCursor` is null rather than stopping on an empty page. Requires the `records:r:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.lookup_records_by_body(
    type="intake_form",
    field="ssn",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**type:** `str` — The record type to look up (for example, `intake_form`).
    
</dd>
</dl>

<dl>
<dd>

**field:** `str` — Name of the lookup field declared on the schema. For a field marked sensitive, this body variant is required (the GET variant rejects looking up by a sensitive value). For a lookup declared over several fields, give those field names joined with commas (`status,area`) and put the values in `values`.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match. Mutually exclusive with `from`/`to`, `prefix` and `values`. Sensitive fields can only be looked up by exact value, and only through this body variant. For a lookup over several fields, use `values` instead.
    
</dd>
</dl>

<dl>
<dd>

**values:** `typing.Optional[typing.List[str]]` 

Exact values to match, for a lookup declared over several fields — one value per field, in the order the lookup declares them. Mutually exclusive with `value`; a single-element list is exactly `value` written uniformly.

You may supply fewer values than the lookup declares, as long as they are a leading run of its fields: on a lookup over `[status, area, owner]` you can match `status`, or `status` and `area`, but never `area` alone. Supplying fewer values returns the records grouped by the fields you left unspecified; `sortFrom`/`sortTo` then need every value, because sorting is only continuous within one fully specified combination.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup (requires `to`; non-sensitive fields only). Mutually exclusive with `value` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup (requires `from`).
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Prefix to match (string, non-sensitive fields only). Mutually exclusive with `value` and `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**sort_from:** `typing.Optional[str]` — Inclusive lower bound on the lookup field's sort key, narrowing a `value` match to records whose sort key is at or after this point. Use with `value`; may be combined with `sortTo` to bound both ends. Give the bound in the same form as the sorted field's own values — epoch milliseconds when the lookup sorts by `createdAt` or `lastUpdated`. Records with no value for the sorted field are never included in a bounded window.
    
</dd>
</dl>

<dl>
<dd>

**sort_to:** `typing.Optional[str]` — Inclusive upper bound on the lookup field's sort key, narrowing a `value` match to records whose sort key is at or before this point. Use with `value`; may be combined with `sortFrom`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor — pass the `nextCursor` returned by the previous page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of records to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[RecordLookupRequestOrder]` — Sort direction for the returned results: `asc` (default) or `desc`.
    
</dd>
</dl>

<dl>
<dd>

**include_payload:** `typing.Optional[bool]` — When true, externalized record payloads are returned in full in this response (capped per page). Defaults to false, which returns only the indexed projection.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">get_record_tombstone</a>(...) -> RecordResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the tombstone left behind when a record was hard-deleted, confirming the deletion and recording when it happened. Look it up using the deleted record's original ID. Requires the `records:r:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.get_record_tombstone(
    id="id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.records.<a href="src/vectros/records/client.py">get_record_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of past versions for a record, as a paginated `{data, nextCursor}` page. This is available only when the record type's schema has audit history enabled (the default); if it is disabled, the endpoint returns 409. Requires the `records:r:<type>` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.records.get_record_versions(
    id="550e8400-e29b-41d4-a716-446655440000",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The Vectros-assigned UUID of the record whose version history you want.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page; omit it for the first page.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Schemas
<details><summary><code>client.schemas.<a href="src/vectros/schemas/client.py">list_schemas</a>(...) -> SchemaPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of the record schemas defined in your account. Filter by `userId` or `scope` to scope to an owner, by `surface` to list the types bindable to one surface, or by `recordType` to resolve the single schema for a type directly. Filtering by `surface=user` lists your account-wide identity schemas regardless of the calling context (a `user`-surfaced schema always has one tenant-wide home). Filtering by `surface=entity` reads your own app context's entity schemas AND the tenant-wide home together, newest first, through a single cursor — an entity schema may live in either home depending on where its namespace is placed (see `POST /v1/namespaces`). Filtering by `record` or `document` lists within the calling context only. Requires the `schemas:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.schemas.list_schemas(
    user_id="550e8400-e29b-41d4-a716-446655440000",
    scope="org:6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    surface="document",
    record_type="intake_form",
    start_from="b3BhcXVlLWN1cnNvci1mcm9tLXRoZS1wcmV2aW91cy1wYWdl",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Filter to schemas owned by this user — the Vectros-assigned UUID of a user in your account. Use `GET /v1/users?externalId=` to resolve a UUID from your own external id.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Filter to schemas carrying this scope value, as a single `namespace:value` entry — for example `org:6ba7b810-9dad-11d1-80b4-00c04fd430c8` or `group:eng-team`. `org` and `client` are reserved namespace names; others are namespaces you registered yourself. Resolve a namespace's UUID from your own identifier with `GET /v1/entities/{namespace}?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**surface:** `typing.Optional[str]` — Filter to schemas bindable to this surface: `record`, `document`, `user`, or `entity` — identity entities in any namespace (`org`, `client`, or one you registered) bind under the single `entity` surface. Returns only schemas whose allowed surfaces include the given one — useful for listing, say, document types separately from record types. `user` is fully account-wide: filtering by it lists your account's identity schemas regardless of the calling context, since a `user`-surfaced schema always has one tenant-wide home. `entity` is NOT account-wide in the same sense: an entity schema is homed in whichever app context its namespace is placed in (or the tenant-wide home for a tenant-placed namespace), so filtering by `entity` reads your own context's entity schemas together with the tenant-wide ones — a different caller context can see a different result. `record` and `document` list within the calling context only.
    
</dd>
</dl>

<dl>
<dd>

**record_type:** `typing.Optional[str]` — Resolve the single schema for this record type — the natural handle for a schema, and the direct alternative to remembering its opaque id. Returns a one-element page, or an empty page if no such schema exists. Resolved in the calling context for record and document types; combine with `surface=user` to resolve an account-wide identity schema, or `surface=entity` to resolve one from your own context plus the tenant-wide home (see the `surface` parameter). A type name may have several schemas in one context — a shared base, plus per-owner variants declared via `basedOn` — and resolution shadows by ownership: your own `userId`- or `scope`-owned variant wins if you have one, otherwise the shared base. For a scoped credential the owner is always your own token identity; `userId`/`scope` here only apply as an explicit owner selector for a root API key (a scoped credential's own identity always governs resolution, and a `scope` filter still narrows the result afterward regardless of credential type).
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page. The cursor is **opaque** — echo it back unchanged, and do not parse it or construct one. Keep every other query parameter identical while paging: a cursor is valid only for the exact query that returned it, and reusing one against a different query is rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of schemas to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schemas.<a href="src/vectros/schemas/client.py">create_schema</a>(...) -> SchemaResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Defines a new record type with optional field definitions, validation rules, and lookup indexes. Idempotent by `typeName` within the same ownership scope: re-creating an existing `typeName` returns the existing schema rather than failing. The response's `created` field (and the HTTP status — 201 when created, 200 when an existing schema was returned) tells the two apart. To reconcile an existing schema to the submitted shape instead of returning it unchanged, set `?upsert=true` (this also requires the `schemas:u` scope; only legal schema changes are applied — migration-locked changes are rejected). Requires the `schemas:c` scope to create. Being returned the existing schema on a collision is a read of that schema's data and additionally requires the `schemas:r` scope — a credential holding `schemas:c` alone receives a `400` ("already in use") on collision instead of the schema.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi, FieldDef, LookupDef

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.schemas.create_schema(
    type_name="intake_form",
    display_name="Client Intake Form",
    description="Captures initial client information",
    fields=[
        FieldDef(
            field_id="first_name",
            field_type="string",
            required=True,
            searchable=True,
        ),
        FieldDef(
            field_id="email",
            field_type="string",
            required=True,
        )
    ],
    lookup_fields=[
        LookupDef(
            field_name="email",
            unique=True,
        )
    ],
    capabilities={
        "auditHistory": True
    },
    allowed_surfaces=[
        "record"
    ],
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `SchemaRequest` 
    
</dd>
</dl>

<dl>
<dd>

**upsert:** `typing.Optional[bool]` — When `true`, if a schema with the same `typeName` already exists it is reconciled to the submitted shape (additive fields, lookups, renderHints, and `active` are applied) instead of being returned unchanged; `typeName` and migration-locked lookup attributes (`rangeEnabled`/`sortBy`/`sensitive`) cannot be changed and a request to do so is rejected. A re-applied upsert whose declared shape is unchanged is a no-op (no schema-version bump). Defaults to `false`. Requires the `schemas:u` scope.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schemas.<a href="src/vectros/schemas/client.py">get_schema</a>(...) -> SchemaResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single record schema by its id. Requires the `schemas:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.schemas.get_schema(
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The id of the schema to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schemas.<a href="src/vectros/schemas/client.py">update_schema</a>(...) -> SchemaResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates a record schema. Fields you omit are preserved; `typeName` is immutable and cannot be changed. Collection fields (`fields`, `lookupFields`, `renderHints`, `capabilities`) are replaced in full when supplied. Requires the `schemas:u` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.schemas.update_schema(
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    type_name="intake_form",
    display_name="Client Intake Form",
    allowed_surfaces=[
        "record"
    ],
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The id of the schema to update.
    
</dd>
</dl>

<dl>
<dd>

**request:** `SchemaRequest` 
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schemas.<a href="src/vectros/schemas/client.py">delete_schema</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes a record schema. The request is refused with 409 if records of this type still exist — delete those records first, since every record must reference a live schema. A lineage base (a schema other schemas declare `basedOn`) also cannot be deleted while any such variant still exists — delete the variant schema(s) first. Requires the `schemas:d` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.schemas.delete_schema(
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The id of the schema to delete.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schemas.<a href="src/vectros/schemas/client.py">get_schema_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes to a record schema, newest first (create, update, and delete). Schema version history is always recorded — there is no per-schema toggle to disable it. Requires the `schemas:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.schemas.get_schema_versions(
    id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `str` — The id of the schema whose version history you want.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor — pass the `nextCursor` value from the previous page to fetch the next one.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Search
<details><summary><code>client.search.<a href="src/vectros/search/client.py">content</a>(...) -> SearchResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs a hybrid, semantic, or text search across the content you have indexed. By default it returns both documents and records in a single unified result set; pass `contentTypes` to narrow the results (for example `["documents"]`). Each result carries a `sourceType` discriminator (`PartnerDocument` or `GenericRecord`) so you can branch on the type. Requires the `search:r` scope.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from vectros import VectrosApi

client = VectrosApi(
    token="<token>",
    base_url="https://yourhost.com/path/to/api",
)

client.search.content(
    query="patient intake form diabetes",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The search query, expressed in natural language or as keywords. Required.
    
</dd>
</dl>

<dl>
<dd>

**mode:** `typing.Optional[SearchRequestMode]` — How results are ranked. HYBRID combines semantic and keyword ranking (recommended). SEMANTIC ranks by vector similarity only. TEXT ranks by keyword matching only. Defaults to HYBRID.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — The maximum number of results to return. Must be between 1 and 100. Defaults to 20.
    
</dd>
</dl>

<dl>
<dd>

**offset:** `typing.Optional[int]` — The number of results to skip, for pagination. Must be between 0 and 200. Defaults to 0.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Restrict results to content owned by this user — the Vectros-assigned UUID of a user in your account. Use `GET /v1/users?externalId=` to look up a user's ID from your own identifier.
    
</dd>
</dl>

<dl>
<dd>

**scope:** `typing.Optional[str]` — Restrict results to content carrying this scope value, in `namespace:value` form (a value is 1-128 chars: a letter or digit first, then letters, digits, `_` or `-`) — for example `group:eng-team`, `org:<id>`, or `client:<id>`. Scope values are attached to records and documents at creation (the `scopes` field). Use `GET /v1/entities/{namespace}?externalId=` to look up an entity's ID from your own identifier.
    
</dd>
</dl>

<dl>
<dd>

**filters:** `typing.Optional[typing.Dict[str, FilterValue]]` — Field-level filters applied to your document and record metadata. Each key is a field name, and top-level keys are AND-combined. Each value is one of: a scalar (string, number, or boolean) for an exact match; an array of scalars to match any one of them; or an operator map for ranges, negation, and membership. The supported operators are a closed set: `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte` (each takes a scalar) and `$in`, `$nin` (each takes an array of scalars). Operators within one map are AND-combined, so `{"price":{"$gte":100,"$lte":500}}` expresses a closed range; `$in` and `$nin` may not be combined with other operators. Numbers and booleans are matched by type, so the field must have been ingested under a typed schema; dates may be sent as ISO 8601 strings or epoch milliseconds. Unknown operators, non-scalar operands, and malformed field names are rejected with a 400.
    
</dd>
</dl>

<dl>
<dd>

**text_mode:** `typing.Optional[SearchRequestTextMode]` — How query terms are matched by the keyword engine when the mode is TEXT or HYBRID. OR matches content containing any query term (broadest recall). AND requires every term to be present (higher precision). PHRASE requires the terms to appear as a contiguous sequence. COMPLEX enables the full query syntax, including boolean operators, field-scoped queries, and range filters. When omitted, defaults to PHRASE (with a slop of 3) in HYBRID mode and OR in TEXT mode.
    
</dd>
</dl>

<dl>
<dd>

**min_similarity:** `typing.Optional[float]` — The minimum semantic similarity score a result must have to be included, on a scale of 0.0 to 1.0. Results scoring below this threshold are dropped. Applies when the mode is SEMANTIC or HYBRID.
    
</dd>
</dl>

<dl>
<dd>

**min_text_relevance:** `typing.Optional[float]` — A relative relevance floor for the keyword leg, given as a fraction from 0 to 1 of the TOP result's score — results scoring below (top score × this value) are dropped. Keyword scores are unbounded and depend on the query, so this is a relative cutoff rather than an absolute score: for example, 0.5 keeps only results at least half as relevant as the best hit. Applies when the mode is TEXT or HYBRID. Omit it (or use a value of 0 or less) to keep all keyword matches.
    
</dd>
</dl>

<dl>
<dd>

**slop:** `typing.Optional[int]` — The phrase-match slop: the number of intervening positions allowed between query terms when `textMode` is PHRASE (0 means the terms must be exactly adjacent). Ignored for the other text modes.
    
</dd>
</dl>

<dl>
<dd>

**unique_documents:** `typing.Optional[bool]` — When true, returns at most one chunk per source item, so a single document or record cannot appear more than once in the results.
    
</dd>
</dl>

<dl>
<dd>

**content_types:** `typing.Optional[typing.List[SearchRequestContentTypesItem]]` — Narrow results to specific content types. When omitted, empty, or set to both values, the search returns all content (documents and records) in a single unified result set, which is the default behavior. Each returned result carries a `sourceType` discriminator so you can tell documents and records apart. Pass `["documents"]` for documents only, or `["records"]` for records only.
    
</dd>
</dl>

<dl>
<dd>

**folder_id:** `typing.Optional[str]` — Restrict results to content (documents or records) in this EXACT folder. Folders hold a mix of documents and records. Provide the Vectros-assigned UUID of a folder; use `GET /v1/folders` to list your folders. To match a folder AND all of its descendants, use `rootFolderId` instead.
    
</dd>
</dl>

<dl>
<dd>

**root_folder_id:** `typing.Optional[str]` — Restrict results to content (documents or records) anywhere under this folder subtree — the folder itself and all of its descendants. Provide the Vectros-assigned UUID of a top-level (root) folder. Applies to documents and records alike. Use `folderId` instead to match a single exact folder.
    
</dd>
</dl>

<dl>
<dd>

**type_name:** `typing.Optional[str]` — Restrict results to content of a specific schema type — for example `patient`, `intake_form`, or `runbook`. Applies to both documents and records: any item whose bound schema type matches is kept. On its own it narrows both documents and records to that type; combine it with `contentTypes` to narrow within a single content type — for example `contentTypes: ["documents"]` with `typeName: "runbook"` searches only runbook documents. Untyped content (ingested without a schema) never matches a `typeName` filter.
    
</dd>
</dl>

<dl>
<dd>

**created_after:** `typing.Optional[str]` — Restrict results to content created at or after this ISO-8601 UTC timestamp. Useful for incremental queries (for example, finding anything ingested in the last hour), for isolating just-created content from older history, and for time-bounded analytics. Pair it with `createdBefore` to define a window.
    
</dd>
</dl>

<dl>
<dd>

**created_before:** `typing.Optional[str]` — Restrict results to content created at or before this ISO-8601 UTC timestamp. Pair it with `createdAfter` to define a window, or use it alone to find content older than a cutoff (for example, in archival or cleanup workflows).
    
</dd>
</dl>

<dl>
<dd>

**require_complete:** `typing.Optional[bool]` — A fail-closed override. When true, the request returns HTTP 503 instead of partial results if one of the search engines is unavailable. Defaults to false, in which case an outage degrades to the surviving engine and the response carries `degraded: true` along with the failed engines in `degradedLegs`. Set this to true only when complete results are required and a degraded answer is unacceptable.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

