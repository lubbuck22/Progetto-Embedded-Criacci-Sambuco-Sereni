# Orologio Digitale Multifunzione su FPGA Basys 3

## Descrizione del Progetto
L'obiettivo di questo progetto è comprendere e implementare le funzionalità di un sistema embedded su una scheda FPGA programmata in VHDL[cite: 5]. Nello specifico, il sistema replica il comportamento di un classico orologio digitale da polso[cite: 6, 19]. L'architettura è stata sviluppata utilizzando il modello di una macchina a stati finiti (FSM), garantendo un'impostazione gerarchica che favorisce un'efficiente gestione e la reusability del codice[cite: 8, 9]. 

## Funzionalità
* **Orologio:** Il sistema visualizza l'ora in formato 24h, con la precisione di un minuto[cite: 15]. L'orologio può essere impostato a piacimento tramite i pulsanti della scheda e continua a funzionare in background anche quando il display è in uso per altre modalità[cite: 29].
* **Cronometro:** Un cronometro con precisione al centesimo di secondo che può essere visualizzato sul display a sette segmenti[cite: 16]. È possibile avviarlo, fermarlo e azzerarlo liberamente[cite: 16].
* **Timer (Conto alla rovescia):** Una funzione sveglia che permette di impostare un conto alla rovescia personalizzato[cite: 26]. Al termine del tempo stabilito, il display e i led si illuminano, mentre un buzzer esterno riproduce un suono di allarme[cite: 28].

## Controlli Interfaccia Utente
L'interazione con il sistema avviene tramite i cinque pulsanti integrati sulla scheda Basys 3[cite: 21]:
* **LEFT:** Scorre e alterna in ordine le modalità principali: cronometro, timer e sveglia/orologio[cite: 30].
* **RIGHT:** Avvia, interrompe o fa riprendere il conteggio nelle modalità timer e cronometro[cite: 24, 27].
* **DOWN (Reset):** Azzera i valori correnti del timer e del cronometro[cite: 25, 27].
* **UP e CENTER:** Permettono di impostare i valori del timer per il conto alla rovescia e di configurare l'ora corrente dell'orologio[cite: 26, 29].

## Requisiti Hardware e Collegamenti Circuitali
Il progetto è progettato per operare su una scheda FPGA Basys 3 (prodotta da Digilent) e alimentata via cavo USB 2.0[cite: 201, 203]. Per la funzione di allarme sonoro, è stato integrato un circuito esterno costituito da un buzzer e un transistor NPN (2N2222A) che funge da interruttore[cite: 205].

**Configurazione Pinout per il Buzzer:**
* **Alimentazione (VCC):** Il Pin JB6 della scheda è collegato al terminale collettore del transistor NPN[cite: 207].
* **Segnale Logico:** Il Pin JB1 della scheda è collegato al terminale base del transistor NPN tramite una resistenza da 1 kΩ[cite: 208].
* **Connessione Buzzer:** L'emettitore del transistor NPN è collegato al terminale positivo del buzzer, mentre il terminale negativo del buzzer va al Pin JB5 (GND) della scheda[cite: 213, 216].

## Architettura del Sistema
Il progetto è suddiviso gerarchicamente in vari moduli VHDL[cite: 43]:
* **project_main:** Modulo principale ("top-level") che si interfaccia direttamente con i segnali hardware della scheda FPGA, come i pulsanti, lo switch di reset, il clock a 100 MHz e il display[cite: 64, 65, 67, 68, 69].
* **clk_wiz_0:** Un IP core che trasforma il segnale di clock a 100 MHz fornito dalla Basys 3 in un clock di sistema sincronizzato a 5 MHz[cite: 80, 81].
* **display_7seg:** Gestisce il multiplexing del display a sette segmenti garantendo una lettura chiara e senza flickering delle cifre[cite: 86, 100].
* **btn_listener:** Gestisce il *debouncing* hardware dei pulsanti fisici, assicurando che la pressione generi un singolo e pulito impulso di un ciclo di clock[cite: 102, 103].
* **state_changer:** Cuore della logica di controllo. Implementa la macchina a stati finiti (FSM), determinando la corretta transizione tra i vari stati (es. `CRON_COUNT`, `TIMER_SET`, `WATCH`) in risposta agli input utente[cite: 109, 111, 114, 118, 119].
* **state_handler:** Esegue le operazioni di conteggio e logica temporale per l'orologio, il cronometro e il timer a seconda dello stato attuale calcolato dalla FSM[cite: 127, 128]. Si avvale di sottomoduli divisori di frequenza per mantenere un calcolo temporale accurato[cite: 142].
* **music_player:** Modulo dedito alla generazione dell'onda quadra per attivare il buzzer e riprodurre l'allarme[cite: 156].

## Utilizzo Risorse e Consumi
Durante le fasi di implementazione, l'hardware mostra ottime prestazioni di efficienza[cite: 244]:
* **Potenza Dissipata:** Il consumo energetico totale si attesta attorno a 0.17 W[cite: 244]. Si suddivide in una potenza dinamica del 58% (legata soprattutto alle risorse MMCM per i divisori di clock) e una potenza statica del 42%[cite: 211, 236, 255].
* **Risorse FPGA:** Vengono utilizzate circa il 33.96% delle risorse IO, il 22.89% delle LUT e il 20.00% dei blocchi MMCM[cite: 209, 245].

## Autori
**Joseph Criacci, Matilde Sambuco, Leonardo Sereni** [cite: 4]
Progetto sviluppato per il corso di *EMBEDDED ELECTRONIC SYSTEMS - UNIVERSITÀ DI PERUGIA, ANNO ACCADEMICO 2025-26*[cite: 2].
