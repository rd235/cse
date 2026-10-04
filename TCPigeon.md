# TCPigeon

Un principe e una principessa vivono reclusi in due torri lontane.

Per comunicare tra loro usano dei piccioni viaggiatori. Nella torre del principe c'è l'ufficiale delle comunicazioni del principe; analogamente, nella torre della principessa c'è il suo ufficiale delle comunicazioni.

Quando il principe o la principessa devono inviare un messaggio, consegnano le parole, una alla volta e nell'ordine corretto, al proprio ufficiale.

L'ufficiale della torre destinataria deve consegnare le parole nello stesso ordine in cui sono state spedite, senza perderne nessuna.

Ci sono però tre problemi:

* ogni piccione viaggiatore può trasportare un solo foglietto, che può contenere al massimo due numeri e una parola;
* un piccione può perdersi e impiegare molto tempo per arrivare a destinazione;
* raramente un piccione può essere vittima dei cacciatori e, in questo caso, il foglietto non arriva a destinazione.

Proveremo a risolvere il problema introducendo una difficoltà alla volta.

## Piccioni svizzeri

I piccioni svizzeri portano i foglietti seguendo sempre la strada più breve, volano in fila ordinata, non si superano e non vengono colpiti dai cacciatori.

La soluzione è semplice: l'ufficiale che deve spedire un messaggio scrive ogni parola su un foglietto, senza utilizzare i numeri, e affida i foglietti ai piccioni nell'ordine corretto.

L'ufficiale che riceve un piccione consegna immediatamente la parola al principe o alla principessa.

**(Gioco #0)**

## Piccioni bavaresi

I piccioni bavaresi non vengono colpiti dai cacciatori, ma, a causa della troppa birra che talvolta consumano, possono perdersi e arrivare in ritardo. I foglietti possono quindi essere consegnati in un ordine diverso da quello di spedizione.

La soluzione consiste nell'usare il primo dei due numeri disponibili sul foglietto come **numero di sequenza** della parola.

Ogni ufficiale mantiene due contatori:

* il numero di parole spedite;
* il numero di parole già consegnate.

Entrambi hanno valore iniziale pari a zero.

Quando il principe o la principessa affidano una parola da spedire all'ufficiale, quest'ultimo:

* prende un foglietto e vi scrive la parola;
* incrementa di uno il contatore delle parole spedite;
* scrive il nuovo valore come numero di sequenza nel primo campo numerico del foglietto;
* affida il foglietto a un piccione.

Quando un piccione arriva all'ufficiale destinatario:

* se il numero di sequenza è uguale al numero di parole già consegnate più uno:
    * consegna la parola;
    * incrementa il contatore delle parole consegnate;
    * continua a consegnare le eventuali parole già arrivate e messe da parte, finché è presente il foglietto con numero di sequenza uguale al nuovo valore del contatore più uno;
* altrimenti, mette da parte il foglietto in attesa che arrivino quelli mancanti.

**(Gioco #1)**

## Piccioni piccioni

I piccioni piccioni possono perdersi e arrivare in ritardo, ma possono anche essere vittime dei cacciatori, sebbene ciò accada raramente.

Ogni ufficiale mantiene due contatori:

* il numero di parole spedite;
* il numero di parole già consegnate.

La soluzione si basa sull'idea di inviare un messaggio di conferma per ogni parola ricevuta.

I foglietti utilizzati per spedire una parola contengono:

* la parola;
* il numero di sequenza della parola nel primo campo numerico;
* `0` nel secondo campo numerico.

I foglietti utilizzati come conferma contengono:

* `0` nel primo campo numerico;
* nessuna parola;
* nel secondo campo numerico, il numero di sequenza della parola da confermare.

Ogni parola viene ritrasmessa se, dopo un intervallo di tempo sufficiente perché un piccione possa compiere il viaggio di andata e ritorno, non è arrivata alcuna conferma.

### Tabelle degli ufficiali

Ogni ufficiale organizza sulla propria lavagna due tabelle:

* **in uscita**, per le parole ancora da confermare;
* **in entrata**, per le parole ricevute ma non ancora consegnate.

Entrambe le tabelle hanno una riga per ogni parola e contengono almeno il numero di sequenza e la parola.

Le righe della tabella **in uscita** contengono inoltre un campo per la ritrasmissione. In questo campo viene aggiunto un *tick* (una spunta) per ogni minuto di attesa della conferma.

### Invio di una parola

Quando un ufficiale riceve dal principe o dalla principessa una parola da spedire:

* inserisce la parola nella tabella **in uscita**, assegnandole un numero di sequenza ottenuto incrementando di uno il contatore delle parole spedite;
* prende un foglietto;
* vi scrive la parola, il numero di sequenza e `0` nel secondo campo numerico;
* affida il foglietto a un piccione.

### Ricezione di un piccione

Quando arriva un piccione, l'ufficiale:

* prende il foglietto dalla zampetta e lo legge;
* se sul foglietto è presente una parola:
    * prende un nuovo foglietto;
    * scrive nel secondo campo numerico il numero di sequenza della parola appena ricevuta;
    * lascia vuoti il campo della parola e il primo campo numerico, che viene impostato a `0`;
    * affida il nuovo foglietto a un piccione;
    * se il numero di sequenza ricevuto è minore o uguale al contatore delle parole già consegnate, oppure se la parola è già presente nella tabella **in entrata**, cestina il foglietto;
    * altrimenti inserisce il numero di sequenza e la parola nella tabella **in entrata**;
* finché nella tabella **in entrata** è presente la parola con numero di sequenza successivo al contatore delle parole consegnate:
    * consegna la parola al principe o alla principessa;
    * cancella la riga dalla tabella **in entrata**;
    * incrementa il contatore delle parole consegnate;
* se sul foglietto non è presente una parola:
    * cerca nella tabella **in uscita** una parola con numero di sequenza uguale al numero di conferma indicato nel secondo campo numerico;
    * se la trova, cancella la relativa riga dalla tabella **in uscita**.

### Ritrasmissione

Allo scadere di ogni minuto:

* aggiunge un *tick* a tutte le righe della tabella **in uscita**, nel campo dedicato alla ritrasmissione;
* per ogni riga che raggiunge quattro *tick*:
    * ritrasmette la parola, scrivendo su un nuovo foglietto la parola e il numero di sequenza (`0` nel campo della conferma);
    * affida il foglietto a un piccione;
    * cancella tutti i *tick* di quella riga.

### Percorso dei piccioni

Quando a un piccione viene consegnato un foglietto:

* il piccione estrae una carta:
    * se è **verde**, prende il percorso veloce (2 minuti);
    * se è **gialla**, prende il percorso lento (3 minuti);
    * se è **rossa**, il foglietto viene perduto;
* il piccione segue il filo del colore e si ferma alla prima tappa.

In alternativa si possono usare carte da gioco:

* **cuori** e **quadri** = percorso veloce;
* **fiori** = percorso lento;
* **picche** = foglietto perduto.

Quando è trascorso un minuto, il piccione si sposta alla tappa successiva. Se raggiunge la destinazione, consegna il foglietto all'ufficiale.

**(Gioco #2)**

# Piccioni sindacalizzati

Il problema sembra risolto e il metodo funziona. Purtroppo, però, i nostri ufficiali devono affrontare la rivolta dei piccioni, costretti a fare troppi viaggi avanti e indietro.

Il sindacato dei piccioni propone una soluzione: le informazioni sulle parole già ricevute possono essere comunicate ogni minuto, indicando il numero di sequenza dell'ultima parola consegnata.

Inoltre, se il principe e la principessa hanno una fitta corrispondenza, la conferma delle parole ricevute può essere aggiunta ai normali messaggi in uscita.

### Invio di un messaggio

Quando un ufficiale riceve dal principe o dalla principessa una sequenza di parole da spedire, inserisce le parole nella tabella **in uscita**, assegnando a ciascuna un numero progressivo.

Ogni ufficiale mantiene:

* un contatore delle parole spedite;
* un contatore delle parole già consegnate;
* una tabella **in uscita**;
* una tabella **in entrata**.

Entrambe le tabelle hanno una riga per ogni parola, contenente il numero di sequenza e la parola.

Le righe della tabella **in uscita** contengono inoltre il campo per la ritrasmissione, nel quale viene aggiunto un *tick* per ogni minuto di attesa della conferma.

### Ricezione di un piccione

Quando arriva un piccione, l'ufficiale:

* prende il foglietto dalla zampetta e lo legge;
* cancella dalla tabella **in uscita** tutte le righe con numero di sequenza minore o uguale al numero di parole consegnate indicato nel foglietto;
* se il numero di parola è `0`, oppure se il numero di sequenza indica una parola già consegnata o già presente nella tabella **in entrata**, cestina il foglietto;
* altrimenti inserisce la parola e il relativo numero di sequenza nella tabella **in entrata**.

### Operazioni allo scadere di ogni minuto

Allo scadere di ogni minuto:

* finché nella tabella **in entrata** è presente una parola con numero di sequenza successivo a quello dell'ultima parola consegnata:
    * consegna la parola;
    * incrementa il contatore delle parole consegnate;
    * cancella la relativa riga dalla tabella **in entrata**;
* per tutte le righe della tabella **in uscita** che hanno quattro *tick*:
    * cancella tutti i *tick*;
* per tutte le righe della tabella **in uscita** senza *tick*:
    * prepara un foglietto contenente:
        * il numero di sequenza della parola;
        * il valore attuale del contatore delle parole consegnate;
        * la parola;
    * affida il foglietto a un piccione;
* per tutte le righe della tabella **in uscita**, aggiunge un *tick*;
* se nell'ultimo minuto sono arrivati piccioni con un numero di parola maggiore di `0`, ma non ne è partito nessuno:
    * prepara un foglietto contenente:
        * `0` come numero di sequenza della parola;
        * il valore attuale del contatore delle parole consegnate;
        * nessuna parola;
    * affida il foglietto a un piccione.

### Percorso dei piccioni

Quando a un piccione viene consegnato un foglietto:

* il piccione estrae una carta:
    * se è **verde**, prende il percorso veloce (2 minuti);
    * se è **gialla**, prende il percorso lento (3 minuti);
    * se è **rossa**, il foglietto viene perduto;
* il piccione segue il filo del colore e si ferma alla prima tappa.

In alternativa si possono usare carte da gioco:

* **cuori** e **quadri** = percorso veloce;
* **fiori** = percorso lento;
* **picche** = foglietto perduto.

Quando è trascorso un minuto, il piccione si sposta alla tappa successiva. Se raggiunge la destinazione, consegna il foglietto all'ufficiale.

**(Gioco #3)**

## Cosa c'entra tutto questo?

Non è un caso che la famiglia di protocolli che consente a Internet di
funzionare si chiami **TCP/IP**.

I protocolli di Internet vengono descritti nei documenti pubblicati dalla
**IETF** (*Internet Engineering Task Force*), chiamati **RFC** (*Request for
Comments*). Le RFC sono ormai migliaia, ma **IP** e **TCP** svolgono ancora
un ruolo centrale.

### IP: consegnare pacchetti attraverso Internet

IP è il cuore dell'infrastruttura di rete: il suo nome significa **Internet Protocol**.

Grazie a IP viene realizzata l'astrazione di **internetwork**: un insieme di
reti collegate tra loro che, dal punto di vista di chi le utilizza, si comporta
come se fosse un'unica grande rete.

Affidando a Internet un *pacchetto*, una breve sequenza di byte, e
specificando un indirizzo di destinazione, normalmente ci si aspetta che il
pacchetto venga consegnato a destinazione in un tempo compatibile con gli scopi
della comunicazione, se il destinatario è raggiungibile.

È un po' come compilare una cartolina e imbucarla.

Se l'indirizzo di destinazione è corretto, ci si aspetta che la cartolina venga
recapitata nella cassetta delle lettere del destinatario, che potrà leggere il
messaggio, per esempio:

> Saluti da Bologna

Può capitare che una cartolina venga perduta o che impieghi molto tempo per
essere recapitata, ma normalmente arriva a destinazione.

Una volta imbucata e presa in carico dal servizio postale, non sappiamo quale
percorso seguirà: potrebbe essere trasportata in treno, in aereo, via nave o in
camion. Queste informazioni non interessano né al mittente né al destinatario:
ciò che conta è che la cartolina arrivi a destinazione in tempo utile.

Allo stesso modo, un pacchetto IP viene smistato e fatto transitare attraverso
le diverse reti che compongono Internet fino a raggiungere la destinazione. I
dettagli del percorso interessano soprattutto a chi progetta e implementa i
servizi e le infrastrutture di rete, non a chi utilizza IP.

### TCP: rendere affidabile la comunicazione

**TCP** (*Transmission Control Protocol*) è il protocollo che consente di
instaurare un dialogo affidabile tra due interlocutori.

In particolare:

* l'oggetto della comunicazione TCP è una coppia di sequenze di byte, una per ciascuna direzione della comunicazione: l'applicazione a un estremo invia una sequenza di byte all'altro estremo e, contemporaneamente, l'applicazione all'altro estremo può inviare una propria sequenza di byte nella direzione opposta;
* ciascuna sequenza può avere lunghezza indefinita: in ogni momento è possibile aggiungere uno o più byte ai dati da trasmettere;
* i byte di ciascuna sequenza vengono consegnati nello stesso ordine in cui sono stati inviati;
in ciascuna sequenza ricevuta non ci sono byte mancanti;
* TCP non può fare miracoli: se l'infrastruttura di comunicazione si interrompe completamente, gli ultimi byte inviati possono andare perduti. In questo caso:
	* per ciascuna direzione, i byte ricevuti costituiscono comunque una sottosequenza iniziale dei byte trasmessi;
	* se il collegamento viene ripristinato, è possibile continuare il dialogo mantenendo le garanzie offerte da TCP;
* TCP viene realizzato utilizzando IP come servizio di comunicazione sottostante.

È proprio quest'ultimo punto quello che abbiamo affrontato con **TCPigeon**:

> **Come è possibile realizzare un servizio affidabile di comunicazione di
sequenze (TCP) utilizzando un servizio inaffidabile di comunicazione di
pacchetti (IP)?**

### TCPigeon e TCP/IP

In TCPigeon:

* i **piccioni** rappresentano il servizio offerto da IP;
* il **dialogo tra il principe e la principessa** rappresenta la comunicazione TCP;
* i **foglietti** attaccati alle zampette dei piccioni rappresentano i pacchetti IP;
* le **parole del messaggio** rappresentano gli elementi della sequenza scambiata tramite TCP.

Nel TCP reale, gli elementi della sequenza trasmessa sono **byte**, non parole.

Quando ci sono molti byte da trasferire, TCP li organizza in segmenti che
vengono trasportati all'interno di pacchetti IP, fino a rispettare la
dimensione massima consentita. Anche il numero di sequenza utilizzato da TCP è
espresso in termini di byte.

In TCPigeon abbiamo scelto di lavorare con **sequenze di parole**, anziché con
byte o caratteri, per rendere più semplici da comprendere i concetti di:

* numerazione e ordinamento;
* conferma della ricezione;
* ritrasmissione dei dati perduti.

In questo modo i numeri di sequenza restano piccoli e facilmente gestibili
durante il gioco, senza dover contare i caratteri o preoccuparsi di come
suddividere una sequenza di byte nei diversi pacchetti.

TCPigeon non è quindi una riproduzione completa di TCP, ma un modello
semplificato che permette di sperimentare alcune delle idee fondamentali alla
base della comunicazione affidabile costruita sopra un servizio di rete che, da
solo, non offre garanzie di consegna, ordine o assenza di duplicati.

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org)

