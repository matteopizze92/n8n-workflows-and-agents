# Automazione Estrapolazione Contabilità e Contratto — Analisi Premi Fornitori
Workflow n8n che automatizza il confronto tra dati contabili e contratti di fornitura per fornitori GDO. Un agente AI legge il contratto (PDF) e la contabilità (Google Sheets), compila automaticamente un foglio di analisi con i calcoli dei premi (cifra fissa e %), e consegna il file completato.
## Architettura
Form Trigger → Copia Template → Cerca File → Seleziona Contabilità
                                              ↓
                                    Seleziona Contratto
                                              ↓
                               Scarica Contratto (PDF) → Estrai Testo
                                              ↓
                                   AI Agent (GPT)
                                   ↓            ↓
                          Google Sheets    HTTP Request
                          (Leggi Dati)    (Scrivi Analisi)
                                              ↓
                                    Rinomina File → Download XLSX
                                              ↓
                                        Form Completamento
## Componenti Principali
| Nodo | Funzione |
|------|----------|
| **Form Trigger** | Form iniziale per avviare il processo |
| **Copy file** | Copia il template "Foglio Analisi Fornitore" su Google Drive |
| **Search files** | Cerca i file nella cartella del fornitore |
| **Form - Contabilità** | Dropdown per selezionare il file di contabilità |
| **Form - Contratto** | Dropdown per selezionare il contratto di fornitura |
| **Download file** | Scarica il contratto PDF da Google Drive |
| **Extract from File** | Estrae il testo dal PDF del contratto |
| **AI Agent** | Agente GPT che legge contabilità + contratto e compila l'analisi |
| **Get row(s) in contabilità** | Tool Google Sheets per leggere i dati contabili |
| **HTTP Request** | Tool per scrivere i dati nel foglio di analisi |
| **Get row(s) in Analisi** | Tool per leggere lo stato attuale del foglio di analisi |
| **Update file** | Rinomina il file completato con nome fornitore |
| **HTTP Request - Download** | Esporta il foglio completato in XLSX |
| **Form** | Pagina di completamento con link al file |
| **Error Trigger** | Notifica email in caso di errore |
## Stack Tecnico
- **Orchestrazione:** n8n
- **LLM:** OpenAI GPT (con reasoning effort configurabile)
- **Storage:** Google Drive (file) + Google Sheets (dati strutturati)
- **Input:** Form n8n multi-step
- **Output:** Google Sheets Excel export
## Flusso Operativo
1. **Avvio** — L'utente compila il form iniziale
2. **Setup** — Viene copiato un template pre-configurato di "Foglio Analisi Fornitore"
3. **Selezione file** — L'utente seleziona il file di contabilità e il contratto di fornitura dalla cartella del fornitore
4. **Estrazione contratto** — Il PDF viene scaricato e il testo viene estratto
5. **Elaborazione AI** — L'agente:
   - Legge i dati contabili dal Google Sheets selezionato
   - Analizza il testo del contratto
   - Calcola fatturati al netto dei premi
   - Compila il foglio di analisi con: anno, fornitore, linee merceologiche, fatturati, cifre fisse, percentuali premi
6. **Completamento** — Il file viene rinominato, esportato in XLSX e reso disponibile all'utente
## Mappatura Celle Foglio Analisi
| Contenuto | Cella |
|-----------|-------|
| Anno | B1 |
| Fornitore | B2 |
| Nome Linea 1/2/3 | B4-B6 |
| Totale Fatturato Annuo | E1 |
| Fatturato per Linea | E4-E6 |
| Cifre fisse (contratto vs contabilità) | B10-C27 |
| % Premi fine anno (contratto vs contabilità) | B31-D45 |
## Setup
1. Importare `workflow.json` in n8n
2. Configurare le credenziali:
   - Google Drive OAuth2
   - Google Sheets OAuth2
   - OpenAI API
   - Gmail OAuth2 (per error notification)
3. Creare su Google Drive:
   - Cartella "Generazione contabilita e contratto e analisi premi"
   - Template "Foglio Analisi Fornitore" con strutture celle pre-configurate
4. Configurare il foglio di analisi con le formule per totali, delta e premi calcolati
5. Attivare il workflow
## Note
- Le celle con formule (totali, delta, premi calcolati) NON vengono sovrascritte dall'AI
- L'agente opera in italiano
- Il reasoning effort del modello è configurabile (low/medium/high)
- Errori gestiti automaticamente con notifica email
---
