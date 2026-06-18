# mil-numpad / pad-v2

Två trådlösa handwired numpads — ZMK-config för **Tenstar Robot Pro Micro nRF52840** + **1N4148**-dioder.

- **mil-numpad** — 17 tangenter (standard numpad-layout)
- **pad-v2** — 18 tangenter (extra knapp på rad 2, mer kompakt layout)

**Status: båda fungerar ✅** (USB + BLE, ZMK Studio aktiverat, BLE-profilväxling, bootloader-combo)

Bara `pad-v2` byggs i [build.yaml](build.yaml) just nu — `mil-numpad` är klar och flashad. Lägg till raden tillbaka om du behöver bygga om den.

---

## mil-numpad

### Layout

Standard 17-tangents numpad: 4 visuella kolumner × 5 visuella rader.
`+` och `Enter` är 2u höga med switchen i nedre halvan, `0` är 2u bred
med switchen i vänstra halvan.

```
┌─────────┬─────┬─────┬─────┐
│ NumLock │  /  │  *  │  -  │
├─────────┼─────┼─────┼─────┤
│    7    │  8  │  9  │     │
├─────────┼─────┼─────┤  +  │
│    4    │  5  │  6  │     │
├─────────┼─────┼─────┼─────┤
│    1    │  2  │  3  │     │
├─────────┴─────┼─────┤ Ent │
│       0       │  .  │     │
└───────────────┴─────┴─────┘
```

### Matris (5 rader × 4 kolumner, `row2col`)

| | C0 | C1 | C2 | C3 |
|---|---|---|---|---|
| R0 | NumLock | / | * | - |
| R1 | 7 | 8 | 9 | — |
| R2 | 4 | 5 | 6 | **+** |
| R3 | 1 | 2 | 3 | — |
| R4 | 0 | — | . | **Enter** |

### Pin-mappning

| Silk | pro_micro pos | Roll |
|---|---|---|
| `100` | 6  | R0 — NumLock, /, *, - |
| `024` | 5  | R1 — 7, 8, 9 |
| `017` | 2  | R2 — 4, 5, 6, + |
| `020` | 3  | R3 — 1, 2, 3 |
| `022` | 4  | R4 — 0, ., Enter |
| `010` | 16 | C0 — NumLock, 7, 4, 1, 0 |
| `111` | 14 | C1 — /, 8, 5, 2 |
| `113` | 15 | C2 — *, 9, 6, 3, . |
| `115` | 18 | C3 — -, +, Enter |

---

## pad-v2

### Layout

18 tangenter — som mil-numpad fast med `Del`/`-`/`+` omorganiserat och en extra `+` på rad 2:

```
┌─────────┬─────┬─────┬─────┐
│ NumLock │  /  │  *  │ Del │
├─────────┼─────┼─────┼─────┤
│    7    │  8  │  9  │  -  │
├─────────┼─────┼─────┼─────┤
│    4    │  5  │  6  │  +  │
├─────────┼─────┼─────┼─────┤
│    1    │  2  │  3  │     │
├─────────┴─────┼─────┤ Ent │
│       0       │  .  │     │
└───────────────┴─────┴─────┘
```

### Matris (5 rader × 4 kolumner, `row2col`)

| | C0 | C1 | C2 | C3 |
|---|---|---|---|---|
| R0 | NumLock | / | * | **Del** |
| R1 | 7 | 8 | 9 | **−** |
| R2 | 4 | 5 | 6 | **+** |
| R3 | 1 | 2 | 3 | — |
| R4 | 0 | — | . | **Enter** |

### Pin-mappning

| Silk | pro_micro pos | Roll |
|---|---|---|
| `010` | 16 | R0 — NumLock, /, *, Del (flyttad från `006`/`008` p.g.a. UART-konflikt) |
| `017` | 2  | R1 — 7, 8, 9, − |
| `020` | 3  | R2 — 4, 5, 6, + |
| `022` | 4  | R3 — 1, 2, 3 |
| `024` | 5  | R4 — 0, ., Enter |
| `106` | 9  | C0 — NumLock, 7, 4, 1, 0 |
| `104` | 8  | C1 — /, 8, 5, 2 |
| `011` | 7  | C2 — *, 9, 6, 3, . |
| `100` | 6  | C3 — Del, −, +, Enter |

> **Obs:** undvik pinnar `006` (pro_micro 0) och `008` (pro_micro 1) — de är RX/TX-pinnar för UART och kan vara reserverade av Zephyr som default.

---

## Gemensamt

### Diodriktning

**Katoden (svart streck) pekar mot KOLUMNEN.**

Det är `row2col` — rader är drive-pinnar (output high), kolumner läses med pull-down. Dioderna sitter mellan switchens icke-rad-pinne och kolumn-tråden, med anod-änden vid switchen och katod-änden vid kolumnen.

### Bygga firmware

GitHub Actions bygger automatiskt vid varje push (se [build.yaml](build.yaml)).

Hämta `.uf2` från [Actions-fliken](../../actions) → senaste körningen →
Artifacts → `firmware.zip` → packa upp → `pad-v2-nice_nano__zmk-zmk.uf2` (eller mil-numpad-motsvarigheten).

### Flasha

**Första gången** (innan bootloader-comboen finns i firmware):

1. Koppla Pro Micron via **USB-C (datakabel)** till datorn.
2. **Kortslut RST mot GND två gånger snabbt** (~0,5 s mellan trycken).
3. USB-disken **`NICENANO`** dyker upp.
4. Dra `.uf2`-filen till disken. Pro Micron startar om automatiskt.

**Efterföljande gånger** (när bootloader-comboen är aktiv):

1. Håll `NumLock` + `0` + `Enter` samtidigt → `NICENANO`-disk poppar upp.
2. Drag-and-drop `.uf2`-filen.

> macOS poppar upp en tangentbordsinställningsguide vid första anslutning.
> Stäng den — numpaden kan inte slutföra layout-identifieringen utan Shift.

### Combos

| Tryck samtidigt | Vad det gör |
|---|---|
| `0` + `.` | Lås upp ZMK Studio (krävs innan du kan ändra keymap från webbläsaren) |
| `NumLock` + `0` + `Enter` | Tvinga in DFU-bootloader (när chassit är monterat och RST/GND ej går att nå) |

### Bluetooth / BLE-profiler (pad-v2)

ZMK stödjer **5 BLE-profiler** (0-4). Varje profil = en parkopplad enhet. Numpaden minns parkopplingarna mellan omstarter.

#### Hold-tap på NumLock

På `pad-v2` är `NumLock` en hold-tap:

- **Snabbtryck** (< 250 ms) → Num Lock som vanligt
- **Håll inne** (> 250 ms) → BLE-layern aktiv så länge du håller

#### BLE-layer bindings

Håll `NumLock` och tryck en av dessa:

| Tangent | Funktion |
|---|---|
| `/` | Nästa BLE-profil (`BT_NXT`) |
| `*` | Föregående BLE-profil (`BT_PRV`) |
| `Del` | Rensa nuvarande profil (`BT_CLR`) — för att para om |
| `7` | Välj profil 0 (`BT_SEL 0`) |
| `8` | Välj profil 1 |
| `9` | Välj profil 2 |
| `-` | Välj profil 3 |
| `4` | Välj profil 4 |
| `5` | Tvinga USB-output (`OUT_USB`) |
| `6` | Tvinga BLE-output (`OUT_BLE`) |
| `+` | Toggla USB ↔ BLE (`OUT_TOG`) |

#### Typiska flöden

**Para första gången (Dator A):**
1. Håll `NumLock` + tryck `7` → numpaden börjar advertise på profil 0
2. På Dator A: Bluetooth-inställningar → para med "Pad V2"

**Lägg till Dator B:**
1. Håll `NumLock` + tryck `8` → tom profil 1, advertise
2. På Dator B: para via Bluetooth

**Byt mellan datorerna:**
- Håll `NumLock` + `7` → Dator A
- Håll `NumLock` + `8` → Dator B

**Para om (rensa felaktig parkoppling):**
1. Håll `NumLock` + tryck `Del` → rensar nuvarande profil
2. Para om från datorn

### Batteri-inställningar (`.conf`)

Sömn och idle-timeout finns i [pad-v2.conf](config/boards/shields/pad-v2/pad-v2.conf):

| Config | Vad det styr | Nuvarande |
|---|---|---|
| `CONFIG_ZMK_SLEEP` | Aktiverar deep sleep | `y` |
| `CONFIG_ZMK_IDLE_SLEEP_TIMEOUT` | Tid utan tryck innan deep sleep (ms) | `900000` (15 min) |
| `CONFIG_ZMK_STUDIO` | ZMK Studio aktivt | `y` |

Batterinivå rapporteras automatiskt via BLE — syns i macOS Bluetooth-menyn (klicka Bluetooth-ikonen i menyraden).

### Anpassa keymap

#### Variant A: ZMK Studio (live i webbläsaren — rekommenderas)

1. Koppla numpaden via USB-C.
2. Tryck **`0` + `.`** samtidigt → låser upp Studio.
3. Öppna **https://zmk.studio** i Chrome eller Edge.
4. Klicka "Connect" → välj numpaden.
5. Drag-and-drop tangenter, spara — ändringen sker direkt utan att bygga om.

> **Tips:** välj `Backspace` (BSPC) — inte `Keypad Backspace` — för svensk locale.
> Samma sak för `.` → använd `DOT`, inte `KP_DOT` (annars blir det `,`).

#### Variant B: Edit + bygg om

Redigera:
- [pad-v2.keymap](config/boards/shields/pad-v2/pad-v2.keymap)
- eller [mil-numpad.keymap](config/boards/shields/mil-numpad/mil-numpad.keymap)

Pusha → Actions bygger ny `.uf2` → flasha enligt ovan.

Tangentkoder finns i [ZMK keycode-reference](https://zmk.dev/docs/keymaps/list-of-keycodes).

### Verktyg & referenser

- [Keyboard Layout Editor](https://www.keyboard-layout-editor.com/) — designa layout
- [Plate & Case Builder (swillkb)](http://builder.swillkb.com) — generera DXF för plate
- [Keyboard Firmware Builder (kbfirmware)](https://kbfirmware.com) — visualisera matris
- [ZMK Studio](https://zmk.studio) — live keymap-editor
- [keyboardchecker.com](https://keyboardchecker.com) — testa att alla tangenter registreras

### Build-guider

- [Matt3o — Hand-wiring a custom keyboard](https://matt3o.com/hand-wiring-a-custom-keyboard/)
- [Golem — Diodes](https://golem.hu/guide/diodes/)
- [Iskra handwire numpad (närmast detta bygge)](https://github.com/vostoklabs/Iskra-handwire-numpad)
- [ZMK — New Keyboard Shield](https://zmk.dev/docs/development/hardware-integration/new-shield)
- [ZMK Studio — setup för egna shields](https://zmk.dev/docs/features/studio)
- [ZMK BLE profiles & output switching](https://zmk.dev/docs/features/bluetooth)
