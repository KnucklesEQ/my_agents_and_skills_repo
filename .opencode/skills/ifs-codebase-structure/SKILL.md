---
name: ifs-codebase-structure
description: "Use for initial navigation of local IFS Core code under Core_Files, to understand the recurring folder structure without remapping the whole tree: version folders, checkout roots, modules, model/source/server areas, and common artifact locations."
---

# IFS Codebase Structure

## Purpose

Use this skill as a reusable folder-structure map for local IFS Core code under `Core_Files/`.

Its purpose is to avoid remapping the recurring folder layout at the start of each session. It describes where things usually live, not how IFS behavior, Marble syntax, PL/SQL conventions, projections, or product semantics work.

Do not perform broad recursive scans just to rediscover the structure described here. Inspect concrete files only when the user's request requires evidence.

## Scope

The source root covered by this skill is relative to the current workspace:

```text
Core_Files/
```

This skill covers local IFS Core layer code only. Other IFS layers (Base, Customization, Configuration) are outside its scope.

## Version Roots

Version folders live directly under `Core_Files/` and use this format:

```text
Core_Files/<major>.<minor>.<patch>/
Core_Files/<major>.<minor>.<patch>/checkout/
```

Examples:

```text
Core_Files/24.2.13/checkout/
Core_Files/25.1.3/checkout/
```

`checkout/` is the functional root of an IFS Core version.

Root handling:

- `Core_Files/`: inspect only immediate children to identify version folders.
- One version: use `Core_Files/<version>/checkout/`.
- Multiple versions: do not choose automatically; ask if one version is needed. Some tasks may compare versions.
- `Core_Files/<version>/`: use `Core_Files/<version>/checkout/`.
- `Core_Files/<version>/checkout/`: treat directly as the functional root.
- Missing `checkout/`: say so and ask for clarification.

## Functional Root

The functional root contains module folders plus a small number of repository-level files.

```text
Core_Files/<version>/checkout/<module>/
```

Example module folders include `fndbas/`, `appsrv/`, `enterp/`, `invent/`, `order/`, `proj/`, `purch/`, `discom/`, `shpmnt/`, `payled/`, `genled/`, `wo/`, `person/`, and `docman/`.

This is not a complete module inventory. Available modules must be checked in the selected version's `checkout/` folder.

Module names are structural identifiers, not behavior evidence.

## Module Layout

Most functional modules use this layout:

```text
<module>/
├── deploy.ini
├── manualdeploy/
├── model/
├── server/
├── source/
└── test/
```

Not every module has every folder. The repeated module-name pattern is expected:

```text
<module>/model/<module>/
<module>/source/<module>/
```

## Special Module: fndbas

`fndbas` is a special foundational module and should not be used as the representative example for normal business-module structure.

Observed `fndbas` layout can include extra framework, build, install, and template folders:

```text
fndbas/
├── build/
├── deploy.ini
├── ifsinstaller/
├── ifsinstallerupdater/
├── manualdeploy/
├── model/
├── nobuild/
├── server/
├── source/
├── template/
└── test/
```

It still has primary paths such as `fndbas/model/fndbas/` and `fndbas/source/fndbas/`.

## Model Area

Primary path:

```text
<module>/model/<module>/
```

Primary area for Marble and IFS model artifacts.

Common extensions: `.entity`, `.client`, `.projection`, `.fragment`, `.utility`, `.enumeration`, `.overview`, `.report`, `.dimension`, `.filter`.

Example paths, not required file pairs:

```text
order/model/order/CustomerOrderHandling.projection
order/model/order/CustomerOrder.entity
```

## Source Areas

Primary path:

```text
<module>/source/<module>/
```

Common subareas: `database/`, `projection/`, `replication/`, `workflow/`.

### database/

```text
<module>/source/<module>/database/
```

Primary area for PL/SQL, database definitions, service files, views, storage, reports, install scripts, and database-changing scripts.

Common extensions: `.plsql`, `.plsvc`, `.storage`, `.views`, `.ins`, `.sql`, `.cre`, `.upg`, `.cdb`, `.rdf`, `.apv`.

Examples: `order/source/order/database/CustomerOrder.plsql`, `order/source/order/database/CustomerOrder.views`, `order/source/order/database/CustomerOrdersHandling.plsvc`.

Notes: `.upg` and `.cdb` are database scripts; `.rdf` is report-related; subfolders such as `BIServices/` can appear under `database/`.

### projection/

```text
<module>/source/<module>/projection/
<module>/source/<module>/projection/packages/<ProjectionName>/...
```

`.projection` model files usually live under:

```text
<module>/model/<module>/
```

They are normally not stored under `source/<module>/projection/`.

`source/<module>/projection/` may contain projection implementation artifacts, commonly under `packages/`. Some integration-oriented projections may have Java code here, but do not assume Java is the default implementation path.

Example: `order/source/order/projection/packages/CustomerOrdersHandling/com/ifsworld/projection/CustomerOrdersHandlingActionsImpl.java`.

### replication/

```text
<module>/source/<module>/replication/
```

Common replication-related naming includes `RSP` and `RRP`.

Examples: `order/source/order/replication/CustomerOrderRSP.plsql`, `order/source/order/replication/CustomerOrderRRP.plsql`.

### workflow/

```text
<module>/source/<module>/workflow/
<module>/source/<module>/workflow/bpmn/
```

When present, `bpmn/` is the observed area for workflow/BPMN artifacts, commonly `.bpmn` files.

### Less Common Source Areas

Some modules contain additional source areas:

```text
<module>/source/<module>/client/
<module>/source/<module>/webapp/
<module>/source/<module>/fsmigtool/
```

These areas are structurally valid but not the usual Marble model or PL/SQL database locations. Examples observed locally include `webapp/` in modules such as `appsrv` and `gisint`, and `fsmigtool/` in `appsrv`. In particular, `.client` model files normally live under `<module>/model/<module>/`, not under `<module>/source/<module>/client/`.

## Server Area

```text
<module>/server/
```

Common server areas include reports and lobby assets:

```text
<module>/server/reports/
<module>/server/lobby/
<module>/server/reports/layouts/
<module>/server/reports/schemas/
<module>/server/lobby/datasources/
<module>/server/lobby/elements/
<module>/server/lobby/pages/
```

Some lobby XML files may also appear directly under `server/lobby/`.

Secondary server areas can include:

```text
<module>/server/translation/
<module>/server/connect_config/
<module>/server/profiles/
```

Server areas are structurally valid, but they are not the primary Marble model or PL/SQL database areas.

## Reports Across Areas

Report-related artifacts can appear in several structural areas:

```text
<module>/model/<module>/*.report
<module>/source/<module>/database/*.rdf
<module>/server/reports/layouts/*.rdl
<module>/server/reports/schemas/*.xsd
```

`.report` files are model artifacts. `.rdf` files commonly appear under `source/<module>/database/`. `.rdl` and `.xsd` files commonly appear under `server/reports/`.

## Secondary Areas

These paths are secondary for normal Marble model and PL/SQL database navigation:

```text
build/
nobuild/
template/
ifsinstaller/
ifsinstallerupdater/
manualdeploy/
test/
```

They may still matter when the user's request involves deployment, build, installation, tests, manual deployment, upgrades, migration, release, support updates, or lifecycle behavior.

## Canonical Path Summary

Use this summary as a compact reminder of recurring locations, not as an exhaustive inventory.

Primary structural areas:

```text
<module>/model/<module>/
<module>/source/<module>/database/
```

Other recurring structural areas:

```text
<module>/source/<module>/replication/
<module>/source/<module>/projection/
<module>/source/<module>/workflow/
<module>/server/reports/
<module>/server/lobby/
```

This list is a structural map, not a mandatory search workflow.

## Boundary

This skill describes local folder structure only. It does not define IFS functional behavior, Marble syntax, PL/SQL conventions, projection semantics, or product documentation rules.

For concrete implementation guidance, inspect the relevant local files.
