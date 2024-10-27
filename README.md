## ViralAdaptSim

ViralAdaptSim is a Python-based simulation that models viral adaptation and immune response interactions. The goal is to explore how viruses mutate and adapt in response to immune system pressures, creating a simulated environment that can aid research and education on immune dynamics, viral evolution, and pathogen behavior.

## Key Features

Viral Mutation and Evolution: Simulates the process of viral mutations, survival, and adaptation within a host environment.
Immune Response Modeling: Models an immune system's adaptive responses to evolving viral strains.
Visualization of Evolution: Tracks and plots the fitness progression of both viral and immune responses across generations.
Customizable Simulation Parameters: Allows adjustment of parameters such as mutation rate, population size, selection pressure, and number of generations.

## Objectives
Model the evolutionary "arms race" between pathogens and host immune systems.
Provide insights into immune adaptation and viral mutation processes.
Serve as an educational tool for teaching immune dynamics and evolutionary biology.

## Getting Started

## Create the Python File:

Open a terminal and navigate to the directory where you saved the file.

Run the Python file with the following command:

```bash
python setup_and_run_simulation.py
```
## Setup and runsimulation

```bash

import sys
import subprocess
import os
import json
import tkinter as tk
from tkinter import filedialog

# Virtual environment directory
venv_dir = "env"

# Function to create a virtual environment if not already created
def ensure_virtualenv():
    if not os.path.exists(venv_dir):
        print("Creating virtual environment...")
        subprocess.check_call([sys.executable, "-m", "venv", venv_dir])

# Function to activate virtual environment and install packages
def install_in_virtualenv():
    # Path to Python executable in the virtual environment
    python_executable = os.path.join(venv_dir, "bin", "python")
    subprocess.check_call([python_executable, "-m", "pip", "install", "numpy", "matplotlib", "requests", "plotly", "dash"])

# Set up and activate the virtual environment
ensure_virtualenv()
install_in_virtualenv()

# Activate the virtual environment's Python executable for further use
python_executable = os.path.join(venv_dir, "bin", "python")

# Run the simulation code within the virtual environment
simulation_code = """
import numpy as np
import matplotlib.pyplot as plt
import requests
import json
import plotly.graph_objects as go
import tkinter as tk
from tkinter import filedialog

# Define Virus class with mutation and adaptation features
class Virus:
    def __init__(self, name, mutation_rate, resistance_level, infectiousness, virulence):
        self.name = name
        self.mutation_rate = mutation_rate
        self.resistance_level = resistance_level
        self.infectiousness = infectiousness  # Higher infectiousness means faster spread
        self.virulence = virulence  # Higher virulence means more severe impact on host
        self.genome = np.random.randint(0, 2, 10)

    def mutate(self):
        mutation_mask = np.random.rand(len(self.genome)) < self.mutation_rate
        self.genome = np.where(mutation_mask, 1 - self.genome, self.genome)
        self.resistance_level += self.mutation_rate / 10

# Define ImmuneSystem class
class ImmuneSystem:
    def __init__(self, adaptation_rate):
        self.adaptive_response = adaptation_rate
        self.memory_cells = []
        self.strength = 0.8  # Represents overall immune system strength

    def recognize_virus(self, virus):
        match = np.sum(self.memory_cells == virus.genome) if len(self.memory_cells) > 0 else 0
        return match

    def respond_to_virus(self, virus):
        response_effectiveness = max(0, self.strength - virus.resistance_level + self.adaptive_response)
        if response_effectiveness < 0.5:
            self.adapt(virus)
        return response_effectiveness

    def adapt(self, virus):
        if len(self.memory_cells) == 0 or not np.array_equal(self.memory_cells, virus.genome):
            self.memory_cells = virus.genome
            self.adaptive_response += 0.05

# Define sicknesses with default values
sickness_types = {
    "Common Cold": {"mutation_rate": 0.05, "resistance_level": 0.1, "infectiousness": 0.6, "virulence": 0.2},
    "Flu": {"mutation_rate": 0.1, "resistance_level": 0.3, "infectiousness": 0.7, "virulence": 0.4},
    "Pneumonia": {"mutation_rate": 0.15, "resistance_level": 0.5, "infectiousness": 0.6, "virulence": 0.6},
    "COVID-19": {"mutation_rate": 0.2, "resistance_level": 0.7, "infectiousness": 0.9, "virulence": 0.5},
    "Ebola": {"mutation_rate": 0.3, "resistance_level": 0.9, "infectiousness": 0.5, "virulence": 0.9}
}

# Function to update sickness data from local JSON file using file dialog
def update_data_from_file():
    root = tk.Tk()
    root.withdraw()  # Hide the main tkinter window
    file_path = filedialog.askopenfilename(
        title="Select a JSON file to load sickness data",
        filetypes=(("JSON files", "*.json"), ("All files", "*.*"))
    )

    if not file_path:
        print("No file selected, using default values.")
        return

    try:
        with open(file_path, 'r') as f:
            data = json.load(f)
            for key, value in data.items():
                if isinstance(value, dict):
                    sickness_types[key] = value
        print("Data successfully loaded from the selected file.")
    except Exception as e:
        print(f"Failed to load data from the file: {e}. Using default values.")

# Ask if user wants to load data from a local file
use_file = input("Would you like to load sickness data from a local JSON file? (y/n): ").lower()
if use_file == 'y':
    update_data_from_file()

# Let the user choose a sickness
print("Choose a sickness to simulate:")
for i, sickness in enumerate(sickness_types.keys()):
    print(f"{i+1}. {sickness}")

choice = int(input("Enter the number of your choice: ")) - 1
sickness_name = list(sickness_types.keys())[choice]
sickness = sickness_types[sickness_name]

# Display and allow customization of parameters
print(f"Simulating {sickness_name} with default mutation rate {sickness['mutation_rate']} and resistance level {sickness['resistance_level']}.")
mutation_rate = float(input(f"Enter a custom mutation rate for {sickness_name} (default is {sickness['mutation_rate']}): ") or sickness["mutation_rate"])
resistance_level = float(input(f"Enter a custom resistance level for {sickness_name} (default is {sickness['resistance_level']}): ") or sickness["resistance_level"])

# Initialize simulation parameters
generations = 100
virus = Virus(sickness_name, mutation_rate=mutation_rate, resistance_level=resistance_level, infectiousness=sickness["infectiousness"], virulence=sickness["virulence"])
immune_system = ImmuneSystem(adaptation_rate=0.1)

# Run the simulation
virus_resistance_history = []
immune_effectiveness_history = []
infectiousness_history = []
virulence_history = []

for generation in range(generations):
    virus.mutate()  # Mutate the virus
    effectiveness = immune_system.respond_to_virus(virus)
    virus_resistance_history.append(virus.resistance_level)
    immune_effectiveness_history.append(effectiveness)
    infectiousness_history.append(virus.infectiousness)
    virulence_history.append(virus.virulence)

# Plot the results with Plotly
fig = go.Figure()
fig.add_trace(go.Scatter(x=list(range(generations)), y=virus_resistance_history, mode='lines', name='Virus Resistance'))
fig.add_trace(go.Scatter(x=list(range(generations)), y=immune_effectiveness_history, mode='lines', name='Immune Effectiveness'))
fig.add_trace(go.Scatter(x=list(range(generations)), y=infectiousness_history, mode='lines', name='Infectiousness'))
fig.add_trace(go.Scatter(x=list(range(generations)), y=virulence_history, mode='lines', name='Virulence'))

fig.update_layout(
    title=f"Virus and Immune System Dynamics for {sickness_name}",
    xaxis_title="Generations",
    yaxis_title="Levels",
    legend=dict(x=0, y=1)
)

fig.show()
"""

# Save the simulation code to a temporary file and execute it
with open("temp_simulation.py", "w") as f:
    f.write(simulation_code)

# Run the simulation script within the virtual environment
subprocess.check_call([python_executable, "temp_simulation.py"])

# Clean up the temporary file
os.remove("temp_simulation.py")



```

## Prerequisites

Python 3.7+: Make sure Python is installed on your system.
Installation
## Clone the Repository:
```bash
git clone https://github.com/YOUR_USERNAME/ViralAdaptSim.git
cd ViralAdaptSim
```
## Run the Simulation Script:



```bash
python virusevolve.py
```
The script automatically sets up a virtual environment, installs necessary packages (numpy, matplotlib), and runs the simulation.

## Usage

## Adjust Simulation Parameters:

You can modify parameters in virusevolve.py, such as:
population_size: Number of digital organisms in the population.
genome_length: Length of each organism's genome.
mutation_rate: Probability of mutation per genome bit.
generations: Number of generations for the simulation.
Run the Simulation: Execute the script to simulate viral evolution and observe fitness changes.

## Output
The simulation generates a graph that shows the average fitness of the population over generations, illustrating the immune system's adaptation against viral evolution.
Project Structure
virusevolve.py: Main simulation script.
.gitignore: Excludes virtual environment and other unnecessary files.
README.md: Project description and setup instructions.

## LICENSE: License file containing the GNU GPL v3 license text.

This project is licensed under the GNU General Public License v3 (GPL v3), which ensures that any adaptations or derived versions of ViralAdaptSim remain open-source and freely accessible. You can read the full license text in the LICENSE file.

## GNU GPL v3
Ensures Open Collaboration: Any modified versions of ViralAdaptSim must be released as open-source, supporting community-driven research and improvements.
Promotes Free and Accessible Research: By using the GPL v3, ViralAdaptSim encourages the open sharing of information, making it ideal for collaborative scientific and educational work.
For more details on the GNU GPL v3, visit the GNU website.

Contributing
Contributions are welcome! If you have ideas for new features, optimizations, or improvements, feel free to fork the repository, create a pull request, or open an issue to discuss your ideas.




