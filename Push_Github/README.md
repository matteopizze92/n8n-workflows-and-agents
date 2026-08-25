README.md — Copia e incolla su GitHub
# n8n Backup Workflow to GitHub
Workflow n8n per il backup automatico di un workflow su una repository GitHub tramite API.
## Funzionalità
Quando triggerato manualmente:
1. Recupera il workflow n8n specificato tramite l'API n8n
2. Salva il JSON del workflow nella repository GitHub configurata
3. Il file viene creato/aggiornato con messaggio di commit automatico
## Prerequisiti
- Istanza n8n self-hosted o cloud
- Credenziali **n8n API** configurate in n8n
- Credenziali **GitHub API** con permessi di scrittura sulla repository
## Installazione
1. Importa il file `.json` in n8n via **Import from File** o **Import from URL**
2. Configura le credenziali nei nodi:
   - **Get a workflow**: seleziona la credenziale n8n API
   - **Edit a file**: seleziona la credenziale GitHub API
3. Modifica i parametri:
   - `workflowId`: ID del workflow da backupare
   - `owner`: username GitHub
   - `repository`: nome della repo GitHub
## Utilizzo
1. Apri il workflow in n8n
2. Clicca **Execute Workflow**
3. Il workflow selezionato verrà salvato come `{nome_workflow}.json` nella repo
## Struttura della Repo GitHub
La repository dovrebbe avere questa struttura:
n8n-workflows/
├── README.md
├── Workflow1.json
├── Workflow2.json
└── ...

---
