# Database Schema Changes

!!! info "This section of the documentation is intended for developers"

[EF Core database migrations](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/) are used to allow existing Confab databases to be updated alongside the database schema (as new features are added in future Confab versions). This page covers the database migration steps that should be performed any time the database schema is changed.

Provided commands below should be run in the Visual Studio Package Manager Console (the .NET Core CLI can also be used).

## Creating a Migration

A new database migration should be created after any database schema changes. Replace `MigrationName` with a descriptive name for this migration.

```

Add-Migration MigrationName

```