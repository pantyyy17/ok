# CARPATIA 7.10.1 — verificarea finală a fluxurilor

Această revizie include interfața și reparațiile de recuperare din 7.10.0, descrise în `RAPORT_7_10_0.md`, plus o corecție suplimentară confirmată în motorul de restaurare.

## Corecții suplimentare

Restaurarea unei copii manuale poate întâlni un serviciu sau un MTU scris numai parțial, după o întrerupere ori un refuz Windows. Motorul compara anterior obiectul complet cu starea de dinainte; un amestec valid de componente salvate era clasificat drept modificare externă. Astfel, atât reluarea cât și anularea puteau rămâne blocate.

Motorul folosește acum verificarea comună pentru operații compuse. Sunt acceptate numai componente care încă au valoarea salvată de dinainte sau valoarea țintă. O a treia valoare este refuzată înainte de scriere. Rezultatul este recitit după scriere, iar jurnalul rămâne păstrat la eșec. Nu sunt inventate originale și nu sunt suprascrise automat valori necunoscute.

Au fost adăugate teste PowerShell executabile care folosesc ramura reală de scriere cu IO simulat: servicii și MTU, restaurare și anulare întrerupte, plus schimbări externe care trebuie să provoace zero scrieri. Testul de integrare include 19 aserțiuni PowerShell.

Confirmările din interfață pornesc acum pe „Anulează”, păstrează focusul în dialog la navigarea cu Tab și readuc focusul la acțiunea inițială după închidere. Dacă acea acțiune nu mai există, focusul revine la modulul curent.

## Verificare ca utilizator

Suita Chromium a fost extinsă la inventarierea aplicațiilor Debloat, GPU, TEMP și Startup, diagnosticul aplicației, citirea hardware, navigare și confirmări. Sunt verificate în continuare căutarea, filtrarea, restaurarea profilului blocat și reaplicarea lui, anularea, Ctrl+K și layoutul de 1024 px.

| Verificare executată | Rezultat |
| --- | --- |
| `npm test`, Node 24.19.0, PowerShell activat | **271 teste trecute, 0 eșuate, 0 omise** |
| Chromium 154, DOM real cu bridge Windows/Electron simulat | **26 scenarii trecute**, fără erori JavaScript/CSP/resurse |
| Parsare PowerShell 7.5.4 | **32 scripturi**, zero erori |
| Revizie independentă profile/ledger/runner/autorizare/launch/build | Nu au fost identificate alte defecte concrete în acest pas; testele țintite au trecut |

Capturile și rezultatele sunt în `validation/7.10.1/`. Datele hardware sunt fixture-uri de test. Nu au fost măsurate FPS, temperaturi, consum sau latențe reale.

## Limite păstrate

Operațiile reale Windows, PowerShell 5.1, restartul și installerul necesită validarea pe VM/hardware Windows. Lipsa cheii publice originale continuă să blocheze buildul comercial; licențierea nu a fost eliminată. Cele cinci tweakuri GPU ambigue rămân blocate la aplicare. Alertele moderate din uneltele de build sunt documentate în raportul npm anterior și nu au fost mascate prin retrogradări incompatibile.

Un original efectiv absent sau un jurnal corupt nu poate fi reconstruit în siguranță. Aceste cazuri rămân diagnostice explicite, cu celelalte restaurări accesibile. Această livrare repară defectele reproduse și testate; nu reprezintă o garanție că orice configurație Windows este lipsită de probleme.
