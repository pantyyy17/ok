# CARPATIA 7.15.0 — un singur munte, operații mai ușoare

## Interfață simplificată

Fiecare profil CPU, GPU și Internet are un singur buton: muntele. Profilul inactiv deschide analiza și confirmarea aplicării; profilul activ folosește același munte pentru confirmarea restaurării. După restaurare revine la analiză. Un jurnal invalid deschide diagnosticul backupului, fără scrieri și fără un buton aparent mort.

Munții au fețe și creste noi, zăpadă, contur și etichete mai clare. Animația de repaus folosește un singur strat de opacitate per munte; animațiile continue de filtre și contururi au fost eliminate. Mișcarea redusă și oprirea animațiilor în afara Dashboardului sunt păstrate.

Nivelul rămâne vizibil pe card. Adaptorul, DNS-ul și serviciile opționale sunt grupate în „Opțiuni avansate”. Adaptoarele se citesc numai când zona este deschisă. Un clic pe Internet fără adaptor selectat deschide și focalizează această zonă, fără aplicare.

Starea de lucru și progresul apar imediat, înainte de răspunsul Windows. Sunt prevenite clicurile duplicate, iar răspunsurile vechi nu pot debloca sau rescrie o operație curentă. Anularea confirmării nu mai pornește o nouă inventariere. Focusul tastaturii revine după reactivarea butonului.

## Cauza întârzierilor și schimbarea transportului Windows

Versiunea anterioară pornea PowerShell separat pentru fiecare citire și fiecare setare. După fiecare scriere recitea întregul inventar; restaurarea făcea inventar înainte și după fiecare pas. Actualizarea interfeței repeta verificările tuturor profilurilor și încărca pagini ascunse.

Noua versiune folosește o singură sesiune PowerShell pentru fiecare operație de analiză, aplicare sau restaurare. Cererile sunt secvențiale și corelate, cu limite de intrare/ieșire. Aceleași motoare verifică tipurile, salvează originalul înainte de scriere și recitesc rezultatul. Jurnalul profilului este salvat înaintea fiecărei modificări; operațiile nu sunt trimise ca un lot fără evidența pașilor.

- Citirile profilului sunt grupate. Confirmarea reface verificările necesare; nu reutilizează rezultate expirate.
- Pentru CPU Ultra, inventarele complete sunt reduse la 1 la analiză, 2 la aplicare și 2 la restaurare, indiferent de numărul de pași. Citirile țintite și verificările reale ale motoarelor rămân active.
- Preflightul cere doar Windows build, RAM utilizabil și GPU. Nu mai scanează discuri, baterii, carcasă sau alte componente fără legătură.
- Starea salvată a profilurilor și inventarul centrului de recuperare nu pornesc verificări Windows ale tuturor profilurilor. Verificarea live este cerută explicit și este etichetată separat de rezumatul salvat.
- Paginile ascunse nu mai sunt reîncărcate după fiecare modificare de pe Dashboard.
- Avertismentele driverelor sunt păstrate separat, fără să strice protocolul JSON. Ieșirile de host/verbose/debug nu sunt confundate cu un răspuns.

Timeouturile și oprirea arborelui de procese păstrează blocarea operației până la confirmarea terminării. Dacă oprirea rămâne incertă, atât workerul cât și runnerul obișnuit refuză alte comenzi Windows în acea sesiune.

Reducerea numărului de porniri și scanări este verificată prin teste. Mediul Linux nu permite măsurarea timpului real de aplicare pe PC-ul clientului. Captura `profile-feedback.json` măsoară doar primul cadru de feedback într-un browser cu răspuns Windows simulat și întârziat; nu este benchmark de Windows sau FPS.

## Corecții de restaurare

Pașii sunt eliminați din sesiune numai după confirmarea persistentă a restaurării. O citire finală nereușită păstrează pașii pentru reîncercare; un eșec la salvarea sesiunii nu mai ascunde un pas restant. Setările deja configurate și originalele individuale doar reutilizate nu sunt restaurate accidental în rollback.

Un pas fără jurnal nu este declarat restaurat din presupunere. În rollbackul operației curente poate fi demonstrat că nu a început numai prin capturi tipizate identice ale aceleiași ținte. Pentru valori de alimentare, captura ține cont de planul nou activat de un pas anterior. Originalele istorice lipsă rămân erori de diagnostic; nu sunt înlocuite cu valori implicite inventate. Jurnalele valide UTF-8 cu BOM sunt acceptate.

Centrul de recuperare refuză ID-uri duplicate, izolează planul confirmat de modificările obiectului afișat și păstrează rezultatele parțiale dacă o citire eșuează. Reîncercarea rămâne disponibilă după eșec. Un succes al restaurării nu mai este prezentat drept eșec doar pentru că reîncărcarea listei a eșuat. Copiile fără scrieri nu mai oferă acțiuni inutile de reluare/anulare.

## Verificări și limite

Rezultatele exacte sunt în `validation/7.15.0/tests.txt`, `results.json` și `powershell.json`. Sunt incluse regresii pentru transportul persistent, oprirea proceselor, citiri grupate, intenția salvată înainte de scriere, restaurări întrerupte, BOM, clicuri repetate, confirmări, focus și meniul cu un singur buton.

- **503 teste trecute**, zero eșecuri, anulări sau omisiuni.
- **77 scenarii Chromium trecute**, fără erori JavaScript/CSP/resurse.
- **46 scripturi PowerShell** parsează fără erori.

Testele PowerShell folosesc procese reale `pwsh` cu motoare/cmdleturi simulate. Scenariile Chromium folosesc un bridge Windows simulat și chei temporare de test pentru activare. Scrierile reale Windows, PowerShell 5.1, restartul, installerul și timpii pe hardware-ul clientului rămân de validat conform `VALIDARE_WINDOWS.md`.

ZIP-ul conține surse, fără EXE verificat. Controlul AMD din 7.14 rămâne sursă opțională, fără binar inclus. Nu sunt schimbate cheile editorului, integrarea Shopify, protecțiile Windows sau catalogul Ultra. Configurarea producției, SDK-ul extern pentru helperul AMD și celelalte limite de lansare documentate în rapoartele anterioare rămân valabile.

Actualizează într-un folder nou și păstrează backupurile și cheile existente. Mediul cloud nu a fost republicat.
