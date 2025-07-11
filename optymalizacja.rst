EXPLAIN
~~~~~~~

W PostgreSQL wykonano 3 zapytania: 

.. code-block:: sql

	EXPLAIN ANALYZE
	SELECT * FROM Kandydat;

.. code-block:: sql

	EXPLAIN ANALYZE
	SELECT * FROM Kandydat
	WHERE imie LIKE '%Adam%';

.. code-block:: sql

	EXPLAIN ANALYZE
	SELECT * FROM Kandydat
	WHERE nazwisko LIKE '%wski%';

Wyniki pomiarów czasu:

+-----------+-----------------+----------------+-------------+
| Zapytanie | Czas planowania | Czas wykonania | Czas według |
|           | według EXPLAIN  | według EXPLAIN | \timing     |
+===========+=================+================+=============+
| 1         | 0.141 ms        | 0.108 ms       | 1.101 ms    |
+-----------+-----------------+----------------+-------------+
| 2         | 0.296 ms        | 0.133 ms       | 1.169 ms    |
+-----------+-----------------+----------------+-------------+
| 3         | 0.186 ms        | 0.117 ms       | 0.848 ms    |
+-----------+-----------------+----------------+-------------+


- 0,952 ms,

- 10,881 ms,

- 0,762 ms.

Ponieważ zestaw danych liczy jedynie 300 rekordów, brak indeksów nie stanowi problemu, tabela może zostać przeszukana w całości z użyciem Seq Scan. W tym przypadku optymalizacja nie stanowi konieczności. Założenie indeksów będzie konieczne dla tabel liczących tysiące lub miliony rekordów, gdyż czas przeszukiwania takiej tabeli w całości będzie nieakceptowalny.
