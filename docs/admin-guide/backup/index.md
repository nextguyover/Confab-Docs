# Backing Up Confab

Regular backups ensure that if anything goes wrong with your Confab instance, whether that be recovering from a large number of [spam comments](../abuse-mitigation/index.md#comment-spam) or the database gets corrupted, you can easily roll back your instance to it's state at a previous point in time.

To fully backup a Confab instance, the following files/directories must be backed up:

- `Confab/appsettings.json` → Your backend configuration file
- `Confab/Database/*` → The database files that store all comment data (1)
    { .annotate }

    1. This is the default database location, and can be customised in the [backend configuration](../../config/index.md#database-location)

To restore from a previous backup, simply do a fresh Confab install, then copy the above files to their original locations.