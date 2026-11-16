# moNa2 trackball reference

## Activation keys

Hold the indicated key(s) for about 200 ms, roll in the desired direction,
then release. For two-key combos, press both within 50 ms and hold both.
Use only one gesture/scroll trigger at a time.

```text
Right half
              Y   U   I   O   P    O+P = virtual desktops
          ;   H   J   K   L   :    L+: = window placement
          "   N   M   ,   .   /    ; = history, : = tabs, " = Dia Spaces
```

The colon is the physical former minus position (base position 20).
The normal minus is still available by holding Space and pressing G.
The base semicolon and double quote taps remain unchanged; their holds
have new gesture roles.

| Hold | Layer | Short tap |
| --- | --- | --- |
| `;` | 12 (History) | Semicolon |
| `:` | 7 (Tabs) | Colon |
| `"` | 13 (Dia Spaces) | Double quote |
| O+P | 8 (Virtual desktops) | Cmd+Tab on Mac / Alt+Tab on Windows |
| L+: | 9 (Window placement) | Command on Mac / Windows key on Windows |

Both combos are available on the base and mouse layers (including Windows mode).
P alone still enters scroll layer 6.

## Gesture cheat sheet

| Hold | Left | Right | Up | Down |
| --- | --- | --- | --- | --- |
| `;` | Back page | Forward page | — | — |
| `:` | Previous tab | Next tab | New tab | Close tab |
| `"` | Previous Dia Space | Next Dia Space | — | — |
| O+P | Previous virtual desktop | Next virtual desktop | — | — |
| L+: | Left half | Right half | Next display | Fill display |

### Exact shortcuts sent to host

| Gesture | macOS / default | Windows mode |
| --- | --- | --- |
| History left | Cmd+[ | HID Consumer AC Back |
| History right | Cmd+] | HID Consumer AC Forward |
| Tabs left | Ctrl+Shift+Tab | Ctrl+Shift+Tab |
| Tabs right | Ctrl+Tab | Ctrl+Tab |
| Tabs up | Cmd+T | Ctrl+T |
| Tabs down | Cmd+W | Ctrl+W |
| Dia left | Option+Cmd+Left | Alt+Ctrl+Left |
| Dia right | Option+Cmd+Right | Alt+Ctrl+Right |
| Desktop left | Ctrl+Left | Win+Ctrl+Left |
| Desktop right | Ctrl+Right | Win+Ctrl+Right |
| Window left | Ctrl+Option+Cmd+. | Win+Left |
| Window right | Ctrl+Option+Cmd+/ | Win+Right |
| Window up | Ctrl+Option+Cmd+Up | Win+Shift+Right |
| Window down | Ctrl+Option+Cmd+, | Win+Up |
| Mouse layer D | Cmd+Ctrl+D | D |

Back/Forward use dedicated consumer keys in Windows mode; these are
standard HID usages but support depends on the host/app. On Mac use the
specified Cmd+bracket shortcuts because ZMK's keycode support table
does not confirm Mac support for the consumer Back/Forward usages.
The other requested actions have no universally supported dedicated
single HID key for the exact behavior, so they use shortcuts.

Mac window placement/display gestures send the custom shortcuts configured
in your window manager. Fill display means fill the available display
without entering macOS native full-screen (a separate Space).
Windows uses native Snap, maximize and move-to-right-monitor shortcuts;
the equivalent fill behavior is ordinary maximize, and the monitor action
moves to the right monitor (not a guaranteed arbitrary-display cycle).
Dia Spaces are application-specific: on Windows, assign the listed
Ctrl+Alt+Left/Right shortcuts in Dia Settings > Shortcuts if needed.
The Mac virtual desktop shortcuts must be enabled in System Settings >
Keyboard > Keyboard Shortcuts > Mission Control.

## Normal trackball and mouse layer

| Activation | Action |
| --- | --- |
| Move trackball | Normal pointer; automatically activates layer 5 after idle |
| Hold Tab or Esc | Manually activate mouse layer 5 |
| Hold P | Scroll (layer 6) |
| Hold LANG1 | Alternate scrolling (layer 3) |
| Hold Space | Precision pointer (layer 2, 1/3 sensitivity) |

| Key with mouse layer active | Action |
| --- | --- |
| J | Left click |
| K | Middle click |
| L | Right click |
| M | Mouse Button 4 (often Back) |
| . | Mouse Button 5 (often Forward) |
| D | macOS Dictionary / Look Up; Windows: ordinary D |

Hover over or select a word before pressing D. In automatic mouse mode,
D is excluded from the position-based exit list, as are the existing
mouse buttons. Tap Z or another non-excluded key to exit before ordinary
typing; holding Tab/Esc is a way to deliberately enter mouse mode.
Automatic activation uses a 300 ms idle requirement and 10 s timeout.

## Recognition

The five gesture groups retain eager mode, stroke size 5, movement threshold 6,
100 ms idle timeout and 120 ms cooldown. Cursor movement is suppressed
during a gesture. Windows conditional layers are evaluated before Mac
gesture layers in the ZMK input listener, and gestures take priority over
scroll/precision overrides. Real hardware direction and timing still require
testing (COROPIT sensor has X/Y inversion enabled).

## Source files

- [Keymap and mode toggle](../config/mona2.keymap)
- [Tab gestures](../config/gesture/gesture_L7.dtsi)
- [Virtual desktop gestures](../config/gesture/gesture_L8.dtsi)
- [Window placement gestures](../config/gesture/gesture_L9.dtsi)
- [Page history gestures](../config/gesture/gesture_L12.dtsi)
- [Dia Spaces gestures](../config/gesture/gesture_L13.dtsi)
- [Input listener and layer listeners](../boards/shields/mona2/mona2.dtsi)
- [Auto-mouse configuration](../boards/shields/mona2/mona2_r.overlay)
- [ZMK keycodes](https://v0-3-branch.zmk.dev/docs/keymaps/list-of-keycodes)
- [Windows keyboard shortcuts](https://support.microsoft.com/en-us/accessibility/windows/keyboard-shortcuts-in-windows)

## Firmware smoke-test checklist

- Boot in Mac mode; verify Ctrl/Option/Cmd order, colon and Space+G minus.
- Check every arrow in the gesture table; verify the unused directions do nothing.
- Verify ordinary cursor, mouse clicks, scrolling, precision, and D lookup.
- Toggle Windows mode with LANG2+A; repeat and verify Ctrl on the Command position.
- Test Windows history consumer keys and Dia's configured shortcut.
- Toggle back; ensure Mac gestures and dictionary return.
