HOME-NETFIX — KOČKA, DORT A DINOSAURUS
=====================================

Tento balíček obsahuje webovou verzi hry, serverové přihlášení, registraci bez schvalování, správu rolí a souhlasem chráněné vzdálené ovládání hry.

SOUBORY
- index.js: server Express, přihlášení, účty, administrace a fronta vzdálených příkazů
- public/index.html: samotná hra + mobilní/PC přihlášení a ovládací panel
- package.json: potřebné Node.js závislosti a příkaz ke spuštění

LOKÁLNÍ SPUŠTĚNÍ
1. Nainstaluj Node.js 18 nebo novější.
2. Otevři terminál ve složce projektu.
3. Spusť `npm install`.
4. Nastav heslo administrátora a tajný klíč relací; například v Linuxu/macOS:
   `MOJE_HESLO='sem-dej-silne-heslo' SESSION_SECRET='dlouhy-nahodny-tajny-retezec' npm start`
   Ve Windows PowerShellu lze před spuštěním nastavit `$env:MOJE_HESLO='...'` a `$env:SESSION_SECRET='...'` a potom spustit `npm start`.
5. Otevři `http://localhost:10000`. Pro druhé zařízení na stejné Wi-Fi použij IP adresu počítače, který server spustil. Pro Prahu ↔ Brno je potřeba aplikaci nasadit na veřejně dostupný HTTPS server.

NASAZENÍ NA RENDER
- Nahraj obsah ZIPu do svého repozitáře; `index.js` má start příkaz `node index.js`.
- Build command: `npm install`; Start command: `npm start`.
- V nastavení služby přidej Environment Variables: `MOJE_HESLO` (silné heslo správce), `SESSION_SECRET` (dlouhý náhodný řetězec), volitelně `ADMIN_USERNAME` (výchozí `Admin`).
- Pro zachování účtů po restartu připoj persistent disk a nastav `DATA_DIR` na cestu k němu (např. `/var/data`). Bez perzistentního úložiště se souborová databáze může při redeployi ztratit.
- Pro jednoduchost se používá souborová databáze a paměťová přítomnost; provozuj jen jednu instanci serveru.

PRVNÍ ÚČET A REGISTRACE
- Pokud nastavíš `MOJE_HESLO`, server vytvoří účet `Admin` (nebo jméno z `ADMIN_USERNAME`) s tímto heslem.
- Pokud server nemá žádného správce a proměnná není nastavena, první účet vytvořený registrací se stane správcem. Na veřejném serveru proto vždy nastav `MOJE_HESLO` ještě před spuštěním.
- Další registrace jsou aktivní ihned; uživatel je přihlášen jako hráč. Správce může později měnit role nebo účty mazat.
- Jestli v existujícím `databaze.json` byly účty v seznamech `schvaleni` a `cekajici`, server je při prvním spuštění převede do nového formátu. Čekající účty se stanou běžnými aktivními hráči, jak jsi požadoval. Hesla budou uložena jako scrypt hashe. Před aktualizací si původní `databaze.json` zazálohuj.

VZDÁLENÉ OVLÁDÁNÍ
1. Hráč na cílovém zařízení se přihlásí, v panelu zaškrtne „Povolit vzdálené ovládání této hry“ a stiskne „Spustit hru“.
2. Správce se přihlásí ze svého zařízení, v sekci Administrace najde cílového hráče v seznamu online relací.
3. Správce může posílat Start, Stop, Reset a směry šipek joysticku.
4. Cílový hráč může souhlas kdykoliv vypnout tlačítkem „Účet“ a odškrtnutím souhlasu. Server nesdílí obrazovku ani polohu — pouze herní stav a příkazy ovládání.

Poznámka: administrace umí zobrazit online relace a informaci, zda je hra otevřená; neukazuje obrazovku hráče. Vzdálené ovládání je omezeno na hru, není to vzdálený přístup k počítači.
