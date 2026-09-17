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
