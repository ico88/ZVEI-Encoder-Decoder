# ZVEI-Encoder-Decoder
Questo applicativo genera e decodifica sequenze di toni radio ZVEI 2. Utilizza l'algoritmo Goertzel per rilevare con precisione le 6 frequenze. Il decoder è ottimizzato per gestire le brevissime pause tra toni identici, un problema critico nelle comunicazioni radio veloci.

# 📻 ZVEI-2 Tone Generator and Decoder (Python/Colab)

Un applicativo Python per la **generazione** e la **decodifica** di sequenze di toni radio selettivi **ZVEI 2**, comunemente utilizzati nei sistemi di comunicazione e allarme radio.

L'implementazione è ottimizzata per gestire le critiche tempistiche dei segnali ZVEI, in particolare la brevissima pausa tra i toni consecutivi.

---

## 🎯 Obiettivo del Progetto

Il progetto mira a fornire una piattaforma per:

1.  **Generare** segnali audio (`.wav`) di sequenze ZVEI 2 personalizzate.
2.  **Decodificare** con precisione tali segnali audio, anche in presenza di toni identici consecutivi (es. `950000`), una sfida comune nei sistemi con tempi di tono/pausa ridotti.

---

## 🛠️ Tecnologia Utilizzata

* **Linguaggio:** Python
* **Librerie Principali:** `numpy`, `scipy.io.wavfile`, `ipywidgets`
* **Algoritmo di Decodifica:** **Goertzel Algorithm** (versione ottimizzata) 

[Image of Goertzel algorithm flow chart]


### ⚙️ Parametri ZVEI 2

L'applicativo è configurato per le tempistiche standard ZVEI 2:

| Parametro | Durata (secondi) | Descrizione |
| :--- | :--- | :--- |
| **TONE_DURATION** | $0.070 \text{ s}$ ($70 \text{ ms}$) | Durata di ciascun tono. |
| **TONE_PAUSE** | $0.020 \text{ s}$ ($20 \text{ ms}$) | Breve pausa tra i toni. |

---

## 💻 Come Usare l'Applicativo (Google Colab)

Questo progetto è ideale per l'esecuzione in un ambiente **Google Colab**, in quanto è diviso in due blocchi sequenziali:

### 1. Generazione del Segnale (Blocco 1)

Il primo blocco crea il file audio (`selettiva_generata.wav`).

* **Input:** Devi inserire la sequenza ZVEI 2 desiderata (es. `950000`) nel campo `sequence_input`.
* **Output:** Viene generato un file `.wav` salvato localmente e scaricabile.

### 2. Decodifica e Analisi (Blocco 2)

Il secondo blocco esegue l'analisi del segnale generato.

* **Funzione Chiave:** La funzione `goertzel_detect` calcola la potenza per le frequenze ZVEI note, utilizzando una **soglia di potenza elevata** (`MIN_GOERTZEL_POWER_THRESHOLD = 7.0`) per ignorare i segnali residui nella pausa di $20 \text{ ms}$.
* **Logica di Avanzamento:** Il decoder utilizza una logica di **salto aggressivo** (`ADVANCE_STEP = 95 \text{ ms}`) quando un nuovo tono viene rilevato, per saltare Tono + Pausa e garantire la velocità. Altrimenti, avanza cautamente per trovare il segnale successivo.
* **Filtro Postumo:** Viene applicato un **filtro di *debouncing* finale** per rimuovere eventuali toni identici consecutivi registrati a causa dell'overlap del segnale.

**Istruzioni Rapide:**

1.  Esegui il **Blocco 1** inserendo la tua sequenza.
2.  Esegui il **Blocco 2**.
3.  Clicca su **"Decodifica Audio Salvato"** per visualizzare il risultato.

---

## 💡 Sfida Tecnica (Toni Consecutivi)

La maggiore sfida affrontata è stata la decodifica affidabile di sequenze con toni identici (es. `950000`). Data la finestra di analisi di $70 \text{ ms}$ e una pausa di soli $20 \text{ ms}$, la rilevazione del silenzio è estremamente difficile.

L'approccio risolutivo è stato l'uso combinato di:

* **Alta Soglia Goertzel:** Per distinguere chiaramente il tono dalla pausa/rumore.
* **Logica di Salto a Tempo:** Per forzare l'avanzamento al punto in cui dovrebbe trovarsi il tono successivo.
