# CARPATIA 7.13.0 — Performanță & Tweaks și GPU Studio

## Navigare și servicii

Există un singur tab **Performanță & Tweaks**, cu pictograma fulger păstrată. Pagina de profiluri ULTRA duplicată a fost eliminată din interfață; profilurile CPU/GPU/Internet rămân în Dashboard. Rutele vechi de navigare ajung în tabul unificat. Motoarele și restaurarea backupurilor ULTRA mai vechi sunt păstrate.

SysMain, WSearch și DiagTrack sunt organizate în aceeași pagină. Fiecare are rolul și compromisurile explicate, stare citită din Windows și acțiune manuală. SysMain poate ajuta pornirea aplicațiilor, WSearch indexarea/căutarea, iar DiagTrack diagnosticul Windows. Nu sunt oprite automat pentru a simula un câștig de performanță. După scanare, explicațiile inițiale sunt înlocuite de rândurile cu starea reală.

## GPU Studio

- Alegerea executabilului jocului, starea preferinței Windows, aplicare cu original, recitire și restaurare directă pentru acel joc.
- Alegerea este păstrată în datele locale pentru redeschidere. Selectorul validează un singur fișier .exe și verifică operațiile în curs înainte și după dialog.
- Aplicarea nu mai este blocată de toate backupurile CPU/catalog independente. Originalul GPU al executabilului rămâne protejat de motorul său; o restaurare de copie neterminată continuă să blocheze modificările incompatibile.
- Un Apply nereușit recitește starea și expune imediat originalul păstrat. Butoanele nu pretind succes până la recitirea preferinței reale.
- Șase valori opționale: încărcare, temperatură, frecvență nucleu/memorie, consum și ventilator. Graficul păstrează maximum 48 de citiri, la 5 secunde, numai când pagina este vizibilă. Senzorii nedisponibili apar cu liniuță; nu se inventează valori.
- Senzorii provin din serverul local LibreHardwareMonitor deja acceptat de aplicație; nu este instalat automat. Datele de frecvență și ventilator nu sunt confundate cu cele CPU.

Aceasta este preferința grafică Windows și monitorizare. Nu există încă integrare NVAPI/ADL/driver pentru overclock, undervolt, limită de putere sau curba ventilatorului ca în MSI Afterburner. Aceste controale nu sunt simulate prin cursoare fără efect.

## Profiluri și rezultate

După aplicare, Dashboardul arată numărul de opțiuni modificate, deja configurate, reutilizate și omise. Rezumatul este salvat cu sesiunea și separat de potrivirile citite acum din Windows. Un rezumat informativ invalid nu blochează restaurarea originalelor valide. Un eșec al ultimei scrieri în jurnal declanșează rollback fără restaurarea accidentală a originalelor individuale doar reutilizate de profil.

Aceste numere arată operațiile realizate, nu FPS câștigate. Nu au fost adăugate setări de securitate sau servicii oprite automat în cele trei profiluri. Ajustările pentru 7,4 GB RAM și lipsa High Performance sunt păstrate.

## Protecție Windows

Panoul separat citește Microsoft Defender, protecția în timp real, Tamper Protection, modul pasiv și existența originalului CARPATIA. Oprirea protecției are confirmare explicită și folosește motorul oficial existent, cu original și restaurare. Este disponibilă numai când Windows raportează starea completă, Defender rulează normal, iar setarea nu este protejată de Tamper Protection.

Starea este recitită după confirmare și după operație. Motorul revalidează protecția înainte de scriere și cere confirmarea reală a opririi, nu doar succesul apelului de preferință. Nu sunt modificate Tamper Protection, politicile, serviciile sau driverele pentru a ocoli restricțiile Windows. Când setarea este protejată, există acces direct la pagina oficială Securitate Windows. Restaurarea originalului rămâne accesibilă fără licență activă; eventualele erori păstrează originalul.

Oprirea protecției reduce protecția calculatorului și nu garantează performanță mai mare. Nu este inclusă în profilurile automate și nu este aplicată la pornire. Windows poate reactiva protecția conform propriilor politici. Nu au fost efectuate schimbări reale de protecție în mediul de lucru.

## Verificări

- **414 teste trecute**, zero eșecuri, anulări sau omisiuni, inclusiv toate regresiile PowerShell simulate.
- **66 scenarii Chromium trecute**, fără erori JavaScript/CSP/resurse. Sunt verificate navigarea unificată, serviciile, starea protejată Defender/accesul oficial, GPU selectare/aplicare/recitire/restaurare, telemetria/graficul, layoutul compact și rezumatele Dashboard, împreună cu fluxurile anterioare.
- **41 scripturi PowerShell** parsează fără erori. Testele Defender folosesc API-uri simulate: verifică interzicerea scrierii cu protecție activă/necunoscută și respingerea unui succes aparent fără modificarea stării reale.
- Verificările IPC includ selecția GPU persistentă, respingerea intrărilor invalide și aplicarea alături de originale Dashboard independente.

Logurile, capturile și măsurătorile rendererului sunt în `validation/7.13.0/`. Captura GPU folosește valori etichetate „Date de test Chromium”; acestea nu sunt rezultate ale unui PC Windows. Măsurătorile rendererului Linux nu reprezintă un benchmark de joc sau de installer.

## Limite de lansare

Mediul disponibil este Linux. Installerul, scrierile reale, restartul, driverele și protecția Windows nu au fost validate pe Windows. Nu se afirmă creșteri de FPS sau eliminarea tuturor bugurilor posibile. Controlul hardware GPU complet rămâne de implementat.

Cheia publică de producție nu este configurată în aceste surse și buildul rămâne blocat până la configurarea locală prin manager. Nu au fost generate sau distribuite chei private. Conectarea Shopify/email și validarea installerului rămân pașii pentru lansare. Cele 15 alerte moderate ale dependențelor de build și cele cinci reguli GPU ambigue blocate la aplicare, documentate anterior, rămân neschimbate.

ZIP-ul conține sursele. Actualizează într-un folder nou și păstrează datele de backup și cheile existente. Mediul cloud nu a fost republicat.
