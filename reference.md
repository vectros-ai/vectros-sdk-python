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

Returns a page of per-subject PHI read-access rows: who read which subject's PHI, when, against which record, and whether any sensitive value was actually revealed in plaintext. Metadata only — never the PHI itself. This is the disclosure-accounting surface from which a covered entity derives its HIPAA §164.528 accounting of disclosures. Provide at least one query axis: a subject (`subjectType` + `subjectId`, plus optional `clientId`) within a `contextId` for the primary accounting query; `resourceId` within a `contextId` for 'who read this record'; `callerKeyId` for 'what did this credential read' (account-wide forensic); or `contextId` alone to enumerate a whole context. `from`/`to` bound the time window. Results are scoped to your account, derived from your token — never from input. Requires the `access-log:r` scope.
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
    client_id="client_xyz789",
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

**subject_type:** `typing.Optional[str]` — Kind of subject to account for: `user`, `client`, or `org`.
    
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

**client_id:** `typing.Optional[str]` — Further restrict a subject query to reads about a single nested client/patient.
    
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

Lists all of your scoped API keys (`ssk_*`) across both your live and test environments. Revoked keys are excluded. Requires the `keys:r` scope.
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

Creates a scoped API key (an `ssk_*` secret) that inherits its permissions from an existing access profile in your account. The call is idempotent on the combination of tenant, context, user, and key name: re-issuing the same request returns the existing key WITHOUT re-disclosing its raw secret. The raw key is returned ONLY in this response — store it securely, as it cannot be retrieved again. Requires the `keys:c` scope.
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

**user_id:** `str` — The user the key binds to. May be a `HUMAN` or a `SERVICE` user (a service user is the typical agent or bot case). An access profile must already exist for this context and user — the key references that profile; it does not create one.
    
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

**resource:** `typing.Optional[str]` — Filter by resource type. One of `documents`, `records`, `search`, `schemas`, `folders`, `clients`, `orgs`, `users`, `usage`, `auth`, `models`, `ping`, `rag`, `chat`, `ask`, `erasure-requests`, or `export`.
    
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

Creates a new access profile under the given app context. This call is idempotent by `principalId`: if a profile with the same `principalId` already exists, the existing profile is returned (with status 200) instead of creating a duplicate. You must provide exactly one of `scopes` (an inline list of scopes) or `roleId` (a reference to a role); supplying both, or neither, is rejected. `identityOverrides` may set only `orgId` and `clientId`; any other key (including the account identifier or `userId`) is rejected. If you use a scoped credential, the profile's effective scopes may not exceed your own; a root API key (`sk_`) is exempt. Requires the `profiles:c` scope.
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

Creates a new app context. This call is idempotent by `contextId`: if an app context with the same `contextId` already exists, the existing app context is returned (with status 200) instead of creating a duplicate. The reserved `contextId` value `vectros-admin` cannot be created through this endpoint; it is provisioned automatically for your account. Requires the `app-contexts:c` scope.
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

Creates a new role under the given app context. This call is idempotent by `roleId`: if a role with the same `roleId` already exists, the existing role is returned (with status 200) instead of creating a duplicate. If you use a scoped credential, the role's scopes may not exceed your own; a root API key (`sk_`) is exempt. Requires the `profiles:c` scope.
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
                "read",
                "write"
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

Updates an access profile. This is a partial update: any field you omit (or send as null) keeps its existing value. A profile must reference either inline `scopes` or a `roleId`, never both — so setting `scopes` clears any `roleId`, and setting `roleId` clears any inline `scopes`. The `contextId` and `principalId` are immutable. Status changes (for example active to suspended) take effect within about five minutes. If you use a scoped credential, the profile's effective scopes may not exceed your own; a root API key (`sk_`) is exempt. Requires the `profiles:u` scope.
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

Deletes an access profile. Within about five minutes (the access-profile cache lifetime), token minting for this principal in this context will be denied. Requires the `profiles:d` scope.
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
                "read",
                "write"
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

Returns full usage detail for the requested calendar month, broken down by category (search, documents, and records) with per-category credit estimates and a split between your live and test environments. Defaults to the current month when `year` and `month` are omitted. Requires the `billing:r` scope on scoped tokens; API keys always have access.
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

**context_id:** `typing.Optional[str]` — App context id. When supplied, the `contexts` breakdown is restricted to that single app context; your account-wide totals are unaffected.
    
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

Returns the access profiles for the given principal across all of your contexts. Use this to answer questions like "which apps does this user have access to?" — for example, to build a member-access summary. Results are confined to your account. Requires the `profiles:r` scope.
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

Creates a short-lived JWT bearer token restricted to specific actions and, optionally, to a particular user, organization, or client. Use this to hand a narrowly-scoped credential to a browser or downstream service so it never sees your root API key. Only callable with a root API key (`sk_*`).
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
            "orgId": [
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

**expires_in_seconds:** `typing.Optional[int]` — How long the token remains valid, in seconds. Maximum 86400 (24 hours); defaults to 3600 (1 hour).
    
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

Invite a new member to one of your app contexts by email. Creates a pending user with a pre-resolved access profile (their permissions on accept) and signs an invitation token. This call is idempotent on the combination of context and email: re-inviting the same email in the same context rotates the token and resends the invitation rather than creating a duplicate. Returns HTTP 201 on a new invite or a successful resend. Returns 409 if that email already belongs to an active or suspended member of the app context, or already has an identity elsewhere in your account (an email can currently belong to only one tenant per account, i.e. your test and live environments cannot share an email). When `sendEmail` is false, the response includes the raw token and a ready-to-use accept link so you can deliver the invitation through your own email provider. Requires the `admin:users` scope.
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

Resend an outstanding invitation, identified by its email and app context. Rotates the invitation token and extends its expiry, then (when `sendEmail` is true) re-delivers the email. Rotating the token invalidates any previously issued link for this invitation, so only the newest link works. The invitee's pending permissions are left unchanged. Requires the `admin:users` scope.
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

## Identity
<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">list_clients</a>(...) -> ClientPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of clients in your account. Narrow the results with `orgId` or `userId`, or use `externalId` for an exact lookup by your own identifier. For schema-bound clients, you can also query by a schema-declared lookup field using `type`, `field`, and one lookup mode (`value` for equality, `from`/`to` for a range, or `prefix`). The response is a `{data, nextCursor}` envelope; pass `nextCursor` back as `startFrom` to fetch the next page. Requires the `clients:r` scope.
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

client.identity.list_clients(
    org_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    user_id="550e8400-e29b-41d4-a716-446655440000",
    external_id="patient_789",
    start_from="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    type="client_v1",
    field="industry",
    value="healthcare",
    prefix="health",
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

**org_id:** `typing.Optional[str]` — Return only clients belonging to this organization. Pass the Vectros-assigned UUID of an org; use `GET /v1/orgs?externalId=` to resolve your own identifier to this UUID.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Return only clients owned by this user. Pass the Vectros-assigned UUID of a user; use `GET /v1/users?externalId=` to resolve your own identifier to this UUID.
    
</dd>
</dl>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Look up a client by your own `externalId`. Returns a single-element list, or an empty list if no client matches.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of clients to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**type:** `typing.Optional[str]` — Record type of the schema whose lookup fields you are querying. Must be supplied together with `field` and exactly one lookup mode.
    
</dd>
</dl>

<dl>
<dd>

**field:** `typing.Optional[str]` — Name of the lookup field to filter by. Must be declared as a lookup field on the schema identified by `type`. Supply it together with `type` and exactly one lookup mode.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match for `field` (equality mode). Mutually exclusive with `from`/`to` and `prefix`. Not allowed for a sensitive field — use `POST /v1/clients/lookup` instead so the value is not exposed in the URL.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup. Requires `to`, and is only allowed on non-sensitive fields that have range queries enabled. Mutually exclusive with `value` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup. Requires `from`.
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Match clients whose value for `field` starts with this prefix. Only allowed on string fields that have range queries enabled. Mutually exclusive with `value` and `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[ListClientsRequestOrder]` — Sort direction for the results: `asc` (the default) or `desc`.
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">create_client</a>(...) -> ClientResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new client identity in your account. This call is idempotent on `externalId`: if a client with the same `externalId` already exists, the existing record is returned instead of creating a duplicate. Requires the `clients:c` scope.
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

client.identity.create_client(
    external_id="patient_789",
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

**request:** `ClientRequest` 
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_client</a>(...) -> ClientResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single client by its Vectros-assigned UUID. Requires the `clients:r` scope.
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

client.identity.get_client(
    id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
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

**id:** `str` — The Vectros-assigned UUID of the client to retrieve.
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">update_client</a>(...) -> ClientResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates mutable fields on an existing client. Omitted fields are preserved (a null does not clear a field); when `payload` is supplied it replaces the stored payload in full rather than being deep-merged. Requires the `clients:u` scope.
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

client.identity.update_client(
    id="id",
    external_id="patient_789",
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

**request:** `ClientRequest` 
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">delete_client</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes the client. This action cannot be undone. Requires the `clients:d` scope.
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

client.identity.delete_client(
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">lookup_clients</a>(...) -> ClientPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Body-based equivalent of the `type`/`field`/`value` lookup on `GET /v1/clients`. Use this when looking up by a sensitive (blind-indexed) field: the value travels in the request body rather than the URL. The `GET` list rejects a sensitive field's value and directs you here. The response is a `{data, nextCursor}` envelope. Requires the `clients:r` scope.
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

client.identity.lookup_clients(
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_client_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes to a client, newest first. Identity auditing is always on, so this history is always available; sensitive field values are redacted in each version. The response is a `{data, nextCursor}` envelope. Requires the `clients:r` scope.
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

client.identity.get_client_versions(
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

**id:** `str` — The Vectros-assigned UUID of the client whose history you want.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">list_orgs</a>(...) -> OrgPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of organizations in your account. Filter by `userId` to return only the organizations owned by a specific user, or by `externalId` for an exact lookup using your own identifier. You can also query schema-declared lookup fields by supplying `type` and `field` together with one lookup mode (`value` for equality, `from`/`to` for a range, or `prefix`). Requires the `orgs:r` scope.
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

client.identity.list_orgs(
    user_id="550e8400-e29b-41d4-a716-446655440000",
    external_id="clinic_001",
    start_from="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    type="org_v1",
    field="region",
    value="us-east",
    prefix="us-",
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

**user_id:** `typing.Optional[str]` — Return only organizations owned by this user, given as the Vectros-assigned ID (UUID) of a user. To resolve a user ID from your own identifier, call `GET /v1/users?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**external_id:** `typing.Optional[str]` — Look up an organization by your own identifier. Returns a list with the single matching organization, or an empty list if none matches.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `typing.Optional[int]` — Maximum number of organizations to return per page (1–100; defaults to 20).
    
</dd>
</dl>

<dl>
<dd>

**type:** `typing.Optional[str]` — The schema record type whose lookup fields you want to query. Must be supplied together with `field` and exactly one lookup mode (`value`, `from`/`to`, or `prefix`).
    
</dd>
</dl>

<dl>
<dd>

**field:** `typing.Optional[str]` — The schema-declared lookup field to filter on. Must be one of the lookup fields defined on the schema named by `type`. Supplied together with `type` and one lookup mode.
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match for `field` (equality lookup). Mutually exclusive with `from`/`to` and `prefix`. Not allowed for a sensitive field — use `POST /v1/orgs/lookup` instead so the value is not exposed in the URL.
    
</dd>
</dl>

<dl>
<dd>

**from:** `typing.Optional[str]` — Inclusive lower bound for a range lookup. Requires `to`, and is only supported on non-sensitive fields that have range lookups enabled. Mutually exclusive with `value` and `prefix`.
    
</dd>
</dl>

<dl>
<dd>

**to:** `typing.Optional[str]` — Inclusive upper bound for a range lookup. Requires `from`.
    
</dd>
</dl>

<dl>
<dd>

**prefix:** `typing.Optional[str]` — Match all values for `field` that start with this prefix. Only supported on string fields that have range lookups enabled. Mutually exclusive with `value` and `from`/`to`.
    
</dd>
</dl>

<dl>
<dd>

**order:** `typing.Optional[ListOrgsRequestOrder]` — Sort direction for lookup results: `asc` (ascending, the default) or `desc` (descending).
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">create_org</a>(...) -> OrgResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new organization in your account. This call is idempotent on `externalId`: if an organization with the same `externalId` already exists, the existing record is returned instead of creating a duplicate. Requires the `orgs:c` scope.
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

client.identity.create_org(
    external_id="clinic_001",
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

**request:** `OrgRequest` 
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_org</a>(...) -> OrgResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves a single organization by its Vectros-assigned ID, returning its current name, status, payload, and schema binding. Requires the `orgs:r` scope.
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

client.identity.get_org(
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

**id:** `str` — The Vectros-assigned ID (UUID) of the organization to retrieve.
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">update_org</a>(...) -> OrgResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates the mutable fields of an organization. Omitted fields are preserved (a null value does not clear a field), and the `payload` object is replaced in full when supplied rather than deep-merged. Requires the `orgs:u` scope.
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

client.identity.update_org(
    id="id",
    external_id="clinic_001",
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

**request:** `OrgRequest` 
    
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">delete_org</a>(...)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes an organization. This action cannot be undone. Requires the `orgs:d` scope.
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

client.identity.delete_org(
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">lookup_orgs</a>(...) -> OrgPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Looks up organizations by a schema-declared field value, with the search criteria sent in the request body instead of the URL. Use this when looking up by a sensitive field: the value travels in the body and is never exposed in the URL. This is the body-based equivalent of the `type`/`field`/`value` lookup on `GET /v1/orgs`, which rejects sensitive-field values and directs you here. Returns a `{data, nextCursor}` envelope. Requires the `orgs:r` scope.
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

client.identity.lookup_orgs(
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

<details><summary><code>client.identity.<a href="src/vectros/identity/client.py">get_org_versions</a>(...) -> ModelDataVersionPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the audit trail of changes made to an organization, newest first. Version history is always recorded for identity entities, and sensitive field values are redacted in the history. Requires the `orgs:r` scope.
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

client.identity.get_org_versions(
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

**id:** `str` — The Vectros-assigned ID (UUID) of the organization whose history you want.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value from the previous page to fetch the next page of history.
    
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

Returns a paginated list of the users in your account. Pass `externalId` to look up a single user by your own identifier. To filter on schema-declared lookup fields, supply `type` and `field` together with one lookup mode: `value` (exact match), `from`+`to` (range), or `prefix`. Requires the `users:r` scope.
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
    start_from="550e8400-e29b-41d4-a716-446655440000",
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

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
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

Creates a user identity in your account. The operation is idempotent on `externalId`: if a user with the same `externalId` already exists, the existing record is returned instead of creating a duplicate. Requires the `users:c` scope.
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

Updates mutable fields on an existing user (such as email, status, payload, or schema binding). The `type` field is immutable after creation. This endpoint also activates an invited user: a PUT that moves a PENDING user to ACTIVE and carries `inviteToken`, `externalSubject`, and `emailVerifiedAttestation=true` completes the invitation. Requires the `users:u` scope.
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

Permanently deletes a user identity. This cannot be undone. If the user is a pending invitation, the associated access profile created for that invitation is also removed. Requires the `users:d` scope.
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

Looks up users by a schema lookup field, with the query criteria carried in the request body rather than the URL. Use this when looking up by a sensitive (blind-indexed) field: the value is blind-indexed server-side and never appears in the URL, request logs, or proxies. The query semantics are identical to the GET /v1/users lookup, which rejects sensitive-field values and directs you here. Returns a page in the `{data, nextCursor}` envelope. Requires the `users:r` scope.
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

## Documents
<details><summary><code>client.documents.<a href="src/vectros/documents/client.py">list_documents</a>(...) -> DocumentPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of your documents, optionally filtered by folder (`folderId`) and/or owner (`userId`, `orgId`, or `clientId`). The response is a `{data, nextCursor}` envelope; pass `nextCursor` back as `startFrom` to fetch the next page. Requires the `documents:r` scope.
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
    org_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    client_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    folder_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    start_from="doc_prev123",
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

**org_id:** `typing.Optional[str]` — Filter by owning organization — the Vectros-assigned UUID of an organization. To resolve from your own identifier, call GET /v1/orgs?externalId=.
    
</dd>
</dl>

<dl>
<dd>

**client_id:** `typing.Optional[str]` — Filter by associated client — the Vectros-assigned UUID of a client. To resolve from your own identifier, call GET /v1/clients?externalId=.
    
</dd>
</dl>

<dl>
<dd>

**folder_id:** `typing.Optional[str]` — List only documents in this folder (the Vectros folder ID). Can be combined with the owner filters.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor — pass the `nextCursor` returned by the previous page.
    
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

Creates a document from a raw text string and queues it for asynchronous indexing so it becomes searchable. Requires the `documents:c` scope.
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

Partially updates a document using an RFC 7386 JSON Merge Patch. The `payload` object is deep-merged: keys you send overwrite existing values (recursing into nested objects), a key set to `null` is deleted, and keys you omit are preserved — unlike PUT, which replaces the whole payload. Top-level fields (`title`, `storeText`, `folderId`, `schemaId`, ownership) are set when present and left unchanged when omitted; sending a top-level field as `null` is rejected. Supplying `text` re-ingests the document body (same as PUT). `indexMode` and `externalId` are immutable and rejected if present. The merged result is validated against the bound schema. Pass `expectedVersion` for optimistic concurrency (409 on conflict). Requires the `documents:u` scope.
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

Finds documents of a given type by a schema-declared lookup field. The document must be bound to a schema (via `schemaId`) that declares the field as a lookup field. A lookup on a sensitive field is rejected here, because the value would appear in the URL query string; use POST /v1/documents/lookup (the request-body variant) for a sensitive field instead. Results are paginated: set `limit` for the page size and feed the returned `nextCursor` back as `startFrom` to fetch the next page. The response is a `{data, nextCursor}` envelope. Requires the `documents:r` scope.
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
    start_from="550e8400-e29b-41d4-a716-446655440000",
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

**field:** `str` — The name of the lookup field declared on the schema.
    
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

**start_from:** `typing.Optional[str]` — Pagination cursor — pass the `nextCursor` returned by the previous page.
    
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

**field:** `str` — Name of a lookup field declared on the schema. For a sensitive field, this body variant is required (the GET variant rejects sensitive-field lookups).
    
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

Returns the full extracted or ingested text body for documents that were stored with `storeText=true`. Returns 404 when the document does not exist or when no text is available (because `storeText` was false, or extraction has not yet completed). Requires the `documents:r` scope.
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

Starts a file-based document by returning a short-lived presigned S3 PUT URL. Upload the file bytes directly to `uploadUrl`; the document is then automatically queued for text extraction and asynchronous indexing. Requires the `documents:c` scope.
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

**index_mode:** `typing.Optional[FileUploadRequestIndexMode]` — Indexing strategy applied after the file is processed and its text is extracted. `HYBRID` runs both BM25 keyword and dense-vector semantic indexing (recommended). `SEMANTIC` indexes only as dense vectors. `TEXT` indexes only with BM25. `NONE` is store-only (archival): the file is still uploaded and its text extracted, but it is not search-indexed — retrievable by id/download and structured-field lookup only. Optional: omit to inherit the bound schema's default index mode. If neither this field nor the schema specifies one, the request is rejected. When both are set, this per-file value wins.
    
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

**user_id:** `typing.Optional[str]` — Owning user ID — the Vectros-assigned UUID of a user in your account. Optional. With an API key, sets the document's owner explicitly. With a scoped token, must match the token's identity claim (if set) or fall within its data scope.
    
</dd>
</dl>

<dl>
<dd>

**org_id:** `typing.Optional[str]` — Owning organization ID — the Vectros-assigned UUID of an organization in your account. Optional. With an API key, sets the document's owning organization explicitly. With a scoped token, must match the token's identity claim (if set) or fall within its data scope.
    
</dd>
</dl>

<dl>
<dd>

**client_id:** `typing.Optional[str]` — Associated client ID — the Vectros-assigned UUID of a client in your account. Optional. With an API key, sets the document's client explicitly. With a scoped token, must match the token's identity claim (if set) or fall within its data scope.
    
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

## Compliance
<details><summary><code>client.compliance.<a href="src/vectros/compliance/client.py">create_erasure_request</a>(...) -> ErasureRequestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Submits a right-to-erasure request for a single end-subject (a user, client, or organization). Erasure removes exactly the data the subject solely owns across the declared contexts, plus the subject's identity and lookup rows. It never touches another account's data and never cascades into another subject's data. The request is asynchronous: it returns 202 with a `requestId`; poll `GET /v1/erasure-requests/{id}` until the job completes to obtain the completion certificate. Requires a root API key — a scoped credential is rejected with 403.
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

**subject_type:** `ErasureRequestSubjectType` — The kind of end-subject to erase: `user`, `client`, or `org`.
    
</dd>
</dl>

<dl>
<dd>

**subject_id:** `typing.Optional[str]` — The platform id of the subject to erase (the userId, clientId, or orgId returned when the subject was created). Provide exactly one of `subjectId` or `externalId`.
    
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

**subject_type:** `typing.Optional[ExportRequestSubjectType]` — The kind of subject to export: `user`, `client`, or `org`. Required only when `scope` is `subject`.
    
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

Returns a paginated list of your folders. Pass `parentFolderId` to list the direct children of a specific folder (tree navigation); omit it for a flat list across your account. You can also filter by owner using `userId`, `orgId`, or `clientId`. Results are returned as a `{data, nextCursor}` envelope — pass `nextCursor` as `startFrom` to fetch the next page. Requires the `folders:r` scope.
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
    org_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    user_id="550e8400-e29b-41d4-a716-446655440000",
    client_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    start_from="fld_prev123",
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

**org_id:** `typing.Optional[str]` — Filter to folders owned by this organization — the Vectros-assigned UUID of an organization. Use `GET /v1/orgs?externalId=` to resolve your own external ID to this UUID.
    
</dd>
</dl>

<dl>
<dd>

**user_id:** `typing.Optional[str]` — Filter to folders owned by this user — the Vectros-assigned UUID of a user. Use `GET /v1/users?externalId=` to resolve your own external ID to this UUID.
    
</dd>
</dl>

<dl>
<dd>

**client_id:** `typing.Optional[str]` — Filter to folders associated with this client — the Vectros-assigned UUID of a client. Use `GET /v1/clients?externalId=` to resolve your own external ID to this UUID.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` value from the previous page to fetch the next page; omit it for the first page.
    
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

Creates a folder to organize your documents and records. If `parentFolderId` is omitted, the folder is created under your context's default root folder. Requires the `folders:c` scope.
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

Partially updates a folder using an RFC 7386 JSON Merge Patch. The `name`, `description`, and ownership fields (`userId`, `orgId`, `clientId`) are applied when present and left unchanged when omitted; sending any of these as null is rejected, because clearing a field is not supported in this release (omit it instead). `slug` and `parentFolderId` are immutable — a folder cannot be re-slugged or moved via the API — and the request is rejected if either is present. Pass `expectedVersion` for optimistic concurrency (you get a 409 if the folder changed since you last read it). Requires the `folders:u` scope.
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

Returns a paginated list of records in your account as a `{data, nextCursor}` page. Supply exactly one of `type`, `folderId`, or `recent=true` to choose the mode: `type` lists all records of a single type; `folderId` lists all records in a folder (any type); and `recent=true` returns the account-wide recently-updated feed across all types, newest first. You may combine `type` with `folderId` to list a single type within a folder. The owner filters (`userId`, `orgId`, `clientId`) further narrow the type and folder modes; the `recent` feed is standalone and ignores all filters. Each token only sees the record types it is scoped to read. Requires the `records:r` scope. By default the response returns the indexed projection of each record; set `includePayload=true` to include full payloads.
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
    org_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    client_id="f47ac10b-58cc-4372-a567-0e02b2c3d479",
    start_from="550e8400-e29b-41d4-a716-446655440000",
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

**org_id:** `typing.Optional[str]` — Filter to records owned by this organization. The value is the Vectros-assigned UUID of an organization; resolve one via `GET /v1/orgs?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**client_id:** `typing.Optional[str]` — Filter to records owned by this client (requires `userId` or `orgId` as well). The value is the Vectros-assigned UUID of a client; resolve one via `GET /v1/clients?externalId=`.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` returned by the previous page to fetch the next page; omit it for the first page.
    
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

Creates a new record of a given type. The `payload` is validated against that type's schema before the record is stored. Identify the type by sending `typeName`, `schemaId`, or both (they must agree); if you send only `schemaId`, the type is taken from that schema. Optionally supply an `externalId` to make the create idempotent — if a record with the same `externalId` already exists in your context, that existing record is returned unchanged instead of a duplicate being created. Requires the `records:c:<type>` scope.
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

Partially updates a record using an RFC 7386 JSON Merge Patch. The `payload` object is deep-merged into the existing payload: keys you send overwrite (recursing into nested objects), a key set to null is deleted, and keys you omit are left unchanged — so you can change a single field without re-sending the rest (unlike the full-replacement PUT). Top-level fields (`status`, `folderId`, `userId`, `orgId`, `clientId`) are set when present and left unchanged when omitted; sending a top-level field as null is rejected (clearing a top-level field is not supported in this release — omit it instead). `typeName`, `schemaId`, `externalId`, and `indexMode` are immutable and rejected if present. The merged result is validated against the schema. Pass `expectedVersion` to make the patch conditional (optimistic concurrency, 409 on conflict). Requires the `records:u:<type>` scope.
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

<details><summary><code>client.records.<a href="src/vectros/records/client.py">lookup_records</a>(...) -> RecordLookupResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Finds records by the value of a lookup field declared on the type's schema. Provide exactly one lookup mode: `value` (exact match), `from`+`to` (inclusive range, ascending by value), or `prefix` (string fields only, ascending). Range and prefix lookups are not supported on a sensitive field, because its value is stored as a blind index and has no sortable order. An exact-`value` lookup on a sensitive field is also rejected on this GET endpoint — the value must not appear in the URL — so use the `POST /v1/records/lookup` body variant for sensitive fields. Results are paginated: set `limit` for the page size and pass the returned `nextCursor` back as `startFrom` for the next page. Requires the `records:r:<type>` scope.
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
    prefix="jane",
    start_from="550e8400-e29b-41d4-a716-446655440000",
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

**value:** `typing.Optional[str]` — Exact value to match. Mutually exclusive with `from`/`to` and `prefix`. Rejected for a sensitive field — use `POST /v1/records/lookup` instead so the value is not exposed in the URL.
    
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

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page; omit it for the first page.
    
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

<details><summary><code>client.records.<a href="src/vectros/records/client.py">lookup_records_by_body</a>(...) -> RecordLookupResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Body-based equivalent of `GET /v1/records/lookup`. Use this when looking up by a sensitive field: the value travels in the request body (and is blind-indexed server-side) instead of in the URL query string, so it never lands in access, CDN, or proxy logs. The GET variant rejects an exact-value lookup on a sensitive field and directs you here. Non-sensitive exact-value, range (`from`+`to`), and prefix lookups also work here. Returns the same `{data, nextCursor}` envelope and uses the same pagination as the GET variant. Requires the `records:r:<type>` scope.
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

**field:** `str` — Name of the lookup field declared on the schema. For a field marked sensitive, this body variant is required (the GET variant rejects looking up by a sensitive value).
    
</dd>
</dl>

<dl>
<dd>

**value:** `typing.Optional[str]` — Exact value to match. Mutually exclusive with `from`/`to` and `prefix`. Sensitive fields can only be looked up by exact value, and only through this body variant.
    
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

Returns a paginated list of the record schemas defined in your account. Filter by `userId` or `orgId` to scope to an owner, by `surface` to list the types bindable to one surface, or by `recordType` to resolve the single schema for a type directly. Filtering by an identity surface (user, org, or client) lists your account-wide identity schemas regardless of the calling context; filtering by record or document lists within the calling context. Requires the `schemas:r` scope.
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
    org_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    surface="document",
    record_type="intake_form",
    start_from="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
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

**org_id:** `typing.Optional[str]` — Filter to schemas owned by this organization — the Vectros-assigned UUID of an organization in your account. Use `GET /v1/orgs?externalId=` to resolve a UUID from your own external id.
    
</dd>
</dl>

<dl>
<dd>

**surface:** `typing.Optional[str]` — Filter to schemas bindable to this surface: record, document, user, org, or client. Returns only schemas whose allowed surfaces include the given one — useful for listing, say, document types separately from record types. The identity surfaces (user, org, client) are account-wide: filtering by one lists your account's identity schemas regardless of the calling context, whereas record and document list within the calling context.
    
</dd>
</dl>

<dl>
<dd>

**record_type:** `typing.Optional[str]` — Resolve the single schema for this record type — the natural handle for a schema, and the direct alternative to remembering its opaque id. Returns a one-element page, or an empty page if no such schema exists. Resolved in the calling context for record and document types; combine with `surface=user`, `org`, or `client` to resolve an account-wide identity schema. Mutually exclusive with `userId`/`orgId` — when supplied, `recordType` takes precedence.
    
</dd>
</dl>

<dl>
<dd>

**start_from:** `typing.Optional[str]` — Pagination cursor. Pass the `nextCursor` from the previous page to fetch the next page; omit it for the first page.
    
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

Defines a new record type with optional field definitions, validation rules, and lookup indexes. Idempotent by `typeName` within the same ownership scope: re-creating an existing `typeName` returns the existing schema rather than failing. Requires the `schemas:w` scope.
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

Updates a record schema. Fields you omit are preserved; `typeName` is immutable and cannot be changed. Collection fields (`fields`, `lookupFields`, `renderHints`, `capabilities`) are replaced in full when supplied. Requires the `schemas:w` scope.
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

Permanently deletes a record schema. The request is refused with 409 if records of this type still exist — delete those records first, since every record must reference a live schema. Requires the `schemas:w` scope.
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

**org_id:** `typing.Optional[str]` — Restrict results to content belonging to this organization — the Vectros-assigned UUID of an organization in your account. Use `GET /v1/orgs?externalId=` to look up an organization's ID from your own identifier.
    
</dd>
</dl>

<dl>
<dd>

**client_id:** `typing.Optional[str]` — Restrict results to content associated with this client — the Vectros-assigned UUID of a client in your account. Use `GET /v1/clients?externalId=` to look up a client's ID from your own identifier.
    
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

