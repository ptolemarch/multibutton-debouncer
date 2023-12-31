# Introduction

Responds to complicated button presses across multiple buttons and repeated
button presses. Given a set of buttons, presses can be recognized as belonging
to any of those buttons, or to any subset of those buttons. Distinguishes
between short presses and long presses, which may be combined in any order.
Also recognizes an extra-long press, which may not be followed by additional
presses.

Builds upon Adafruit's excellent `adafruit_debouncer` library to provide an
alternative to that library's `Button` class.

# Dependencies

This package depends on:

- Adafruit-CircuitPython-Debouncer
- Adafruit-Blinka

# Usage Example

```python
import time
import board  # provided by Adafruit-Blinka
from multibutton_debouncer import MultiButton

keep_on_ticking = True
def stop_ticking():
    nonlocal keep_on_ticking
    keep_on_ticking = False

buttons = MultiButton(board.D18, board.D5)

# a short press
buttons.set_callback([board.D18], ".", lambda: print("boop"))
buttons.set_callback([board.D5], ".", lambda: print("beep"))

# a long press
buttons.set_callback([board.D18], "_", lambda: print("boooop"))
buttons.set_callback([board.D5], "_", lambda: print("beeeep"))

# repeated presses
buttons.set_callback([board.D18], "...___...", lambda: print("SOS"))  repea

# repeated presses across multiple buttons
buttons.set_callback([board.D18, board.D5], "_..__", lambda: print("Two Bits!"))

# an extra-long press across multiple buttons
buttons.set_callback([board.D18, board.D5], "~", stop_ticking)

while keep_on_ticking:
    time.sleep(1/60)

    # `poll()` MUST be called frequently to update state
    # (this will be familiar to users of Adafruit-Debouncer)
    buttons.poll()
```

# Updates and Feedback

The documentation is pretty limited. That'll be fixed presently, but until
then, you may wish to look at the public methods of `MultiButton` as well as the
`Press` enumeration.

This is my first contribution to the Python ecosystem. I will be grateful for
any feedback you may have.
