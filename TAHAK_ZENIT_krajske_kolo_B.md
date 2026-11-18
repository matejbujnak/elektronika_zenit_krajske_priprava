# ZENIT v elektronike – krajské kolo, kategória B
## Rýchly ťahák na poslednú chvíľu

> Spracované z úloh krajských kôl 2018 – 2025 (35. – 42. ročník).
> **Najdôležitejšie zistenie:** teoretické otázky sa každý rok **opakujú takmer doslova**, len sa menia čísla.
> Ak si prečítaš sekciu 2 a pochopíš postupy, máš veľkú časť testu v suchu.

---

## 0. Ako prebieha deň (aby ťa nič neprekvapilo)

| Časť | Čo sa robí | Na čo si dať pozor |
|---|---|---|
| **Teória** | Google Formulár, ~30 otázok, **1 hodina**, každá otázka 1 bod | Povolené: **čistý papier, pero, kalkulačka**. Test treba **ODOSLAŤ** (býva limit napr. do 9:45). |
| **Prax** | Dostaneš schému + zoznam súčiastok → navrhneš **jednostranný plošný spoj** (KiCad) → nahráš súbor → vyrobíš dosku (fotocesta, leptanie) → osadíš SMD súčiastky → oživíš | **Autorouter je ZAKÁZANÝ!** Hodnotí sa aj kompaktnosť a ergonómia návrhu. |

**Taktika na test:**
1. Najprv prejdi všetko a odklikaj ľahké otázky (jednotky, prevody, hradlá, „kto objavil“) – to je ~10 bodov za 5 minút.
2. Potom výpočty – vždy si to rozpíš na papier, **jednotky prepočítaj na základné** (mm → m, mA → A, kΩ → Ω).
3. Pri „Check all that apply“ (viac odpovedí) zaškrtni **všetky** správne, nie len jednu.
4. Nič nenechávaj prázdne – tipnúť je lepšie ako nič.
5. **Posledných 5 minút = kontrola a ODOSLAŤ.** Neodoslaný test = 0 bodov.

---

## 1. Vzorce, ktoré musíš mať v hlave

### Základ
| Čo | Vzorec |
|---|---|
| Ohmov zákon | `U = R · I` |
| Výkon | `P = U · I = I²·R = U²/R` |
| Energia | `E [Wh] = P [W] · t [h]` |
| Kapacita batérie | `Q [Ah] = E [Wh] / U [V]` = `I · t` |
| Výdrž batérie | `t [h] = Q [Ah] / I [A]` |
| Odpor drôtu | `R = ρ · l / S` (pozor: predlžovačka má **2 vodiče** → dĺžka × 2!) |

### Zapájanie súčiastok
| | Sériovo | Paralelne |
|---|---|---|
| **Rezistory** | `R = R1 + R2 + …` | `1/R = 1/R1 + 1/R2 + …` (dva: `R1·R2/(R1+R2)`) |
| **Kondenzátory** | `1/C = 1/C1 + 1/C2 + …` (opačne ako R!) | `C = C1 + C2 + …` |
| **Cievky** (bez väzby) | `L = L1 + L2` | ako rezistory |
| **Napäťové zdroje** | napätia sa **sčítajú**, aj vnútorné odpory sa sčítajú | rôzne napätia → **nesmú sa** zapájať paralelne |
| **Prúdové zdroje** | rôzne prúdy → **nesmú sa** zapájať do série | prúdy sa **sčítajú** |

- n rovnakých rezistorov R paralelne = **R/n**.
- Batéria článkov: sériovo `U·n`, `R·n`; paralelne `U` ostáva, `R/n`.

### Delič napätia
`Uvýst = Uvst · R2 / (R1 + R2)` (R2 = spodný rezistor, ku zemi)
- Na **polovicu** → R1 : R2 = **1 : 1**
- Na **desatinu** → R1 : R2 = **9 : 1** (tj. pomer **1:9**) – NIE 1:10!
- Na tretinu → 2 : 1

### Kondenzátor, cievka, filtre
- Doskový kondenzátor: **`C = ε0 · εr · S / d`**, ε0 = 8,854·10⁻¹² F/m. **S v m², d v m!**
  - Kapacita rastie **priamo úmerne s plochou**, nepriamo úmerne so vzdialenosťou.
  - Olej / FR4 (εr > 1) kapacitu **zvýši** εr-krát.
- Reaktancie: `XL = 2π·f·L`, `XC = 1 / (2π·f·C)`
- RC časová konštanta: `τ = R·C`, medzná frekvencia `f = 1 / (2π·R·C)`
- **Dolná priepusť** (sériový R, ku zemi C) → na výstupe je **stredná hodnota** vstupného napätia.
- **Horná priepusť** (sériový C, ku zemi R).

### Striedavý prúd, sieť
- 230 V v zásuvke je **efektívna (RMS)** hodnota. Špička = 230·√2 ≈ 325 V.
- Stredná hodnota sínusu = **0 V**.
- Trojfáza 3×230/400 V: `P = √3 · 400 · I` → `I = P / (√3 · U_združené)` (napr. 150 kW → ~218 A)
- Transformátor: `U2/U1 = N2/N1` (napr. 1150 z : 60 z na 230 V → **12 V**)

### Tolerancia, teplotný koeficient
- 1000 Ω ±5 % → **950 až 1050 Ω**
- Teplotný koeficient: `R = R0 · (1 + α·ΔT)`, 1 ppm = 0,000 001
  - napr. +100 ppm/°C, ΔT = 75 °C → 1000·(1 + 0,0075) = **1007,5 Ω**
  - kladný koeficient + ohrev → odpor **stúpne**

### Predradný rezistor k LED
`R = (U_napájanie − U_LED − U_tranzistor) / I_LED` → zaokrúhli na hodnotu z radu **E12** (radšej vyššiu, aby LED nedostala viac prúdu).
**Rad E12:** 10, 12, 15, 18, 22, 27, 33, 39, 47, 56, 68, 82

### Zenerov stabilizátor
`R = (U_vst_MAX − Uz) / Iz_max` → počítaj s **najvyšším** vstupným napätím (vtedy tečie najviac prúdu).

---

## 2. Opakujúce sa otázky z testov – S RIEŠENÍM

### 2.1 Jednotky a veličiny (každý rok 2 otázky!)
| Veličina | Jednotka |
|---|---|
| napätie | **Volt (V)** |
| prúd | **Ampér (A)** |
| odpor | **Ohm (Ω)** |
| vodivosť (= 1/odpor) | **Siemens (S)** |
| kapacita | **Farad (F)** |
| indukčnosť | **Henry (H)** |
| frekvencia | **Hertz (Hz)** – počet periód za sekundu |
| magnetická indukcia | **Tesla (T)** |
| magnetický tok | **Weber (Wb)** |
| intenzita **elektrického** poľa | **V/m** |
| intenzita **magnetického** poľa | **A/m** |
| výkon | **Watt (W)** – súčin okamžitej hodnoty napätia a prúdu |

### 2.2 Predpony – prevody (každý rok!)
| p | n | µ (u) | m | k | M | G |
|---|---|---|---|---|---|---|
| 10⁻¹² | 10⁻⁹ | 10⁻⁶ | 10⁻³ | 10³ | 10⁶ | 10⁹ |

- 20 kA = **20 000 A**
- 20 nA = **0,000 000 020 A**
- 20 pA = **0,000 000 000 020 A**
- 0,1 kV = **100 V**

### 2.3 Batéria / akumulátor (každý rok!)
- 6 V, 12 W, 24 h → 12·24 = 288 Wh → 288/6 = **48 Ah**
- 12 V, 6 W, 24 h → 144 Wh / 12 V = **12 Ah**
- 12 V, 24 W, 8 h → 192 Wh / 12 V = **16 Ah**
- 12 V, 10 Ah → **120 Wh**
- 24 Wh, 12 V, záťaž 6 Ω → I = 2 A, P = 24 W → **1 h**
- čelovka 300 mA, akumulátor 2,4 Ah → **8 h**

### 2.4 Doskový kondenzátor / parazitná kapacita (každý rok!)
- Plôška 3×3 mm, FR4 0,1 mm, εr = 4,5:
  `C = 8,854e-12 · 4,5 · (0,003·0,003) / 0,0001` = **3,586 pF**
- Platne 1 m × 1 m, 1 mm, vzduch → **8,854 nF**
- 200×300 mm, 1 mm, olej εr = 2,2 → **1,168 nF**
- Polygón 20×20 mm, 0,2 mm, εr = 4,55 → **80,6 pF**
- Pozor na chyták: v zadaní je aj permeabilita µ – **tá sa pri kapacite NEPOUŽÍVA**.

### 2.5 Výkonové výpočty
- **Predlžovačka:** vyhrievač 230 V / 3,6 kW, 20 m, 11,4 mΩ/m
  - R_vyhrievača = 230²/3600 = 14,69 Ω
  - R_kábla = **2 vodiče** × 20 m × 0,0114 = 0,456 Ω
  - I = 230 / (14,69 + 0,456) = 15,19 A
  - P = I² · 0,456 = **≈ 105 W**
  - (pasce: 54 W = zabudol si na 2 vodiče; 111,7 W = prúd počítaný bez odporu kábla)
- Ohrievač 230 V / 2 kW → R = U²/P = **26,45 Ω**
- MOSFET R_DSon = 10 mΩ, prúd 25 A → P = I²R = 625 · 0,01 = **6,25 W**
- Rezistor 1,1 kΩ, 120 mA → U = **132 V**, P = I²R = **15,8 W** → vyhovuje 20 W, 500 W, aj 2× 2,2 kΩ/10 W paralelne
- Zosilňovač 20 W do 8 Ω → do 4 Ω (ideálny zdroj napätia) **40 W** (P = U²/R)
- Zosilňovač 20 W + reproduktor 200 W (rovnaká impedancia) → **nič neštandardné sa nestane**
- Elektráreň 1200 MW, 400 kV → I = P/(√3·U) ≈ **1732 A**

### 2.6 Zdroje – ideálne
| | Ideálny **napäťový** zdroj | Ideálny **prúdový** zdroj |
|---|---|---|
| vnútorný odpor | **nulový** | **nekonečný** |
| napätie naprázdno | U (napr. 1 V) | **nekonečné** |
| napätie nakrátko | – | **nulové** |
| prúd nakrátko | nekonečný | I |

- Reálny zdroj: 10 V naprázdno, Ri = 50 Ω, záťaž 50 Ω → **5 V** (delič 1:1)
- Opačne: naprázdno 10 V, na 50 Ω záťaži 5 V → **Ri = 50 Ω**
- Ideálny voltmeter = **nekonečný** vstupný odpor; ideálny ampérmeter = **nulový** odpor.
- Ampérmeter sa zapája **sériovo**, voltmeter **paralelne**.

### 2.7 „Kto objavil“
| Zákon | Osoba |
|---|---|
| U = R·I | **Georg Ohm** |
| súčet prúdov v uzle / napätí v slučke | **Gustav Kirchhoff** |
| elektromagnetická indukcia | **Michael Faraday** |
| náhradný zdroj napätia / prúdu | Thévenin / Norton |
| bezdrôtová telegrafia (Slovák) | Jozef Murgaš |

### 2.8 Súčiastky – vlastnosti (opakuje sa!)
- **Termistor** – odpor sa mení s **teplotou**. **NTC**: teplota ↑ → odpor **klesá**. PTC: naopak.
- **Fotoodpor** – mení sa so **svetlom**.
- **Varistor** – odpor sa mení s **napätím** (ochrana proti prepätiu).
- **Varikap** – dióda, ktorej **kapacita** sa mení s **napätím** (v závernom smere).
- **Zenerova dióda** – pracuje v **závernom smere** (prieraz), stabilizuje napätie.
- **Elektrolytický kondenzátor** – najcitlivejší na **teplotu**, teplo mu skracuje životnosť.
  - Pravidlo: **každých 10 °C menej = 2× dlhšia životnosť**. 85 °C/2000 h pri 40 °C → 2^4,5 ≈ 22× → **≈ 50 000 h**
- **Polarizované** kondenzátory: **elektrolytický, tantalový**. Do striedavých obvodov: keramický, zvitkový (fóliový), papierový, metalizovaný, sľudový.
- **Kremíková dióda**: prúd s napätím rastie **exponenciálne**; úbytok ~**0,6–0,7 V**; s rastúcou teplotou úbytok **klesá** (~ −2 mV/°C). LED má úbytok vyšší (1,8–3,3 V podľa farby).
- **Tyristor** po skončení zapínacieho impulzu **ostane zopnutý**, tranzistor sa vypne.
- **Optočlen** = **galvanické oddelenie**.
- **ESD** = **elektrostatický výboj**. Citlivé sú MOSFETy a CMOS obvody. Bavlnené oblečenie, bosý, nie syntetika.
- **Autotransformátor** – výstup **nie je galvanicky oddelený** → nebezpečné!
- Transformátor sa prehrieva / saturuje → **zvýšiť počet primárnych závitov**.
- Tavná poistka 1 A pri 1,01 A → **pravdepodobne sa neprepáli**.
- Elektronická poistka: potrebuje napájanie, je vratná (reverzibilná), čas vybavenia sa dá nastaviť.
- Súčiastka realizujúca odpor = **rezistor** (napísať malými písmenami!).
- Materiál plošných spojov = **FR4**.
- Optické vlákno = **sklo alebo plast**.
- Čítač v laboratóriu = meranie **frekvencie**.
- Multimeter po slovensky = **mnohomer** :)

### 2.9 Tranzistory a zosilňovače
- Prúdový zosilňovací činiteľ = **h21 (h21E, β)** = pomer **IC / IB v lineárnej (aktívnej) oblasti**.
- **Spoločný emitor** – zosilňuje napätie aj prúd, **otáča fázu o 180°**.
- **Spoločný kolektor = emitorový sledovač** – napäťové zosilnenie **≈ 1**, neotáča fázu.
- Spoločná báza – neotáča fázu.
- **Darlington** – veľmi vysoké zosilnenie (dva tranzistory v jednom).
- Bipolárne tranzistory (NPN, PNP) sú riadené **prúdom** (do bázy); FET/MOSFET sú riadené **napätím (poľom)** na hradle (gate).
  - Šípka v značke NPN ide **von** z emitora („**N**ot **P**ointing i**N**“), PNP **dnu**.
- Trieda A = nízka účinnosť, nízke skreslenie.
- **PWM** – výkon sa riadi **striedou (činiteľom plnenia)**.

### 2.10 Operačný zosilňovač
- **Ideálny OZ:** nekonečný vstupný odpor, **nulový** výstupný odpor, nekonečné zosilnenie (otvorená slučka), nekonečná šírka pásma.
- **Napäťový sledovač** = neinvertujúci so zosilnením 1 (výstup spojený s – vstupom).
- **Invertujúci:** `A = −Rf / R1` (zosilnenie −2 → Rf = 2·R1)
- **Neinvertujúci:** `A = 1 + Rf / R1`
- **Komparátor** – bez spätnej väzby, porovnáva dve napätia.
- **Integrátor** – kondenzátor v spätnej väzbe.

### 2.11 Číselné sústavy (každý rok jedna otázka!)
| Dec | Bin | Hex |
|---|---|---|
| 15 | **1111** | F |
| 16 | 10000 | **10** |
| 17 | 10001 | **11** |
| 32 | **100000** | 20 |
| 165 | 10100101 | A5 |
| 255 | 11111111 | FF |

- Hex → bin: každú hex číslicu prepíš na 4 bity (F=1111, D=1101, B=1011, 8=1000).
- 17,5 dec = 10001,1 bin (áno, dá sa).

### 2.12 Logické hradlá (každý rok!) – kedy je výstup **1**
| Vstupy A B | AND | OR | NAND | NOR | XOR |
|---|---|---|---|---|---|
| 0 0 | 0 | 0 | **1** | **1** | 0 |
| 0 1 | 0 | **1** | **1** | 0 | **1** |
| 1 0 | 0 | **1** | **1** | 0 | **1** |
| 1 1 | **1** | **1** | 0 | 0 | 0 |

- **Bistabilný** klopný obvod (flip-flop, R-S) **pamätá** stav neobmedzene dlho. Monostabilný dá jeden impulz, astabilný kmitá.
- Batériové zariadenie na rok → **CMOS** (najmenšia spotreba).
- Logické úrovne (bolo v praxi 2025):
  - **CMOS:** L < 0,3·Vcc, H > 0,7·Vcc
  - **TTL (5 V):** L < 0,8 V, H > 2,0 V

### 2.13 Prevodníky, osciloskop
- **ADC** = analógovo-číslicový, **DAC** = číslicovo-analógový prevodník.
- n-bitový prevodník má **2ⁿ úrovní**: 4 bity = **16**, 8 bitov = **256**, 10 bitov = 1024.
- Nyquist: vzorkovanie 1 Msps → max. frekvencia signálu **500 kHz**.
- Osciloskop na digitálne signály: šírka pásma aspoň **~5× vyššia** ako frekvencia signálu (10 MHz → **75 MHz** model).
- Zachytiť prvý paket a neprepísať ho → spúšťanie **Single**.

### 2.14 Označenie SMD rezistorov
3 číslice: **dve číslice + počet núl**; 4 číslice: **tri číslice + počet núl**.
- 103 = 10 000 = **10 kΩ**
- 243 = **24 kΩ**
- 472 = 4,7 kΩ
- 2201 = 220·10 = **2,2 kΩ**
- 4R7 = 4,7 Ω (R = desatinná čiarka)

### 2.15 Rôzne „chytáky“
- Transformátor so 4 rovnakými vinutiami: 2 do série primár, 2 paralelne sekundár → prevod 2:1 → **115 V**.
- E12 na 3 dekády → 3 × 12 = **36** hodnôt.
- Elektromagnetické vlny vo vákuu: **300 000 km/s**.
- Točené páry (krútená dvojlinka) → potlačenie rušenia (indukovaného napätia).
- Blokovacie kondenzátory 100 nF pri IO → filtrujú **vysokofrekvenčné** rušenie, musia byť **čo najbližšie k napájacím pinom IO**.

---

## 3. Praktická časť – návrh DPS + výroba + osadenie

### 3.1 Čo bolo zadané v minulých rokoch (rovnaký formát každý rok)
| Rok | Zariadenie | Kľúčové súčiastky |
|---|---|---|
| 2018 | Hracia kocka | NE555, CD4017, diódy, LED |
| 2019 | Modul k Arduinu | 74HC595, 7-seg displej, enkóder, NTC |
| 2020 | Tester kontinuity | NE555, 4013 (D flip-flop), BC817, BSS84 |
| 2021 | Elektronická poistka | LM2903 komparátor, 74HC74, TL431, P-MOSFET |
| 2022 | Tester ESD náramkov | TL072, MC14001 (NOR), LED |
| 2023 | Merač V-A charakteristík | HEF4060 čítač, prúdové zrkadlá |
| 2024 | Merač saturácie cievok | TLC3702, MOSFET, vysoký prúd |
| 2025 | Logická sonda | TLV1812, HEF4538 (monostabil), BC847 |

→ Typicky: **5 – 30 SMD súčiastok, púzdra 1206, SOT23, SO8, SO14** + pár väčších dielov.

### 3.2 Pravidlá, ktoré sa opakujú každý rok (DODRŽAŤ = body)
- [ ] **Jednostranný** plošný spoj
- [ ] **Bez autoroutra!** (Ťahať spoje ručne.)
- [ ] Šírka spoja **min. 0,6 mm**, medzera **min. 0,6 mm** (výnimočne krátko 0,4 mm, nikdy menej)
- [ ] **Žiadne polygóny** (zalievanie medi) – iba ak je výslovne povolené pre vysoký prúd
- [ ] Ak sa nedá prejsť → **drôtová prepojka** (0 Ω rezistor / drôt) – tiež na vrchnej strane
- [ ] **Dosky sa nevŕtajú** → všetko je osadené na **vrchnej strane** (spoje kresli na **F.Cu**)
- [ ] Rozmer kategória B: posledné roky **max. 75 × 100 mm**
- [ ] **Kompaktný a usporiadaný** návrh dostáva viac bodov ako „rozťahaný“
- [ ] **Ergonómia:** konektory a ovládacie prvky na **hranu dosky**, LED v logickom poradí, **popisy k LED v medenej vrstve** (aby boli vidno na vyleptanej doske)
- [ ] Súbor nahrať pomenovaný **súťažným číslom**, napr. `B06.kicad_pcb` (+ PDF, ak ho chcú)
- [ ] **Prečítaj si celé zadanie 2×** – vždy je tam niečo špecifické (umiestnenie konektorov, rozostup svoriek, presná poloha tlačidla…)

### 3.3 Postup v KiCade (rýchlo a bez chýb)
1. Skontroluj, že **schéma = zadanie** a každá súčiastka má **správny footprint** (1206, SOT-23, SOIC-8/14 …). Toto je najčastejšia chyba!
2. **Board Setup → Net Classes:** šírka spoja 0,6 mm, clearance 0,6 mm. Tak to nemusíš strážiť ručne.
3. Nakresli obrys dosky v **Edge.Cuts**.
4. **Rozmiestnenie je 80 % práce.** Najprv IO, okolo nich ich súčiastky podľa schémy, konektory na kraj. Otáčaj súčiastky tak, aby sa „ratsnest“ (tenké čiary) čo najmenej križovali.
5. Ťahaj spoje (klávesa **X**), všetko na **F.Cu**. Najprv napájanie a zem, potom signály.
6. Kde sa nedá prejsť – pod 1206 rezistorom sa dá často pretiahnuť jeden spoj, inak prepojka.
7. Ak sa motív opakuje (ako v 2025), navrhni jednu časť a skopíruj ju.
8. Pridaj text (číslo súťažiaceho, popisy LED) do **F.Cu**.
9. Spusti **DRC** (Inspect → Design Rules Checker) – 0 chýb, 0 nespojených sietí!
10. Ulož, nahraj súbor. **Export PDF v mierke 1:1** (ak treba, zrkadlovo – podľa pokynov komisie).

**Klávesy v KiCade (PCB editor):** `M` posun, `R` otočiť, `X` spoj, `D` ťahať spoj, `Delete` zmazať, `Backspace` zmazať celý spoj, `E` vlastnosti, `B` prepočítať zónu, `Ctrl+Shift+F` DRC (podľa verzie).

**Časový plán návrhu:** cca 1/3 času rozmiestnenie, 1/3 ťahanie spojov, potom kontrola. Neťahaj to do nekonečna – ak doska nebude vyrobená a osadená, prídeš o veľa bodov za funkčnosť.

### 3.4 Spájkovanie SMD – postup
1. **Pocínuj jednu plôšku** súčiastky.
2. Pinzetou prilož súčiastku, nahrej pocínovanú plôšku → súčiastka „prilepená“.
3. Zaspájkuj druhú stranu, potom znovu prvú (aby bola pekná).
4. **IO (SO8/SO14):** skontroluj **pin 1** (bodka/zárez), prichyť dva protiľahlé rohy, skontroluj polohu, potom zvyšok. Mostíky odstráň **odsávacím lankom + tavidlom**.
5. Poradie: najprv **nízke** súčiastky (rezistory, kondenzátory), potom IO, nakoniec **vysoké** (konektory, prepínače, veľké kondenzátory).
6. Tavidlo je kamarát. Po spájkovaní dosku umyť (hlavne pri vysokých odporoch – zvodové prúdy).

**POLARITA – kontroluj 2×:**
- **Dióda:** prúžok = **katóda**
- **LED:** katóda = kratšia nožička / zelená značka / skosená strana (pri SMD často značka na spodku)
- **Tantalový kondenzátor:** prúžok = **PLUS (+)** ← pozor, opačne ako pri elektrolyte!
- **Elektrolyt:** prúžok = **mínus (−)**
- **IO:** bodka / zárez = **pin 1**
- **Tranzistor SOT-23:** 1 = B (G), 2 = E (S), 3 = C (D) – ale **vždy podľa footprintu v návrhu**

### 3.5 Oživenie (body za funkčnosť!)
1. **Pred zapnutím:** zrakom skontroluj mostíky cínu a otočenie súčiastok.
2. **Multimetrom zmeraj odpor medzi napájaním a zemou** – nesmie tam byť skrat (0 Ω).
3. Napájaj z laboratórneho zdroja s **prúdovým obmedzením** (napr. 50–100 mA).
4. Ak odber vyskočí na limit → okamžite vypni, hľadaj skrat / otočenú súčiastku.
5. Zmeraj napájacie napätie priamo na pinoch IO.
6. Choď po signáli od vstupu k výstupu a meraj, kde to prestane fungovať.
7. Typické chyby: studený spoj, mostík medzi pinmi IO, otočená dióda/LED, zlá hodnota rezistora (pomýlené kotúčiky/pásky), prerušený spoj po leptaní.

---

## 4. Posledné rady

- **Vyspi sa.** Unavený mozog robí chyby v prevodoch jednotiek.
- Zober si: **kalkulačku (s batériou!)**, perá, preukaz/ISIC, pitie a niečo na jedenie.
- Ak smú mať vlastné náradie (opýtaj sa organizátora): jemná pinzeta, tavidlo, odsávacie lanko, multimeter, lupa.
- V teste sa neboj „bláznivých“ možností (Banič, „prestane vesmír existovať“) – sú tam pre zábavu, takmer nikdy nie sú správne.
- Pri obrázkových otázkach (schematické značky, grafy) sa riaď sekciou 2.8 – 2.10.
- Ak sa v praxi na niečom zasekneš na viac ako 10 minút, choď ďalej a vráť sa k tomu.

**Veľa šťastia! Elektroniku vieš, toto je len o tom nestratiť zbytočné body.** ⚡
