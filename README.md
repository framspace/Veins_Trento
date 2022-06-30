# Istruzioni

## Indice

* [Ecosistema](#ecosistema)
* [Importare il progetto](#importare-il-progetto)
* [Contenuto del progetto](#contenuto-del-progetto)
  * [Modificare il raggio di trasmissione](#modificare-il-raggio-di-trasmissione)
* [Eseguire la simulazione](#eseguire-la-simulazione)
* [Visualizzare i risultati](#visualizzare-i-risultati)
* [Troubleshooting](#troubleshooting)
* [Link utili](link-utili)
&nbsp;

## Ecosistema

Il file `Progetto_Trento_Veins.zip` è un progetto Omnet che andrà importato successivamente senza estrarlo.

Se si possiede già l'ecosistema di software necessario (SUMO, OMNeT++, INET, Veins) basta importare il progetto come descritto in seguito.

Se non si hanno già i vari software installati il modo più rapido per aprirlo è scaricare la virtual machine "Instant Veins" dal sito ufficiale: http://veins.car2x.org/download/

Istant Veins è una virtual machine che comprende al suo interno tutti i software necessari, funzionanti con delle impostazioni predefinite.

Il progetto è stato testato per la versione `5.2-i1`

## Importare il progetto
Accendere la Virtual Machine (ad esempio con Virtualbox), e importare al suo interno il file .zip del progetto.
Per evitare errori di esecuzione, il file va inserito nella directory home/src, dove si trovano gli altri progetti predefiniti.

Sempre nel menù `Activities` si trovano Omnet e il demone veins-launchd, vanno eseguiti entrambi.

Una volta in Omnet, sulla scheda a sinistra troveremo i vari progetti predefiniti, importiamo il nostro con `click destro -> import`

Dal menù selezioniamo `General` e poi `Existing Projects into Workspace`

Nel prossimo menù selezioniamo `Select archive file`, `Browse` e selezioniamo il file .zip, il resto delle opzioni dovrebbero essere già corrette.

A questo punto il progetto dovrebbe essere stato importato correttamente, e le impostazioni personalizzate mantenute.

Al progetto mancano i file .h e .cc della definizione dei messaggi, questo è voluto per evitare problemi di compatibilità tra versioni diverse. Verrano generati automaticamente appena verrà eseguita la build del progetto.

Se si riscontrano problemi con il linker o il gestore dei messaggi, leggere la sezione finale di questo readme: `Troubleshooting`

## Contenuto del progetto

Il progetto contiene varie cartelle, quelle importanti sono tre: `Simulations`, `Src` e `Results`.

La cartella Src contiene i file C++ del progetto, in particolare contiene `VeinsInetSampleApplication.cc\/.h` e `VeinsInetSampleMessage.msg`

La cartella Simulations contiene una sottocartella chiamata "simulazione" contenente tutti i file sumo e i file di impostazione della simulazione.

La cartella Results conterrà i file di risultato ottenuti al termine della simulazione.

### Modificare il raggio di trasmissione
All'interno della cartella `Simulazione` troviamo il file `omnetpp.ini` contenente le impostazioni della simulazione Omnet.
Una di queste è:
`*.node[*].wlan[0].radio.transmitter.communicationRange= 100m`

Per modificare il raggio di trasmissione basta modificare la distanza in metri di questa impostazione.

Nota:
> CommunicationRange è una impostazione propria del RadioMedium UnitDiskRadio, tipi diversi di RadioMedium necessitano impostazioni diverse. Mantenerla per tipi diversi di trasmissione non dovrebbe causare errori ma in certi casi potrebbe. A quel punto basterà commentare la riga con un `#`

## Eseguire la simulazione
Per eseguire la simulazione entrare nella cartella `Simulations`, poi in `simulazione`, click destro sul file omnetpp.ini e selezionare `Run as -> Omnet++ Simulation`.

Questo aprirà un'IDE di simulazione, in alto a sinistra ci sono i comandi temporali per eseguirla, per ottenere il risultato alla massima velocità utilizzare `Express`.

## Visualizzare i risultati
Una volta terminata la simulazione, saranno stati generati dei file nella cartella `Results`, sempre sotto `Simulazione`.

Aprire il file `.vec` genererà un altro file `.anf`.
Questo file contiene tutti i risultati registrati e permette di visualizzarli direttamente da Omnet.

In particolare ci interessano gli scalari `Distanza` e `TTL`.

I risultati sono esportabili in formato .csv per ulteriori analisi.

Per ottenere il numero di nodi raggiunti basta sommare il numero di distanze o di TTL registrati.
Dato che la formattazione di un .csv elenca in righe i nodi con relativa distanza e TTL, questo calcolo non è altro che il numero di righe del .csv, escludendo le eventuali righe utilizzate per gli indici della tabella.

## Troubleshooting

Se le impostazioni del progetto non venissero importate correttamente è necessario impostarle manualmente.

Entriamo nelle impostazioni con `Click destro -> properties` sul progetto.

Selezioniamo `Omnet++ -> makemake` selezioniamo `src:makemake[...]` e clicchiamo su `Options`

Sotto `Compile` assicuriamoci che tutte e tre le spunte siano selezionate.

Sotto `Custom` assicuriamoci che vi sia scritto `MSGC:=$(MSGC) --msg6`
Questo indica quale versione del gestore dei messaggi utilizzare.


Se si hanno errori inerenti ai messaggi, molto probabilmente il problema è dovuto alla versione di `Nedtool` utilizzata per la loro compilazione. Questo errore è risolvibile entrando in `src` e cancellando i file `VeinsInetSampleMessage.cc` e `VeinsInetSampleMessage.h`.
Alla prossima build verranno generati nuovamente.

## Link utili

Manuale Omnet++ (In certi punti deprecato): https://doc.omnetpp.org/omnetpp/manual/

Manuale IDE Omnet++: https://doc.omnetpp.org/omnetpp/

Documentazione Veins: http://veins.car2x.org/documentation/

Pagina Istant Veins: http://veins.car2x.org/documentation/instant-veins/

Documentazione SUMO: https://sumo.dlr.de/docs/

Nel caso si volessero installare i software, il modo migliore per farlo è installarli singolarmente ed importarli.
Per evitare ogni tipo di errore questo va fatto nell'ordine: 

- [SUMO](https://sumo.dlr.de/docs/Downloads.php)
- [Omnet++](https://omnetpp.org/download/), una volta installato chiederà se installare INET internamente, in rari casi questa installazione può generare errori.
- [INET](https://inet.omnetpp.org/Download.html)
- [Veins](http://veins.car2x.org/download/)

Omnet fornisce una guida di installazione a: https://doc.omnetpp.org/omnetpp/

Nonostante non siano stati riscontrati problemi di compatibilità tra varie versioni dei diversi software, per la scelta della versione riferirsi alla pagina di compatibilità di Veins: http://veins.car2x.org/download/ (In fondo alla pagine)
