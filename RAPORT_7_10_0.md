# CARPATIA 7.10.0 — interfață nouă și recuperarea profilurilor

Această versiune continuă sursele corectate 7.9.0. Păstrează identitatea CARPATIA V7 PRO REDLINE, licențierea și motoarele existente. Livrarea este un ZIP cu surse; nu include un EXE validat.

## Blocajul raportat: „există deja un backup, dar nu îl pot restaura”

Au fost reproduse și corectate mai multe cauze distincte:

- Butoanele de restaurare CPU/GPU/Internet trimiteau utilizatorul către setările individuale. O sesiune de profil putea rămâne activă după restaurarea originalelor, iar acea listă nu mai conținea nimic. Butoanele execută acum restaurarea profilului prin endpointul propriu, după confirmare. Sesiunile fără pași sau cu originale deja restaurate pot fi finalizate fără scrieri Windows fictive, apoi profilul poate fi reaplicat.
- Un singur jurnal de profil invalid putea împiedica afișarea tuturor celor trei profiluri. Acum este afișat separat ca diagnostic, fără a ascunde stările celorlalte.
- Inventarul de rețea vechi nu includea câmpul `success`. Centrul de recuperare trata astfel o citire reușită drept eșec. Scriptul confirmă explicit succesul; coordinatorul acceptă și forma veche cu inventar valid.
- Un jurnal Internet gol putea fi blocat de un profil independent activ. El poate fi acum finalizat fără modificări Windows și fără ștergerea originalelor altor module.
- Tranzacțiile de copie manuală corupte blocau inclusiv accesul la restaurarea originalelor automate. Acum sunt afișate ca diagnostic; originalele automate rămân accesibile. Aplicarea de modificări noi rămâne blocată când starea unei tranzacții nu este cunoscută.
- Tranzacțiile valide fără rânduri nu pot fi sursa unor scrieri Windows. Sunt păstrate și afișate ca „Fără scrieri”, fără a bloca alte copii.
- Modificarea istoricului sau a unui mesaj de diagnostic putea invalida confirmarea restaurării, chiar dacă originalele și valorile reale nu se schimbaseră. Compararea urmărește acum originalele și starea efectivă.
- Un fișier de istoric cu JSON `null` sau un jurnal Internet corupt putea opri întregul inventar. Erorile sunt prezentate ca diagnostic, fără a ascunde celelalte opțiuni de recuperare.

Nu sunt șterse sau reconstruite artificial originale lipsă. Un backup invalid, o schimbare externă reală sau un refuz Windows rămân erori explicite. Setările nu sunt declarate restaurate numai pentru a debloca butoanele.

## Copii manuale

Crearea copiei are un serviciu separat, cu raport persistent salvat înainte de citirea identității calculatorului. Eșecul identității lasă un raport nereușit; eșecul copierii originalelor lasă o copie parțială, cu capturile valide accesibile. Erorile interfeței de progres nu întrerup capturarea.

Capturile nereversibile și cele cinci setări GPU cu ținte ambigue sunt omise explicit. Operațiile neaplicabile raportate de motor sunt omise distinct de erori. O copie goală nu este declarată reușită. Capturile păstrează manifestul și amprentele verificate înainte de restaurare.

Copiile întrerupte după salvarea unor fișiere pot fi redeschise din Istoric chiar dacă raportul final nu a fost scris. Restaurarea verifică în continuare fișierele reale, identitatea și amprentele; simpla existență a unei înregistrări nu permite scrierea în Windows.

## Interfață

- Dashboard refăcut: carduri CPU/GPU/Internet, nivel și restaurare directă, monitorizare compactă și acces imediat la recuperare.
- Centru de backup împărțit în profiluri, originale automate, copii manuale și istoric, cu numere calculate din inventarul real.
- Progres la capturare, erori și stări parțiale vizibile. Tranzacțiile nevalide nu afișează butoane false de reluare/anulare.
- Căutare/navigare rapidă cu Ctrl+K, săgeți, Enter și Escape; căutare în catalog și revenirea focusului la închidere.
- Navigație, tipografie, spațiere și suprafețe noi în aceeași paletă roșu/negru; stilul se adaptează la desktopuri de 1024 px fără scroll orizontal.
- Mod de mișcare redusă și păstrarea politicii CSP. Fără biblioteci noi, fonturi externe sau telemetrie.

## Verificări executate

| Verificare | Rezultat |
| --- | --- |
| `npm test`, Node 24.19.0, teste PowerShell activate | **270 teste trecute, 0 eșuate, 0 omise** |
| Chromium 154, interfață reală cu bridge Electron simulat | **17 scenarii trecute**, fără erori JS/CSP/resurse |
| Parsare PowerShell 7.5.4 | **31 scripturi**, zero erori |
| Fișiere incluse în installer și sintaxa JavaScript | Verificate de suita de teste |
| Manifestul HTML | 155 ID-uri unice, fără duplicate |
| Dependențe instalate | Electron 45.0.0, electron-builder 26.18.0, versiunea surselor 7.10.0 |

Scenariile Chromium includ explicit profil blocat → confirmarea restaurării directe → finalizarea sesiunii → analiză și reaplicare. Testele backend verifică și toate cele trei profiluri cu originale deja restaurate. Se verifică separat anularea, schimbările externe, originalele lipsă, tranzacțiile corupte, copierea parțială și fișierele de istoric invalide.

Rezultatele și capturile sunt în `validation/7.10.0/`. Datele hardware din capturile de ecran sunt fixture-uri de test, nu benchmarkuri.

## Limite înainte de distribuire

Operațiile reale Windows, PowerShell 5.1, restartul, driverele și installerul nu au fost executate aici. Sunt necesare testele VM Windows existente înainte de utilizarea comercială. Nu se afirmă câștiguri de FPS sau latență.

Cheia publică originală `license-public.pem` lipsește în continuare; verificarea de build refuză distribuirea. Nu a fost înlocuită cheia editorului. Cele cinci tweakuri GPU ambigue rămân blocate la aplicare, cu restaurarea originalelor existente păstrată. Graful dependențelor nu a fost schimbat: cele 15 alerte moderate documentate pentru uneltele de build rămân în raportul npm anterior.

Pentru un backup deja corupt sau un original efectiv absent, această versiune oferă diagnosticul și acces la celelalte restaurări; nu poate inventa valorile care nu au fost salvate.
