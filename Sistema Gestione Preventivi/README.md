# Sistema Gestione Preventivi
Workflow n8n per l'automazione della qualificazione e gestione delle richieste di preventivo. Utilizza AI (GPT) per analizzare le richieste in input, classificarle, generare brief commerciali e notificare il team.
## Architettura
Form Input → AI Qualification → IF/ELSE
                                    ├─ proceed → Lookup Listino → Assegna Stato/Priorità → AI Offer Brief → Salva Sheet → Notifica
                                    └─ ask_clarification → Email Chiarimento al Lead
## Funzionalità
| Fase | Descrizione |
|------|-------------|
| **Form** | Form web per raccogliere: azienda, email, servizio, budget, urgenza, fonte, messaggio |
| **AI Qualification** | GPT analizza la richiesta e struttura i dati in JSON (status: "Nuova richiesta" / "Da qualificare") |
| **Lookup Listino** | Consulta Google Sheets per prelevare prezzo base e tempo stimato dal listino servizi |
| **Classificazione** | Assegna priorità (Alta/Normale) e status in base a listino e budget |
| **AI Offer Brief** | Genera brief commerciale sintetico con: sintesi, servizio/prezzo, rischi, bozza email |
| **Salva Richiesta** | Archivia tutto in Google Sheets (tab "Richieste") |
| **Notifica** | Invia email al team commerciale con tutti i dati + brief AI |
| **Email Chiarimento** | Se info mancanti, invia email automatica al lead per chiarimenti |
| **Error Handling** | Trigger errori → notifica email al team |
## Dipendenze esterne
- **n8n** (self-hosted o cloud)
- **Google Sheets** - foglio con tab "Listino" e "Richieste"
- **OpenAI API** - modello GPT-5.6-LUNA (qualificazione + brief)
- **Gmail** - invio notifiche email
## Setup
1. Importare il JSON in n8n
2. Configurare le credenziali:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
3. Aggiornare l'ID del Google Sheet nel nodo "Lookup Listino" e "Salva Richiesta"
4. Aggiornare l'indirizzo email destinatario nei nodi "Notifica Commerciale" e "Notify Workflow Error"
5. Attivare il workflow
## Struttura foglio Google Sheets
**Tab "Listino"** - Colonne: Servizio, Prezzo base, Tempo stimato, Note
**Tab "Richieste"** - Colonne: Request ID, Timestamp, Azienda, Email, Servizio, Budget, Urgenza, Fonte, Prezzo base, Tempo stimato, Status, Priorità, Messaggio
## Campi Form
| Campo | Tipo | Obbligatorio |
|-------|------|:------------:|
| Nome Azienda | text | ✓ |
| Email | email | ✓ |
| Servizio richiesto | dropdown (Automazione, Lead generation, Report, Integrazione API, Altro) | ✓ |
| Budget | dropdown (Indefinito, 1-5k, 5-10k, 10-25k, >25k EUR) | ✓ |
| Urgenza | dropdown (Bassa, Media, Alta) | ✓ |
| Fonte contatto | dropdown (Sito web, LinkedIn, Referral, Evento, Altro) | ✓ |
| Messaggio | textarea | ✓ |

---
