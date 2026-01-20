**OBJECTIVE:**
Building a physics-informed digital twin of a lithium-ion cell to study fast-charging trade-offs between charge time, temperature rise, and degradation risk, and design a constraint-aware charging strategy.

**KEY QUESTION WE ARE ANSWERING:**
How can we reduce charge time while keeping temperature and lithium plating risk within acceptable limits?

**SYSTEM BOUNDARIES:**
1) Modeling one lithium-ion cell (not a pack)
2) Inputs: charge current, ambient temperature

Now, why are we choosing these inputs and why only these two inputs? 
We choose :

Charge Current I(t) : It is responsible for setting the reaction rate, overpotential, heat generation and lithium plating risk.

Ambient temperature Tₐ : It is responsible for setting the heat rejection ability and baseline kinetics.

Everything else that might matter in our simulations such as SOC, internal temperature, overpotentials, degradation state is **internal state**, not an **external input**.

Other common candidates that we are intentionally excluding are :

a) Electrolyte Concentration : This is because electrolyte concentration is not directly measurable in a real BMS. Moreover, it is already **implicitly** captured in the voltage + resistance dynamics in an equivalent circuit model (ECM).

b) Anode Potential : This is also not measurable in real cells, therefore we are approximating its effect via voltage + SOC + temperature (plating proxy).

c) Cooling airflow / heat transfer coefficient : We parametrize this as one constant (hA). Treating it as an input would add unnecesarry complexity without insight at cell level.

3) Outputs: terminal voltage, SOC, temperature, SOH proxy
4) Time scale: one or a few charging events (not lifetime simulation yet)
We are choosing only one or a few charging events because the real question we are attempting to answer is how we can reduce charge time without violating constraints. Lifetime degradation is a separate problem.
We still track State of Health Estimation (SOH) though, but we use it as consequence of charging behaviour. To check if faster charge is causing more degradation than baseline.

**WHAT THE MODEL MUST DO (SUCCESS CRITERIA):**
1) Simulate realistic voltage response during charging
2) Predict temperature rise under fast charging
3) Estimate SOC in real time (not just integrate perfectly)
4) Enforce safety constraints during charging
5) Show a clear improvement over baseline CC–CV charging

**WHAT WE EXPLICITLY WILL NOT MODEL:**
1) Electrolyte diffusion PDEs
2) Detailed SEI chemistry
3) Cell-to-pack effects
4) Mechanical stress


**MODELLING & SIMULATION**

In our data :

“Negative current corresponds to discharge”

We will be using NASA AMES Battery Dataset

<img width="1006" height="620" alt="image" src="https://github.com/user-attachments/assets/96185c22-cee8-4240-8c09-7e1554419950" />







SOC Analysis of Data (Using Current Load) :



<img width="1072" height="625" alt="image" src="https://github.com/user-attachments/assets/17000391-1296-4444-8b46-2051b36624cc" />


Identifying OCV data points in our dataset:

<img width="1074" height="645" alt="image" src="https://github.com/user-attachments/assets/30a31782-271e-493b-92c9-823da1fc5ea8" />

<img width="353" height="34" alt="image" src="https://github.com/user-attachments/assets/d6e44657-30c4-4cef-89b5-5fffb8fdbfcf" />


Estimated internal resistance R0 = 0.1855 Ohm

