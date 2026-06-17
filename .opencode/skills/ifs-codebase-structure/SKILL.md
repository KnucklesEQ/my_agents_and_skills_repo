---
name: ifs-codebase-structure
description: Use when working with local IFS Core source folders under Core_Files to understand the stable codebase structure, version roots, checkout roots, modules, Marble model files, PL/SQL database files, projection implementation areas, workflows, reports, lobby assets, and secondary source areas.
---

# IFS Codebase Structure

Use this skill as the baseline structural map for local IFS Core source code.

The goal is to avoid repeatedly remapping the same folder structure at the start of each session. Treat this as structural context only. It does not replace inspecting concrete files when a specific implementation question requires evidence.

Do not perform broad recursive scans just to rediscover this structure. Use this skill as the known baseline, and inspect only the concrete paths needed for the user's current request.

Do not use this skill for official online IFS documentation research. This skill describes local Core folder structure only.

## Scope

The relevant local source area is:

```text
Core_Files/
```

The code under this folder is IFS Core layer code. Other IFS layers are outside the scope of this skill unless the user explicitly provides them.

## Root Layout

Local IFS Core code is expected under:

```text
Core_Files/
```

Inside `Core_Files/`, one or more version folders may exist:

```text
Core_Files/<version>/
```

Each version folder is expected to contain a fixed `checkout/` folder:

```text
Core_Files/<version>/checkout/
```

The `checkout/` folder is the functional root of the IFS Core code.

Examples:

```text
Core_Files/24.2.13/checkout/
Core_Files/25.1.3/checkout/
```

## Root Normalization

If the user provides `Core_Files/`, inspect only the immediate children to identify version folders.

If exactly one version folder exists, use:

```text
Core_Files/<version>/checkout/
```

If multiple version folders exist, ask the user which version to use.

If the user provides `Core_Files/<version>/`, use:

```text
Core_Files/<version>/checkout/
```

If the user provides `Core_Files/<version>/checkout/`, treat it directly as the functional root.

If no `checkout/` folder exists under the selected version, say so and ask for clarification.

## Functional Root

The functional root contains module folders and a small number of repository-level files.

Pattern:

```text
Core_Files/<version>/checkout/<module>/
```

Representative modules:

```text
fndbas/
appsrv/
enterp/
invent/
order/
proj/
purch/
discom/
shpmnt/
payled/
genled/
wo/
person/
docman/
```

Module names are structural identifiers. Do not rely on module names alone as proof of behavior.

## Common Module Layout

Most functional modules use this pattern:

```text
<module>/
├── deploy.ini
├── manualdeploy/
├── model/
├── server/
├── source/
└── test/
```

Not every module has every folder. Some modules may omit `test/`, `server/`, or other optional areas.

The repeated module-name pattern is expected:

```text
<module>/model/<module>/
<module>/source/<module>/...
```

## FNDBAS

`fndbas` is a special foundational module.

The name is commonly explained as a contraction of "Framework Based Functionality" or "IFS Base Functionality".

It contains broad framework/base functionality used throughout IFS. All IFS components make extensive use of `fndbas` and have it as a static dependency.

Do not treat `fndbas` as a normal representative business module when inferring common module structure or implementation patterns.

Observed `fndbas` layout can include extra framework, build, install, and template areas:

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

When investigating `fndbas` specifically, it still has its own primary paths such as `fndbas/model/fndbas/` and `fndbas/source/fndbas/...`.

## Marble And IFS Model Files

Primary model path:

```text
<module>/model/<module>/
```

This is the main location for Marble and IFS model artifacts.

Common extensions:

```text
.entity
.client
.projection
.fragment
.utility
.enumeration
.overview
.report
.dimension
.filter
```

Operational report definitions can appear as model artifacts, for example with `.report` files.

Representative examples:

```text
order/model/order/CustomerOrderHandling.projection
order/model/order/CustomerOrder.client
order/model/order/CustomerOrder.entity
order/model/order/CanBeFinalized.filter
```

## PL/SQL And Database Logic

Primary database path:

```text
<module>/source/<module>/database/
```

This is the main location for PL/SQL, database definitions, service files, views, storage, reports, install scripts, and database-changing scripts.

Common extensions:

```text
.plsql
.plsvc
.storage
.views
.ins
.sql
.cre
.upg
.cdb
.rdf
.apv
```

Representative examples:

```text
order/source/order/database/CustomerOrder.plsql
order/source/order/database/CustomerOrder.storage
order/source/order/database/CustomerOrder.views
order/source/order/database/CustomerOrdersHandling.plsvc
```

`.upg` and `.cdb` files are important database scripts. They may modify database structure or data and should not be dismissed as irrelevant when the user's question involves database changes, upgrades, support updates, or data model evolution.

`.rdf` files are related to operational reports and commonly appear under `source/<module>/database/`.

The `database/` area may also contain secondary subfolders. For example, `BIServices/` can appear under `database/` and is related to PowerBI/BI services.

## Replication

Replication path:

```text
<module>/source/<module>/replication/
```

Common replication-related naming includes `RSP` and `RRP`.

Example pattern:

```text
order/source/order/replication/CustomerOrderRSP.plsql
order/source/order/replication/CustomerOrderRRP.plsql
```

## Projection Source Implementation

Projection source path:

```text
<module>/source/<module>/projection/
```

Important nuance: `.projection` files usually live under:

```text
<module>/model/<module>/
```

The `source/<module>/projection/` area may contain generated or hand-written implementation code, commonly under package folders.

Observed pattern:

```text
<module>/source/<module>/projection/packages/<ProjectionName>/...
```

Representative Java implementation example:

```text
order/source/order/projection/packages/CustomerOrdersHandling/com/ifsworld/projection/CustomerOrdersHandlingActionsImpl.java
```

## Workflow

Workflow path:

```text
<module>/source/<module>/workflow/
```

Observed substructure:

```text
<module>/source/<module>/workflow/bpmn/
```

## Server Areas

Server path:

```text
<module>/server/
```

Primary server areas commonly include reports and lobby assets:

```text
<module>/server/reports/
<module>/server/lobby/
```

Server report subfolders commonly include:

```text
<module>/server/reports/layouts/
<module>/server/reports/schemas/
```

Lobby subfolders commonly include:

```text
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

These secondary areas exist but are not the main Marble or PL/SQL code locations.

## Secondary Or Less Common Source Areas

Some modules contain additional source areas outside the primary `database/`, `replication/`, `projection/`, and `workflow/` paths.

Observed secondary or less common examples:

```text
<module>/source/<module>/client/
<module>/source/<module>/webapp/
<module>/source/<module>/fsmigtool/
```

Examples observed locally include `source/<module>/client/` in a small number of modules, `webapp/` in modules such as `appsrv` and `gisint`, and `fsmigtool/` in `appsrv`.

These areas are structurally valid but not the usual Marble or PL/SQL locations.

## Secondary Areas

The main areas of interest are usually Marble model files and PL/SQL/database code.

Treat these paths as secondary unless the user asks about deployment, build, installation, generated assets, tests, manual deployment, web assets, upgrades, or migration:

```text
build/
nobuild/
template/
ifsinstaller/
ifsinstallerupdater/
manualdeploy/
test/
```

Do not claim these folders are useless. They are simply not the primary Marble or PL/SQL code locations.

Upgrade, migration, release, support-update, and installer scripts may be important when the user's question involves database structure, data changes, deployment, or lifecycle behavior.

## Structural Summary

The primary structural areas are:

```text
<module>/model/<module>/
<module>/source/<module>/database/
<module>/source/<module>/replication/
<module>/source/<module>/projection/
<module>/source/<module>/workflow/
<module>/server/reports/
<module>/server/lobby/
```

This list is a structural map, not a mandatory search workflow.

## Scope Boundary

This skill describes local folder structure only.

It does not define IFS functional behavior, Marble syntax, PL/SQL conventions, projection semantics, or product documentation rules.

For concrete implementation guidance, inspect the relevant local files.
