

# 🚖 Rideshare Demand Forecasting & Dispatch Optimization

**A "Digital Twin" simulation of a rideshare network that couples Machine Learning (Demand Forecasting) with Operations Research (Real-time Dispatch) to minimize rider wait times and maximize fleet efficiency.**

-----

## 📖 Project Overview

Matching drivers to riders in a stochastic urban environment is a complex logistical challenge. This project simulates a city grid, generates synthetic demand based on realistic patterns (Poisson distribution), and compares two dispatch strategies:

1.  **Baseline (Random/Greedy):** Assigning the first available driver.
2.  **Optimal (Hungarian Algorithm):** A global optimization approach that minimizes the total aggregate wait time for the entire system.

**Key Result:** The Optimal strategy reduced average rider wait times by **\~40%** compared to the baseline.


-----

## 🛠️ Technical Architecture

The pipeline consists of four distinct modules:

### 1\. Data Engineering & Simulation

  * **Synthetic Data Generation:** Created a spatio-temporal dataset using **Poisson Processes** to mimic realistic arrival rates.
  * **Feature Engineering:** Implemented **Cyclical Time Encoding** (Sine/Cosine transformation) to preserve temporal continuity for ML models (e.g., 23:59 is close to 00:01).

### 2\. Demand Forecasting (The "Oracle")

  * **Model:** Random Forest Regressor (`sklearn`).
  * **Goal:** Predict demand ($\lambda$) for specific zones in 15-minute buckets to pre-position drivers.
  * **Validation:** Strict chronological splitting (Train on Past, Test on Future) to prevent data leakage.

### 3\. Dispatch Logic (The "Engine")

  * **Physics:** Utilized **Manhattan Distance (L1 Norm)** for realistic urban travel time calculation.
  * **Optimization:** Employed the **Hungarian Algorithm** (`scipy.optimize.linear_sum_assignment`) to solve the Bipartite Matching problem between the set of Available Drivers and Active Requests.

-----

## 💻 Tech Stack

  * **Language:** Python
  * **Data Manipulation:** Pandas, NumPy
  * **Machine Learning:** Scikit-Learn (Pipeline, ColumnTransformer)
  * **Optimization:** SciPy
  * **Visualization:** Plotly (Interactive dashboards)

-----

## 🚀 How to Run

1.  **Clone the repository**

    ```bash
    https://github.com/Rakabi007/Rideshare-Demand-Forecasting-to-Reduce-Driver-Idle-Time-and-Slash-Rider-Wait-Time.git
    cd rideshare-optimization
    ```

2.  **Install dependencies**

    ```bash
    pip install numpy pandas scipy scikit-learn plotly
    ```

3.  **Run the Simulation**
    Open the Jupyter Notebook `Rideshare_Simulation.ipynb` and execute all cells. The simulation will:

      * Generate 5 days of training data.
      * Train the Random Forest model.
      * Run the 4-hour live dispatch simulation.
      * Render the comparison charts.



## 🧠 Key Learnings & Challenges

  * **Imputing Zeroes:** Real-world data is sparse. I learned to use Cartesian Products to generate "zero-demand" rows so the model learns when *not* to expect riders.
  * **Global vs. Local Optimization:** A greedy approach (nearest driver) feels intuitive but fails at scale. Global optimization (Hungarian Algo) requires more compute but delivers superior system-level efficiency.
  * **Cyclical Features:** Linear time features confuse models; mapping time to a unit circle significantly improved forecast stability.



## 🔮 Future Improvements

  * **Traffic Variance:** Incorporate stochastic travel times (traffic jams) rather than constant speeds.
  * **Surge Pricing:** Implement dynamic pricing logic when Demand \> Supply.
  * **Reinforcement Learning:** Replace the heuristic dispatch with a DQN agent.


