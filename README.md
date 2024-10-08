# pi-pico-macropad-kmk-based

Credits to [jaysongiroux](https://github.com/jaysongiroux/pico-macro-pad) for making a 3d model for the macropad in question.

## Prerequisites

* Raspberry Pi Pico board or any board able to handle [kmk_firmware](https://github.com/KMKfw/kmk_firmware) (e.g. ESP32)
* Circuit Python enabled on said board [link here](https://circuitpython.org/)
* Text editor of your choice

## Instructions

1. Follow the guide over at [Citcuit Python](https://circuitpython.org/)
1. Follow the instructions to get kmk_firmware up and running [here](https://github.com/KMKfw/kmk_firmware/blob/main/docs/en/Getting_Started.md)
1. Copy all contents here and paste them in your board's directory.
1. All should work.

>**Note**: If you require any new layouts, simply open `code.py` and edit the following data:

```Python
# Matrix 3x3 keymap, 9 keys in total
# This used to map your keys properly, every new keymap is simply a new [] list
keyboard.keymap = [
    [
        KC.NUMPAD_7, KC.NUMPAD_8, KC.NUMPAD_9,
        KC.NUMPAD_4, KC.NUMPAD_5, KC.NUMPAD_6,
        KC.NUMPAD_1, KC.NUMPAD_2, KC.NUMPAD_3,
    ],
    [
        KC.F22, KC.F23, KC.F24,
        KC.F19, KC.F20, KC.F21,
        KC.F16, KC.F17, KC.F18,
    ],
    [
        KC.F19, KC.NO, KC.NO,
        KC.F16, KC.F17, KC.F18,
        KC.F13, KC.F14, KC.F15,
    ],
    [
        back_nav, forward_nav, KC.LCTRL(KC.LGUI(KC.Q)),
        KC.LGUI(KC.L), KC.LGUI(KC.F7), find_all_intellij,
        KC.LGUI(KC.O), KC.LALT(KC.F7), KC.LGUI(KC.F12),
    ]
]

# This is the data being used to show your keys on the small display 
keymap_legend = [
    [
        [
            "7, 8, 9"
        ],
        [
            "4, 5, 6"
        ],
        [
            "1, 2, 3"
        ]
    ],
    [
        [
            "F22, F23, F24"
        ],
        [
            "F19, F20, F21"
        ],
        [
            "F16, F17, F18"
        ]
    ],
    [
        [
            "F19, N/A, N/A"
        ],
        [
            "F16, F17, F18"
        ],
        [
            "F13, F14, F15"
        ]
    ],
    [
        [
            "BCK, FWD, LCK"
        ],
        [
            "LIN, FUS, FND"
        ],
        [
            "CLS, USG, MTD"
        ]
    ]
]
```

> For new layouts, please also update here:
```Python
# This here is for displaying data on the small monitor. Notice `layer=n`
# This notifies on which layer to display your text
display.entries = [
    TextEntry(text="Layer: ", x=0, y=0),
    TextEntry(text="NUMPAD_MODE", x=64, y=12, layer=0, x_anchor="M"),
    TextEntry(text="0", x=38, y=0, layer=0),
    TextEntry(text=str(keymap_legend[0][0]), x=64, y=24, layer=0, x_anchor="M"),
    TextEntry(text=str(keymap_legend[0][1]), x=64, y=36, layer=0, x_anchor="M"),
    TextEntry(text=str(keymap_legend[0][2]), x=64, y=48, layer=0, x_anchor="M"),
    TextEntry(text="F_MODE_WIN", x=64, y=12, layer=1, x_anchor="M"),
    TextEntry(text="1", x=38, y=0, layer=1),
    TextEntry(text=str(keymap_legend[1][0]), x=64, y=24, layer=1, x_anchor="M"),
    TextEntry(text=str(keymap_legend[1][1]), x=64, y=36, layer=1, x_anchor="M"),
    TextEntry(text=str(keymap_legend[1][2]), x=64, y=48, layer=1, x_anchor="M"),
    TextEntry(text="F_MODE_MAC", x=64, y=12, layer=2, x_anchor="M"),
    TextEntry(text="2", x=38, y=0, layer=2),
    TextEntry(text=str(keymap_legend[2][0]), x=64, y=24, layer=2, x_anchor="M"),
    TextEntry(text=str(keymap_legend[2][1]), x=64, y=36, layer=2, x_anchor="M"),
    TextEntry(text=str(keymap_legend[2][2]), x=64, y=48, layer=2, x_anchor="M"),
    TextEntry(text="INTELLI_MAC", x=64, y=12, layer=3, x_anchor="M"),
    TextEntry(text="3", x=38, y=0, layer=3),
    TextEntry(text=str(keymap_legend[3][0]), x=64, y=24, layer=3, x_anchor="M"),
    TextEntry(text=str(keymap_legend[3][1]), x=64, y=36, layer=3, x_anchor="M"),
    TextEntry(text=str(keymap_legend[3][2]), x=64, y=48, layer=3, x_anchor="M"),
]

# This is used to change between modes with the rotary encoder
# Do not edit this unless you know what you're trying to accomplish
def on_move_do(state):
    if state is not None and state['direction'] == -1:
       display.reset_wake_timer()
       if keyboard.active_layers[0] > 0:
           keyboard.active_layers[0] -= 1
       else:
           keyboard.active_layers[0] = len(keyboard.keymap) - 1
    elif state is not None and state['direction'] != -1:
        display.reset_wake_timer()
        if keyboard.active_layers[0] < len(keyboard.keymap) - 1:
            keyboard.active_layers[0] += 1
        else:
            keyboard.active_layers[0] = 0
```
