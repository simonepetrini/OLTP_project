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
