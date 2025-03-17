import numpy as np
import pandas as pd

# Given data
data = pd.DataFrame({
    'Product': [1, 2, 3, 4],
    'Demand': [1000, 300, 50, 50],
    'Ordering_Cost': [20, 50, 50, 50],
    'Unit_Cost': [50, 50, 30, 30],
    'Holding_Cost_Rate': [0.15, 0.15, 0.15, 0.15]
})

# Strategy 1 - Independent EOQ
data['EOQ_Independent'] = np.sqrt((2 * data['Demand'] * data['Ordering_Cost']) / (data['Unit_Cost'] * data['Holding_Cost_Rate']))

# Strategy 2 - All products ordered with the same frequency
total_demand_cost = (data['Demand'] * data['Unit_Cost']).sum()
total_ordering_cost = data['Ordering_Cost'].sum()
total_holding_cost_rate = (data['Unit_Cost'] * data['Holding_Cost_Rate']).sum()
EOQ_same_freq = np.sqrt((2 * total_ordering_cost * data['Demand'].sum()) / total_holding_cost_rate)
data['EOQ_Same_Frequency'] = EOQ_same_freq

# Strategy 3 - Tailored Aggregation
common_ordering_cost_shared = 150  # given common cost

data['EOQ_Tailored'] = np.sqrt(
    (2 * data['Demand'] * (data['Ordering_Cost'] + common_ordering_cost_shared)) /
    (data['Unit_Cost'] * data['Holding_Cost_Rate'])
)

# Display results
print(data[['Product', 'Demand', 'EOQ_Independent', 'EOQ_Same_Frequency', 'EOQ_Tailored']])
