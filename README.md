# soc-design-and-planning-new
From RTL to Reality — physical design with OpenLANE &amp; Sky130 PDK | VSD SoC Design and Planning Workshop
# Digital VLSI SoC Design and Planning — RTL to GDSII

> Organised by VSD (VLSI System Design) in partnership with NASSCOM, this two-week workshop covered the full RTL-to-GDSII flow for digital VLSI SoC design from the ground up. What you'll find here is my personal documentation — lab runs, results, and the key ideas I took away from each day.

---

 ## Day 1 — Getting Started with OpenLANE, Sky130 PDK & Open-Source EDA

#### Understanding the Chip Package
- The component we casually point to as "the chip" on any circuit board is really just its outer packaging — a protective enclosure housing the actual silicon die within. The die itself communicates with the outside world through wire bonding, where hair-thin metallic wires bridge the die's bonding pads to the exposed pins of the package.

#### Inside the Chip: Core, Pads, and Die
Peel back the package and the silicon die reveals itself: all electrical signals travelling in or out of the chip must pass through pads lined along its periphery. Everything enclosed within those pads — the logic gates, flip-flops, and interconnects doing the real work — belongs to the core. The die, which is the unit that gets cut from a silicon wafer, is simply the core and pads taken together.

- Foundry — the manufacturing plant where silicon wafers are processed and chips are physically built.
- Foundry IPs — specialized blocks like PLLs and SRAMs that require intimate knowledge of the fabrication process to implement correctly.
- Macros — pre-designed, reusable blocks of purely digital logic that plug into a design without any process-level complexity.

#### From Software to Silicon — The ISA Bridge

Layers of Translation Between Software and Hardware

- A compiler takes the original C source and converts it into architecture-specific assembly, targeting an instruction set like RISC-V that the target processor understands.
- An assembler then reduces that human-readable assembly down to binary machine code — the raw sequence of 1s and 0s that the processor actually executes.
- For those binary instructions to have any meaning in hardware, the processor must be built around an RTL description that faithfully implements the same instruction set the compiler targeted.
- That RTL description is then pushed through synthesis and a full place-and-route flow, eventually becoming the physical transistor-level layout that gets fabricated onto silicon.

#### Why Open-Source EDA Matters

- A fully open-source ASIC design flow needs three pieces in place: openly available RTL source (drawn from places like opencores.org), a complete EDA toolchain covering synthesis, place-and-route, and verification, and PDK data — the process-specific design rules and standard cell libraries tied to a given fabrication node.

- For most of the industry's history, PDKs sat behind NDAs and proprietary licensing, putting real chip design out of reach for students and hobbyists. That changed in June 2020, when Google teamed up with SkyWater Technology to release Sky130 as the first fully open-source PDK — a genuine turning point for accessible VLSI design.

Historically, PDKs were proprietary and distributed only under NDAs, making chip design inaccessible to most people. This changed in **June 2020**, when Google collaborated with SkyWater Technology to release the **Sky130 PDK** as the world's first open-source process design kit — a massive milestone for the VLSI community.

#### Setting Up and Invoking OpenLANE

The very first step is to navigate to the OpenLANE working directory and launch the tool in **interactive mode**, which lets us run each stage step-by-step.

- **Step 1** :
  
```bash
cd /home/vscode/Desktop/OpenLane
make mount
./flow.tcl -interactive
package require openlane 1.0.2
```

- **Step 2** :
  
#### Preparing the Design

Before running synthesis, we prepare the design to merge the cell LEF and technology LEF files, and set up the run directory.

```tcl
prep -design picorv32a
```

- **Step 3** :
  
#### Running Synthesis

```tcl
run_synthesis
```

- **Step 4** :
  
After synthesis completes, we can calculate the **flop ratio** — a useful sanity check:

```
Flop Ratio = (No. of D Flip-Flops) / (Total No. of Cells)
           = 1613 / 15762
           ≈ 0.1023  →  ~10.23%
```
---

