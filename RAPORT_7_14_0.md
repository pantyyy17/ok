# CARPATIA 7.14.0 — Ultra extins și sunet de vânt

## Profiluri și restaurare

Ultra adaugă șapte opțiuni cu original și restaurare. Nivelurile Low, Medium și High sunt păstrate. Profilul standard poate avea maximum 13 pași CPU, 8 GPU și 11 Internet; serviciul auxiliar opțional poate adăuga încă un pas. Opțiunile incompatibile sunt omise explicit înainte de capturarea originalului.

- CPU: eliminarea limitei Windows pentru frecvența maximă și dezactivarea suspendării selective USB, numai pe alimentare AC. Limitele termice și firmware rămân active.
- GPU: AllowAutoGameMode și dezactivarea economisirii legăturii PCIe pe alimentare AC. Acestea sunt politici Windows, fără overclock automat.
- Internet: economisirea adaptorului, EEE și moderarea întreruperilor, numai când driverul declară explicit suportul și valoarea cerută.

Operațiile NIC noi folosesc GUID-ul fizic, nu un nume sau un index care poate desemna alt adaptor după restart. Validarea refuză schimbarea țintei, a keywordului sau a valorii salvate. Originalele păstrează separat câmpurile nesuportate, iar restaurarea refuză o schimbare a suportului sau valori externe necunoscute. EEE și moderarea întreruperilor au resurse distincte; backupurile de rețea mai vechi rămân protejate de verificările de suprapunere.

Scrierile adaptorului folosesc NoRestart și sunt recitite. Pentru efectul în driver poate fi necesar restart. Accesul refuzat și valorile necunoscute sunt erori; nu sunt mascate ca opțiuni incompatibile. Originalele existente, sesiunile întrerupte și restaurarea copiilor manuale sunt păstrate.

Aceste ajustări pot crește consumul, temperatura și utilizarea CPU. Moderarea întreruperilor și politicile de economisire nu sunt potrivite tuturor adaptoarelor. Nu au fost măsurate sau promise câștiguri de FPS, ping ori stabilitate.

## Sunetul munților

Sunetul metalic a fost înlocuit cu o rafală originală sintetizată, cu două straturi filtrate de zgomot, atac lin și coadă scurtă. Nu sunt folosite sunete extrase din jocuri. Durata rafalei este 0,70–0,77 secunde; fișierul demonstrativ are 1,1 secunde, incluzând coada și liniștea de final.

O singură rafală poate fi activă. Hover și click nu se dublează, bufferul este reutilizat, iar mute, ieșirea din pagină, pierderea focusului și ascunderea ferestrei opresc sunetul. AudioContext se deschide numai după un gest permis de browser și se suspendă când nu este utilizat.

## Reglaj hardware GPU opțional — surse experimentale

Există un card manual în GPU Studio și cod propriu pentru un helper AMD ADLX. Nu este copiat cod MSI Afterburner. Singurul setter este frecvența maximă GPU, prin API-ul oficial; nu sunt modificate tensiuni, frecvențe de memorie, ventilatoare, limite de putere sau protecții termice.

Sunt cerute identificarea exactă a GPU-ului, o interfață cu frecvențe absolute, limite și pași verificabili și un original care poate fi restaurat. Interfețele cu offset, stările ambigue, driverele sau plăcile fără suport sunt refuzate. Overclockul NVIDIA și Intel nu este implementat. Suportul pentru laptopuri/APU nu este presupus după numele procesorului.

Aplicarea este manuală, cu previzualizare, token unic și confirmare. Frecvența deja configurată nu generează un backup fictiv. Originalul este publicat atomic înainte de scriere; driverul este recitit, iar restaurarea verifică revenirea exactă la original. Modificările externe detectate păstrează jurnalul și blochează suprascrierea automată. Restaurarea rămâne accesibilă fără licență activă.

**Nu este inclus un executabil Windows pentru control hardware.** Fără helper compilat separat și hardware compatibil, cardul arată indisponibil și nu simulează reglajul. SDK-ul AMD, headerele, mostrele și DLL-urile nu sunt incluse. `GPU_OVERCLOCK.md` documentează buildul extern, protocolul și obligațiile licenței SDK pentru distribuție comercială.

Compilarea adaptorului cu headerele oficiale pe Linux verifică numele API și tipurile C++, nu ABI-ul Windows. Buildul MSVC, interogarea reală, limitele, aplicarea, restaurarea și stabilitatea pe GPU rămân de validat pe Windows.

## Verificări

Rezultatele finale, capturile și măsurătorile sunt în `validation/7.14.0/`:

- 445 de teste trecute, zero eșecuri, anulări sau omisiuni. Sunt incluse regresii de profil, snapshot, NIC, IPC, licențiere, UI și politica nativă GPU cu driver simulat.
- 72 de scenarii Chromium trecute, fără erori JavaScript/CSP/resurse. Cele șase scenarii noi GPU verifică explicit un bridge simulat, nu un driver sau hardware real.
- 44 de scripturi PowerShell parsează fără erori. Testele NIC folosesc cmdleturi simulate, cu 48 de aserțiuni.
- 58 de aserțiuni C++ verifică politica GPU, originalele, rollbackul și serializarea stării cu driver și stocare simulate. Compilarea cu headerele externe ADLX trece fără avertismente; nu reprezintă un build Windows.
- Captura GPU etichetează senzorii „Date de test Chromium”, iar controlul hardware apare indisponibil. WAV-ul este randat din sintetizatorul de producție, fără clipping.

## Starea lansării

ZIP-ul conține surse, fără installer verificat. Mediul disponibil este Linux; operațiile reale Windows, restartul, driverele și benchmarkurile de joc nu au fost validate. Ghidul `VALIDARE_WINDOWS.md` include pașii suplimentari pentru Ultra și controlul GPU separat.

Cheia publică de producție nu este configurată; buildul rămâne blocat până la configurarea locală prin manager. Nu au fost generate sau distribuite chei private. Shopify/email și validarea installerului rămân pași de lansare. Cele 15 alerte moderate de build documentate anterior și cele cinci reguli GPU ambigue blocate la aplicare rămân neschimbate.

Extrage actualizarea într-un folder nou și păstrează backupurile și cheile existente. Mediul cloud nu a fost republicat.
