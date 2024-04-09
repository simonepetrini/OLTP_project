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

...      .. image:: 
