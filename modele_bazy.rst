Modele bazy danych
~~~~~~~~~~~~~~~~~~

Konceptualny
^^^^^^^^^^^^

Encje i ich atrybuty:

- Kandydat:
	
	- PESEL - PK (klucz podstawowy),
	
	- imie,
	
	- nazwisko,
	
	- telefon,
	
	- kodpocztowy,
	
	- sredniamaturalna,

- Aplikacja:
	
	- PESEL - FK (klucz obcy),
	
	- IDwydzialu - FK,
	
	- datarekrutacji,

	- statusaplikacji,

- Wydział:
	
	- IDwydzialu - PK
		
	- nazwawydzialu.

Związki:

- Kandydat - Aplikacja (1:1),

- Aplikacja - Wydział (N:1).

Aplikacja jest encją słabą, nie posiada własnego klucza. Jest identyfikowana na podstawie encji Kandydat, od której zależy jej istnienie.

.. przedstawić w notacji Chena

Logiczny
^^^^^^^^

.. schemat ERD, np. w notacji Barkera

.. figure:: diagramy/barker_erd.png
	
	Schemat ERD w notacji Barkera

.. normalizacja

Tabela przed normalizacją:

	Rekrutacja - (Pesel... ) - wszystkie dane w jednej tabeli, wszystkie encje w jednej krotce

Model logiczny - pierwotna tabela zostaje podzielona na kilka innych połączonych kluczami. Tabele po normalizacji:

	Kandydaci - Klucz główny - PESEL

	Wydziały - Klucz główny - wydzial

	Aplikacje - Klucz obcy - PESEL -> Kandydat(PESEL) - krotka: PESEL, wydzial, datarekrutacji, statusaplikacji

Fizyczny
^^^^^^^^

implementacja z użyciem SQLite

z użyciem PostgreSQL
