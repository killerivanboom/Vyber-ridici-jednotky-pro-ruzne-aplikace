[Co dodělat ]: #
[pojmy ]: #

# Výběr řídící jednotky pro různé aplikace


$${\color{#FFA500}E9 \space \color{#4682B4}A1 }$$

## Cíle

- **Orientovat se** v základních typech a architekturách řídicích jednotek (MCU, MPU, embedded systémy, PLC, iPC, programovatelná relé).
- **Rozlišovat klíčové technické parametry** (výpočetní výkon vs. spotřeba, typy a velikosti pamětí RAM/Flash/EEPROM, determinismus a reakční doba v reálném čase).
- **Zhodnotit provozní odolnost a robustnost** hardwaru (krytí IP, teplotní rozsah, vibrace, rušení EMC, srovnání spotřební vs. průmyslové techniky).
- **Navrhnout a technicko-ekonomicky obhájit** optimální řídicí jednotku pro konkrétní praktickou aplikaci podle I/O bilance, rozhraní a prostředí.

## Ověření cílů

Výběr řídící jednotky pro různé aplikace

1. Příklady řídících jednotek 
2. Jejich základní vlastnosti z hlediska výpočetního výkonu a velikosti paměťového prostoru
3. A z hlediska odolnosti
4. Příklady použití v praxi (kde se používají MCU, a kde ř. j. s MPU)

%%
1. Správné vysvětlení pojmů, architektur a zkratek z oblasti řídicích systémů.
2. Schopnost posoudit vliv prostředí na výběr hardwaru a dešifrovat IP kód.
3. Vypracování rozhodovací matice pro volbu vhodné platformy (MCU vs. PLC vs. iPC).
4. Návrh konkrétní konfigurace řídicí jednotky na základě zadané I/O bilance a provozních podmínek.
5. Kritická technická oponentura (audit) nevhodně navrženého řešení.
%%


---

## Úlohy


### 1. Základní pojmy a architektury řídicích jednotek

*Časová dotace: 10–15 minut | Úvodní úloha*

Doplňte do níže uvedené tabulky význam zkratek, základní princip a typický příklad reálného nasazení nebo zástupce:

| Zkratka / Pojem          | Co zkratka znamená (česky/anglicky) | Základní charakteristika (architektura, kde běží program)                                 | Typický zástupce                  | Příklad nasazení                           |
| :----------------------- | :---------------------------------- | :---------------------------------------------------------------------------------------- | :-------------------------------- | ------------------------------------------ |
| **MCU**                  |   Mikrokontroller                                  | Integrovaný čip (CPU + RAM + Flash na jednom křemíku), deterministický běh bez OS / RTOS  | např. ESP32, PIC16LF1xxx, RP2040  |  Čidla, chytre hodinky, dalkove ovladace,                                           |
| **MPU**                  |   Mikroprocesor                                  | Samostatný procesor vyžadující externí RAM a úložiště, často běží plnohodnotný OS (Linux) |      Rasberry Pi4                             |          Ridici terminaly, multimedialni systémy v autech                                  |
| **Embedded**             |         Vestaveny systém                            |        Ucelovy pc. Systém zamereny na vykonavani jedne konkretni ulohy                                                                                   | Embedded PLC, embedded PC         | Bílá technika, bankomaty, plynové kotle... |
| **PLC**                  |     Programovatelny logicky automat                                | Průmyslový automat pro cyklické řízení procesů, vysoká odolnost, modulární/kompaktní      |      Schneder Modicon                             |                        Rizeni vyrobnich linek, automatizace budov                    |
| **iPC**                  |     Prumyslove PC                                |     Klasicke PC v odolnem sasi (IP krytí vyoke)                                                                                      |        Siemens Microbox                           |        Sber dat- sledovani                                    |
| **Programovatelné relé** |         Progra. Rele                            | Zjednodušené malé PLC pro méně náročné úlohy (nahrazuje časovače a relé)                  | např. Siemens LOGO!, Eaton easyE4 |        Rizeni osvetleni, spinani cerpadel, automatizace obecne                                    |

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **SoC (System on Chip):** Čip integrující CPU, GPU, paměť i bezdrátové moduly (např. Wi-Fi/BT) na jediném substrátu (např. v telefonech, ESP32).
> - **DSP (Digital Signal Processor):** Specializovaný procesor s architekturou optimalizovanou pro bleskové matematické operace (filtrace zvuku, FFT, řízení motorů).
> - **FPGA (Field-Programmable Gate Array):** Programovatelné hradlové pole umožňující vytvořit libovolný digitální obvod přímo na hardwarové úrovni s nulovou programovou latencí.
> Programovatelné hradlové pole. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2005, poslední editace 10. 1. 2024 [cit. 2026-09-14]. Dostupné z: [https://cs.wikipedia.org/wiki/Programovateln%C3%A9_hradlov%C3%A9_pole](https://cs.wikipedia.org/wiki/Programovateln%C3%A9_hradlov%C3%A9_pole)
> Systém na čipu. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2007, poslední editace 7. 6. 2024 [cit. 2026-09-14]. Dostupné z: [https://cs.wikipedia.org/wiki/Syst%C3%A9m_na_%C4%8Dipu](https://cs.wikipedia.org/wiki/Syst%C3%A9m_na_%C4%8Dipu)
>Digitální signálový procesor. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2006, poslední editace 28. 2. 2026 [cit. 2026-09-14]. Dostupné z: [https://cs.wikipedia.org/wiki/Digit%C3%A1ln%C3%AD_sign%C3%A1lov%C3%BD_procesor](https://cs.wikipedia.org/wiki/Digit%C3%A1ln%C3%AD_sign%C3%A1lov%C3%BD_procesor)

<details>
<summary> :bulb: Tip k doplnění tabulky: </summary>
<p>Uvědomte si zásadní rozdíl: U MCU je program nahrán přímo ve vnitřní paměti Flash procesoru a startuje okamžitě po zapnutí (desítky milisekund). U MPU a iPC systém nejprve zavádí operační systém z disku/SD karty do paměti RAM (sekundy až desítky sekund).</p>
</details>

:star2: **Bonusová otázka k úloze 1:**

Proč se u bezpečnostních aplikací v letectví nebo jaderné energetice stále upřednostňují jednoduché mikrořadiče nebo FPGA před moderními vícejádrovými procesory s gigabajty RAM?

*Vaše odpověď:*

`...`

---

### 2. Parametry, paměti a provozní odolnost (IP krytí)
*Časová dotace: max. 15 minut | Úvodní úloha

1. **Typy pamětí:**
   - Jaký je zásadní rozdíl mezi pamětí **RAM**, **Flash** a **EEPROM** v mikrokontroléru/PLC z hlediska uchování dat po odpojení napájení a rychlosti zápisu?
   - RAM (Random Access Memory):

Uchování dat: Volatilní (po odpojení napájení se data ztratí).

Rychlost zápisu: Extrémně rychlá (řádově nanosekundy), neomezený počet zápisů. Slouží pro běh programu a proměnné za provozu.


Flash paměť:

Uchování dat: Nevolatilní (data zůstávají zachována i bez napájení).

Rychlost zápisu: Pomalejší, zápis probíhá v blocích (stránkách) a před zápisem je obvykle nutné blok smazat. Má omezený životní cyklus (počet cyklů přepsání). Slouží pro uložení samotného firmwaru/programu.


EEPROM:

Uchování dat: Nevolatilní (data zůstávají zachována).

Rychlost zápisu: Pomalejší než RAM, ale umožňuje zápis a mazání po jednotlivých bytech (na rozdíl od Flash). Používá se pro ukládání konfiguračních parametrů, provozních stavů nebo naměřených dat, která se často mění a nesmí se ztratit při výpadku proudu.

2. **Reálný čas a determinismus:**
   - Proč pro řízení rychlého technologického děje (např. reakce na nouzové zastavení do 5 ms) použijeme spíše **MCU / PLC** než běžný operační systém na **MPU** (např. Raspberry Pi s OS Linux)?

   - Determinismus: MCU a PLC pracují deterministicky – přesně víme, jak dlouho bude trvat vykonání instrukce nebo obsloužení vstupu/výstupu. Žádné skryté procesy nečekaně nezdrží reakci.

   - Běžný operační systém (např. Linux na Raspberry Pi): Je non-real-time (pokud nemá speciální RT patch). Jádro systému může kdykoliv pozastavit vaši aplikaci kvůli správě paměti, procesů nebo síťové komunikaci (tzv. jitter). Zpoždění tak může přesáhnout požadovaných 5 ms, což je u bezpečnostních funkcí nepřípustné.

   - 
3. **Odolnost a IP krytí:**
   - Dešifrujte označení **IP68** (co přesně znamená první číslice 6 a druhá číslice 8).
   - Jaké minimální krytí IP musí mít zařízení určené pro instalaci venku pod přístřeškem, kde hrozí stříkající voda a prach?
   - Jak se liší konstrukce běžného kancelářského PC od **průmyslového PC (iPC)** (např. z hlediska chlazení, napájení, vibrací a konektorů)?

   - ýznam označení IP68:

První číslice (6): Úplná ochrana před vniknutím prachu (prachotěsné).

Druhá číslice (8): Ochrana proti trvalému ponoření do vody (za podmínek specifikovaných výrobcem, obvykle hloubka nad 1 metr).

Minimální krytí pro venkovní použití pod přístřeškem (stříkající voda a prach):

Minimální doporučené krytí je IP54 (chrání před prachem a stříkající vodou ze všech směrů). Pro vyšší spolehlivost v průmyslovém prostředí se často používá IP65 (chrání před prachem a tryskající vodou).

---

### 3. Rozhodovací matice platforem (MCU vs. PLC vs. iPC) 

*Časová dotace: 20–25 minut | :star: Klasifikovaná inženýrská úloha na známky*

Jste v pozici nezávislého konzultanta automatizace. Tři různí zákazníci požadují navrhnout optimální kategorii řízení.

#### Příklad aplikace (vzorové řešení):
- **Vzorová aplikace 0 – Automatická vjezdová závora na parkoviště:** Jednoduchý jednoúčelový systém s indukční detekční smyčkou vozidla, bezpečnostní optozávorou, koncovými spínači polohy ramene, motorem závory (vpřed/vzad) a výstražným semaforem (červená/zelená). Požadavek na jednoduchou správu správcem objektu a spolehlivý chod v rozváděči u vjezdu.

#### Popis zadaných aplikací pro studenty:
1. **Aplikace A – Chytrý pokojový termostat (IoT):** Bateriově napájený přístroj měřící teplotu a vlhkost v místnosti, zobrazující údaje na e-ink displeji a odesílající data přes protokol ZigBee/Wi-Fi do domácí brány. Plánovaná sériová výroba: 10 000 kusů ročně.
2. **Aplikace B – Automatická balicí linka:** Průmyslová linka ve výrobní hale. Obsahuje 28 optických snímačů, 14 pneumatických válců, 3 dopravníkové pásy s asynchronními motory a bezpečnostní světelnou závoru. Vyžaduje se nepřetržitý provoz 24/7 a snadná údržba podnikovým elektrikářem.
3. **Aplikace C – Kontrolní stanice optické jakosti svarů:** Pracoviště se 2 vysokorychlostními průmyslovými GigE kamerami snímajícími svary na karoserii automobilu. Snímky v rozlišení 4K jsou analyzovány neuronovou sítí v reálném čase, vady jsou označeny a ukládány do podnikové relační databáze (SQL / MES).

#### Váš úkol:
Vyplňte rozhodovací matici. Jako vzor poslouží vyplněný sloupec pro **Vzorovou aplikaci 0**. Přiřaďte každé aplikaci nejvhodnější platformu (**MCU / Embedded SoC**, **Kompaktní/modulární PLC**, **Průmyslové PC – iPC**) a doplňte multikriteriální posouzení:

| Kritérium hodnocení                                                                                   | **Vzorová aplikace 0 (Vjezdová závora - VZOR)**                                                                                                                                                                           | Aplikace A (Pokojový termostat) | Aplikace B (Balicí linka) | Aplikace C (Kamerová kontrola svarů) |
| :---------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------ | :------------------------ | :----------------------------------- |
| **Doporučená platforma** *(MCU / PLC / iPC)*                                                          | **Programovatelné relé / kompaktní PLC** *(např. Siemens LOGO!, Eaton easyE4)*                                                                                                                                            | **MCU / Embedded SoC** *(např. ESP32 / ARM Cortex-M)* | **Modulární PLC** *(např. Siemens S7-1200 / S7-1500)* | **Průmyslové PC (iPC)** *(např. Advantech / Siemens IPC)* |
| **Pořizovací cena HW na 1 kus** *(nízká < 500 Kč / střední 5–30 tis. Kč / vysoká > 50 tis. Kč)*       | **Střední** *(cca 3 500 – 6 000 Kč)*                                                                                                                                                                                      | **Nízká** *(< 500 Kč za čip/desku při sériové výrobě)* | **Vysoká** *(dle rozšiřujících modulů, cca 15–30 tis. Kč)* | **Vysoká** *(> 50 tis. Kč vč. licencí a rámů pro kamery)* |
| **Primární programovací jazyk** *(C/C++/MicroPython vs. IEC 61131-3 ST/LAD vs. Python/C#/C++ pod OS)* | **FBD / LAD** *(grafické funkční bloky nebo liniové schéma dle IEC 61131-3)*                                                                                                                                              | **C / C++ / MicroPython** | **IEC 61131-3 (LAD / ST)** | **Python / C# / C++** *(pod OS Windows/Linux)* |
| **Klíčový technický argument pro volbu** *(např. spotřeba, determinismus, grafický výkon)*            | Montáž přímo na DIN lištu v rozváděči, integrovaný displej pro nastavení časovačů přímo na místě, robustní reléové výstupy pro motor a semafor, napájení 24 V DC / 230 V AC bez nutnosti vývoje vlastního plošného spoje. | Minimální spotřeba energie pro bateriový provoz, integrovaná bezdrátová konektivita (ZigBee/Wi-Fi), extrémně nízká cena při sériové výrobě 10 000 ks. | Deterministické řízení I/O, vysoká spolehlivost v průmyslovém prostředí 24/7, snadná diagnostika a údržba v jazyce LAD. | Masivní výpočetní výkon pro zpracování obrazu a AI (neuronové sítě), podpora vysokorychlostních průmyslových kamer (GigE Vision). |
| **Hlavní riziko při volbě špatné platformy** *(proč by neuspěly ostatní dvě varianty)*                | **MCU:** Nutnost vývoje vlastní desky, nízká odolnost vůči venkovnímu rušení a obtížný servis údržbou.<br>**iPC:** Zbytečně extrémní cena (> 30 tis. Kč), dlouhý start po výpadku napájení a vysoká spotřeba.             | **PLC / iPC:** Obrovská spotřeba (vybily by baterii během několika hodin), nemožnost sériové výroby v tisících kusů kvůli vysoké ceně a rozměrům. | **MCU:** Chybí průmyslové krytí, obtížný servis pro elektrikáře, nutnost vývoje desky.<br>**iPC:** Zbytečně drahé, složitý OS náchylný k zamrznutí, nevhodné pro přímé spínání 24V prvků bez I/O karet. | **MCU / PLC:** Nemají dostatečný výpočetní výkon pro 4K video a AI, chybí nativní podpora GigE Vision kamer a SQL databází. |

---

### 4. Návrh a konfigurace řídicí jednotky pro čerpací stanici

1. **Sestavte tabulku I/O bilance** a spočtěte celkový počet signálů. Připočtěte rezervu min. 20 % pro budoucí rozšíření:

| Typ signálu | Požadavek aplikace (kusy) | Popis signálů v aplikaci | Počet po započtení rezervy (+20 %) |
| :--- | :--- | :--- | :--- |
| **Digitální vstup (DI)** | 4 | 3× plovákový spínač + 1× termistorové relé | **5** *(4.8 zaokrouhleno nahoru)* |
| **Digitální výstup (DO) – reléový** | 3 | 2× stykač čerpadel + 1× výstražný maják | **4** *(3.6 zaokrouhleno nahoru)* |
| **Digitální výstup (DO) – tranzistorový** | 0 | - | **0** |
| **Analogový vstup (AI)** | 1 | 1× hydrostatická sonda hladiny (4–20 mA) | **2** *(1.2 zaokrouhleno nahoru)* |
| **Analogový výstup (AO)** | 1 | 1× řízení otáček frekvenčního měniče (0–10 V) | **2** *(1.2 zaokrouhleno nahoru)* |

---

### 5. Technický audit a oponentura nevhodného návrhu

1. **Zpracujte písemný audit rizik (minimálně 4 fatální technická selhání):**

| Oblast auditu | Zjištěná vada v amatérském návrhu | Fyzikální mechanismus selhání (proč to selže) | Následek pro stroj nebo obsluhu |
| :--- | :--- | :--- | :--- |
| **Elektromagnetická kompatibilita (EMC)** | Absence odrušení a použití Arduina v silovém prostředí | Napěťové špičky (indukční ráz) z cívek hydraulických ventilů proniknou přes tenké vodiče do MCU a způsobí jeho zablokování nebo samovolný restart. | Nekontrolovaný pohyb lisu, přiskřípnutí obsluhy. |
| **Mechanická a teplotní odolnost** | PLA plast a přímá montáž na vibrující lis | PLA měkne již při teplotách kolem 60 °C; vibrace lisu způsobují únavový lom materiálu a postupné uvolňování komponent. | Rozpad krytu, obnažení živých částí 230V/400V. |
| **Konektivita a propojení vodičů** | DuPont propojovací kabely bez jističů a aretace | Tyto kabely nemají žádnou mechanickou pojistku proti vytažení a přechodový odpor pinů roste oxidací v prašném prostředí. | Přerušení signálu, ztráta kontroly nad strojem. |
| **Funkční bezpečnost (Safety)** | Nouzový stop řešený softwarově v čipu ATmega328P | Software může selhat, zamrznout v nekonečné smyčce nebo vykazovat bug; MCU nedisponuje hardwarovou redundancí dle norem Safety. | Tlačítko E-Stop selže a nezastaví pohyb při nouzi. |

---

### 6. Rozšiřující inženýrská výzva: TCO a životní cyklus v automatizaci

1. Srovnejte obě varianty v níže uvedené tabulce a uveďte předpokládaná skrytá rizika a náklady v horizontu 10–15 let:

| Aspekt životního cyklu | Varianta 1 (Custom Embedded MCU) | Varianta 2 (Průmyslový standard / PLC) |
| :--- | :--- | :--- |
| **Dostupnost náhradních dílů za 10 let** | Nízká (komponenty na desce se obměňují v řádu let, výrobce čipu jej může ukončit) | Vysoká (garance dostupnosti náhradních dílů a kompatibility 10–15+ let) |
| **Servisovatelnost podnikovým elektrikářem** | Nulová (vyžaduje vývojáře s kódem v C/C++ a pájecí stanici) | Vysoká (běžný údržbář diagnostikuje stav přes LED / displej a nahradí kus za kus) |
| **Doba odstávky linky při poruše CPU** | Dlouhá (dny až týdny – nutnost vyvinout/vyrobit novou desku nebo najít kód) | Krátká (hodiny – výměna modulu za záložní ze skladu a nahraní zálohy programu) |
| **Cena vývojových nástrojů a licencí IDE** | Nízká (Free/Open-source IDE, např. VS Code / PlatformIO) | Vyšší (jednorázová licence inženýrského sw, např. TIA Portal) |
| **Závěrečné doporučení (kterou variantu vybrat a proč)** | **Nedoporučuje se** pro průmyslové linky kvůli obrovským prostojům a skrytým nákladům na servis. | **Jednoznačně doporučit** – vyšší počáteční investice se mnohonásobně vrátí díky spolehlivosti a nízkému TCO. |
