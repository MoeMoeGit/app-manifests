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

## Application resources

Optional application resource data lives at `apps/<app-id>/resources-v1.json`. The public envelope schema is `schemas/resources-v1.schema.json`. Run `./scripts/check-resources` before committing. Publishing also checks tracked paths and the committed envelopes.

## Application updates

Public update metadata lives at `apps/<app-id>/update-v1.json`; its schema is `schemas/update-v1.schema.json`. It contains only the latest stable semantic version, publication time, and the matching release page. It must not contain credentials or private release assets.

After a stable release is published, update the file in one change:

1. Increase `revision`.
2. Set `version` without a leading `v`.
3. Copy the release publication time as UTC and make `release_url` end in the matching `v<version>` tag.
4. Run `./scripts/check-resources`, review and commit the diff, then run `./scripts/publish` so Gitee and GitHub expose the same revision.

Clients treat Gitee as the primary source and GitHub as a mirror. Publishing the application release without publishing this manifest means existing clients will not discover that release.
