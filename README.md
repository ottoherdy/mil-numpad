# mil-numpad

Trådlöst 17-tangents numpad — ZMK-config för handwired build med
**Tenstar Robot Pro Micro nRF52840** + **1N4148**-dioder.

**Status: fungerar ✅** (USB + BLE, ZMK Studio aktiverat)

## Layout

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

## Matris (5 rader × 4 kolumner, `row2col`)

| | C0 | C1 | C2 | C3 |
|---|---|---|---|---|
| R0 | NumLock | / | * | - |
| R1 | 7 | 8 | 9 | — |
| R2 | 4 | 5 | 6 | **+** |
| R3 | 1 | 2 | 3 | — |
| R4 | 0 | — | . | **Enter** |

Diodriktning: **katoden (svart streck) pekar mot KOLUMNEN**.
(Det är `row2col` — rader är drive-pinnar, kolumner läses med pull-down.)

## Pin-mappning (Tenstar Robot / nice!nano v2 footprint)

Silkscreen-labels är nRF52840 GPIO-pins.

| Silk | pro_micro pos | Roll |
|---|---|---|
| `100` | 6  | R0 — röd diod-tråd: NumLock, /, *, - |
| `024` | 5  | R1 — 7, 8, 9 |
| `017` | 2  | R2 — 4, 5, 6, + |
| `020` | 3  | R3 — 1, 2, 3 |
| `022` | 4  | R4 — 0, ., Enter |
| `010` | 16 | C0 — svart kolumn-tråd: NumLock, 7, 4, 1, 0 |
| `111` | 14 | C1 — /, 8, 5, 2 |
| `113` | 15 | C2 — *, 9, 6, 3, . |
| `115` | 18 | C3 — -, +, Enter |

## Bygga firmware

GitHub Actions bygger automatiskt vid varje push (se [build.yaml](build.yaml)).

Hämta `.uf2` från [Actions-fliken](../../actions) → senaste körningen →
Artifacts → `firmware.zip` → packa upp → `mil-numpad-nice_nano__zmk-zmk.uf2`.

## Flasha

Tenstar Robot saknar fysisk reset-knapp. Bootloader triggas så här:

1. Koppla Pro Micron via **USB-C (datakabel)** till datorn.
2. **Kortslut RST mot GND två gånger snabbt** (~0,5 s mellan trycken).
   Pinnarna sitter intill varandra på höger sida. Pincett eller metallgem
   funkar bra.
3. USB-disken **`NICENANO`** dyker upp.
4. Dra `.uf2`-filen till disken. Pro Micron startar om automatiskt.
5. Numpaden dyker upp som HID-tangentbord ("Mil Numpad").

> macOS poppar upp en tangentbordsinställningsguide vid första anslutning.
> Stäng den — numpaden kan inte slutföra layout-identifieringen utan Shift.

## Anpassa keymap

### Variant A: ZMK Studio (live i webbläsaren — rekommenderas)

1. Koppla numpaden via USB-C.
2. Tryck **`0` + `.`** samtidigt → låser upp Studio.
3. Öppna **https://zmk.studio** i Chrome eller Edge.
4. Klicka "Connect" → välj numpaden.
5. Drag-and-drop tangenter, spara — ändringen sker direkt utan att bygga om.

### Variant B: Edit + bygg om

Redigera [mil-numpad.keymap](config/boards/shields/mil-numpad/mil-numpad.keymap),
pusha → Actions bygger ny `.uf2` → flasha enligt ovan.

Tangentkoder finns i [ZMK keycode-reference](https://zmk.dev/docs/keymaps/list-of-keycodes).

## Verktyg & referenser

- [Keyboard Layout Editor](https://www.keyboard-layout-editor.com/) — designa layout
- [Plate & Case Builder (swillkb)](http://builder.swillkb.com) — generera DXF för plate
- [Keyboard Firmware Builder (kbfirmware)](https://kbfirmware.com) — visualisera matris
- [ZMK Studio](https://zmk.studio) — live keymap-editor
- [keyboardchecker.com](https://keyboardchecker.com) — testa att alla tangenter registreras

## Build-guider

- [Matt3o — Hand-wiring a custom keyboard](https://matt3o.com/hand-wiring-a-custom-keyboard/)
- [Golem — Diodes](https://golem.hu/guide/diodes/)
- [Iskra handwire numpad (närmast detta bygge)](https://github.com/vostoklabs/Iskra-handwire-numpad)
- [ZMK — New Keyboard Shield](https://zmk.dev/docs/development/hardware-integration/new-shield)
- [ZMK Studio — setup för egna shields](https://zmk.dev/docs/features/studio)
