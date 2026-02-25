# Griffino-Plugins-Submit

The official submission and review repository for Griffino plugins.

All plugins published to the [Griffino-Plugins](https://github.com/GriffinGuard/Griffino-Plugins) registry — including official GriffinGuard plugins — must go through the review process in this repository. There are no privileged submission paths.

## Submission Process

1. Fork this repository
2. Create a directory under `submissions/` named after your plugin ID (e.g. `submissions/com.yourname.pluginname/`)
3. Add the required files (see [Required Files](#required-files))
4. Open a pull request — CI will run automatically
5. Wait for Maintainer review
6. Once approved and merged, the CD pipeline will build your image and publish the plugin to the registry

## Required Files

```
submissions/{pluginId}/
  submission.json         ← Submission metadata (see .example/)
  plugin.manifest.json    ← Plugin declaration file
  plugin.boot.yml         ← Container runtime configuration
  config.boot.json        ← Admin configuration schema
  config.user.json        ← User configuration schema (optional)
  i18n/                   ← Localization files (optional)
```

## Plugin ID Convention

Plugin IDs must follow the reverse-domain format:

```
com.{authorName}.{pluginName}
```

Examples: `cc.griffino.telegrambot`, `com.morcherlf.videotool`

## Image Requirements

- **Auxiliary images** (third-party pre-built): must be listed in the [approved image whitelist](https://github.com/GriffinGuard/Griffino-Plugins/blob/main/approved-images.json). If your plugin requires an image not on the list, CI will flag it and a Maintainer will review it as part of the PR.
- **Main service image**: built automatically by the CD pipeline from your source repository at the specified commit. The image will be published to `ghcr.io/griffinguard/{pluginId}:{version}`.

## Review Criteria

Maintainers will check the following before approving a PR:

- `permissionsRequested` matches actual code behavior
- No abuse of RabbitMQ or Redis ACL permissions
- Capability roles and types are appropriate
- Any unapproved auxiliary images are safe and from reputable sources
- The source repository builds successfully at the specified commit

## CI Checks

Every PR triggers automatic checks:

- `submission.json` format validation
- `plugin.manifest.json` format validation
- Plugin ID consistency between files
- Required files presence check
- Auxiliary image whitelist verification

## MAINTAINERS

See [MAINTAINERS](./MAINTAINERS) for the list of reviewers.
