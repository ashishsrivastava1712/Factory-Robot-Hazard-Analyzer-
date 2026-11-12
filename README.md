# Factory Robot Hazard Analyzer - Smart Workplace Safety System 

A reliable Java application designed to evaluate and predict workplace safety risks when industrial robots work alongside human operators on a factory floor.

---

## What Does This Project Do? (In Simple Terms)

In a modern smart factory, automated robotic arms assemble, weld, and move parts in the exact same spaces where human technicians walk and work. If a robotic arm loses precision or if machinery starts wearing out, the risk of an accident increases significantly.

This tool acts like a **digital safety auditor**:
1. **Reads Robot Arm Precision:** Checks how accurately the robotic arm is moving (from `0.0` to `1.0`).
2. **Monitors Worker Density:** Counts how many technicians are standing within the robot's immediate work zone (from `1` to `20` people).
3. **Checks Machine Health:** Records whether neighboring machinery is in a `Worn`, `Faulty`, or `Critical` mechanical condition.
4. **Calculates a Single Safety Score:** Combines all three measurements into an easy-to-understand **Hazard Risk Score** so supervisors can take immediate action if danger levels rise.

Even without a technical background, anyone can run the tool, enter current floor measurements, and immediately see whether the area is safe.

---

## Key Concepts Covered

Here are the essential computer science and software design principles demonstrated in this project, explained simply:

- **Classes & Domain Modeling (`RobotHazardInput`):**
  Creating dedicated digital templates to package and transport related safety data together safely.
- **Fail-Fast Input Validation (`HazardInputValidator`):**
  Checking inputs immediately upon entry (verifying arm precision is within range and numbers aren't invalid or missing) before doing any math, preventing crashes down the line.
- **Custom Domain Exceptions (`RobotSafetyException`):**
  Creating tailored error alerts rather than generic system errors so users get clear, helpful messages like `"Error: Arm precision must be 0.0-1.0"`.
- **Type-Safe Enums (`MachineryState`):**
  Using fixed sets of valid options (`Worn`, `Faulty`, `Critical`) rather than free-form text to prevent typos and mistakes.
- **The Strategy Design Pattern (`MachineryRiskStrategy`):**
  Isolating risk evaluation rules for each machine condition into its own dedicated class (`WornRiskStrategy`, `FaultyRiskStrategy`, `CriticalRiskStrategy`). This avoids giant, messy `if-else` blocks and makes adding new machine states simple.
- **The Factory Design Pattern (`MachineryRiskFactory`):**
  A centralized "dispatch office" that automatically creates and supplies the right evaluation strategy based on the current machine condition.
- **Defensive Programming & Immutability:**
  Keeping fields `final` so once safety telemetry is captured, it cannot be accidentally changed during calculation.
- **Modular Step-by-Step Evolution (GitFlow):**
  Incrementally developing the system across 8 progressive use-case branches (`UC1` to `UC8`) with automated JUnit tests at every milestone.

---

## How the Risk Score is Calculated

The safety index combines two risks: the danger from arm inaccuracy plus the danger of having people near degraded machinery.

$$\text{Hazard Risk Score} = \left((1.0 - \text{Arm Precision}) \times 15.0\right) + \left(\text{Worker Density} \times \text{Machine Risk Factor}\right)$$

### Quick Example:
If arm precision is `0.5`, there are `10` workers nearby, and the machine state is `Critical` (multiplier `3.0`):
- Arm risk: $(1.0 - 0.5) \times 15.0 = 7.5$
- Machine & worker risk: $10 \times 3.0 = 30.0$
- **Total Hazard Score:** $7.5 + 30.0 = 37.5$

---

## Machinery Status Levels

| Status Level | Risk Multiplier | Meaning |
|:---|:---:|:---|
| **Worn** | `1.3` | Equipment has standard operational wear; minor vibration present |
| **Faulty** | `2.0` | Warning detected; equipment is malfunctioning |
| **Critical** | `3.0` | Severe mechanical defect; highest danger level |

---

## System Workflow

```
[ 1. User Input ]
Enter Arm Precision (e.g. 0.5)
Enter Worker Density (e.g. 10)
Enter Machinery State (e.g. Critical)
       ↓
[ 2. Safety Validation ]
Checks numbers: Is precision 0.0-1.0? Are workers 1-20? Is state valid?
       ↓ (If invalid: displays friendly RobotSafetyException message)
[ 3. Strategy Selection ]
Factory picks the right calculation rule based on machine state.
       ↓
[ 4. Score Output ]
Displays: "Robot Hazard Risk Score: 37.5"
```

---

## Progressive Development Milestones

The project was constructed across 8 structured iterations:

1. **UC1:** Prints the initial startup message and system banner.
2. **UC2:** Collects telemetry input from the user via the console.
3. **UC3:** Implements the basic mathematical hazard calculation formula.
4. **UC4:** Adds input boundary checks (e.g., precision must be 0.0 to 1.0).
5. **UC5:** Refactors validation logic into dedicated helper methods.
6. **UC6:** Introduces `RobotSafetyException` for domain-specific error reporting.
7. **UC7:** Replaces plain string checks with a structured `MachineryState` enum.
8. **UC8:** Refactors the entire engine into a clean OOP architecture using Strategy & Factory patterns.

---

## Technologies Used

- **Language:** Java 17
- **Build System:** Apache Maven
- **Testing:** JUnit 5 (Jupiter)
- **Version Control:** Git & GitHub (GitFlow Branching)
