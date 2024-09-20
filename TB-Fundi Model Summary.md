# TB-Fundi Model Summary

The **TB-Fundi** model is an agent-based simulation designed to study the transmission of tuberculosis (TB) across a spatially and demographically structured population. The flexibility of the model allows researchers to simulate TB spread across different regions by modifying input data. Below is an overview of the key steps and mathematical foundations that govern the workflow of the model.

### Workflow Overview:
1. **Population Setup**:
   - **Demographic Data**: The model uses real-world demographic data to create a simulated population. Individuals are assigned ages and grouped into households based on observed household composition data.
   - **Spatial Setup**: The population is distributed across a spatial grid. The probability \( p_{i \to l} \) that a household \( i \) is located in a grid cell \( l \) is based on the population density in the area:
     \[
     p_{i \to l} = \frac{C \cdot s_i \cdot n_l}{\sum_i s_i \cdot \sum_l n_l}
     \]
     where \( s_i \) is the size of household \( i \) and \( n_l \) is the number of people in grid cell \( l \). \( C \) is a normalizing constant ensuring that the sum of all probabilities equals 1.

2. **Contact Network Formation**:
   - **Household, School, and Workplace Contacts**: Based on the locations, age, and household data, the model establishes contact networks for interactions within households, schools, and workplaces.
   - **Community Contacts**: For community contacts, TB-Fundi uses a probabilistic approach based on the **Universal Visitation Law of Human Mobility**, which predicts the likelihood of individuals from different locations interacting. The probability \( q_{ij} \) that an individual in grid cell \( i \) meets someone from grid cell \( j \) is:
     \[
     q_{ij} = \frac{C \cdot \rho_i \cdot r_j \cdot f_{\text{home}}}{d_{ij}} \cdot \log \left( \frac{f_{\max}}{f_{\min}} \right)
     \]
     where:
     - \( \rho_i \) is the population density of location \( i \),
     - \( r_j \) is the radius of grid cell \( j \),
     - \( d_{ij} \) is the distance between grid cells \( i \) and \( j \),
     - \( f_{\text{home}} \) is the frequency of visits to the home location (typically set to 1 visit per day),
     - \( f_{\max} \) and \( f_{\min} \) are the maximum and minimum frequencies of visits to other locations.
   This formula ensures that individuals closer to each other have a higher chance of contact.

3. **Initialization of Epidemic**:
   - **Initial Infections**: The epidemic can be initiated by randomly selecting a set of individuals to be infected or by simulating external introductions of the disease over time. The probability of an individual being infected from an external source at time \( t \) is:
     \[
     \beta_{\text{ext}}(t) = 1 - e^{-\epsilon \cdot S(t)}
     \]
     where \( \epsilon \) is the external attack rate, and \( S(t) \) is the fraction of susceptible individuals at time \( t \).

4. **Disease Progression**:
   - **Stages of Disease**: Individuals can move through several stages of infection: latent (infection), minimal disease, subclinical TB, clinical TB, recovery, or death. The transitions between these stages are governed by transition rates between each state.
   - **Transition Probability**: The probability \( p_{m \to n} \) of an individual transitioning from state \( m \) to state \( n \) is based on the duration in the current state and a rate parameter \( r_{m \to n} \):
     \[
     p_{m \to n} = 1 - e^{-r_{m \to n} \cdot \Delta t_m}
     \]
     where \( \Delta t_m \) is the time the individual has spent in state \( m \).

5. **Transmission Dynamics**:
   - **Transmission Probability**: The probability \( t_{\text{setting}} \) of an individual becoming infected through contact with an infectious person (in the subclinical or clinical stage) is calculated for different settings (home, school, workplace, community):
     \[
     t_{\text{setting}} = 1 - e^{-\lambda_{\text{setting}} \cdot \Delta t}
     \]
     where \( \lambda_{\text{setting}} \) is the transmission risk in the setting and \( \Delta t \) is the time step.
   - **Setting-Specific Risk**: The transmission risk in a particular setting (household, school, workplace, community) is given by:
     \[
     \lambda_{\text{setting}} = \sum_{k \in \text{contacts}} \frac{\beta \cdot c_{\text{setting}, k \to a} \cdot I_k}{s_{\text{setting}}}
     \]
     where:
     - \( \beta \) is the transmission probability per contact,
     - \( c_{\text{setting}, k \to a} \) is the number of contacts between individuals of different age groups,
     - \( I_k \) is an indicator of whether an individual \( k \) is infectious,
     - \( s_{\text{setting}} \) is the number of individuals linked to the respective setting.

6. **Simulation Execution**:
   - The model simulates the spread of TB over discrete time steps (e.g., weekly). During each step, the infection status, disease progression, and recovery of individuals are updated based on contact probabilities and disease transition rates.

7. **Results and Calibration**:
   - **Calibration**: To ensure accuracy, TB-Fundi can be calibrated using historical data to establish a "steady-state" distribution of disease states. This involves running simulations over extended periods (e.g., 10 years) to reach equilibrium in the disease progression states.
   - **Comparison with Real Data**: After calibration, the model can be used to simulate specific time periods and compare the outcomes (e.g., TB incidence) with actual epidemiological data from different regions.

### Summary of Key Steps:
1. **Create a simulated population** based on demographic data.
2. **Set up contact networks** using the Universal Visitation Law for communities and established networks for households, schools, and workplaces.
3. **Initialize the epidemic** by randomly infecting individuals or simulating infections from external sources.
4. **Simulate disease progression** through different stages (infection, subclinical, clinical, recovery).
5. **Calculate transmission probabilities** based on contact rates in different settings.
6. **Run the simulation** over time to track how TB spreads in the population.
7. **Calibrate and validate results** by comparing with historical data to ensure the model accurately reflects real-world transmission.

By providing a clear mathematical basis for the model's operations, this summary helps to clarify the steps and processes involved in simulating TB transmission with the TB-Fundi model.