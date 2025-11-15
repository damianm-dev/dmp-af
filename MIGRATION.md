# Migration Guide: dbt-af to dmp-af

This guide helps you migrate from the original [dbt-af](https://github.com/Toloka/dbt-af) project to this fork, dmp-af.

## Table of Contents

1. [Overview](#overview)
2. [Why Migrate?](#why-migrate)
3. [Migration Steps](#migration-steps)
    - [Update Package Installation](#1-update-package-installation)
    - [Update Import Statements](#2-update-import-statements)
    - [Update Function Calls](#3-update-function-calls)
    - [Update Class Names](#4-update-class-names)
    - [Update Configuration Variables](#5-update-configuration-variables)
    - [Update CLI Scripts](#6-update-cli-scripts)
    - [Update Documentation References](#7-update-documentation-references)
    - [Update dbt Project Configuration](#8-update-dbt-project-configuration)
4. [Testing Your Migration](#testing-your-migration)
5. [Breaking Changes](#breaking-changes)
6. [Compatibility](#compatibility)
7. [Support](#support)
8. [Staying with dbt-af](#staying-with-dbt-af)
9. [Changelog](#changelog)

## Overview

**dmp-af** is a fork of dbt-af (version 0.14.6) with ongoing enhancements and modifications by `dmp-labs`. The
core functionality remains the same, but package names and identifiers have been updated.

## Why Migrate?

- Active maintenance and development
- New features and improvements
- Community-driven enhancements
- Same Apache 2.0 license

## Migration Steps

### 1. Update Package Installation

Simply replace `dbt-af` with `dmp-af` in your dependencies:

```diff
# requirements.txt
- dbt-af
+ dmp-af

# pyproject.toml
[project]
dependencies = [
-   "dbt-af",
+   "dmp-af",
]
```

Then reinstall:

```bash
pip install -U dmp-af
```

### 2. Update Import Statements

Update the imports in your Airflow DAG files from `dbt_af` to `dmp_af`:

```diff
- from dbt_af.dags import compile_dbt_af_dags
- from dbt_af.conf import Config, DbtDefaultTargetsConfig, DbtProjectConfig
+ from dmp_af.dags import compile_dmp_af_dags
+ from dmp_af.conf import Config, DbtDefaultTargetsConfig, DbtProjectConfig
```

**Key imports to update:**

```diff
- from dbt_af.dags import compile_dbt_af_dags
+ from dmp_af.dags import compile_dmp_af_dags
- from dbt_af.conf import Config
+ from dmp_af.conf import Config
- from dbt_af.conf import DbtDefaultTargetsConfig
+ from dmp_af.conf import DbtDefaultTargetsConfig
- from dbt_af.conf import DbtProjectConfig
+ from dmp_af.conf import DbtProjectConfig
```

### 3. Update Function Calls

The main DAG compilation function has been renamed:

```diff
- dags = compile_dbt_af_dags(
+ dags = compile_dmp_af_dags(
    manifest_path='/path/to/manifest.json',
    config=config,
)
```

### 4. Update Class Names

Several class names have been updated:

```diff
# Graph builder
- from dbt_af.builder import DbtAfGraph
+ from dmp_af.builder import DmpAfGraph

# Maintenance config
- from dbt_af.parser.dbt_node_model import DbtAFMaintenanceConfig
+ from dmp_af.parser.dbt_node_model import DmpAfMaintenanceConfig
```

### 5. Update Configuration Variables

If you have any custom configuration or variable names:

```diff
# Variable names
- dbt_af_config = Config(...)
+ dmp_af_config = Config(...)

- dbt_af_graph = DbtAfGraph.from_manifest(...)
+ dmp_af_graph = DmpAfGraph.from_manifest(...)
```

### 6. Update CLI Scripts

If you're using the manifest tests script:

```diff
- dbt-af-manifest-tests --manifest_path=./target/manifest.json
+ dmp-af-manifest-tests --manifest_path=./target/manifest.json
```

### 7. Update Documentation References

Update any documentation or comments in your codebase:

```diff
- # Using dbt-af for distributed dbt runs
+ # Using dmp-af for distributed dbt runs

- See https://github.com/Toloka/dbt-af for details
+ See https://github.com/dmp-labs/dmp-af for details
```

### 8. Update dbt Project Configuration

Your `dbt_project.yml` configuration remains the same - no changes needed:

```yaml
# These settings work with both dbt-af and dmp-af
models:
  sql_cluster: "dev"
  daily_sql_cluster: "dev"
  py_cluster: "dev"
  bf_cluster: "dev"
```

## Testing Your Migration

After migrating, test that your DAGs are built correctly:

```bash
# Test your DAG file compiles successfully
python your_dmp_af_dag.py
```

If the script runs without errors, your DAGs are correctly configured. Then:

1. **Check Airflow DAGs:**
    - Ensure DAGs appear in Airflow UI
    - Verify DAG structure looks correct
    - Test a sample DAG run

## Breaking Changes

### From dbt-af 0.14.6 to dmp-af

**Package Renaming:**

- Package name: `dbt-af` → `dmp-af`
- Python module: `dbt_af` → `dmp_af`
- Main function: `compile_dbt_af_dags()` → `compile_dmp_af_dags()`
- Main class: `DbtAfGraph` → `DmpAfGraph`
- Config class: `DbtAFMaintenanceConfig` → `DmpAfMaintenanceConfig`

**Behavioral Changes:**

- None - all functionality remains identical

**Configuration Changes:**

- None - all configuration options remain the same

**Version Numbering:**

- dmp-af will continue version numbering from dbt-af (starting at 0.14.6)

## Compatibility

- **Python**: 3.10, 3.11, 3.12, 3.13
- **Airflow**: 2.6.3+, tested up to 2.11.0
- **dbt-core**: 1.7-1.10

## Support

If you encounter issues during migration:

1. Check this guide for common scenarios
2. Review the [examples](examples/README.md) directory
3. Open an issue on [GitHub](https://github.com/dmp-labs/dmp-af/issues)
