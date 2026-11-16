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

## macOS-first keyboard layout

The default modifier order (left to right) is **Ctrl, Option, Command**,
matching a MacBook. The rightmost home-row key formerly labeled `-`
now taps `:`; the ordinary minus remains on **hold Space + G**.
The adjacent `;` key still taps semicolon. Hold Space + the `;` or
`"` position to type `*` or `/` without Alt-code macros.

| Physical modifier position | macOS (default) | Windows mode |
| --- | --- | --- |
| Left | Control | Windows |
| Middle | Option | Alt |
| Right | Command | Ctrl |
| Shift | Shift | Shift |

Hold `LANG2` and tap `A` to toggle Windows mode; repeat to return to Mac.
This is independent of Bluetooth profiles and resets to Mac on firmware reboot.

## Trackball gestures

**[Full trackball guide, exact shortcuts and Windows equivalents](docs/TRACKBALL_GESTURES.md)**

Hold one trigger for about 200 ms, roll the trackball, then release.

| Hold | Left | Right | Up | Down |
| --- | --- | --- | --- | --- |
| `;` | Back page | Forward page | — | — |
| `:` (former `-`) | Previous tab | Next tab | New tab | Close tab |
| `"` | Previous Dia Space | Next Dia Space | — | — |
| `O` + `P` | Previous desktop | Next desktop | — | — |
| `L` + `:` | Left half | Right half | Next display | Fill display |

When mouse layer 5 is active (hold Tab/Esc or move the ball normally),
press **D** for macOS Look Up (Cmd+Ctrl+D). On Windows it remains D.

Normal mouse clicks (J/K/L), mouse buttons 4/5 (M/period), P scrolling,
LANG1 alternate scrolling and Space precision mode are retained.

## Navigation (hold LANG1)

| Navigation-layer action | macOS/default | Windows mode |
| --- | --- | --- |
| Start of line | Cmd+Left | Home |
| End of line | Cmd+Right | End |
| Start of document | Cmd+Up | Ctrl+Home |
| End of document | Cmd+Down | Ctrl+End |
| Selection screenshot | Cmd+Shift+4 | Win+Shift+S |
| Full screenshot | Cmd+Shift+3 | Win+PrintScreen |

Windows mode uses conditional layers to select OS-specific gesture
processors; the physical gesture triggers stay the same. The macOS
window movement shortcuts assume your existing window manager settings.
On Windows, native Snap and monitor shortcuts are used; Dia Spaces
may require assigning Ctrl+Alt+Left/Right in Dia settings.

The existing layer IDs 0–11 are retained. Layers 12–13 add history and
Dia gestures, 14–18 select Windows gesture processors, and 19 suppresses
the Mac-only dictionary shortcut in Windows mouse mode.
