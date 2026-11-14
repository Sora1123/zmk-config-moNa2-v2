COROPITを使用する方は以下のようにコードを編集してください。

mona2_r.overlay

修正前
```
  trackball_central: trackball_central@0 {
        status = "okay";
        compatible = "pixart,pmw3610";  //トラボセンサ用のドライバとバインド
        reg = <0>;
        spi-max-frequency = <2000000>;
        irq-gpios = <&gpio0 2 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>; //P0.02を指定(MOTION)
        cpi = <600>;
        //swap-xy;
        //invert-x; //COROPIT版ではコメントアウトを外す
        //invert-y; //COROPIT版ではコメントアウトを外す
        evt-type = <INPUT_EV_REL>;
        x-input-code = <INPUT_REL_X>;
        y-input-code = <INPUT_REL_Y>;
    };
};

```
**修正後**
```
  trackball_central: trackball_central@0 {
        status = "okay";
        compatible = "pixart,pmw3610";  //トラボセンサ用のドライバとバインド
        reg = <0>;
        spi-max-frequency = <2000000>;
        irq-gpios = <&gpio0 2 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>; //P0.02を指定(MOTION)
        cpi = <600>;
        //swap-xy;
        invert-x; //COROPIT版ではコメントアウトを外す
        invert-y; //COROPIT版ではコメントアウトを外す
        evt-type = <INPUT_EV_REL>;
        x-input-code = <INPUT_REL_X>;
        y-input-code = <INPUT_REL_Y>;
    };
};

```

## macOS / Windows compatibility mode

This configuration defaults to its original macOS-oriented layout. It also includes a
toggleable Windows mode so the **same physical positions** remain comfortable on both systems.

### Switching modes

- **Hold `LANG2` (the left thumb key that accesses layer 4) and tap `A`** to toggle Windows mode.
- Toggle it again to return to macOS mode.
- The toggle does not switch Bluetooth connections; choose the desired computer using
  the existing Bluetooth profile keys on layer 4.
- Mode selection resets to macOS after a firmware restart; the mode is not stored per Bluetooth profile.

### Modifier positions

| Existing physical position | macOS/default | Windows mode |
| --- | --- | --- |
| Mac Control (`LCTRL`) | Control | Windows key (`LEFT_WIN`) |
| Mac Command (`LEFT_WIN`) | Command | Ctrl (`LCTRL`) |
| Mac Option (`LEFT_ALT`) | Option | Alt (`LEFT_ALT`) |
| Shift | Shift | Shift |

This keeps the Command-key muscle memory for common commands such as copy/paste,
undo, and select all. On Windows, app switching uses the **Option-position key +
Tab** (Alt+Tab), rather than Command+Tab. The Mac Control position is the Windows
key for system-level actions. Other keyboard layers, letter keys, trackball gestures,
and Bluetooth key positions stay unchanged.

### Navigation (hold `LANG1` for layer 3)

| Navigation-layer action | macOS/default | Windows mode |
| --- | --- | --- |
| Command + Left | Beginning of line | Home |
| Command + Right | End of line | End |
| Command + Up | Beginning of document | Ctrl+Home |
| Command + Down | End of document | Ctrl+End |
| Selection screenshot | Command+Shift+4 | Windows+Shift+S |
| Full screenshot | Command+Shift+3 | Windows+PrintScreen |

The existing tab/scroll and mouse gestures are preserved. Note that some gestures
and the Alt-numpad character macros were already Windows-specific and remain as-is.

**Implementation:** `windows_mode` is layer 10 (transparent except the two
modifier positions and the `LANG1` layer-tap); `windows_nav` is layer 11 and
activates automatically when Windows mode and navigation layer 3 are active.
The Windows `LANG1` key uses standard `&lt 3 LANG1` so that the original
`&to 0` tap macro cannot unexpectedly disable Windows mode. All other
existing layers keep their original numbers and bindings.
