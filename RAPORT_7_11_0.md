# CARPATIA 7.11.0 — activare și pregătirea primei lansări

Clientul poate introduce codul primit după cumpărare sau importa fișierul .txt. Activarea validă se salvează și este reverificată la următoarea pornire. Interfața păstrează stilul Redline și munții animați CPU/GPU/Internet din 7.10.3.

## Corecții și comportament

- Ecran nou de activare cu stări de verificare, eroare, succes, detalii despre client/expirare, reîncercare și introducerea unui cod nou. După succes, butonul DESCHIDE CARPATIA permite intrarea explicită; activarea deja salvată intră direct la pornire.
- Codurile copiate din email pe mai multe linii sunt normalizate fără modificarea caracterelor semnate. Intrarea brută rămâne limitată la 8192 de caractere. Un cod greșit nu suprascrie licența validă și nu este salvat.
- Importul nativ deschide selectorul sistemului numai din fereastra autorizată, acceptă un singur .txt, limitează dimensiunea și verifică strict UTF-8. Anularea păstrează activarea existentă. Nu permite schimbarea licenței în timpul unei operații Windows.
- Restaurarea rămâne accesibilă fără licență activă. Lipsa sau invaliditatea cheii publice produce o stare explicită și dezactivează încercările care nu pot reuși.
- Răspunsurile vechi de stare nu suprascriu o activare mai nouă. Tastatura rămâne în dialog, Ctrl+K nu deschide căutarea în spatele lui, iar închiderea readuce focusul.
- Cheia publică este parsată o singură dată pe instanța verificatorului; semnătura și expirarea licenței salvate sunt verificate în continuare la autorizare. Interfața folosește un singur timer pentru expirare și grupează verificările la revenirea în fereastră. Nu sunt raportate câștiguri de FPS.
- Materialul privat nu este acceptat drept cheie publică în verificatorul clientului, verificarea de build, manager și server.

## Emitere și Shopify

- Emiterea independentă de magazin a fost extrasă în `seller/fulfillment-core.js`. Shopify verifică semnătura webhookului, plata finalizată și varianta configurată înainte să o apeleze.
- Codul și emailul complet sunt salvate atomic în SQLite. Reîncercările păstrează același cod, expeditor, subiect, conținut și URL de descărcare. Retrimiterile webhook și restartul nu emit coduri duplicate.
- Baza reține amprenta editorului și refuză schimbarea accidentală a cheii. Migrarea bazelor mai vechi verifică o licență existentă înainte de asociere; emailurile vechi deja încercate cu conținut necunoscut trec în verificare manuală.
- Au fost întărite validarea comenzilor de test și respingerea caracterelor de control în adrese/expeditor. ID-urile Shopify mari rămân exacte.
- `node seller/status.js /srv/carpatia-data` raportează numai numerele de licențe și stările livrării, fără coduri sau adrese.
- Managerul arată folderul privat, amprenta publică și potrivirea cu proiectul; exportă numai cheia publică. Documentația explică prima lansare pe PC și păstrarea aceleiași perechi după mutarea de la Shopify la site propriu.

## Verificări păstrate

- **343 teste trecute**, zero eșecuri, anulări sau omisiuni, cu Node 24.19.0 și PowerShell 7.5.4. Sunt păstrate atât rularea completă fără izolare de procese, cât și comanda oficială `npm test` cu `CARPATIA_PWSH` configurat. Operațiile Windows sunt simulate în aceste teste Linux.
- **51 scenarii Chromium trecute**, fără erori JavaScript/CSP/resurse. Activarea folosește Ed25519 și stocare reale cu chei temporare de test printr-o legătură CDP; operațiile Windows și selectorul de fișier sunt simulate. Sunt acoperite codul greșit, codul valid împărțit pe linii, succesul, redeschiderea, înlocuirea invalidă, importul/anularea, lipsa cheii publice, restaurarea și layoutul compact. Sunt reverificate și munții animați, catalogul, confirmările, restaurarea și reaplicarea profilurilor.
- **36 scripturi PowerShell** parsează fără erori; această verificare nu le execută.
- Integritatea includerii modulelor și activelor în installer este verificată de teste. Nu a fost compilat sau rulat un EXE Windows.

Loguri și capturi: `validation/7.11.0/`. Capturile afișează exclusiv date de test. `validation/source-manifest.json` inventariază hashurile surselor; nu este o semnătură digitală a editorului.

## Configurare necesară înainte de vânzare

Aceasta este prima lansare, fără licențe de producție emise. Nu am generat chei de producție în mediul partajat. `license-public.pem` lipsește încă, iar `npm run release:check` blochează intenționat buildul. Pe PC-ul editorului, `LICENTA_MANAGER.bat` creează o singură pereche într-un folder privat extern și copiază numai cheia publică în proiect. Vezi `LICENTIERE.md`.

Automatizarea Shopify nu este conectată live: lipsesc găzduirea privată persistentă, configurarea webhookului, secretele serverului/Resend, expeditorul verificat și URL-ul installerului. Nu au fost trimise emailuri sau procesate plăți reale. Un adaptor de plată pentru site-ul viitor trebuie implementat separat, reutilizând emiterea și cheia existente. Nu există blocare per dispozitiv sau revocare offline la refund.

Înainte de distribuție sunt necesare compilarea și validarea installerului pe Windows 10/11, inclusiv activarea, restartul, backupul/restaurarea și operațiile hardware reale. Cele cinci opțiuni GPU cu ținte ambigue rămân blocate la aplicare, cu restaurarea păstrată. Cele 15 alerte moderate ale dependențelor de build, documentate în auditul anterior, nu sunt rezolvate de această revizie; dependențele nu au fost schimbate.

ZIP-ul conține sursele și instrumentele editorului. Clienților se distribuie numai installerul validat. Mediul cloud nu a fost republicat.
