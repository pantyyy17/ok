# CARPATIA 7.10.3 — butoane de munte animate

CPU, GPU și Internet folosesc acum trei butoane în formă de vârf de munte: contur roșu, fațete, creastă luminoasă și simbolul modulului. Forma clicabilă este triunghiul, nu dreptunghiul transparent din jur. Titlurile și stările sunt păstrate în zona lată, iar descrierile sunt separate sub munte.

Animațiile folosesc straturi SVG/CSS: lumină pe contur, lumină care urcă spre vârf, respirația fațetelor și reflexie diagonală. La hover/focus reacționează mai intens, iar în timpul operației crește viteza. Etichetele rămân fixe. Tastatura are contur de focus vizibil; pauza de vizibilitate existentă oprește animațiile în afara Dashboardului, în afara ecranului și când fereastra este ascunsă. Setarea „reduce motion” dezactivează mișcarea.

Au fost păstrate ID-urile, butoanele native, nivelurile, confirmarea și restaurarea CPU/GPU/Internet. Nu au fost adăugate librării, canvas, particule sau bucle JavaScript pentru animație.

## Verificări

- **63 teste vizate** de UI, profiluri, mișcare și includere în pachet: trecute, zero eșecuri/omisiuni.
- **38 scenarii Chromium**: trecute, fără erori JS/CSP/resurse. Includ forma de hit-test, deplasarea efectivă a luminii în timp, starea de lucru, pauza în afara Dashboardului, „reduce motion”, layoutul de 1024 px, confirmarea/restaurarea/reaplicarea profilurilor.
- Revizie vizuală desktop, compactă și detaliu: fără coliziuni între etichete și vârf.
- GIF: captură din DOM real, 48 cadre, aproximativ 6 secunde, date Windows simulate. Nu este benchmark de performanță.

Rezultatele și previzualizările sunt în `validation/7.10.3/`. Binarul GIF nu este necesar aplicației; animațiile aplicației sunt SVG/CSS.

Aceasta este o modificare de interfață. Limitele Windows/installer, cheia publică originală lipsă și celelalte observații de livrare din `RAPORT_7_10_2.md` rămân valabile. Nu se afirmă că au fost rulate din nou toate testele motoarelor Windows în acest pas vizual.
