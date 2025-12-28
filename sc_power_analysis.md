# CMOS Inverter Short-Circuit Power Analysis

## Derivation of Short-Circuit Power Formula

During the switching transition of a CMOS inverter, there exists a short period when both the PMOS and NMOS transistors are partially ON, creating a direct current path from Vdd to ground. This results in short‑circuit (or crossover) current \(I_{sc}(t)\). The instantaneous short‑circuit power is \(V_{dd} \cdot I_{sc}(t)\). Averaged over one switching period \(T = 1/f\), the short‑circuit power is:

\[
P_{sc} = \frac{1}{T} \int_{0}^{T} V_{dd} \, I_{sc}(t) \, dt
\]

If we approximate the current pulse as a triangular shape with peak current \(I_{sc\_peak}\) and duration \(t_{sc}\) (the time during which both transistors are simultaneously conducting), the integral simplifies to:

\[
P_{sc} = V_{dd} \cdot \left( \frac{1}{2} I_{sc\_peak} \cdot t_{sc} \right) \cdot 2f
\]

The factor 2 arises because a short‑circuit pulse occurs on both rising and falling edges. Therefore the final expression becomes:

\[
\boxed{P_{sc} = I_{sc\_peak} \cdot V_{dd} \cdot t_{sc} \cdot f}
\]

where:
- \(I_{sc\_peak}\) is the peak short‑circuit current (A),
- \(V_{dd}\) is the supply voltage (V),
- \(t_{sc}\) is the duration of the short‑circuit pulse (s),
- \(f\) is the switching frequency (Hz).

## Relationship of \(I_{sc\_peak}\) and \(t_{sc}\) to Input and Output Slew Rates

The short‑circuit parameters are strongly influenced by the **input slew rate** (\(SR_{in}\)) and the **output slew rate** (\(SR_{out}\)).

* **Peak current** \(I_{sc\_peak}\): Occurs when the input voltage passes through the region \(V_{th,n} < V_{in} < V_{dd} - |V_{th,p}|\). For a given technology, the peak current scales roughly linearly with the supply voltage and is also affected by the relative slew rates. If the input transition is fast (large \(SR_{in}\)) while the output transition is slow (small \(SR_{out}\)), the output voltage lags far behind the input, keeping both transistors in saturation longer and producing a larger \(I_{sc\_peak}\).

* **Short‑circuit duration** \(t_{sc}\): The time window during which both transistors conduct. It is approximately the overlap of the input transition time (\(t_{in}\)) and the output transition time (\(t_{out}\)). When the input slew is fast and the output slew is slow, the overlap time increases, leading to a larger \(t_{sc}\).

In summary:
- Faster input slew rates **increase** both \(I_{sc\_peak}\) and \(t_{sc}\).
- Slower output slew rates (due to larger capacitive load) **increase** \(t_{sc}\) and can also increase \(I_{sc\_peak}\) because the output voltage stays in the mid‑rail region longer.

Therefore, to minimize short‑circuit power, designers should aim for **matched input and output slew rates** (i.e., similar rise/fall times) and avoid extremely fast input transitions driving heavily loaded nodes.

## Effect of Supply Voltage \(V_{dd}\)

Short‑circuit power exhibits a **quadratic dependence** on the supply voltage. This arises because:

1. The peak short‑circuit current \(I_{sc\_peak}\) is approximately proportional to \(V_{dd}\) (for long‑channel devices) or even super‑linear in deep‑submicron technologies.
2. The duration \(t_{sc}\) is often relatively independent of \(V_{dd}\) for a given slew condition.
3. The explicit \(V_{dd}\) factor in the formula multiplies the current.

Thus:

\[
P_{sc} \propto V_{dd}^{2}
\]

Reducing the supply voltage is one of the most effective ways to lower short‑circuit power, as well as dynamic switching power.

## Example Calculation for a 45 nm CMOS Inverter

Assume the following parameters:
- Supply voltage \(V_{dd} = 1.0\ \text{V}\)
- Switching frequency \(f = 100\ \text{MHz} = 10^{8}\ \text{Hz}\)
- Input slew (rise/fall time) \(t_{in} = 50\ \text{ps} = 5.0 \times 10^{-11}\ \text{s}\)
- Output load capacitance \(C_{L} = 10\ \text{fF} = 1.0 \times 10^{-14}\ \text{F}\)
- Output slew (approximated from \(C_{L}\) and transistor drive) \(t_{out} \approx 80\ \text{ps} = 8.0 \times 10^{-11}\ \text{s}\)

**Step 1: Estimate \(t_{sc}\)**  
The short‑circuit duration is roughly the overlap of the input and output transitions. A simple approximation is the absolute difference of the slew times:

\[
t_{sc} \approx |t_{out} - t_{in}| = |80\ \text{ps} - 50\ \text{ps}| = 30\ \text{ps} = 3.0 \times 10^{-11}\ \text{s}
\]

(More precise models use the time during which both transistors are in saturation, but this difference gives a reasonable order of magnitude.)

**Step 2: Estimate \(I_{sc\_peak}\)**  
For a minimum‑size 45 nm inverter, typical saturation currents are in the range of \(100\)–\(200\ \mu\text{A}\) per micrometer of gate width. Assume a total effective width of \(1\ \mu\text{m}\) and a current factor of \(150\ \mu\text{A}/\mu\text{m}\) at \(V_{dd}=1.0\ \text{V}\). Then:

\[
I_{sc\_peak} \approx 150\ \mu\text{A} = 1.5 \times 10^{-4}\ \text{A}
\]

**Step 3: Compute \(P_{sc}\)**  

\[
\begin{aligned}
P_{sc} &= I_{sc\_peak} \cdot V_{dd} \cdot t_{sc} \cdot f \\
       &= (1.5 \times 10^{-4}\ \text{A}) \times (1.0\ \text{V}) \times (3.0 \times 10^{-11}\ \text{s}) \times (10^{8}\ \text{Hz}) \\
       &= (1.5 \times 10^{-4}) \times (3.0 \times 10^{-11}) \times 10^{8} \\
       &= 4.5 \times 10^{-7}\ \text{W}
\end{aligned}
\]

Thus, the short‑circuit power for this example is:

\[
\boxed{P_{sc} = 0.45\ \mu\text{W}}
\]

**Step 4: Compare with dynamic (switching) power**  
The dynamic power for the same inverter is:

\[
P_{dyn} = C_{L} V_{dd}^{2} f = (1.0 \times 10^{-14}\ \text{F}) \times (1.0\ \text{V})^{2} \times (10^{8}\ \text{Hz}) = 1.0\ \mu\text{W}
\]

The short‑circuit component is about 45 % of the dynamic power in this scenario, illustrating that it cannot be neglected in high‑speed, low‑load designs.

---

*This analysis provides a foundation for understanding and estimating short‑circuit power in CMOS digital circuits. For accurate simulations, transistor‑level SPICE simulations are recommended, especially in advanced technology nodes where short‑channel effects alter the simple quadratic relationships.*