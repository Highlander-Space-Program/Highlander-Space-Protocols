# Control Box Communication Protocol

## Purpose

Communication protocol between the static fire pad controller and the control box.

## Hardware Layer

Two XBee radios networked together in transparent mode, one in the control box and the other in the pad controller. The pad controller controls the servos and ignitor on the static fire test stand, and the control box allows us to command the pad controller from a safe distance.

## Application Layer

The control box sends commands to the pad controller when a button is pressed. Each command is a single byte sent over the radio. The commands are enumerated as follows:

```C
enum COMMANDS {
  OPEN_NOS2 = 0,
  CLOSE_NOS2 = 1,
  OPEN_NOS1 = 2,
  CLOSE_NOS1 = 3,
  OPEN_N2 = 4,
  CLOSE_N2 = 5,
  OPEN_ETOH = 6,
  CLOSE_ETOH = 7,
  START_1 = 8,
  FILL_1 = 9,
  FILL_2 = 10,
  FILL_3 = 11,
  CLOSE_ALL = 12,
  DECLOSE_ALL = 13,
  ACTIVATE_IGNITER = 14,
  DEACTIVATE_IGNITER = 15,
  ABORT = 16,
  ACTIVATE_SERVOS = 17,
  DEACTIVATE_SERVOS = 18,
  DEABORT = 19,
  CHECK_STATE = 20
};
```

When the pad controller receives a command, it acknowleges with a single byte that contains the states of each servo and the ignitor. A 1 indicates that the servo is open or that an ignitor is triggered.
```
| Bit 7  | Bit 6  | Bit 5  | Bit 4  | Bit 3   | Bit 2  | Bit 1  | Bit 0 |
| NOS2   | NOS1   | N2     | ETOH   | IGNITER | Unspecified             |
```

For example, recieving `10110111` from the pad controller would indicate that the `NOS2`, `N2`, and `ETOH` servos are open.

If the pad controller recieves an invalid command, it ignores it.
