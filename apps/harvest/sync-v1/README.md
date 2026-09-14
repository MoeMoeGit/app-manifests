# Harvest encrypted sync v1

Harvest owns this directory. It does not use `manifest-v1.json`, CCKit authorization, or leases.

The desktop app appends one immutable JSON envelope containing a full syncable-business snapshot per device and sequence under:

```text
devices/<random-device-uuid>/<12-digit-sequence>.json
```

Each envelope exposes only protocol metadata and encrypted bytes. The payload is encrypted with AES-256-GCM; its key is derived from the user-provided shared password with Argon2id. Platform viewer cookies and the password are never included.

Do not edit, rename, or delete an existing pack. Imports are deduplicated by source device and sequence; rows with the same primary key use their update timestamp when available. This is not a multi-writer conflict resolver, so Harvest v1 requires a single active registration operator. Recovery is performed by restoring Git history and synchronizing again.
