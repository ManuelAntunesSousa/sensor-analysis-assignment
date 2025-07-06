![Nexar Logo](https://mma.prnewswire.com/media/2509048/Nexar_Logo.jpg?p=facebook)

> **Confidential – For Internal Use Only**  
> This document contains proprietary information prepared for the Nexar Home Assignment and is intended solely for evaluation purposes.

# 🚦 Stop Sign Behavior Annotation Protocol

## 📌 Objective
To reliably label driver behavior near stop signs using dashcam footage. This protocol supports machine learning models that detect and assess stop sign compliance, enabling robust and fair traffic behavior analysis.

---

## 🧭 Labeling Overview

Annotators will review short video clips (8–12 seconds) recorded from a front-facing dashcam. Each video may or may not contain stop signs or stopping behavior. Your task is to assign **a set of labels per clip** based on:

1. **Presence of a stop sign**  
2. **Driver’s behavior**  
3. **Visibility and obstructions**  
4. **Stop location and brake lights (if visible)**  
5. **Edge cases or unclear situations**

Only use information visible in the video. No speed, map, or sensor data is available.

---

## 🎯 Labeling Schema

### 1. `stop_sign_visible`
Is a stop sign clearly visible in the frame?

- `yes` — Clearly visible (even partially)  
- `no` — No stop sign present  
- `uncertain` — Possibly present but unclear or blocked  

---

### 2. `driver_behavior`
Label only if `stop_sign_visible == yes`

- `full_stop` — Vehicle comes to a full stop (~1s pause)  
- `rolling_stop` — Slows but does not fully stop  
- `no_stop` — No visible deceleration  
- `uncertain_behavior` — Obstructed or unclear motion  

> 💡 *Tip*: Look for a brief full stop in vehicle motion, not just brake lights.

---

### 3. `visibility_conditions` *(Optional — multi-label)*

- `clear` — Good lighting and view  
- `low_light` — Nighttime or poor lighting  
- `occluded_sign` — Stop sign partially blocked  
- `obstructed_view` — Camera view blocked by vehicle/object  

---

### 4. `stop_location` *(Only if `driver_behavior == full_stop`)*

- `at_line` — Stops at clear stop line or intersection  
- `before_sign` — Stops early, before reaching the sign  
- `past_sign` — Passes sign before stopping  
- `not_clear` — Cannot determine  

---

### 5. `brake_lights` *(Only if rear of vehicle is visible)*

- `on` — Brake lights turn on  
- `off` — Brake lights not active  
- `not_visible` — Cannot see lights clearly  

---

## 📦 Label Examples

| Situation                                      | Labels |
|-----------------------------------------------|--------|
| Sign visible, vehicle stops at line           | `stop_sign_visible: yes`, `driver_behavior: full_stop`, `stop_location: at_line` |
| Sign visible, car rolls through               | `stop_sign_visible: yes`, `driver_behavior: rolling_stop` |
| Sign blocked by bus                           | `stop_sign_visible: uncertain` |
| Nighttime, unclear motion                     | `stop_sign_visible: yes`, `driver_behavior: uncertain_behavior`, `low_light` |
| No sign, car slows                            | `stop_sign_visible: no` |

---

## 🧩 Edge Cases & Annotator Guidance

- **No stop line?** Use vehicle motion, not markings  
- **Very short clips?** Label `uncertain` if unsure  
- **Multiple stop signs?** Focus on the one affecting the ego vehicle  
- **No brake lights in frame?** Use `not_visible`  

---

## 🔔 Annotator Feedback

If you encounter an unclear, ambiguous, or unusual clip:

- Mark with: `unclear_case`  
- In the comments field, briefly describe the issue  

Examples:  
- “stop sign possibly hidden by tree”  
- “no full stop but slow motion hard to see”  
- “stop line not visible, unclear behavior”

---

## 📌 Assumptions & Tradeoffs

- **Only front-facing video** is available  
- Annotators are **remote, unfamiliar with the roads**  
- Labeling favors **high precision over recall** — avoid guessing  
- Clips are assumed to already be **trimmed to relevant context**  
- Labels should reflect **only what is clearly visible**  

---

## 🤖 Why This Helps Model Training

This structured protocol ensures:

- Clean labels for supervised learning  
- Clarity between clear and ambiguous cases  
- Improved generalization to real-world driving footage  
- Robustness to diverse lighting, motion, and occlusion  

Supports models for:  
- Visual stop sign detection  
- Driver compliance analysis  
- Behavior-based risk scoring  
- Sequence modeling using temporal patterns

---

## 🧠 Data Quality & Objectivity Reminders

- Use `uncertain` when unsure — this protects model integrity  
- Avoid guessing or assumptions — label **only what’s visible**  
- Do not assume there must be a stop sign — absence is valid  
- Maintain consistency — apply the same rules across all clips

---

> Prepared for Nexar , 07 July 2025  version 3
> **Confidential – Internal Use Only**
