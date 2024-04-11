# OLTP_project

### Case Study

ToysGroup è un’azienda che distribuisce articoli (giocatoli) in diverse aree geografiche del mondo.
I prodotti sono classificati in categorie e i mercati di riferimento dell’azienda sono classificati in regioni di vendita.
In particolare:

1)	Le entità individuabili in questo scenario sono le seguenti:
-	Product
-	Region
-	Sales

2) Le relazioni tra le entità possono essere descritte nel modo seguente:
   
  -	Product e Sales:	Un prodotto puo’ essere venduto tante volte (o nessuna) per cui è contenuto in una o più transazioni di vendita.
                        Ciascuna transazione di vendita è riferita ad uno solo prodotto
     	
-	Region e Sales: Possono esserci molte o nessuna transazione per ciascuna regione
                      Ciascuna transazione di vendita è riferita ad una sola regione

3)	Le entità Product e Region presentano delle gerarchie:

   -	L’entità prodotto contiene, oltre alle informazioni del singolo prodotto, anche la descrizione della categoria di appartenenza. L’entità prodotto contiene quindi una gerarchia: un prodotto puo’ appartenere         ad una sola categoria mentre la stessa categoria puo’ essere associata a molti prodotti diversi.
Esempio: gli articoli ‘Bikes-100’ e ‘Bikes-200’ appartengono alla categoria Bikes; gli articoli ‘Bike Glove M’ e ‘Bike Gloves L’ sono classificati come Clothing.
-	L’entità regione contiene una gerarchia: più stati sono classificati in una stessa regione di vendita e una stessa regione di vendita include molti stati.
Esempio: gli stati ‘France’ e ‘Germany’ sono classificati nella region WestEurope; gli stati ‘Italy’ e ‘Greece’ sono classificati nel mercato SouthEurope

È necessario progettare e implementare fisicamente un database che modelli lo scenario garantendo l’integrità referenziale e la minimizzazione della ridondanza dei dati.
In altre parole, progetta opportunamente un numero di tabelle e di relazioni tra queste sufficiente a garantire la consistenza del dato.

### PROGETTAZIONE LOGICA E CONCETTUALE DELLA BASE DATI:

#### PROGETTAZIONE CONCETTUALE:

![alt text](https://github.com/simonepetrini/OLTP_project/blob/main/Modello%20ER%20-%20Progettazione%20Concettuale.JPG?raw=True)

#### PROGETTAZIONE LOGICA:

![alt text](https://github.com/simonepetrini/OLTP_project/blob/main/Progettazione%20Logica.png?raw=True)

### IMPLEMENTAZIONE FISICA DELLE TABELLE (TRAMITE IL DBMS SQL SERVER):
```sql
CREATE DATABASE ToysGroup;
USE ToysGroup;

--- Creazione delle tabelle delle dimensioni ---
CREATE TABLE REGIONS
(
RegionID INT NOT NULL,
RegionArea nvarchar(30),
RegionMacroArea nvarchar(30)
CONSTRAINT PK_REGIONS PRIMARY KEY (RegionID)
);

CREATE TABLE STATES
(
StateID INT NOT NULL,
StateIntCode char(2),
StateName nvarchar(50),
RegionID INT
CONSTRAINT PK_STATES PRIMARY KEY (StateID),
CONSTRAINT FK_STATES_REGIONS FOREIGN KEY (RegionID) REFERENCES REGIONS(RegionID)
);

CREATE TABLE PRODUCT_CATEGORY
(
ProductCategoryID INT NOT NULL,
ProductCategoryName nvarchar(50)
CONSTRAINT PK_PRODUCT_CATEGORY PRIMARY KEY (ProductCategoryID)
);

CREATE TABLE PRODUCT
(
ProductKey INT NOT NULL,
ProductName nvarchar(50),
ProductCategoryID INT,
ListPrice decimal (10,2),
StandardCost decimal (10,2),
Size nvarchar(50),
Weight decimal(5,1),
Color nvarchar(50),
Description nvarchar(200)
CONSTRAINT PK_PRODUCTS PRIMARY KEY (ProductKey),
CONSTRAINT FK_PRODUCTS_PRODUCT_CATEGORY FOREIGN KEY (ProductCategoryID) REFERENCES PRODUCT_CATEGORY(ProductCategoryID)
);

--- Creazione della tabella dei fatti ---
CREATE TABLE SALES
(
OrderNumber INT NOT NULL,
OrderLine tinyint NOT NULL,
ProductKey INT,
Quantity INT,
UnitPrice decimal (10,2),
SalesAmount decimal (10,2),
OrderDate date,
SalesStateID INT
CONSTRAINT PK_SALES PRIMARY KEY (OrderNumber,OrderLine),
CONSTRAINT FK_SALES_PRODUCTS FOREIGN KEY (ProductKey) REFERENCES PRODUCT(ProductKey),
CONSTRAINT FK_SALES_STATES FOREIGN KEY (SalesStateID) REFERENCES STATES(StateID)
);
```
### FASE DI CARICAMENTO DATI:

```sql
--- Tabella REGIONS ---
INSERT INTO REGIONS (RegionID, RegionArea, RegionMacroArea)
VALUES 
(1,'South Europe','Europe'),
(2,'North Europe','Europe'),
(3,'East Europe','Europe'),
(4,'West Europe','Europe'),
(5,'North America','America'),
(6,'Latin America','America'),
(7,'Middle East','Asia'),
(8,'South Asia','Asia'),
(9,'South East Asia','Asia'),
(10, 'Far East','Asia'),
(11,'North Africa','Africa'),
(12,'Central Africa','Africa'),
(13,'West Africa','Africa'),
(14,'Southern Africa','Africa'),
(15, 'Oceania', 'Oceania');

--- Tabella STATES ---
INSERT INTO STATES (StateID,StateIntCode,StateName,RegionID)
VALUES
(1,'IT','Italy',1),
(2,'ES','Spain',1),
(3,'GR','Greece',1),
(4,'NO','Norway',2),
(5,'SE','Sweden',2),
(6,'RU','Russia',3),
(7,'RO','Romania',3),
(8,'GB','United Kingdom',4),
(9,'DE','Germany',4),
(10,'FR','France',4),
(11,'NL','Netherlands',4),
(12,'US','United States of America',5),
(13,'CA','Canada',5),
(14,'MX','Mexico',5),
(15,'BR','Brazil',6),
(16,'AR','Argentina',6),
(17,'SA','Saudi Arabia',7),
(18,'AE','United Arab Emirates',7),
(19,'IL','Israel',7),
(20,'ID','Indonesia',9),
(21,'PH','Phillippines',9),
(22,'IN','India',8),
(23,'PK','Pakistan',8),
(24,'CN','China',10),
(25,'JP','Japan',10),
(26,'KR','South Korea',10),
(27,'EG','Egypt',11),
(28,'CM','Camerun',12),
(29,'GH','Ghana',13),
(30,'ZA','South Africa',14),
(31,'AU','Australia',15),
(32,'NZ','New Zealand',15);

--- Tabella PRODUCT_CATEGORY ---
INSERT INTO PRODUCT_CATEGORY (ProductCategoryID,ProductCategoryName)
VALUES
(1, 'Dolls'),
(2, 'Educational'),
(3, 'Blocks'),
(4, 'Vehicles'),
(5, 'Outdoor'),
(6, 'Board Games'),
(7, 'Consoles'),
(8, 'Plush Toys'),
(9, 'Artistic and Creative'),
(10, 'Role-Playing'),
(11, 'Robotics'),
(12, 'Collectable');

--- Tabella PRODUCT ---
(1,'Barbie Dreamhouse Playset',1,239.99,190.00,'28.35x69.69x42.91',23.8,'Pink','A comprehensive playset that includes a Barbie doll, furniture, and accessories for creating imaginative scenarios in a dollhouse setting'),
(2,'LeapFrog LeapStart Interactive Learning System',2,132.74,112.56,'4,75x24,51x28,7',1.48,'White','An educational system that engages children in reading, math, and other subjects through interactive books and activities'),
(3,'LEGO Creator Expert Taj Mahal',3,505.67,368.15,'58,5x19x49',7.13,'Black','A highly detailed LEGO set that allows kids and adults to build a miniature version of the iconic Taj Mahal'),
(4,'Hot Wheels Super Ultimate Garage Playset',4,196.23,163.99,'20,32x76,2x86,36',1.00,'Red','A large playset featuring a multi-level garage, race tracks, and various features for playing with Hot Wheels cars'),
(5,'Little Tikes Totsports Easy Hit Golf Set',5,33.90,28.50,'54,61x26,67x19,05',1.78,'Red','A child-friendly golf set designed for outdoor play, including colorful clubs, balls, and a mini-golf course'),
(6,'Settlers of Catan',6,39.90,31.25,'30x30x7',1.31,'Blue','A popular strategy board game where players collect resources to build roads, settlements, and cities on the fictional island of Catan'),
(7,'Nintendo Switch Lite',7,209.99,159.60,'19.5x35.3x9',1.11,'Grey','A handheld gaming console from Nintendo that provides a portable and compact gaming experience for kids'),
(8,'Disney Frozen Elsa Plush Doll',8,25.90,17.85,'10x11x20',0.27,'Blue','A soft and huggable plush doll featuring Elsa, the popular character from Disneys Frozen'),
(9,'Crayola Inspiration Art Case',9,28.95,21.30,'39.4x5.8x28',1.50,'Black','An art set containing a variety of Crayola crayons, colored pencils, markers, and drawing paper for creative expression'),
(10,'Melissa & Doug Fire Chief Role Play Costume Set',10,74.58,65.00,'44x61.5x5',0.77,'Red','A firefighter costume set for kids, including a jacket, helmet, badge, fire extinguisher, and other accessories for imaginative play'),
(11,'LEGO Mindstorms EV3 Robot Kit',11,399.99,286.75,'11x7.5x4.5',0.18,'Grey','A robotics kit that allows users to build and program their own customizable robots using LEGO components'),
(12,'Funko Pop! Marvel Avengers Endgame - Iron Man',12,26.30,21.35,'5x3.81x10.16',0.32,'Red','A collectible vinyl figure from the Funko Pop! series featuring Iron Man in a stylized and iconic design');

--- Tabella SALES ---
INSERT INTO SALES (OrderNumber,OrderLine,ProductKey,Quantity,UnitPrice,SalesAmount,OrderDate,SalesStateID)
VALUES
(00001,1,7,1,209.99,209.99,'2023-12-28',24),
(00001,2,2,1,132.74,132.74,'2023-12-28',24),
(00001,3,4,1,196.23,196.23,'2023-12-28',24),
(00002,1,8,1,25.90,25.90,'2024-01-05',1),
(00002,2,9,2,28.95,57.90,'2024-01-05',1),
(00003,1,3,1,505.67,505.67,'2024-01-06',13),
(00004,1,12,2,26.30,52.60,'2024-01-08',19),
(00004,2,10,1,74.58,74.58,'2024-01-08',19),
(00005,1,1,2,239.99,479.98,'2024-01-12',23),
(00006,1,11,1,399.99,399.99,'2024-01-1',2),
(00006,2,5,1,33.90,33.90,'2024-01-15',2),
(00006,3,6,1,39.90,39.90,'2024-01-15',2),
(00007,1,1,1,239.99,239.99,'2024-01-21',9),
(00007,2,7,1,209.99,209.99,'2024-01-21',9),
(00007,3,4,1,196.23,196.23,'2024-01-21',9),
(00007,4,3,1,505.67,505.67,'2024-01-21',9),
(00008,1,5,1,33.90,33.90,'2024-01-25',20),
(00009,1,12,2,26.30,52.60,'2024-01-28',28),
(00009,2,8,1,25.90,25.90,'2024-01-28',28),
(00010,1,2,1,132.74,132.74,'2024-01-28',31),
(00010,2,4,1,196.23,196.23,'2024-01-28',31),
(00010,3,7,1,209.99,209.99,'2024-01-28',31);
```
Dopo la creazione e l'implementazione del database, il committente richiede di rispondere alla seguenti richieste:

- Fornire una prova della consistenza del DB creato.

```sql
--- Per dimostrare la consistenza del DB, verifico che i campi definiti come Primary Keys siano univoci ---
SELECT ProductKey FROM PRODUCT GROUP BY ProductKey HAVING COUNT(*)>1;
SELECT ProductCategoryID FROM PRODUCT_CATEGORY GROUP BY ProductCategoryID HAVING COUNT(*)>1;
SELECT RegionID FROM REGIONS GROUP BY RegionID HAVING COUNT(*)>1;
SELECT OrderNumber,OrderLine FROM SALES GROUP BY OrderNumber,OrderLine HAVING COUNT(*)>1;
SELECT StateID FROM STATES GROUP BY StateID HAVING COUNT(*)>1;
```
![alt text](https://github.com/simonepetrini/OLTP_project/blob/images/Query1.png?raw=True)

- Esporre l’elenco delle transazioni indicando nel result set il codice documento, la data, il nome del prodotto, la categoria del prodotto, il nome dello stato, il nome della regione di vendita e un campo che indichi che siano passati più di 180 giorni dalla data vendita o meno

```sql
```

- Esporre l’elenco dei soli prodotti venduti e per ognuno di questi il fatturato totale per anno

```sql
```

- Esporre il fatturato totale per stato per anno

```sql
```

- Qual è la categoria di articoli maggiormente richiesta dal mercato?

```sql
```

- Quali sono, se ci sono, i prodotti invenduti?

```sql
```

- Esporre l’elenco dei prodotti cona la rispettiva ultima data di vendita

```sql
```

- Esporre l’elenco dei prodotti cona la rispettiva ultima data di vendita

```sql
```
