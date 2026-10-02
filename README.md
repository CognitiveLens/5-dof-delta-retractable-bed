# 5-dof-delta-retractable-bed
A novel 5-DOF Delta 3D printer mechanism with a bi-axial vertical-linear cam sliding bed.


# 5-DOF Delta Printer: Kinematics via Bi-Axial Linear-Vertical Cam Mechanism

This repository presents a novel, non-standard mechanical concept for a **5-Degrees-of-Freedom (5-DOF) 3D Printer**, built on a **Delta geometry frame** (1200 mm height, Φ 400 mm circular bed). 

The core innovation is a custom **sub-bed tilting and rotation system** that achieves a full **90-degree inclination** without requiring a traditional, bulky Trunnion table, thereby preserving the entire **400x400x400 mm build volume** free of collisions.

---

## 1. Mechanical Architecture & Kinematics

Conventional 5-axis systems rely on a Trunnion design (U-shaped cradle). At a 90° tilt, a standard Trunnion forces half of the build plate to swing downwards below the chassis horizontal zero level, resulting in catastrophic frame collisions unless the usable envelope is drastically downscaled.

This design bypasses the limitation using a **Bi-Axial Linear-Vertical Cam Guide Mechanism (Dual-Plane Constrained Translation)**.

![Full Assembly Front View](full_delta.png)
*Figure 1: Front overview of the 1200mm Delta chassis with the Φ 400mm circular bed in home position.*

### Motion Execution:
* **The Carriage Assembly:** The bed support structure does not rotate on a fixed physical pin. Instead, it is constrained by two sets of rollers translating along horizontal and vertical constraints.
* **Actuation:** Driven by twin **T8 lead screws** coupled to dual **NEMA 17 stepper motors** (wired in parallel/synchronized natively via the Z-axis dual slot on a BTT Octopus control board).

![Base Rail and T8 Lead Screw Assembly](base_drive.png)
*Figure 2: Base layout showing the horizontal profile rails, T8 drive screws, and NEMA 17 stepper positioning.*

* **The Continuous Axis (Theta):** Rotation is handled independently via a low-profile **Lazy Susan bearing plate**, driven by a stepper motor and locked rigidly during indexing using a **1/6 arc segment shoe brake**.

---

## 2. Dynamic Mechanical Assembly (Back-Bed Layout)

The physical backplane configuration shifts the rotation and tilting mechanics fully underneath the planar bed vector to maximize travel workspace.

![Rear Mechanism Detail](rear_kinematics.png)
*Figure 3: Rear view of the tilting carriage. Note the central T8 driving column, custom 3D printed mechanical interfaces, and the coaxial Lazy Susan bearing integration.*

### Key Structural Attributes:
* **Kinetic Energy Recovery Springs:** Mechanical tension springs are deployed at the travel limits. When moving down towards 0° (horizontal), energy is stored in the springs. When tilting up to 90° against maximum gravity, the springs discharge, acting as a mechanical booster to prevent NEMA 17 step-loss.
* **Pre-Tensioned Physical Offsets:** To counteract structural flex under the weight of the massive Φ 400 mm bed assembly, the structural mounts are intentionally misaligned at a **-5 degree pre-bias**. When homing, the system drives into a physical endstop hard-limit, forcing the assembly into a **true geometric 0.00° planar state**, eliminating macroscopic backlash.

---

## 3. Hardware Control Interface (Hacker-Style Integration)

The processing system embraces a raw, high-density **"functional over aesthetic"** deployment inside a dedicated ventilated metal case, maintaining total galvanic isolation between logic controllers and inductive coil loops.

![Power Distribution and Logic Housing](electronics.png)
*Figure 4: Internal layout of the control cabinet featuring the 32-bit BTT mainboard, UART-configured TMC drivers, active Nidec cooling system, and secondary switching controllers.*

* **Mainboard:** BigTreeTech (BTT) Octopus V1.1 (32-bit MCU architecture).
* **Drivers:** TMC2209 configured via native UART for real-time current governance and stallguard safety diagnostics.
* **Bed Heating System:** Custom Φ 400 mm aluminum build plate powered via high-voltage 220V AC mains line, switched safely using an industrial Solid State Relay (SSR) to ensure rapid thermal saturation without overloading the low-voltage DC rails.
* **Host Server:** Dedicated x86 platform running **Debian Linux** hosting the Klipper runtime environment (ensuring immune processing headroom against complex multi-MCU delta math routines).

---
*Note: This architecture is a fully functional, field-tested empirical concept proving that ultra-high angular deployment (90°) is achievable in tight domestic workshop footprints without industrial-grade curved gantry hardware.*
