Migracja bazy danych
~~~~~~~~~~~~~~~~~~~~

Z SQLite do PostgreSQL
^^^^^^^^^^^^^^^^^^^^^^

Proces migracji należy rozpocząć od wykonania kopii zapasowej bazy, na wypadek utraty danych. Następnie należy utworzyć nową bazę danych:

.. code-block:: sql

	CREATE DATABASE migrated_db;

Bazę danych SQLite należy wyeksportować jako plik SQL:

.. code-block:: bash

	sqlite3 sqlite_base.db .dumb > sqlite_base.sql

Może zajść potrzeba konwersji typów danych. Plik można wprowadzić do bazy PostgreSQL z użyciem psql:

.. code-block:: bash

	psql -U username -d migrated_db -f sqlite_base.sql

Migrację można także przeprowadzić za pomocą narzędzia pgloader:

.. code-block:: bash

	pgloader sqlite:///katalog/sqlite_base.db postgresql:///migrated_db

Z PostgreSQL do SQLite
^^^^^^^^^^^^^^^^^^^^^^

Dane trzeba wyeksportować w przenośnym formacie:

.. code-block:: bash

	pg_dump --data-only --column-inserts --no-owner --no-privileges postgresql_base > postgresql_base.sql

	pg_dump --schema-only postgresql_base > schema.sql

Należy przekonwertować typy danych w schemacie na takie, jakie są wspierane przez SQLite. Można to wykonać ręcznie, lub z zastosowaniem odpowiednich programów. Następnie trzeba utworzyć nową bazę danych SQLite:

.. code-block:: bash

	sqlite3 migrated_base.db

Wczytujemy poprawioną schemę:

.. code-block:: bash

	.read schema.sql

Wprowadzamy dane:

.. code-block:: bash

	sqlite3 migrated_base.db < postgresql_base.sql
