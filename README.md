# Compass Maze – Raspberry Pi Sense HAT IoT Game

A physical maze game played by **turning a Raspberry Pi in your hands**. The Pi's Sense HAT reads its compass heading and shows which way you're facing (N, S, E or W) on its 8 × 8 LED matrix. You choose a direction by pointing the Pi and press the Sense HAT button to walk through that door into the next room. Each room is a different colour, and you start in the **Purple** room and have to find your way to the **Orange** room to escape.

Built as an IoT project (class L3) at Nanyang Polytechnic.

## Demo

https://github.com/user-attachments/assets/a92ce848-e533-4d80-95d8-52552b0fa4e2

*Explanation of the rules, then a full playthrough from the Start room to the escape message. (1:54)*

## How to play

1. Hold the Raspberry Pi flat. The **USB ports are the pointer**: whichever way they face is the direction you're choosing.
2. Turn around. The LED matrix shows **N**, **S**, **E** or **W** based on your compass heading in degrees.
3. The LED colour tells you **which room you're in**.
4. When you're facing a door, **press the Sense HAT button** to move into that room.
5. Reach the Orange room and the Sense HAT scrolls **"CONGRATULATIONS! You have escaped! WOOOOOOO!"**

## The maze

The maze is a 3 × 3 grid of coloured rooms:

```
 ┌──────────┬──────────┬──────────┐
 │  Yellow  ┃   Blue   ┊  Purple  │
 │          ┃          ┊ (START)  │
 ├━━━━━━━━━━┼━━━━━━━━━━┼━━━━━━━━━━┤
 │  Green   │   Red    ┃   Pink   │
 │          │          ┃          │
 ├━━━━━━━━━━┼──────────┼━━━━━━━━━━┤
 │  Orange  │ (no room)│  Brown   │
 │  (END)   │          │          │
 └──────────┴──────────┴──────────┘
   ━ ┃  door you can walk through (both ways)
   ┊    one-way door: Blue → Start only
```

| Rule from the design | Meaning |
|---|---|
| Black outline | A door you can go through |
| White outline (Blue ↔ Purple) | Lets you return to the Start room, but you can't leave Start that way |
| Brown room | Dead end, reached only from Pink |

**Route to escape:** Purple (Start) → **S** Pink → **W** Red → **N** Blue → **W** Yellow → **S** Green → **S** Orange (End).

## Hardware

- Raspberry Pi with a **Sense HAT**, using:
  - the **magnetometer** for the compass heading,
  - the **8 × 8 RGB LED matrix** for directions, room colours and the escape message,
  - the **joystick button** to confirm a move.
- The kit was mounted in a clear storage case alongside other components from the module (Arduino boards, breadboard, sensors and displays), but the game itself runs on the Raspberry Pi and Sense HAT.

## How it works

The original source code wasn't kept, so this is a description of the logic as shown in the demo:

1. Read the compass heading in degrees from the Sense HAT and convert it to the nearest direction: N (315°–45°), E (45°–135°), S (135°–225°) or W (225°–315°).
2. Show that letter on the LED matrix in the current room's colour.
3. Store the maze as a map of rooms to their open doors, for example `Purple: {S: Pink}`, `Pink: {N: Purple, W: Red, S: Brown}`, and so on.
4. When the joystick is pressed, check whether the current room has a door in the facing direction. If it does, move into that room; if not, stay put.
5. When the current room becomes Orange, scroll the congratulations message.

The Sense HAT is normally programmed in Python with the [`sense-hat`](https://pythonhosted.org/sense-hat/) library (`sense.get_compass()`, `sense.show_letter()`, `sense.stick`, `sense.show_message()`).

## Rebuilding it

To recreate the project you need a Raspberry Pi with a Sense HAT and Raspberry Pi OS:

```bash
sudo apt install sense-hat
```

Calibrate the magnetometer first (`RTIMULibCal`), because uncalibrated compass readings drift a lot indoors.

## Author

Valerie Tan Wan Xin – Nanyang Polytechnic, class L3
