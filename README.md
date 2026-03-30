# Orologio Digitale Multifunzione su FPGA Basys 3

## Descrizione del Progetto
L'obiettivo di questo progetto è comprendere e implementare le funzionalità di un sistema embedded su una scheda FPGA programmata in VHDL. Nello specifico, il sistema replica il comportamento di un classico orologio digitale da polso. L'architettura è stata sviluppata utilizzando il modello di una macchina a stati finiti (FSM), garantendo un'impostazione gerarchica che favorisce un'efficiente gestione e la reusability del codice. 

## Funzionalità
* **Orologio:** Il sistema visualizza l'ora in formato 24h, con la precisione di un minuto. L'orologio può essere impostato a piacimento tramite i pulsanti della scheda e continua a funzionare in background anche quando il display è in uso per altre modalità.
* **Cronometro:** Un cronometro con precisione al centesimo di secondo che può essere visualizzato sul display a sette segmenti. È possibile avviarlo, fermarlo e azzerarlo liberamente.
* **Timer (Conto alla rovescia):** Una funzione sveglia che permette di impostare un conto alla rovescia personalizzato. Al termine del tempo stabilito, il display e i led si illuminano, mentre un buzzer esterno riproduce un suono di allarme.

## Controlli Interfaccia Utente
L'interazione con il sistema avviene tramite i cinque pulsanti integrati sulla scheda Basys 3:
* **LEFT:** Scorre e alterna in ordine le modalità principali: cronometro, timer e sveglia/orologio.
* **RIGHT:** Avvia, interrompe o fa riprendere il conteggio nelle modalità timer e cronometro.
* **DOWN (Reset):** Azzera i valori correnti del timer e del cronometro.
* **UP e CENTER:** Permettono di impostare i valori del timer per il conto alla rovescia e di configurare l'ora corrente dell'orologio.

## Requisiti Hardware e Collegamenti Circuitali
Il progetto è progettato per operare su una scheda FPGA Basys 3 (prodotta da Digilent) e alimentata via cavo USB 2.0. Per la funzione di allarme sonoro, è stato integrato un circuito esterno costituito da un buzzer e un transistor NPN (2N2222A) che funge da interruttore.

**Configurazione Pinout per il Buzzer:**
* **Alimentazione (VCC):** Il Pin JB6 della scheda è collegato al terminale collettore del transistor NPN.
* **Segnale Logico:** Il Pin JB1 della scheda è collegato al terminale base del transistor NPN tramite una resistenza da 1 kΩ.
* **Connessione Buzzer:** L'emettitore del transistor NPN è collegato al terminale positivo del buzzer, mentre il terminale negativo del buzzer va al Pin JB5 (GND) della scheda.

## Architettura del Sistema
Il progetto è suddiviso gerarchicamente in vari moduli VHDL:
* **project_main:** Modulo principale ("top-level") che si interfaccia direttamente con i segnali hardware della scheda FPGA, come i pulsanti, lo switch di reset, il clock a 100 MHz e il display.
* **clk_wiz_0:** Un IP core che trasforma il segnale di clock a 100 MHz fornito dalla Basys 3 in un clock di sistema sincronizzato a 5 MHz.
* **display_7seg:** Gestisce il multiplexing del display a sette segmenti garantendo una lettura chiara e senza flickering delle cifre.
* **btn_listener:** Gestisce il *debouncing* hardware dei pulsanti fisici, assicurando che la pressione generi un singolo e pulito impulso di un ciclo di clock.
* **state_changer:** Cuore della logica di controllo. Implementa la macchina a stati finiti (FSM), determinando la corretta transizione tra i vari stati (es. `CRON_COUNT`, `TIMER_SET`, `WATCH`) in risposta agli input utente.
* **state_handler:** Esegue le operazioni di conteggio e logica temporale per l'orologio, il cronometro e il timer a seconda dello stato attuale calcolato dalla FSM. Si avvale di sottomoduli divisori di frequenza per mantenere un calcolo temporale accurato.
* **music_player:** Modulo dedito alla generazione dell'onda quadra per attivare il buzzer e riprodurre l'allarme.

## Utilizzo Risorse e Consumi
Durante le fasi di implementazione, l'hardware mostra ottime prestazioni di efficienza:
* **Potenza Dissipata:** Il consumo energetico totale si attesta attorno a 0.17 W. Si suddivide in una potenza dinamica del 58% (legata soprattutto alle risorse MMCM per i divisori di clock) e una potenza statica del 42%.
* **Risorse FPGA:** Vengono utilizzate circa il 33.96% delle risorse IO, il 22.89% delle LUT e il 20.00% dei blocchi MMCM.

## Autori
**Joseph Criacci, Matilde Sambuco, Leonardo Sereni**
Progetto sviluppato per il corso di *EMBEDDED ELECTRONIC SYSTEMS - UNIVERSITÀ DI PERUGIA, ANNO ACCADEMICO 2025-26*.
