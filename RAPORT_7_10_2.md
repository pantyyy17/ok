# CARPATIA 7.10.2 — verificare sistematică și corecții suplimentare

Revizia pornește de la 7.10.1. Au fost analizate din nou motoarele Windows, datele de backup, recuperarea, profilurile, rețeaua, licențierea, uneltele de emitere și interfața. Au fost reparate defectele concrete de mai jos, cu regresii executabile. Nu se declară eliminarea tuturor bugurilor posibile de pe orice configurație Windows.

## Originale și restaurare

- Motoarele Registry și native validează originalele și valorile cerute înainte de jurnal și înainte de prima modificare. Nu mai creează o modificare cu un original de tip incompatibil pe care restaurarea l-ar refuza.
- Capturarea Registry refuză valorile QWORD din afara intervalului sigur JavaScript ±9.007.199.254.740.991. Restaurarea PowerShell a originalelor Int64 istorice păstrează suportul complet; nu sunt rotunjite sau trunchiate valori pentru a trece validarea.
- Restaurarea ULTRA validează selecția și toate rândurile originale: copiile trunchiate, duplicate sau necorespunzătoare selecției sunt refuzate înainte de IO Registry.
- GPU verifică tipul original și întregul șir de preferințe, inclusiv opțiunile HDR; un original de tip neacceptat nu mai este înlocuit pierzându-și tipul.
- Startup validează întregul jurnal înainte de a restaura prima intrare. Services refuză valori delayed-start pe care nu le poate restaura exact.
- Copiile manuale validează capturile împreună, nu doar separat. Originalele contradictorii pentru aceeași țintă produc un raport parțial, fără a declara copia completă. Adaptoarele și numele din manifest nu mai pot dubla sau suprascrie aceeași captură.
- Schemele native de stare, identitatea și originalele salvate sunt validate înainte de previzualizare/tranzacție. Metadatele suplimentare care ar face verificarea de după scriere imposibilă sunt refuzate.
- Previzualizarea restaurării unei valori de alimentare citește planul salvat în backup, nu planul activ între timp.
- Proprietatea originalelor individuale reutilizate rămâne corectă când aplicarea unui profil este întreruptă. Profilul este declarat aplicat numai după confirmarea pașilor în jurnalele persistente.
- Anularea unei copii care nu schimbase nimic nu mai inventează necesitatea unui restart.
- O cale de istoric devenită fișier sau inaccesibilă produce diagnostic, fără să ascundă întregul inventar de recuperare. Fișierele respective sunt păstrate.

## Rețea

- TCP și NIC Power regăsesc adaptorul după GUID chiar dacă numele s-a schimbat. GUID-urile sunt normalizate consecvent.
- Selectarea adaptorului folosește aliasul exact, fără interpretarea caracterelor de wildcard ca selecție pentru alt dispozitiv.
- Modificările proprietăților NIC folosesc obiectele CIM precise și parametrii documentați. API-urile au fost comparate cu documentația oficială MicrosoftDocs.
- DNS aplică/restaurează numai configurația IPv4 salvată. Setările IPv6 care nu au fost capturate rămân separate. Verificarea DNS confirmă și modul static/automat, nu doar o listă de adrese care ar putea coincide cu DHCP.
- Inventarul și planul NET BOOST păstrează GUID-ul. Dacă alt adaptor reutilizează același nume după confirmare, commitul este refuzat înainte de modificări.

## Licențe și serviciul de vânzare

- Istoricul emitentului are aceeași validare la afișare și emitere. Un istoric corupt nu mai poate fi acceptat la emitere după ce fusese declarat invalid în interfață.
- Eșecul salvării istoricului curăță fișierele noi neconfirmate și temporarele; nu lasă coduri orfane după o emitere raportată ca nereușită.
- Managerul de licențe decodează UTF-8 după colectarea octeților. Numele românești împărțite între pachete HTTP nu mai sunt corupte.
- Corpurile HTTP prea mari primesc 413, fără resetarea incorectă a conexiunii; limitele sunt măsurate în octeți.
- Sunt refuzate datele de expirare/client de tip greșit și destinatarii cu adrese evident invalide înainte de emitere.
- CLI-ul utilizează căile canonice verificate, inclusiv în prezența junction/symlink și `..`.
- Au rămas active verificările de semnătură, deduplicarea comenzilor, persistența și protecțiile de retry. Nu au fost trimise emailuri sau emise licențe reale în timpul testelor.

## Interfață și coordonare

- O notificare către un renderer închis/crăpat nu mai transformă o operație Windows finalizată într-un eșec sau într-o întrerupere artificială a recuperării.
- Căutarea globală în catalog elimină categoria și tipul de operație rămase de la o căutare anterioară, astfel încât rezultatele să nu dispară din cauza unui filtru fără legătură.
- Debloat afișează acum rezultatul pe fiecare aplicație, inclusiv mesajele de refuz Windows. Un eșec parțial nu mai trimite utilizatorul să „verifice erorile” după ce lista fusese golită fără detalii.
- Răspunsurile Debloat incomplete sunt tratate ca rezultat neconfirmat, nu ca succes gol. Scanarea nereușită afișează eroarea.

## Rezultate executate

| Verificare | Rezultat |
| --- | --- |
| `npm test`, Node 24.19.0, toate testele PowerShell activate | **301 teste trecute, 0 eșuate, 0 omise** |
| Chromium 154, DOM real cu bridge Windows/Electron simulat | **29 scenarii trecute**, fără erori JS/CSP/resurse |
| Parsare PowerShell 7.5.4 | **36 scripturi**, zero erori |
| Verificarea dependențelor locale ale installerului și sintaxa JavaScript | Incluse în testele trecute |
| Integritatea surselor și arhivei | Manifest SHA-256 și verificare ZIP înainte de livrare |

Regresiile includ operații Windows simulate folosind ramurile reale ale scripturilor, originalele safe/unsafe QWORD, restaurare după redenumirea adaptorului, DNS automat/static, evitarea modificărilor IPv6, erori HTTP și emitere, conflicte între capturi, jurnale și recuperare. Testele HTTP folosesc servere locale și chei temporare în afara proiectului.

Rezultatele sunt în `validation/7.10.2/`. Capturile hardware sunt fixture-uri; nu reprezintă benchmarkuri.

## Limite rămase

Nu au fost executate modificări reale pe Windows, PowerShell 5.1, restartul calculatorului, driverele sau installerul. Sunt necesare scenariile VM/hardware Windows înainte de distribuire. Nu se afirmă câștiguri de FPS sau latență.

Cheia publică originală lipsește; buildul comercial rămâne blocat. Nu a fost generată o cheie înlocuitoare și nu a fost eliminată licențierea. Cele cinci opțiuni GPU cu ținte ambigue rămân blocate la aplicare. Graful npm nu a fost schimbat; cele 15 alerte moderate din uneltele de build rămân documentate în auditul anterior.

Un original efectiv absent sau un jurnal imposibil de interpretat nu poate fi reconstruit automat. Aceste cazuri sunt păstrate și raportate, fără valori inventate sau ștergeri care ar ascunde problema.
