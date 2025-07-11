Projekt bazy danych
===================

Opis bazy danych
----------------

Baza danych przechowuje informacje związane z procesem rekrutacyjnym uczelni. Główna tabela zawiera listę kandydatów ubiegających się o przyjęcie, wraz z ich danymi osobowymi, kontaktowymi, średnią oceną z matur oraz wybranym wydziałem.

Modele bazy danych
------------------

Konceptualny
^^^^^^^^^^^^

Encje i ich atrybuty:

- Kandydat:
	
	- pesel - PK (klucz główny),
	
	- imie,
	
	- nazwisko,
	
	- telefon,
	
	- kodpocztowy,
	
	- sredniamaturalna,

- Aplikacja:
	
	- pesel - FK (klucz obcy),
	
	- idwydzialu - FK,
	
	- datarekrutacji,

	- statusaplikacji,

- Wydział:
	
	- idwydzialu - PK
		
	- nazwawydzialu.

Związki:

- Kandydat - Aplikacja (1:1),

- Aplikacja - Wydział (N:1).

Aplikacja jest encją słabą, nie posiada własnego klucza. Jest identyfikowana na podstawie encji Kandydat, od której zależy jej istnienie. Przyjmujemy, że każdy kandydat składa tylko jedną aplikację.

.. przedstawić w notacji Chena

Logiczny
^^^^^^^^

.. figure:: diagramy/barker_erd.png
	
	Schemat ERD w notacji Barkera

Tabela przed normalizacją

	Rekrutacja - wszystkie dane w jednej tabeli, wszystkie encje w jednej krotce

Model logiczny - pierwotna tabela zostaje podzielona na kilka innych połączonych kluczami. Tabele po normalizacji:

	Kandydaci - pesel to klucz główny
	
	Wydziały - idwydzialu to klucz główny
	
	Aplikacje - pesel jest kluczem zarówno głównym, jak i obcym, IDwydzialu to klucz obcy

Jest to trzecia postać normalna, ponieważ:

- dane są atomowe - każda komórka zawiera tylko jedną wartość (1NF)

- wszystkie atrybuty niekluczowe są zależne od klucza potencjalnego (2NF),

- nie ma zależności przechodnich (3NF).

Fizyczny
^^^^^^^^

Implementacja z użyciem SQLite:

.. code-block:: sql

	CREATE TABLE Kandydat (
	    pesel TEXT PRIMARY KEY,
	    imie TEXT NOT NULL,
	    nazwisko TEXT NOT NULL,
	    kodpocztowy TEXT,
	    telefon TEXT,
	    sredniamaturalna REAL
	);
	
	CREATE TABLE Wydzial (
	    idwydzialu INTEGER PRIMARY KEY,
	    nazwawydzialu TEXT NOT NULL
	);
	
	CREATE TABLE Aplikacja (
	    pesel TEXT,
	    idwydzialu INTEGER,
	    datarekrutacji TEXT NOT NULL,
	    statusaplikacji TEXT,
	
	    PRIMARY KEY (pesel),
	    FOREIGN KEY (pesel) REFERENCES Kandydat(pesel),
	    FOREIGN KEY (idwydzialu) REFERENCES Wydzial(idwydzialu)
	);

Z użyciem PostgreSQL:

.. code-block:: sql

	CREATE TABLE Kandydat (
	    pesel CHAR(11) PRIMARY KEY,
	    imie VARCHAR(100) NOT NULL,
	    nazwisko VARCHAR(100) NOT NULL,
	    kodpocztowy CHAR(6),
	    telefon VARCHAR(20),
	    sredniamaturalna NUMERIC(4, 2)
	);
	
	CREATE TABLE Wydzial (
	    idwydzialu SERIAL PRIMARY KEY,
	    nazwawydzialu VARCHAR(255) NOT NULL
	);
	
	CREATE TABLE Aplikacja (
	    pesel CHAR(11),
	    idwydzialu INTEGER,
	    datarekrutacji DATE NOT NULL,
	    statusaplikacji VARCHAR(30),
	
	    PRIMARY KEY (pesel),
	    FOREIGN KEY (pesel) REFERENCES Kandydat(pesel) ON DELETE CASCADE ON UPDATE CASCADE,
	    FOREIGN KEY (idwydzialu) REFERENCES Wydzial(idwydzialu) ON DELETE RESTRICT ON UPDATE CASCADE
	);

Różnice między zastosowanymi typami danych wynikają z ograniczeń SQLite w zakresie wspieranych typów danych. SQLite nie wspiera również CASCADE.

Opis danych przechowywanych w bazie
-----------------------------------

W obu przypadkach rozmiar zbioru danych pozostaje taki sam; 300 kandydatów, 300 aplikacji (po jednej na kandydata) i 8 wydziałów. Każdy kandydat ma przypisaną średnią ocenę z egzaminów maturalnych.

SQLite
^^^^^^

Podsumowanie kandydatów na wydziałach:

+-------------+-------------------+-----------------------+
| Wydział     | Liczba kandydatów | Średnia ocena z matur |
+=============+===================+=======================+
| Fizyka      | 45                | 68.2                  |
+-------------+-------------------+-----------------------+
| Medycyna    | 43                | 72.8                  |
+-------------+-------------------+-----------------------+
| Matematyka  | 43                | 72.9                  |
+-------------+-------------------+-----------------------+
| Prawo       | 39                | 69.5                  |
+-------------+-------------------+-----------------------+
| Biologia    | 38                | 68.7                  |
+-------------+-------------------+-----------------------+
| Ekonomia    | 34                | 69.3                  |
+-------------+-------------------+-----------------------+
| Filologia   | 30                | 69.1                  |
+-------------+-------------------+-----------------------+
| Informatyka | 28                | 71.4                  |
+-------------+-------------------+-----------------------+

.. image:: diagramy/chart_sqlite.png
	:width: 600
	:height: 450

PostgreSQL
^^^^^^^^^^

Podsumowanie kandydatów na wydziałach:

+-------------+-------------------+-----------------------+
| Wydział     | Liczba kandydatów | Średnia ocena z matur |
+=============+===================+=======================+
| Informatyka | 47                | 70.1                  |
+-------------+-------------------+-----------------------+
| Biologia    | 45                | 70.4                  |
+-------------+-------------------+-----------------------+
| Ekonomia    | 39                | 65.0                  |
+-------------+-------------------+-----------------------+
| Fizyka      | 38                | 69.8                  |
+-------------+-------------------+-----------------------+
| Medycyna    | 37                | 66.6                  |
+-------------+-------------------+-----------------------+
| Filologia   | 35                | 69.7                  |
+-------------+-------------------+-----------------------+
| Matematyka  | 33                | 67.4                  |
+-------------+-------------------+-----------------------+
| Prawo       | 26                | 73.1                  |
+-------------+-------------------+-----------------------+


.. image:: diagramy/chart_postgresql.png
	:width: 600
	:height: 450
