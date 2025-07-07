Modele bazy danych
~~~~~~~~~~~~~~~~~~

Konceptualny
^^^^^^^^^^^^

Encje i ich atrybuty:

- Kandydat:
	
	- PESEL - PK (klucz główny),
	
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

Aplikacja jest encją słabą, nie posiada własnego klucza. Jest identyfikowana na podstawie encji Kandydat, od której zależy jej istnienie. Przyjmujemy, że każdy kandydat składa tylko jedną aplikację.

.. przedstawić w notacji Chena

Logiczny
^^^^^^^^

.. figure:: diagramy/barker_erd.png
	
	Schemat ERD w notacji Barkera

Tabela przed normalizacją:

	Rekrutacja - wszystkie dane w jednej tabeli, wszystkie encje w jednej krotce

Model logiczny - pierwotna tabela zostaje podzielona na kilka innych połączonych kluczami. Tabele po normalizacji:

	Kandydaci - PESEL to klucz główny

	Wydziały - IDwydzialu to klucz główny

	Aplikacje - PESEL jest kluczem zarówno głównym, jak i obcym, IDwydzialu to klucz obcy

Jest to trzecia postać normalna, ponieważ:

- dane są atomowe - każda komórka zawiera tylko jedną wartość (1NF)

- wszystkie atrybuty niekluczowe są zależne od klucza potencjalnego (2NF),

- nie ma zależności przechodnich (3NF).

Fizyczny
^^^^^^^^

.. implementacja z użyciem SQLite

.. z użyciem PostgreSQL
