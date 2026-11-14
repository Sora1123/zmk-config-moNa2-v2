# Trackball gesture cheat sheet (moNa2)

This reference describes the **current** moNa2 firmware. The names in the gesture
source can be misleading; the shortcut column shows the **actual keys sent**, not
a guarantee about the app or operating system's response.

## Which keys activate gestures?

These are positions on the default keymap. **Hold the key(s), move the
trackball left/right/up/down, then release.**

```text
 RIGHT HALF                         Hold to activate
 Top:       Y   U   I   O   P       O+P  -> Layer 8
 Home:  ;   H   J   K   L   -       L+-  -> Layer 9
                                    -    -> Layer 7
```

| Hold this key/combo | Layer | Short tap action | Purpose |
| --- | --- | --- | --- |
| **`-`** at the far-right end of the home row | **7** | `-` | Browser tabs |
| **`O` + `P`** simultaneously | **8** | Alt+Tab | Windows desktops / Task View |
| **`L` + `-`** simultaneously | **9** | GUI key (Win on Windows / Command on Mac) | Mixed utility shortcuts |

The two-key combos are defined at base keymap positions `<8 9>` (O+P) and
`<19 20>` (L+-). They have no explicit `layers` restriction. For combos,
the ZMK default press window is 50 ms. All three triggers are `&lt`
(layer-tap) bindings, with a configured tapping term of 200 ms and balanced
hold/tap behavior. **A quick tap is not the same as a hold.**

## Layer 7: hold `-` and roll the trackball

Source: [gesture_L7.dtsi](../config/gesture/gesture_L7.dtsi)

| Direction | Exact HID shortcut | Typical Windows browser effect | macOS caveat |
| --- | --- | --- | --- |
| **Left** | Ctrl+Shift+Tab | Previous tab | Not the usual Mac shortcut |
| **Right** | Ctrl+Tab | Next tab | App-dependent |
| **Up** | Ctrl+T | New tab | Normally Cmd+T on Mac |
| **Down** | Ctrl+W | Close tab | Normally Cmd+W on Mac |

## Layer 8: hold `O` + `P` and roll the trackball

Source: [gesture_L8.dtsi](../config/gesture/gesture_L8.dtsi)

| Direction | Exact HID shortcut | Typical Windows effect |
| --- | --- | --- |
| **Left** | Win+Ctrl+Left | Previous virtual desktop |
| **Right** | Win+Ctrl+Right | Next virtual desktop |
| **Up** | Win+Tab | Task View |
| **Down** | Win+Ctrl+Shift+Right | Sends this exact combo; **not a standard built-in “move window to next desktop” shortcut** |

The Mac generally uses different shortcuts for Mission Control and Spaces.
These hard-coded gesture bindings **do not change** with the firmware's
Windows-mode toggle.

## Layer 9: hold `L` + `-` and roll the trackball

Source: [gesture_L9.dtsi](../config/gesture/gesture_L9.dtsi)

| Direction | Exact HID shortcut | Typical Windows effect |
| --- | --- | --- |
| **Left** | Left Arrow | Move the insertion point left / navigate left |
| **Right** | Win+T | Focus or cycle taskbar apps |
| **Up** | Ctrl+L | Focus browser address bar |
| **Down** | Win+S | Windows Search |

Note that gesture source node labels such as `vdesk_send_next` and
`taskbar_next` are developer-chosen names; consult these shortcut tables
rather than assuming their labels describe their real effects.

## Normal trackball functions (not gesture strokes)

| Activation | Layer | Pointer behavior |
| --- | --- | --- |
| Move trackball normally | Base / temporary **5** | Pointer movement. The right overlay configures `&zip_temp_layer 5 10000` to activate mouse-button layer 5 on movement |
| Hold **`P`** | **6** | Movement is translated to scroll events using `zip_xy_to_scroll_mapper`, Y inversion, and 1/8 XY scaling |
| Hold **`LANG1`** | **3** | Trackball scrolling via the right overlay's scroller, with horizontal-axis inversion and 1/5 scroll scaling |
| Hold **Space** | **2** | Lower pointer sensitivity (XY scaling 1/3) |

Mouse button bindings in the temporary layer 5:

| Physical key | Mouse function |
| --- | --- |
| **J** | Left click (`MB1`) |
| **K** | Middle click (`MB3`) |
| **L** | Right click (`MB2`) |
| **M** | Button 4 (often Back) |
| **.** (period) | Button 5 (often Forward) |

Automatic mouse-layer behavior has excluded key positions
`<10 17 18 19 21 29 31>` and a configured temporary-layer
parameter of `10000`. Holding multiple scroll/gesture triggers at
the same time may activate overlapping input processors, so use
one mode at a time.

## Gesture recognition settings

All three gesture processors specify:
```dts
enable-eager-mode;
stroke-size = <5>;
movement-threshold = <6>;
idle-timeout-ms = <100>;
gesture-cooldown-ms = <120>;
```

The trackball sensor in
[mona2_r.overlay](../boards/shields/mona2/mona2_r.overlay)
also has `invert-x` and `invert-y` enabled for COROPIT.
If the physical swipe direction does not match the table, check that
sensor inversion setting. Hardware response has not been independently tested.

## Special characters: layer 2

Hold **Space** (layer 2). The old Windows Alt-code macros have been deleted.

| Default key position | Keycode now sent | Expected US-layout character |
| --- | --- | --- |
| **;** (extra right-hand home-row key) | `&kp ASTRK` | `*` |
| **"** (inner right-hand third-row key) | `&kp SLASH` | `/` |

Both are ordinary keystrokes, so output depends on the host's keyboard layout.

## Source of truth

- [Keymap, combo triggers and layers](../config/mona2.keymap)
- [Trackball input listeners and layer 6–9 processors](../boards/shields/mona2/mona2.dtsi)
- [Trackball overlay, automatic mouse mode and layer-3 scroller](../boards/shields/mona2/mona2_r.overlay)
- [ZMK layer-tap documentation](https://zmk.dev/docs/keymaps/behaviors/hold-tap)
- [ZMK combo documentation](https://zmk.dev/docs/keymaps/combos)
