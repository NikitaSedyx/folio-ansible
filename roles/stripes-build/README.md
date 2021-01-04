# stripes-build
This role builds the stripes frontend bundle. Optionally use with the stripes-docker role to serve the webpack from a Docker container.

## Basic usage
Below is a playbook illustrating how to build the frontend from the snapshot branch of platform-complete.
```yml
---
- hosts: all
  roles:
    - role: stripes-build
      stripes_github_project: https://github.com/folio-org/platform-complete
      stripes_github_version: snapshot
      platform_remove_lock: false
```

## Defaults
```yml
---
# Use a preconfigured github stripes platform
with_github: true
stripes_github_project: https://github.com/folio-org/platform-complete
stripes_github_version: HEAD

# set use_refspec to true if specifying a refspec instead of a branch
from_refspec: false
# if from_refspec is true, specifiy stripes_github_refspec
#stripes_github_refspec:
#
# Example using refspec and github:
#
#    stripes_github_refspec: pull/23/head
#    stripes_github_version: pr-23

platform_remove_lock: false

# OR specify stripes configuration here and configure from ansible templates
stripes_core_version: "latest"
stripes_components_version: "latest"
stripes_modules:
  - { name: "@folio/users", version: "latest" }
  - { name: "@folio/items", version: "latest" }
  - { name: "@folio/scan", version: "latest" }
  - { name: "@folio/trivial", version: "latest" }
  - { name: "@folio/tenant-settings", version: "latest" }
  - { name: "@folio/developer", version: "latest" }
  - { name: "@folio/plugin-markdown-editor", version: "latest" }
  - { name: "@folio/plugin-markdown-better", version: "latest" }
  - { name: "@folio/plugin-find-user", version: "latest" }

# disable okapi-console - https://issues.folio.org/browse/STRIPES-264
#  - { name: "@folio/okapi-console", version: "^0.0.1-test" }

# Other relevant vars
stripes_conf_dir: /etc/folio/stripes
stripes_okapi_port: 9130
disable_auth: false
stripes_okapi_url: "http://{{ ansible_default_ipv4.address }}:{{ stripes_okapi_port }}"
# for when the Okapi URL for building is different than for production
stripes_build_okapi_url: "{{ stripes_okapi_url }}"
stripes_tenant: diku
with_sourcemap: true
node_environment:
  NODE_ENV: production
build_webpack: true
build_module_descriptors: true

with_translations: false
webpack_languages: 'en-US,en-GB,da-DK,de-DE,hu-HU'

# Register module descriptors with Okapi, default false
okapi_register_modules: false
# Enable modules for tenant, default false
okapi_enable_modules: false
# Generate strict module descriptors (with requirements), default false
stripes_strict_md: false

# NPM repository settings
folio_npm_base_url: repository.folio.org/repository/
folio_npm_repo: npm-folio

# Use existing stripes platform yarn.lock
use_folio_snapshot: false
folio_snapshot_url: 'https://folio-snapshot-stable.dev.folio.org'

#
# Disabled by default
#
npm_proxy: false
npm_authtoken: ''
```
