<<<<<<< HEAD
# Hunter Gym

Web app di allenamento con quest giornaliere, verifica tra amici, ranghi, item e combattimenti
simulati. Usa React + Vite, Express + TypeScript e MySQL 8.

## Struttura

```text
hunter-gym/
├── compose.yaml                 # MySQL, seed, API e sito
├── backend/
│   ├── sql/schema.sql           # tabelle e catalogo quest
│   ├── scripts/                 # init-db (tabelle e admin) e seed (account demo)
│   ├── api.http                 # esempi di richieste per REST Client
│   └── src/
│       ├── app.ts               # Express, middleware e rotte
│       ├── db.ts                # pool MySQL e transazioni
│       ├── gameConfig.ts        # soglie rango, premi e regole di combattimento
│       ├── middleware/          # requireAuth (sessione) e adminOnly
│       ├── controllers/         # validazione e risposte HTTP
│       ├── routes/              # endpoint per ciascuna funzione
│       ├── services/            # query e logica applicativa
│       └── utils/               # risposte di errore e sessioni
└── frontend/src/
    ├── api/                     # client HTTP (fetch con cookie)
    ├── context/                 # utente collegato
    ├── components/              # layout, navbar e componenti riusabili (card, alert, schede)
    ├── pages/                   # accesso, quest, verifiche, amici, arena, profilo, admin
    ├── utils/                   # formattazione di date ed errori
    └── styles/                  # tokens.css (colori) e global.css (card e layout)
```

## Avvio con Docker Desktop

1. Copia `.env.example` in `.env` nella cartella principale.
2. Da PowerShell, nella cartella con `compose.yaml`:

   ```powershell
   docker compose up --build -d
   docker compose ps
   ```

3. Apri `http://localhost:5173`. Il seed crea tre account demo (password `HunterGym123!`):
   `AlexHunter`, `MiraBlade`, `LeoRogue`.

Per avere un account admin imposta `ADMIN_USERNAME` nel `.env`: se l'account esiste viene promosso,
altrimenti viene creato (servono anche `ADMIN_EMAIL` e `ADMIN_PASSWORD`). Poi esegui di nuovo
`docker compose up --build -d`. Nel menu comparirà la voce **Admin**.

`docker compose down` ferma i container e i dati restano nel volume. `docker compose down -v` elimina
anche il database. I log si leggono con `docker compose logs -f`.

## Avvio senza Docker

Servono Node.js 20+, npm e MySQL 8. Crea `backend/.env` (parti da `backend/.env.example`), poi da
`backend/` esegui `npm ci`, `npm run build`, `npm run db:init`, `npm run seed` e `npm run dev`.
In un altro terminale, da `frontend/`, esegui `npm ci` e `npm run dev`. L'API usa `http://localhost:3000/api`.

## Variabili del `.env`

| Variabile | Significato |
| --- | --- |
| `MYSQL_ROOT_PASSWORD`, `DB_PASSWORD` | Password del database (cambiale prima di esporre il progetto in rete) |
| `FRONTEND_ORIGIN` | Indirizzo del sito che può chiamare l'API (CORS con cookie) |
| `VITE_API_URL` | Indirizzo dell'API usato dal sito (se manca usa lo stesso host, porta 3000) |
| `COOKIE_SECURE` | `true` solo se il sito è in HTTPS |
| `ADMIN_USERNAME`, `ADMIN_EMAIL`, `ADMIN_PASSWORD` | Account admin da creare o promuovere |

## Come funziona il login

Al login il server crea una **sessione**: genera un token casuale, salva nel database solo il suo hash
(tabella `sessions`) e lo manda al browser dentro un cookie **httpOnly**, che JavaScript non può leggere.
Il browser rimanda il cookie a ogni richiesta (`credentials: 'include'`) e il middleware `requireAuth`
controlla che la sessione esista e non sia scaduta (7 giorni). Il logout elimina la sessione dal database.
Il cookie è `SameSite=Lax`; se il sito viene servito in HTTPS imposta `COOKIE_SECURE=true`.

## Funzioni

- Registrazione con email, username e password (senza verifica email) e accesso immediato.
- Quest giornaliere persistenti: 3 facili, 2 medie e 1 hard, escluse quelle del giorno prima.
- Ogni quest va verificata da un amico: senza amici non si possono inviare.
- Premi: l'atleta riceve 10/20/40 XP e 1/1/2 punti sulla statistica; il verificatore metà degli XP.
  I punti stat si fermano al rango Monarca, gli XP continuano.
- Ranghi da E I a Monarca, scelta permanente di una classe Monarca e bonus rango alle statistiche.
- Sei item sbloccati per rango: se ne equipaggia uno e i bonus valgono solo nell'arena.
- Amici, richieste, classifica per XP totali, sfide automatiche e storico combattimenti.
- Console admin (solo admin) per aggiungere XP e punti al proprio account.

## Prove manuali

Dopo l'avvio controlla:

- **Accesso**: registrazione, login, password errata, `GET /api/health`. Dopo il logout le pagine
  protette rimandano al login e una chiamata senza cookie risponde 401.
- **Quest**: alla prima apertura del giorno ci sono 6 quest (3 facili, 2 medie, 1 hard); ricaricando
  restano uguali. Senza amici non si possono inviare.
- **Verifiche**: approvando, l'atleta riceve l'intero premio (10/20/40 XP, +1/+1/+2 stat) e il
  verificatore metà degli XP senza stat. Rifiutando, la quest torna assegnata. La quest che porta a
  33.000 XP dà XP ma nessun punto stat.
- **Ranghi e item**: soglie XP e bonus rango; a 33.000 XP si sceglie una sola classe Monarca; un item di
  rango superiore viene rifiutato.
- **Amici e arena**: ricerca, richieste, accettazione, rimozione, classifica; battaglie solo tra amici,
  massimo 10 al giorno per cacciatore, registro dei turni nel dettaglio.
- **Admin**: solo l'account indicato in `ADMIN_USERNAME` vede la console; con un account normale la
  chiamata diretta risponde 403.
=======
# Prog_Hunter_Gym2222
>>>>>>> 1ca3edab289e33f1f11ceb094d0c4ab4c1fced1b
