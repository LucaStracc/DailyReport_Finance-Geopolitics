# SPECIFICA COMPLETA — MORNING FINANCIAL REPORT
*Documento di progettazione — versione 1.0*

---

## LOGICA GENERALE DEL SISTEMA

### Trigger di generazione
Il report non ha un orario fisso. La generazione si avvia automaticamente quando viene rilevata la pubblicazione del nuovo episodio di **Caffè e Mercati** (investire.biz), il podcast mattutino di riferimento. Questo episodio funge da ancora temporale dell'intera sezione §7 (Echo Chamber) ed è il primo contenuto processato. La generazione è giornaliera, tutti i giorni inclusi sabato e domenica.

### Archivio persistente
Il sistema mantiene un file JSON persistente (`report_memory.json`) aggiornato ad ogni generazione. Contiene:
- Narrativa dominante delle ultime 4 settimane (asse §8A)
- Previsioni espresse dai creator con data e stato (§7 fact-check)
- Posizionamento nel ciclo economico degli ultimi 30 giorni (§8B)
- Snapshot macro settimanale per confronti storici

Questo file è l'unica fonte di continuità tra un report e il successivo. Senza di esso, §8A e il fact-check del §7 non possono funzionare.

### Formato finale
PDF, nessun limite di lunghezza, priorità alla qualità sull'efficienza di generazione. Ogni sezione ha una propria pagina d'inizio.

### Principio di lunghezza adattiva
Nessuna sezione ha un limite di righe o paragrafi. La lunghezza di ogni blocco è determinata da due fattori: la rilevanza intrinseca del contenuto (un dato macro che sorprende il consensus di 0.5pp è più rilevante di uno in linea) e la frequenza con cui l'argomento appare nelle fonti monitorate (se un tema viene citato da più creator, più testate e più analisti nella stessa mattina, riceve più spazio proporzionalmente). Un giorno denso di eventi produrrà un report lungo; un giorno piatto produrrà un report breve. Non si allunga artificialmente e non si accorcia per convenienza.

---

## SEZIONE 1 — EXECUTIVE SUMMARY

### Scopo
Prima pagina del report. Funziona come copertina operativa: chi legge solo questa sezione deve capire cosa conta oggi. Non è un indice — è una selezione editoriale.

### Struttura

**Blocco A — I Cinque Punti**
Esattamente cinque bullet point. Ogni punto:
- È autonomo: si regge senza aver letto il resto
- Risponde implicitamente alla domanda: *questo cambia qualcosa oggi?*
- È scritto in linguaggio da desk, non da giornale. Nessun aggettivo inutile.
- Contiene un solo fatto o una sola dinamica — non due messi insieme
- Ha un riferimento temporale esplicito se il dato è time-sensitive

Come vengono selezionati: l'AI legge l'intero output delle sezioni 2–9 e identifica i cinque elementi con il più alto potenziale di impatto sul mercato nelle prossime 24–48 ore. Non si privilegia automaticamente la macro sulla geopolitica o viceversa. Il criterio è esclusivamente: *quanto potrebbe muovere prezzi o aspettative?*

Esempi di bullet ben costruiti:
- "Fed Waller (non-votante) ha dichiarato che un taglio a marzo è 'prematuro' — i futures ora prezzano il primo taglio a giugno, contro maggio di ieri."
- "Il WTI ha chiuso sotto 74$/barile per la terza sessione consecutiva. Le scorte EIA di mercoledì sono il catalizzatore principale della giornata."
- "Apple riporta i risultati dopo la chiusura: consensus EPS a 2.10$, guidance iPhone in focus dopo i dati deboli di Foxconn."

Esempi da evitare:
- "I mercati sono volatili in attesa di notizie importanti dalla Fed." (vago, non operativo)
- "L'economia globale continua a mostrare segnali contrastanti." (non dice nulla)

**Blocco B — Notizie Non Contenute Altrove**
Questo blocco è redatto per ultimo, dopo che tutte le altre sezioni sono già generate. Raccoglie notizie rilevanti che non sono state incorporate in nessuna delle sezioni precedenti perché:
- Troppo specifiche per la macro
- Non abbastanza grandi per la geopolitica
- Non legate a nessun ticker nel watchlist
- Di natura diversa (es. normativi, regolatori, tecnologici non finanziari ma con impatto economico)

Il blocco può essere vuoto se tutto è già coperto. Non viene mai riempito artificialmente.
Formato: lista non numerata, ogni voce ha fonte e ora di pubblicazione.
Esempi:
- "• [Reuters, 07:14] — La SEC ha avviato un'indagine sui trading pattern di tre ETF crypto settimana scorsa."
- "• [Xinhua, 06:02] — Il governo cinese dichiara stato di emergenza da Covid-19 in tre province del sud-est. Primo focolaio significativo dall'uscita dalla politica Zero-Covid."

---

## SEZIONE 2 — MARKET SNAPSHOT

### Scopo
Fotografia live dei mercati globali al momento di generazione del report. Ogni dato riporta obbligatoriamente il proprio riferimento temporale (chiusura precedente, pre-market, live alle HH:MM).

---

### 2A — TABELLA MERCATI GLOBALI

**Struttura della tabella**
Asse Y (righe) = strumenti finanziari, raggruppati in 6 categorie.
Asse X (colonne) = Borsa/mercato | Orario quotazioni | Apertura ieri | Chiusura ieri | Min ieri | Max ieri | Var% ieri | Apertura oggi (o pre-market) | Var% oggi | Valore attuale | Sparkline 10gg

**Regole sui dati:**
- Se la borsa è chiusa al momento di generazione (es. NYSE prima delle 15:30 CET): si usano i valori pre-market con indicazione esplicita "(pre-market HH:MM)"
- Se il mercato è aperto: valore live con timestamp esatto
- Nessun numero senza contesto temporale

**Codice colore celle:**
- Verde scuro: Var% > +1.5%
- Verde chiaro: Var% tra 0 e +1.5%
- Rosso chiaro: Var% tra 0 e -1.5%
- Rosso scuro: Var% < -1.5%
- Grigio: mercato chiuso o dato non disponibile

**Sparkline:**
Grafico a linea compatto (miniatura, circa 80x30px nel PDF) degli ultimi 10 giorni di chiusura. Non è analisi tecnica. Serve solo per la direzione visiva immediata. Nessun asse, nessuna scala — solo la forma della curva.

**Strumenti inclusi:**

*Azionario Globale*
S&P 500 (CME Globex) | Nasdaq 100 (CME Globex) | Dow Jones (CME Globex) | Russell 2000 (CME Globex) | Nikkei 225 (TSE) | Shanghai Composite (SSE) | Hang Seng (HKEX) | Euro Stoxx 50 (Eurex) | DAX (Xetra) | FTSE MIB (Borsa Italiana) | CAC 40 (Euronext Paris) | FTSE 100 (LSE) | SMI (SIX) | Nifty 50 (NSE India)

*Materie Prime*
WTI Crude (NYMEX) | Brent Crude (ICE) | Oro spot (XAU/USD) | Argento spot (XAG/USD) | Rame (LME o COMEX) | Gas Naturale EU (TTF, ICE)

*Obbligazionario*
US Treasury 2Y (rendimento) | US Treasury 10Y (rendimento) | Bund 10Y (rendimento) | BTP 10Y (rendimento) | [Spread BTP-Bund in colonna dedicata]

*Valute*
EUR/USD | GBP/USD | USD/JPY | USD/CHF | USD/CNH | BTC/USD | ETH/USD

*Termometro del Rischio*
VIX (CBOE) | MOVE Index (rendimento mercato obbligazionario) | CNN Fear & Greed Index (valore numerico + label: Extreme Fear / Fear / Neutral / Greed / Extreme Greed)

*Settori ETF USA*
XLK (Tecnologia) | XLV (Healthcare) | XLF (Finanziari) | XLE (Energia) | XLY (Consumo Discrezionale) | XLP (Consumo Primari) | XLI (Industriali) | XLB (Materiali) | XLU (Utilities) | XLRE (Real Estate) | XLC (Comunicazioni) | SOXX (Semiconduttori) | ITA (Difesa & Aerospazio) | SX7E (Banche Europee)

**Fonte dati:** API TradingView / Yahoo Finance / FRED per i rendimenti. Ogni cella riporta la fonte e il timestamp nella nota a piè di pagina della tabella.

---

**AI — Commento Intermarket Generale**
Posizionato subito sotto la tabella. Non riassume i numeri — quelli si leggono. Spiega le anomalie e le correlazioni inattese della mattina. La lunghezza è proporzionale alla complessità della giornata: se i segnali sono molti e contraddittori, il commento sarà più esteso; se la giornata è lineare, sarà sintetico. Il modello cerca attivamente:
- Divergenze inusuali tra asset correlati (es. rame e azionario divergono)
- Correlazioni inverse che si rompono (es. oro sale con l'azionario)
- Movimenti di valute in contraddizione con i tassi (es. dollaro si rafforza mentre i Treasury cedono)
- Disallineamenti tra volatilità (VIX/MOVE) e la direzione del mercato sottostante
- Settori ETF che si muovono contro l'indice principale (rotazione intra-day)

Se non ci sono anomalie rilevanti, lo dice esplicitamente: "Oggi i principali asset si muovono in modo coerente con il risk-on prevalente. Nessuna divergenza significativa da segnalare."

---

### 2B — CICLO ECONOMICO

**Grafico di posizionamento**
Rappresentazione visiva del classico ciclo economico a quattro fasi (Espansione → Rallentamento → Recessione → Ripresa) con un punto che indica la posizione attuale stimata. Il punto si può trovare in qualsiasi parte del perimetro del ciclo, non solo nei quattro vertici.

**AI — Lettura del Ciclo Economico**
Il modello analizza i dati macro più recenti (tassi, inflazione, PMI, mercato del lavoro, produzione industriale, yield curve) e:
1. Dichiara la fase attuale e spiega perché, citando tutti i segnali rilevanti disponibili — non c'è limite al numero di segnali: se i dati sono molti e coerenti, la spiegazione sarà più ricca; se i segnali sono contrastanti, viene spiegata la contraddizione
2. Indica la direzione di movimento (es. "ci stiamo avvicinando al rallentamento da un'espansione matura")
3. Collega la posizione nel ciclo alle implicazioni teoriche sulle classi di asset: cosa tende a sovraperformare in questa fase secondo la rotazione settoriale del ciclo di Fidelity/BlackRock

Esempio: "I PMI manifatturieri in contrazione in Europa e USA, combinati con un mercato del lavoro ancora rigido e inflazione in discesa lenta, collocano le economie avanzate in una fase di rallentamento avanzato. In questa fase, storicamente, i bond governativi e le utility tendono a sovraperformare; i ciclici e i titoli growth tendono a sottoperformare."

---

### 2C — YIELD CURVE

**Grafico**
Curva dei rendimenti US Treasury alle scadenze: 3M, 3Y, 5Y, 10Y, 30Y.
Asse X: scadenza. Asse Y: rendimento in %.
Per ciascuna scadenza, la curva attuale è confrontata con quella di 30 giorni fa (linea tratteggiata), così da rendere visivamente immediato lo spostamento parallelo o la variazione di forma.

**Nota di commento** (automatica, poche righe):
Segnala se la curva è invertita (3M > 10Y oppure 2Y > 10Y), da quanti giorni, e se l'inversione si è ridotta o ampliata rispetto al mese precedente. Se la curva si è irripidita o appiattita in modo anomalo su uno specifico tratto, lo rileva.

---

## SEZIONE 3 — PORTFOLIO RADAR

### Scopo
Monitoraggio live del watchlist personale. La struttura è identica alla tabella del §2A ma filtrata sui ticker configurati dall'utente.

### Struttura per ogni ticker

**Tabella**
Stessa struttura di §2A: Borsa | Orario quotazioni | Apertura ieri | Chiusura ieri | Min ieri | Max ieri | Var% ieri | Apertura oggi (o pre-market) | Var% oggi | Valore attuale + Sparkline 10gg

Codice colore identico a §2A.

**AI — Citazioni & News (Ricerca Semantica)**
Per ogni ticker, il modello esegue una ricerca semantica all'interno delle fonti di news configurate (Bloomberg, Reuters, Financial Times, WSJ, Sole 24 Ore per titoli italiani) e riporta le citazioni testuali con:
- Nome dell'azienda/ticker
- Frase o paragrafo esatto in cui è citata
- Nome della fonte + URL
- Ora di pubblicazione

Se nessuna fonte ha citato il ticker nelle ultime 24 ore, il blocco scompare completamente. Niente testo generato per riempire lo spazio.

Se presente almeno una citazione, il modello aggiunge:
- Breve riassunto del contenuto (3-4 righe)
- Commento tecnico: la notizia è coerente con il trend recente del titolo? Ha già prezzato o è potenzialmente surprice?

---

## SEZIONE 4 — MACROECONOMIA

### Scopo
Analisi completa di tutti i dati macroeconomici rilevanti pubblicati dall'ultimo report. La sezione si struttura in due blocchi sequenziali: prima la tabella di contesto (4A), poi il testo analitico (4B).

---

### 4A — TABELLA INDICATORI MACRO PRINCIPALI

**Logica della tabella**
Tabella fissa con gli indicatori strutturali delle principali economie. Non dipende dai dati usciti oggi — è sempre presente come contesto di riferimento. Si aggiorna ogni volta che un indicatore viene rivisto o viene pubblicato un nuovo dato.

**Economie incluse (fisse):** USA | Eurozona | Cina
**Economie condizionali:** Russia e UAE entrano solo se ci sono movimenti rilevanti o eventi in corso (es. decisione OPEC+, sanzioni attive, crisi energetica).

**Colonne per ogni indicatore:**
- Indicatore (nome standard)
- Economia
- Valore attuale (con data del dato)
- Dato precedente
- Consensus atteso (se applicabile)
- Prossima pubblicazione (data)
- Segnale: ↑ / ↓ / → (con codice colore)

**Indicatori inclusi per ciascuna economia:**
- Tasso di policy della banca centrale (con data ultima decisione)
- CPI headline YoY (ultimo dato)
- CPI core YoY (ultimo dato)
- PIL trimestrale QoQ annualizzato (versione flash/preliminare/definitiva specificata)
- PMI Manifatturiero (soglia 50 evidenziata)
- PMI Servizi (soglia 50 evidenziata)
- Tasso di disoccupazione
- Saldo commerciale (ultimo dato disponibile)

**Nota sulle revisioni:** Quando un dato viene rivisto rispetto alla versione precedente, la cella mostra il nuovo valore in grassetto e il valore rivisto in grigio tra parentesi. Es: "2.3% (rivisto da 2.1%)"

**AI — Commento tabella**
Subito sotto la tabella, il modello segnala le divergenze più rilevanti tra le economie monitorate, con lunghezza commisurata alla complessità del quadro. Es: "La disinflazione USA è più rapida del previsto mentre l'Eurozona mostra CPI core ancora rigido — una divergenza che giustifica la prudenza della BCE rispetto alla Fed."

---

### 4B — ANALISI TESTO

**Metodo di lavoro (sequenziale)**

**Step 1 — Selezione (AI Valutativa)**
Il modello raccoglie tutti i dati macroeconomici pubblicati dall'ultimo report e li classifica per impatto atteso sul mercato. I criteri sono: sorpresa rispetto al consensus, capacità di modificare le aspettative di politica monetaria, rilevanza sistemica. Solo i dati sopra la soglia vengono inclusi nel testo. Se nessun dato è rilevante, la sezione 4B è assente e rimane solo la tabella 4A.

**Step 2 — Analisi Causa & Effetti (AI Analitica)**
Per ogni dato selezionato, struttura:
- Valore pubblicato vs consensus vs precedente (inline nel testo, evidenziato in grassetto)
- Causa: perché il dato ha questo valore? (fattori strutturali o ciclici)
- Effetto teorico: cosa implica per i prezzi degli asset, per le decisioni di politica monetaria, per i settori esposti
- Commento: interpretazione operativa

**Step 3 — Contesto e Relazioni (AI Contestuale)**
Dopo l'analisi, inserisce il dato in un contesto più ampio:
- Confronto storico quantitativo se sensato: "È la terza volta in 18 mesi che il CPI core USA sorprende al rialzo di almeno 0.2pp — le prime due volte i Treasury 2Y hanno reagito con +15-20 bps nelle successive 48h"
- Relazioni geografiche: il dato di un'economia implica qualcosa per un'altra? (es. "Un PMI manifatturiero tedesco sotto 45 è storicamente correlato con un calo dell'export cinese nelle settimane successive")
- Posizionamento rispetto al ciclo (collegamento con §2B)

**Formato del testo**
Un unico discorso continuo, diviso in sezioni quando i dati sono raggruppabili tematicamente. I nomi degli indicatori e i valori numerici sono in grassetto nel testo (es. **CPI core USA +3.4% YoY**, vs consensus 3.2%). La lunghezza è proporzionale alla rilevanza dei dati: può essere tre paragrafi o dieci. Giorni con pochi dati = sezione breve. Non viene allungata artificialmente.

**Dati di interesse (lista guida, non obbligatoria):**
Politica Monetaria | Inflazione (CPI, PPI, PCE, breakeven 5/10Y) | Crescita (PIL, PMI, ISM, Leading Indicators) | Mercato del Lavoro (disoccupazione, NFP, sussidi, salari) | Consumi (vendite al dettaglio, fiducia consumatori) | Immobiliare (permessi, Case-Shiller, mutui) | Produzione industriale | Commercio estero e Supply Chain (Baltic Dry Index, GSCPI Fed NY) | Credito (spread IG/HY, condizioni bancarie) | Finanze pubbliche (aste Treasury, deficit, rating review)

---

## SEZIONE 5 — GEOPOLITICA

### Scopo
Analizzare gli eventi che modificano i rapporti politici ed economici internazionali e che hanno — o potrebbero avere — conseguenze sugli asset finanziari. La lunghezza è proporzionale al momento storico: può essere una pagina o sei.

### Mappa Visiva
Una mappa del mondo con marker sulle aree di tensione o crisi attive. Ogni marker è codificato per intensità (colore) e tipo (simbolo: conflitto / sanzioni / elezioni / negoziato commerciale / crisi energetica). La mappa è aggiornata ad ogni report e mostra solo i marker attivi — non è storica.

### Struttura per ogni evento

**Header dell'evento**
Nome dell'evento | Paesi coinvolti | Stato attuale (in corso / escalation / de-escalation / risolto) | Intensità finanziaria (Alta / Media / Bassa)

La soglia per l'inclusione è: l'evento ha o può avere un canale di trasmissione identificabile verso uno o più asset finanziari. Senza questo requisito, non viene incluso. Questo filtra automaticamente gli eventi meramente diplomatici senza conseguenze economiche.

**Corpo dell'analisi**
- Descrizione sintetica dell'evento (cosa è successo, chi sono gli attori)
- Asset direttamente esposti (campo obbligatorio — costringe a identificare il canale di trasmissione)
  Es. "Asset esposti: petrolio WTI e Brent, compagnie di trasporto marittimo, ETF difesa (ITA), USD/IRR"
- Analisi cause ed effetti: cosa ha generato questo sviluppo, cosa potrebbe generare
- Scenari: almeno due (escalation vs de-escalation), con indicazione degli asset impattati per ciascuno

**AI — Lista di Monitoraggio (Tripwire List)**
Per ogni evento rilevante, una lista di eventi-spia da osservare nelle prossime sessioni di trading. Serve a capire se il rischio si materializza o si ridimensiona.
Esempio (scenario Iran):
- "Se l'IAEA pubblica un rapporto negativo sull'arricchimento → probabile riapertura del dossier sanzioni → Brent +3/5%"
- "Se l'Iran accetta un incontro bilaterale con i rappresentanti europei → segnale di de-escalation → gas TTF in calo"
- "Se attacchi alle petroliere nel Golfo si intensificano → Baltic Dry Index e noli marittimi in rialzo"

**Fonti primarie:** Reuters | AP | Financial Times | Bloomberg | Wall Street Journal | Der Spiegel (Germania) | Le Monde (Francia) | Al Jazeera (Medio Oriente) | South China Morning Post (Asia). No fonti italiane per la geopolitica internazionale salvo eccezioni specifiche.

**Giorni con geopolitica piatta:** la sezione contiene solo la mappa aggiornata e una riga: "Nessun sviluppo rilevante nelle ultime 24 ore."

---

## SEZIONE 6 — CORPORATE INTELLIGENCE

### Scopo
Monitoraggio delle dinamiche aziendali: trimestrali, comunicati stampa, lanci di prodotto, cambi di management, crisi, revisioni del consensus. Strutturata su tre livelli con profondità crescente.

---

### Livello 1 — Analisi dei Comunicati Stampa

**Trigger:** qualsiasi comunicato stampa ufficiale rilevante pubblicato nelle ultime 24 ore da aziende di media o grande capitalizzazione sui mercati principali. Non limitato al watchlist personale — copertura globale.

**AI — Analisi Retorica, Semantica e Quantitativa**
Il modello non riassume il comunicato — lo legge in controluce su due piani paralleli.

*Piano qualitativo / retorico:*
- **Tono vs precedenti della stessa azienda:** il linguaggio del management è diventato più cauto o più fiducioso? Confronto esplicito con il comunicato del trimestre/anno precedente.
- **Guidance quantitativa vs qualitativa:** la guidance è espressa con numeri precisi ("EPS atteso tra 3.20 e 3.35$") o vaga ("continuiamo ad aspettarci una crescita resiliente")? La seconda è spesso un segnale di incertezza o di visibilità ridotta sul business.
- **Temi enfatizzati vs minimizzati:** cosa occupa il primo paragrafo (di solito il messaggio che il management vuole passare) e cosa appare solo nelle note a fine comunicato?
- **Discrepanze titolo/corpo:** il titolo del comunicato è positivo ma il corpo contiene revisioni al ribasso delle stime? Questo pattern è sistematicamente identificato.
- **Linguaggio di rischio:** l'azienda ha introdotto nuove clausole cautelative ("subject to market conditions", "pending regulatory approval") che non c'erano prima?

*Piano quantitativo:*
- **Performance storica vs attese:** ricavi, EBITDA, utile netto, EPS — dato effettivo vs consensus vs trimestre precedente vs stesso trimestre dell'anno scorso. Evidenziare la sorpresa in punti percentuali.
- **Margini:** gross margin, operating margin, net margin — trend degli ultimi 4-6 trimestri. L'espansione o compressione dei margini è spesso più informativa del numero assoluto.
- **Flusso di cassa:** free cash flow operativo, variazione del debito netto, buyback e dividendi effettuati o annunciati.
- **Guidance forward:** il management ha fornito guidance per il prossimo trimestre o per l'anno? È stata alzata, abbassata o confermata rispetto alle attese precedenti? La guidance è il dato più market-moving in assoluto — viene estratta e confrontata con il consensus degli analisti.
- **Revisione del consensus:** dopo la pubblicazione, il modello cerca se ci sono già state revisioni delle stime da parte degli analisti e le riporta se disponibili.

Output: per ogni comunicato analizzato, con indicazione della fonte e dell'orario di pubblicazione. La lunghezza varia in base alla rilevanza dell'azienda e alla complessità del comunicato: un comunicato trimestrale di Apple riceverà un'analisi molto più estesa di un aggiornamento di guidance di una mid-cap.

---

### Livello 2 — News Ticker Societario

**Feed:** fonte esterna strutturata (Bloomberg Terminal / Reuters Eikon / Refinitiv API). Copertura su tutti i mercati principali, nessun filtro geografico.

**Formato di ogni notizia:**
Nome azienda | Ticker | Borsa | Titolo della notizia | Fonte | Ora | Variazione di prezzo post-notizia (se già rilevabile)

**AI — Scoring di Rilevanza**
Ogni notizia riceve un punteggio su tre dimensioni (scala 1-3):
1. Impatto sul prezzo atteso
2. Rilevanza settoriale sistemica (la notizia impatta solo questa azienda o l'intero settore?)
3. Novità informativa (era già noto o è genuinamente nuovo?)

Pubblicati solo i titoli con score ≥ 2 su almeno due dimensioni su tre. Le notizie con score massimo (3/3/3) sono evidenziate visivamente con un marker colorato.

---

### Livello 3 — I Leader di Mercato sotto la Lente

**Costruzione automatica del panel**
Il panel non è configurato manualmente. Il modello costruisce e aggiorna automaticamente un panel delle aziende a maggiore influenza settoriale, definita come: capitalizzazione di mercato, peso negli indici principali, e capacità di muovere il sentiment settoriale con le proprie dichiarazioni.

Il numero di aziende per settore non è fisso — dipende dalla struttura del settore stesso. Settori frammentati (es. trasporti) avranno pochi nomi. Settori dominati da leader chiari (tecnologia, difesa) avranno panel più ampi. Il panel include necessariamente i leader per capitalizzazione in: Tecnologia & AI | Energia | Finanziari globali | Farmaceutica | Automotive | Difesa | Lusso europeo | Gestori patrimoniali (es. Berkshire Hathaway).

**Contenuto per ogni azienda del panel con sviluppi rilevanti nelle ultime 24h:**
- Posizionamento competitivo attuale
- Mosse strategiche recenti (M&A, capex, partnership)
- Dichiarazioni del management (con citazione diretta se disponibile)
- Variazioni insolite nei volumi di trading
- Movimenti degli insider (Form 4 per USA, notifiche CONSOB per Italia)
- Cambi nel consensus degli analisti (upgrade/downgrade/target price revision)
- Contesto storico: il trend recente delle ultime settimane

**AI — Commento sui Leader**
Per ogni azienda del panel che mostra sviluppi rilevanti nelle ultime 24 ore, il modello produce un'analisi contestuale di lunghezza proporzionale alla rilevanza dell'evento: "Questo sviluppo è coerente con la traiettoria degli ultimi trimestri? Rafforza o indebolisce la narrativa dominante sul titolo?" Un evento strutturale (fusione, cambio CEO, revisione guidance) riceverà più spazio di una notizia operativa minore.

**Trimestrali:** appaiono sia nel Livello 3 (se l'azienda è nel panel) sia nella §9 (Agenda). Nel Livello 3 si fa l'analisi post-pubblicazione; nell'Agenda si anticipa quello che uscirà.

---

## SEZIONE 7 — THE ECHO CHAMBER — LE VOCI DEL MERCATO

### Nota di priorità
Questa è la sezione editorialmente più importante del report. Richiede il massimo sforzo di elaborazione. Il livello qualitativo dell'analisi qui deve essere superiore a qualsiasi altra sezione.

### Trigger e logica di attivazione
La sezione si attiva per ogni creator solo se ha pubblicato contenuto nelle ultime 48 ore. Se un creator non ha pubblicato, il suo blocco non appare. Non viene mai inserito testo di riepilogo generico per riempire l'assenza.

Il trigger principale del report (e questa sezione) è la pubblicazione di Caffè e Mercati (investire.biz). Gli altri creator vengono controllati in parallelo al momento di generazione.

### Creator configurati (panel iniziale)

| Creator | Canale principale | Frequenza tipica | Formato |
|---|---|---|---|
| Marco Casario | Extra (Telegram), YouTube | Quotidiana | Video + testo |
| Investire.biz | Podcast "Caffè e Mercati", YouTube | Quotidiana | Audio + video |
| Vito Lops | YouTube | 3-4 volte/settimana | Video |
| Starting Finance | YouTube | Variabile | Video |
| Ingegneri in Borsa | YouTube | Variabile | Video |

Il panel è configurabile. L'output è sempre in italiano, indipendentemente dalla lingua della fonte.

**Fonti internazionali attive nel panel:**

| Creator / Newsletter | Canale principale | Frequenza tipica | Valore specifico |
|---|---|---|---|
| Jeff Snider | Podcast Eurodollar University, X | Quotidiana | Unico commentatore sistematico sul mercato eurodollar, repo e liquidità bancaria offshore — copre esattamente la categoria di crisi strutturali invisibili (es. repo crisis 2019, stress bancari) che i creator generalisti ignorano |
| George Gammon | YouTube | 3-5 volte/settimana | Spiega meccanismi complessi (repo, Fed, eurodollar) in modo accessibile; spesso anticipa temi che diventano mainstream settimane dopo |
| The Macro Compass | Substack (Alfonso Peccatiello) | 2-3 volte/settimana | Ex gestore istituzionale obbligazionario; profondità tecnica su bond, politica monetaria e posizionamento macro rara nel formato newsletter; complementa e alza il livello tecnico rispetto ai creator italiani |
| The Bear Traps Report | Newsletter (Larry McDonald) | Variabile | Focalizzato su tail risk, eventi di coda e stress sistemico; tra i più seguiti dagli hedge fund per early warning su crisi bancarie, contagio creditizio, stress valutario |

### Struttura per ogni creator (quando ha pubblicato)

**Header**
Nome creator | Titolo del contenuto | Piattaforma | Data e ora di pubblicazione | Durata (se video/podcast) | Link

**Corpo del riassunto**
Il riassunto è strutturato, completo e altamente fedele al contenuto originale. Non è una sintesi vaga — deve permettere di capire il contenuto senza averlo fruito. La lunghezza è determinata dalla densità del contenuto originale e dalla rilevanza del creator: un episodio di Caffè e Mercati da 45 minuti ricco di dati riceverà un riassunto molto più lungo di un video da 8 minuti di contenuto generico. Include:

1. **Tesi principale:** qual è l'argomento centrale e la posizione del creator su di esso? (1-2 frasi)
2. **Argomenti sviluppati:** lista degli argomenti trattati, in ordine di apparizione, con il dettaglio delle analisi fatte su ciascuno
3. **Dati e grafici citati:** tutti i dati numerici menzionati, con fonte se indicata. Se il creator mostra un grafico, viene descritto: "mostra il grafico del PCE USA da gennaio 2022 a oggi, evidenziando il plateau tra settembre e novembre 2023"
4. **Citazioni dirette rilevanti:** le frasi più significative, riportate virgolettate e in italiano (tradotte se necessario)
5. **Posizione su asset specifici:** se il creator esprime una view direzionale su un asset (bullish/bearish su oro, short su EUR/USD, ecc.), viene riportata esplicitamente con il razionale fornito
6. **Previsioni e target:** qualsiasi previsione quantitativa o temporale viene estratta, classificata (breve/medio/lungo termine) e registrata nel `report_memory.json` per il fact-check futuro

**Materiale allegato**
Se il creator cita o mostra grafici, tabelle, articoli o fonti esterne, questi vengono identificati e, dove possibile, inclusi nel report (o referenziati con link).

---

### AI — Mappa del Consenso e delle Divergenze

Posizionata alla fine della sezione, dopo tutti i blocchi individuali.

**Output 1 — Il Consenso**
Su quali temi le fonti convergono questa settimana? Qual è la narrativa dominante? La convergenza viene espressa con i nomi delle fonti che la condividono.
Esempio: "Casario, investire.biz e Vito Lops convergono sull'attesa di un dollaro in rafforzamento nel Q1 per effetto del repricing Fed. Tre su cinque creator citano il mercato del lavoro americano come il dato-chiave della settimana."

**Output 2 — Le Divergenze**
Dove le visioni si contraddicono nettamente? Su quale tema ci sono posizioni opposte tra fonti di pari autorevolezza? Le divergenze sono il contenuto più prezioso della sezione — indicano aree di incertezza genuina.
Esempio: "Su oro: Starting Finance è bullish strutturale (target 2400$/oz entro Q2), Casario ritiene il movimento recente speculativo e non sostenuto dai fondamentali. Divergenza netta su timeframe e driver."

**Output 3 — Il Fact-Check**
Il modello confronta le previsioni espresse nelle ultime due settimane (archiviate nel `report_memory.json`) con i dati reali della §2 e §4. Non demolisce le fonti — separa ciò che è opinione fondata da ciò che è smentito dai dati. Le previsioni corrette vengono annotate quanto quelle errate.
Quando una previsione risulta errata, l'errore viene segnalato con:
- Nome della fonte (non anonimizzato)
- Previsione originale con data
- Dato reale osservato
- Nota esterna al testo: "[Nota: previsione non confermata dai dati. CPI pubblicato il 14/01 è risultato +3.4% contro target del creator di <3.0%]"

---

## SEZIONE 8 — MEMORIA E CONFRONTO STORICO

### Scopo
Aggiungere la dimensione temporale al report. Trasforma lo snapshot quotidiano in uno strumento di navigazione narrativa.

---

### 8A — IL CAMBIO DI NARRATIVA

**Fonte dati:** `report_memory.json` — archivio degli aggiornamenti precedenti.

**Assi di tracking (modulari e configurabili):**
Gli assi non sono fissi. Il sistema identifica automaticamente quali dimensioni narrative sono attualmente più rilevanti e le traccia. Esempi di assi attivabili:
- Aspettative di politica monetaria (numero di tagli/rialzi prezzati per la prossima riunione)
- Appetito per il rischio (risk-on vs risk-off, misurato da VIX, spread creditizi, flussi verso beni rifugio)
- Tema macro dominante (es. "soft landing" vs "higher for longer" vs "stagflazione")
- Narrativa sul dollaro
- Posizionamento sul mercato azionario USA (multipli, positioning istituzionale)
- Sentiment sulla Cina

Gli assi vengono attivati e disattivati in base al momento: in un periodo dominato dalla narrativa Fed, l'asse delle aspettative monetarie è prioritario. Durante una crisi geopolitica, l'asse del risk appetite diventa centrale.

**AI — Tracking della Narrativa Dominante**
Quando il modello rileva uno shift narrativo significativo (definito come: cambio nel consensus della §7, movimento brusco nei futures su tassi, o dichiarazione market-moving da banca centrale), lo segnala esplicitamente con:
- Data del trigger
- Evento scatenante identificato
- Narrativa precedente → Narrativa attuale
- Asset già repriced e asset che potrebbero seguire

Esempio concreto: "Il 14 gennaio il mercato prezzava tre tagli Fed entro dicembre. Oggi ne prezza uno. Il cambio è iniziato l'8 gennaio dopo i dati NFP più forti delle attese (+256k vs consensus 165k). I verbali Fed del 12 gennaio hanno consolidato il repricing. I growth stock che avevano beneficiato dello 'scenario tre tagli' (-XLK -3.2% nelle ultime 5 sessioni) non hanno ancora completamente aggiustato i multipli."

---

### 8B — IL BAROMETRO DEL CICLO NEL TEMPO

**Grafico**
Mostra la posizione nel ciclo economico degli ultimi 30 giorni, un punto per ogni report, tracciando la traiettoria. Permette di vedere visivamente se ci si sta avvicinando a una fase o allontanando. Aggiornamento quotidiano (se tecnicamente non oneroso, altrimenti la data di aggiornamento è esplicitamente indicata).

**Grafico ausiliario: rotazione settoriale 30 giorni**
Mostra la performance relativa degli ETF settoriali del §2A negli ultimi 30 giorni, normalizzata a zero. Quali settori hanno sovraperformato (linee sopra lo zero) e quali hanno sottoperformato (linee sotto), e come è cambiata la rotazione nel tempo. Non ogni settore avrà la stessa curva — la dispersione è l'informazione.

**Confronti storici quantitativi**
Quando la situazione attuale è paragonabile a un precedente storico con un pattern riconoscibile, il modello lo segnala con dati:
Esempio: "Il livello attuale di inversione della curva (2Y-10Y: -85bp) è paragonabile a quello del Q3 2006. In quel precedente, il mercato azionario continuò a salire per altri 12 mesi prima della correzione." Solo se il confronto è genuinamente informativo — mai per riempire spazio.

---

## SEZIONE 9 — L'AGENDA — COSA SUCCEDE OGGI

### Scopo
Lista operativa degli eventi della giornata. Unica fonte di verità. Tono da calendario, non da articolo.

### Struttura

**AI — Briefing Pre-Agenda**
Prima della lista, il modello identifica:
- L'evento con il maggior potenziale di market-moving della giornata
- Il consensus attuale su quell'evento
- Il rischio di sorpresa (positiva o negativa) rispetto alle attese
- Se più eventi della giornata sono correlati (es. dati macro + discorso Fed), il briefing li mette in relazione

Esempio: "Oggi l'evento-chiave è il CPI USA di dicembre (14:30 CET). Il consensus si aspetta +3.2% headline YoY. Un dato sopra 3.4% potrebbe spingere i futures sui tassi a prezzare ulteriori ritardi nel ciclo di tagli — attenzione a Treasury 2Y e Nasdaq."

**Lista degli eventi (ordine cronologico)**

*Dati Macroeconomici*
Orario (con fuso CET esplicito) | Paese (bandiera) | Indicatore | Impatto (🔴 Alto / 🟡 Medio) | Consensus | Precedente

*Trimestrali Aziendali*
Orario (pre-market / after-hours) | Ticker | Nome azienda | Consensus EPS | Consensus Ricavi | Guidance già comunicata | EPS e ricavi attuali (compilati post-pubblicazione, se già disponibili al momento di generazione)

*Eventi Istituzionali e Discorsi*
Orario | Evento | Tipo (🔴 market-moving / ⚪ routine) | Note

Categorie incluse: discorsi di banchieri centrali (con indicazione se membro votante), riunioni OPEC/OPEC+, vertici G7/G20/WTO, keynote aziendali rilevanti (conferenze Apple, Google I/O, NVIDIA GTC, ecc.), aste di titoli di stato con bid-to-cover ratio atteso.

**Agenda weekend**
Il sabato e la domenica l'agenda è più leggera (mercati chiusi, pochi dati macro) ma rimane attiva per: discorsi di banchieri centrali, sviluppi geopolitici, trimestrali tardive, comunicati stampa aziendali.

---

## SEZIONE 10 — ITALIA FOCUS

### Scopo
Una pagina dedicata esclusivamente all'economia italiana. Modellata sul perimetro editoriale del Sole 24 Ore. Non duplica il FTSE MIB (già in §2) né le aziende italiane eventualmente presenti in §3 e §6 — si concentra sul contesto economico, normativo, politico e sociale italiano.

### Struttura

**10A — Quadro Macro Italia**
Tabella sintetica con i principali indicatori italiani: PIL trimestrale | Inflazione (ISTAT) | Disoccupazione | Spread BTP-Bund | Rating outlook (ultima revisione) | Saldo primario bilancio pubblico. Aggiornata ogni volta che escono nuovi dati, altrimenti mostra l'ultimo dato disponibile con data.

**10B — Politica Economica**
Notizie e analisi su: manovre di bilancio, decreti e leggi economicamente rilevanti, dichiarazioni del governo su temi economici (MEF, Palazzo Chigi, Bankitalia), dossier europei con impatto diretto sull'Italia (PNRR, procedure di infrazione, regolamenti in iter). Lunghezza proporzionale alla rilevanza del giorno.

**10C — Aziende e Mercati Italiani**
Notizie corporate italiane non già coperte in §6: aziende non nel watchlist personale né nel panel di §6 che hanno sviluppi rilevanti. Focus su: FTSE MIB e FTSE Italia Mid Cap, privatizzazioni e partecipazioni statali (Cassa Depositi e Prestiti, MEF come azionista), M&A con aziende italiane, rapporti sindacali rilevanti.

**10D — Fisco e Normativa**
Aggiornamenti su: dichiarazioni fiscali, proroghe, circolari dell'Agenzia delle Entrate economicamente rilevanti, normativa bancaria e assicurativa (Banca d'Italia, IVASS, CONSOB), direttive UE in recepimento. Solo le notizie che impattano operativamente imprese o investitori.

**10E — Immobiliare e Infrastrutture**
Dati sul mercato immobiliare italiano (prezzi, transazioni, mutui), grandi opere infrastrutturali, concessioni, appalti pubblici rilevanti. Sezione più breve, presente solo quando ci sono dati o sviluppi degni di nota.

**Fonte primaria:** Sole 24 Ore, Il Corriere Economia, Milano Finanza, ANSA Economia, comunicati ISTAT e Banca d'Italia, Gazzetta Ufficiale (per decreti).

---

## NOTE OPERATIVE FINALI

### Gestione dei giorni weekend e festivi
Il sabato e la domenica la struttura rimane identica ma le sezioni di mercato (§2, §3) mostrano i dati di chiusura del venerdì con indicazione esplicita. Le sezioni analitiche (§4, §5, §7, §10) si attivano solo se ci sono sviluppi rilevanti. Se un giorno è completamente piatto su tutti i fronti, l'Executive Summary lo dice esplicitamente.

### Gestione degli eventi straordinari fuori orario
Per eventi occorsi di notte, nel weekend, o in modalità flash (es. crollo improvviso, decisione Fed d'emergenza):
- §1 Blocco B riceve una nota di emergenza in cima
- §2 riporta i movimenti post-evento con timestamp preciso
- §5 o §6 ricevono un'analisi specifica
Il report successivo conterrà l'analisi completa retrospettiva.

### Struttura del file `report_memory.json`
```json
{
  "narrative_axes": [
    {
      "axis": "fed_expectations",
      "current": "1 taglio prezzato per dicembre 2025",
      "previous": "3 tagli prezzati per dicembre 2025",
      "shift_date": "2025-01-08",
      "shift_trigger": "NFP dicembre: +256k vs consensus 165k"
    }
  ],
  "creator_predictions": [
    {
      "creator": "investire.biz",
      "prediction": "CPI USA sotto 3% entro febbraio",
      "date_made": "2025-01-05",
      "target_date": "2025-02-28",
      "status": "open",
      "outcome": null
    }
  ],
  "cycle_positions": [
    { "date": "2025-01-13", "position": "rallentamento_avanzato", "confidence": "alta" }
  ]
}
```
