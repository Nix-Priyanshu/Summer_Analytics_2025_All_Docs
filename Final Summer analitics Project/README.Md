# Dynamic Real-Time Parking Pricing – Capstone Project

This project implements a real-time dynamic pricing system for urban parking lots based on live data.  
It simulates streaming updates and adjusts prices dynamically using three progressively smarter models:

- **Model 1:** Linear pricing based on occupancy
- **Model 2:** Demand-based pricing (adds queue, traffic, day-type, vehicle weight)
- **Model 3:** Competitive pricing (adjusts against other lots)

Built in **Python** using **Bokeh** for live visualizations, this project reflects how real-world pricing can be optimized under demand and competition.

## Tech Stack Used

| Component           | Tool / Library             |
|--------------------|----------------------------|
| **Language**        | Python 3.11                |
| **Notebook Platform** | Google Colab             |
| **Data Handling**   | pandas, numpy              |
| **Streaming Logic** | Simulated row-by-row loop |
| **Visualization**   | Bokeh (for real-time plots)|
| **Export & Sharing**| HTML (`file_html` via Bokeh), Colab |


---

## Models Implemented

###  Model 1 – Baseline Linear

\[
P_{t+1} = P_t + \alpha \cdot \frac{\text{Occupancy}}{\text{Capacity}}
\]

- Simple model where price increases with how full a parking lot is
- `α` is a tunable constant (e.g., 2)
- Keeps prices between ₹5 and ₹20

---

### Model 2 – Demand-Based Dynamic Pricing

\[
\text{Demand} = a \cdot \frac{\text{Occupancy}}{\text{Capacity}} + b \cdot \text{QueueLength} - c \cdot \text{TrafficLevel} + d \cdot \text{SpecialDayFlag} + e \cdot \text{VehicleWeight}
\]

\[
P = \text{BasePrice} \cdot \left(1 + \lambda \cdot \sigma(\text{Demand}) \right)
\]

- Adds behavioral inputs: queue length, traffic, day type, vehicle
- Uses a sigmoid function to smooth wild swings in demand
- Clipped between ₹5 and ₹20

---

### Model 3 – Competitive Price Adjustment

- Tracks average price of all other lots at the same timestamp
- If **current lot price > others**, reduce it slightly (₹0.7)
- If **lower but not full**, increase it slightly
- Encourages better balance across lots and rerouting potential

---

## 🔁 Architecture Flow

### End-to-End System Overview

1. **Dataset Ingestion**
   - The input CSV file contains parking lot updates over time
   - Includes features like occupancy, capacity, queue, traffic, timestamps, etc.

2. **Streaming Simulation**
   - Instead of processing the full dataset at once, the system reads **row-by-row**
   - This mimics real-time streaming behavior

3. **Pricing Models**
   - Each row is passed through a selected pricing model (`model1`, `model2`, or `model3`)
   - Model computes dynamic price based on current features

4. **Live Visualization**
   - The updated price is streamed into a **Bokeh live plot**
   - Shows real-time pricing curve with time step on x-axis and price on y-axis

5. **HTML Export**
   - Each plot is saved as an interactive `.html` file
   - Viewable offline or shareable via GitHub/Drive


graph TD
    A[Dataset (.csv)] --> B[Row-by-Row Simulator]
    B --> C[Pricing Models<br>(Model 1 / Model 2 / Model 3)]
    C --> D[Bokeh Plot Engine]
    D --> E[Export HTML<br>(model1_plot.html etc)]



## Architecture Diagram

+------------------+
| Dataset (.csv) |
+--------+---------+
|
v
+-----------------------+
| Row-by-Row Simulator |
| (Streaming Loop) |
+----------+------------+
|
v
+--------------------------+
| Pricing Model Block |
| (Model 1 / 2 / 3 logic) |
+----------+---------------+
|
v
+----------------------+
| Bokeh Plot Engine |
| (Live Visual Output) |
+----------+-----------+
|
v
+-----------------------+
| HTML Plot Export |
| (modelX_plot.html) |
+-----------------------+
