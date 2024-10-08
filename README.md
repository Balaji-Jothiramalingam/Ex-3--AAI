## Implementation of Approximate Inference in Bayesian Networks


## Aim: 
   To construct a python program to implement approximate inference using Gibbs Sampling.</br>
## Algorithm:

Step 1: Bayesian Network Definition and CPDs:

Define the Bayesian network structure using the BayesianNetwork class from pgmpy.models.
Define Conditional Probability Distributions (CPDs) for each variable using the TabularCPD class.
Add the CPDs to the network.
Step 2: Printing Bayesian Network Structure:
Print the structure of the Bayesian network using the print(network) statement.
Step 3: Graph Visualization:
Import the necessary libraries (networkx and matplotlib).
Create a directed graph using networkx.DiGraph().
Define the nodes and edges of the graph.
Add nodes and edges to the graph.
Optionally, define positions for the nodes.
Use nx.draw() to visualize the graph using matplotlib.
Step 4: Gibbs Sampling and MCMC:
Initialize Gibbs Sampling for MCMC using the GibbsSampling class and provide the Bayesian network.
Set the number of samples to be generated using num_samples.
Step 5: Perform MCMC Sampling:
Use the sample() method of the GibbsSampling instance to perform MCMC sampling.
Store the generated samples in the samples variable.
Step 6: Approximate Probability Calculation:
Specify the variable for which you want to calculate the approximate probabilities (query_variable).
Use .value_counts(normalize=True) on the samples of the query_variable to calculate approximate probabilities.
Step 7:Print Approximate Probabilities:
Print the calculated approximate probabilities for the specified query_variable.
    
## Program:
Developed By:BALAJI J
Reg.No:212221243001
```
from pgmpy.models import BayesianNetwork
from pgmpy.factors.discrete import TabularCPD
from pgmpy.sampling import GibbsSampling
import networkx as nx
import matplotlib.pyplot as plt
alarm_model = BayesianNetwork(
    [
        ("Burglary", "Alarm"),
        ("Earthquake", "Alarm"),
        ("Alarm", "JohnCalls"),
        ("Alarm", "MaryCalls"),
    ]
)

# Defining the parameters using CPT
from pgmpy.factors.discrete import TabularCPD

cpd_burglary = TabularCPD(
    variable="Burglary", variable_card=2, values=[[0.999], [0.001]]
)
cpd_earthquake = TabularCPD(
    variable="Earthquake", variable_card=2, values=[[0.998], [0.002]]
)
cpd_alarm = TabularCPD(
    variable="Alarm",
    variable_card=2,
    values=[[0.999, 0.71, 0.06, 0.05], [0.001, 0.29, 0.94, 0.95]],
    evidence=["Burglary", "Earthquake"],
    evidence_card=[2, 2],
)
cpd_johncalls = TabularCPD(
    variable="JohnCalls",
    variable_card=2,
    values=[[0.95, 0.1], [0.05, 0.9]],
    evidence=["Alarm"],
    evidence_card=[2],
)
cpd_marycalls = TabularCPD(
    variable="MaryCalls",
    variable_card=2,
    values=[[0.1, 0.7], [0.9, 0.3]],
    evidence=["Alarm"],
    evidence_card=[2],
)

# Associating the parameters with the model structure
alarm_model.add_cpds(
    cpd_burglary, cpd_earthquake, cpd_alarm, cpd_johncalls, cpd_marycalls
)
print("Bayesian Network Structure")
print(alarm_model)
G=nx.DiGraph()
nodes=['Burglary','Earthquake','JohnCalls','MaryCalls']
edges=[('Burglary','Alarm'),('Earthquake','Alarm'),('Alarm','JohnCalls'),('Alarm','MaryCalls')]
G.add_nodes_from(nodes)
G.add_edges_from(edges)
pos={
    'Burglary':(0,0),
    'Earthquake':(2,0),
    'Alarm':(1,-2),
    'JohnCalls':(0,-4),
    'MaryCalls':(2,-4)
    }
nx.draw(G,pos,with_labels=True,node_size=1500,node_color="skyblue",font_size=10,font_weight="bold",arrowsize=20)
plt.title("Bayesian Network: Burglar Alarm Problem")
plt.show()
gibbssampler=GibbsSampling(alarm_model)
num_samples=10000
samples=gibbssampler.sample(size=num_samples)
query_variable="Burglary"
query_result=samples[query_variable].value_counts(normalize=True)
print("\n Approximate probabilities of {}:".format(query_variable))
print(query_result)
````


## Output:
![367682016-e24256d2-e2c5-4fad-9563-bf0f8eb6b9a8](https://github.com/user-attachments/assets/13725c44-c82c-46fa-9cc8-b9731c264e60)

![366297032-02e33578-bf4d-45b9-b675-2fe67e255f00](https://github.com/user-attachments/assets/e76628cc-fa33-42d0-a4ab-e044e7568406)


## Result:
Thus, Gibb's Sampling( Approximate Inference method) is succuessfully implemented using python.
