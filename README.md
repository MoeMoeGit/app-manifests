# App Manifests

Machine-readable runtime manifests shared by multiple applications.

## Layout

Each application owns one versioned manifest:

```text
apps/<app-id>/manifest-v1.json
```

The manifest schema is defined in `schemas/manifest-v1.schema.json`.

## Maintenance

Use the repository helper so identifiers, dates, ordering, revisions, and
duplicate entries are checked consistently:

```bash
./scripts/manage add cckit <device-code> 2027-12-31
./scripts/manage update cckit <device-code> 2028-12-31
./scripts/manage remove cckit <device-code>
./scripts/manage check
```

Review and commit the resulting diff before publishing:

```bash
git diff
git add apps/cckit/manifest-v1.json
git commit -m "data: update cckit manifest"
./scripts/publish
```

`scripts/publish` expects remotes named `github` and `gitee`. It validates all
manifests, pushes the same `main` commit to both hosts, and verifies that both
remote branches point at the same commit.
