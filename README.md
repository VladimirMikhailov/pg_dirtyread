pg_dirtyread 1.0
================

[Forked From pg_dirtyread](https://github.com/omniti-labs/pgtreats)

The pg_dirtyread extension provides the ability to read dead but unvacuumed
rows from a relation.

Homebrew
-------

`
  brew tap VladimirMikhailov/pgtreats
  brew install pg_dirtyread
`

Using
-----

  ```sql
    CREATE EXTENSION pg_dirtyread;

    # Create table and disable autovacuum
    CREATE TABLE versions (id INTEGER PRIMARY KEY, text TEXT);
    ALTER TABLE versions SET (
      autovacuum_enabled = false, toast.autovacuum_enabled = false
    );

    INSERT INTO versions VALUES (1, 'Test'), (2, 'New Test');
    DELETE FROM versions WHERE id = 1;

    SELECT * FROM pg_dirtyread('versions'::regclass) AS t(id INTEGER, text TEXT);
  ```

Manual Installation
--------

To build pg_dirtyread, just do this:

    make
    make install

If you encounter an error such as:

    make: pg_config: Command not found

Be sure that you have `pg_config` installed and in your path. If you used a
package management system such as RPM to install PostgreSQL, be sure that the
`-devel` package is also installed. If necessary tell the build process where
to find it:

    env PG_CONFIG=/path/to/pg_config make && make install
