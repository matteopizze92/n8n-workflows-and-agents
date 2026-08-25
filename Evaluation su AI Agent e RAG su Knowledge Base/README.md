---
# AI Agent — Supporto Commerciale con RAG
Agent AI per supporto commerciale interno di un'agenzia di automazione. Risponde a domande su servizi, prezzi e tempistiche usando un knowledge base RAG e un listino dinamico.
## Architettura
Chat Trigger → Parametri → AI Agent → Evaluation
                       ↓
          ┌────────────┼────────────┐
          ↓            ↓            ↓
   Supabase RAG   Google Sheets   Memory
   (FAQ Servizi)  (Listino/CRUD)  (Buffer)
## Componenti Principali
| Nodo | Funzione |
|------|----------|
| **Chat Trigger** | Widget chat pubblico con UI custom |
| **AI Agent** | Agente GPT-5.6-Luna con system prompt per supporto commerciale |
| **Supabase Vector Store** | RAG su knowledge base FAQ (retrieve-as-tool, topK=20) |
| **Embeddings OpenAI** | Generazione vettori per ricerca semantica |
| **Listino Servizi** | Tool Google Sheets per consultazione prezzi |
| **Aggiungi Servizio** | Tool Google Sheets per inserimento nuovi servizi |
| **Simple Memory** | Buffer window (20 contesti) per conversazioni multi-turno |
| **Error Trigger** | Notifica email automatica in caso di errore |
## Stack Tecnico
- **Orchestrazione:** n8n
- **LLM:** OpenAI GPT-5.6-Luna
- **Vector DB:** Supabase (pgvector)
- **Database:** Google Sheets (listino + FAQ)
- **Embeddings:** OpenAI
## Funzionalità
1. **Risposte su servizi e prezzi** — consulta il listino dinamico da Google Sheets
2. **Knowledge base RAG** — recupera informazioni da FAQ vettorizzate su Supabase
3. **Gestione dinamica listino** — aggiunta nuovi servizi direttamente dall'agente
4. **Memory multi-turno** — contesto conversazionale mantenuto (20 messaggi)
5. **Error handling** — notifica automatica via email in caso di fallimento
6. **UI personalizzata** — chat widget dark theme con CSS custom
## Setup
1. Importare `workflow.json` in n8n
2. Configurare le credenziali:
   - OpenAI API
   - Supabase API (con tabella `documents` per vector store)
   - Google Sheets OAuth2
   - Gmail OAuth2 (per error notification)
3. Creare il foglio Google Sheets con tabella listino (colonne: Servizio, Prezzo base, Tempo stimato, Note)
4. Popolare il vector store Supabase con documenti FAQ
5. Attivare il workflow
## Note
- Il workflow include nodi disabilitati per evaluation/evaluation trigger (testing)
- Il CSS del chat widget è completamente personalizzabile
- L'agente risponde sempre in italiano e indica quale tool ha utilizzato
---
