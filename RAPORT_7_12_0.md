# CARPATIA 7.12.0 — profiluri adaptive, opțiuni și sunet

## Problema din fotografie

CPU Ultra se oprea complet pe un sistem cu 7,4 GB RAM utilizabili deoarece o singură opțiune cerea peste 8 GB. Acum sunt omise numai setările incompatibile și sunt afișate motivele în confirmare. Regula CPU 002 este omisă explicit dacă lista reală de planuri Windows confirmă că High Performance lipsește, inclusiv pe sisteme Modern Standby. Nu se creează sau activează forțat un plan nou. Erorile de acces, inventarele necunoscute și jurnalele corupte continuă să oprească aplicarea.

Dacă disponibilitatea se schimbă după previzualizare, clientul trebuie să analizeze și să confirme din nou înainte de orice scriere. Jurnalele istorice se pot restaura și când hardware-ul actual ar omite o setare.

## Opțiuni și niveluri

| Profil | LOW | MEDIUM | HIGH | ULTRA |
|---|---:|---:|---:|---:|
| CPU | 2 | 3 | 6 | 11 |
| GPU | 2 | 4 | 5 | 6 |
| Internet | 2 | 4 | 6 | 8 |

Acestea sunt numerele maxime de operații ale presetului standard înainte de adaptare; o operație poate modifica mai multe valori. Nu sunt estimări de FPS. Serviciul auxiliar opțional adaugă o operație. Alegerea „Păstrează DNS” elimină operația DNS.

- CPU adaugă preferința EPP 25 și disponibilitatea nucleelor pe alimentare AC, folosind motorul de alimentare existent și păstrând valoarea/planul original. Ultra poate crește temperatura și consumul. Windows Search poate fi oprit numai prin bifarea opțiunii auxiliare; căutarea poate deveni mai lentă.
- GPU extinde controlul capturii istorice/audio Game DVR. Partajarea Windows Media este opțională și este omisă dacă serviciul lipsește. Există acces direct la alegerea executabilului pentru preferința GPU Windows. Nu sunt adăugate chei de driver cu efect neverificat.
- Internet permite Google, Cloudflare, Quad9 sau păstrarea DNS-ului actual pe adaptorul selectat. Nivelurile includ progresiv RSS, auto-tuning Normal, Fast Open și temporizare TCP. Oprirea SSDP este opțională și poate afecta descoperirea UPnP/DLNA.
- Catalogul public are 215 intrări cu motoare, dintre 241 de intrări de audit. Cele cinci reguli noi au descrieri în română și folosesc motoare cu original și restaurare. Regulile vechi și structura originalelor lor nu au fost modificate.

## Backup și restaurare

Reutilizarea unui backup DNS verifică adaptorul salvat și pe cel selectat. Reutilizarea unei valori CPU verifică și planul de alimentare pe care profilul îl va utiliza. Nu mai este preluat un original al altei ținte doar fiindcă ID-ul setării coincide. Backupurile independente rămân utilizabile, iar conflictele reale arată sursa de restaurat.

Captura serviciilor refuză tipurile Registry incompatibile înainte de scriere, prevenind transformarea unui original text/QWORD într-un DWORD imposibil de restaurat exact. Backupul TCP vechi se publică atomic înaintea modificării Windows și refuză suprascrierea unui original existent. Tipul și absența valorilor sunt păstrate.

## Sunet

Cele trei pictograme au un efect scurt de vânt metalic, sintetizat local prin Web Audio, aproximativ 0,34–0,39 secunde. Nu există descărcări audio sau buclă continuă. Prima interacțiune reală de mouse/tastatură permite pornirea sunetului; apoi funcționează la hover/focus/atingere. Butonul SUNET ACTIV/OPRIT memorează preferința. Există limitare de suprapunere, pauză și eliberarea resurselor. Mostra WAV este randată din același sintetizator folosit de aplicație.

## Verificări și limite

- 376 teste automate trecute, zero eșecuri/anulări/omisiuni, cu PowerShell 7.5.4 configurat pentru regresiile Windows simulate.
- 56 scenarii Chromium trecute, fără erori JavaScript/CSP/resurse. Includ scenariul 7,4 GB + High Performance absent, motivele omiterii, DNS/serviciu selectat, mute/unmute cu gest real și randarea PCM a sintetizatorului fără clipping.
- 38 scripturi PowerShell verificate sintactic. Teste țintite suplimentare acoperă validarea DNS strictă și instrumentul de verificare Windows.
- Instrumentul Windows verifică valorile inițiale după restaurare și citește independent originalele. Un restart este declarat verificat numai dacă identitatea instalării coincide și timpul pornirii Windows a avansat. Nivelul implicit este LOW; nu există restart automat. Instrucțiuni: VALIDARE_WINDOWS.md.
- performance.json păstrează două măsurători de renderer Chromium pe Linux, cu mișcare normală/redusă. Acestea nu măsoară FPS, memorie totală a aplicației, Windows sau performanța înainte/după optimizarea unui PC.

Mediul disponibil este Linux. Nu au fost executate scrieri reale, restart, drivere sau installer pe Windows. Testele live Shopify/email și configurarea locală a cheilor de producție rămân necesare înainte de vânzare. Buildul este blocat intenționat până la instalarea cheii publice; nicio cheie privată nu este inclusă. Cele 15 alerte moderate ale dependențelor de build din auditul anterior și cele cinci reguli GPU ambigue blocate la aplicare rămân documentate. Nu se promit toate bugurile eliminate sau câștiguri de FPS.

Arhiva este sursă. Actualizează într-un folder nou și păstrează datele de backup și perechea de chei existentă. Mediul cloud nu a fost republicat.
