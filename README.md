# mil-numpad

Trådlöst 17-tangents numpad — ZMK-config för handwired build med
**Tenstar Robot Pro Micro nRF52840** + **1N4148**-dioder.

## Layout

Standard 17-tangents numpad: 4 visuella kolumner × 5 visuella rader.
`+` och `Enter` är 2u höga, `0` är 2u bred.

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

## Matris (5 rader × 4 kolumner, col2row)

| | C0 | C1 | C2 | C3 |
|---|---|---|---|---|
| R0 | NumLock | / | * | - |
| R1 | 7 | 8 | 9 | — |
| R2 | 4 | 5 | 6 | **+** |
| R3 | 1 | 2 | 3 | — |
| R4 | 0 | — | . | **Enter** |

`+` och `Enter` har switchen i nedre halvan av sin 2u-keycap → hamnar på R2 respektive R4.

## Pin-mappning (nice_nano_v2 / Tenstar Robot)

| Pro Micro | Roll |
|---|---|
| D0 | R0 (röd diod-tråd: NumLock, /, *, -) |
| D1 | R1 (7, 8, 9) |
| D2 | R2 (4, 5, 6, +) |
| D3 | R3 (1, 2, 3) |
| D4 | R4 (0, ., Enter) |
| D5 | C0 (svart kolumn-tråd: NumLock, 7, 4, 1, 0) |
| D6 | C1 (/, 8, 5, 2) |
| D7 | C2 (*, 9, 6, 3, .) |
| D8 | C3 (-, +, Enter) |

## Bygga firmware

1. Pusha repot till GitHub.
2. GitHub Actions bygger automatiskt enligt [build.yaml](build.yaml).
3. Ladda ned `firmware.zip` från Actions → `mil-numpad-nice_nano_v2.uf2`.
4. Koppla Pro Micron via USB, dubbeltryck reset → drag-and-drop `.uf2` till `NICENANO`-disken.

## Anpassa keymap

Redigera [config/boards/shields/mil-numpad/mil-numpad.keymap](config/boards/shields/mil-numpad/mil-numpad.keymap).
Tangentkoder finns i ZMK-dokumentationen.

## Verktyg & referenser

- [Keyboard Layout Editor](https://www.keyboard-layout-editor.com/) — designa layout, exportera JSON
- [Plate & Case Builder (swillkb)](http://builder.swillkb.com) — generera DXF för plate
- [Keyboard Firmware Builder (kbfirmware)](https://kbfirmware.com) — visualisera matris (för referens; vi använder ZMK, inte QMK)

## Build-guider

- [Matt3o — Hand-wiring a custom keyboard](https://matt3o.com/hand-wiring-a-custom-keyboard/)
- [Golem — Diodes](https://golem.hu/guide/diodes/)
- [Iskra handwire numpad (närmast detta bygge)](https://github.com/vostoklabs/Iskra-handwire-numpad)
- [ZMK — New Keyboard Shield](https://zmk.dev/docs/development/hardware-integration/new-shield)
