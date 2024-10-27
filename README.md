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

    # Install packages
    subprocess.check_call([python_executable, "-m", "pip", "install", "numpy", "matplotlib"])

# Set up and activate the virtual environment
ensure_virtualenv()
install_in_virtualenv()

# Activate the virtual environment's Python executable for further use
python_executable = os.path.join(venv_dir, "bin", "python")

# Run the actual simulation code within the virtual environment
simulation_code = """
import numpy as np
import matplotlib.pyplot as plt
import random

# Define Virus class with mutation and adaptation features
class Virus:
    def __init__(self, name, mutation_rate, resistance_level):
        self.name = name
        self.mutation_rate = mutation_rate
        self.resistance_level = resistance_level  # How well it resists immune responses
        self.genome = np.random.randint(0, 2, 10)  # Random initial genome

    def mutate(self):
        mutation_mask = np.random.rand(len(self.genome)) < self.mutation_rate
        self.genome = np.where(mutation_mask, 1 - self.genome, self.genome)
        # Increase resistance level slightly with mutation
        self.resistance_level += self.mutation_rate / 10

# Define ImmuneSystem class that attacks and adapts
class ImmuneSystem:
    def __init__(self):
        self.adaptive_response = 0.1  # Ability to adapt over time
        self.memory_cells = []

    def recognize_virus(self, virus):
        match = np.sum(self.memory_cells == virus.genome) if len(self.memory_cells) > 0 else 0
        return match

    def respond_to_virus(self, virus):
        response_effectiveness = max(0, 1 - virus.resistance_level + self.adaptive_response)
        if response_effectiveness < 0.5:
            self.adapt(virus)
        return response_effectiveness

    def adapt(self, virus):
        if len(self.memory_cells) == 0 or not np.array_equal(self.memory_cells, virus.genome):
            self.memory_cells = virus.genome  # "Remember" virus genome
            self.adaptive_response += 0.05

# Define available sicknesses with different severity levels
sickness_types = {
    "Common Cold": {"mutation_rate": 0.05, "resistance_level": 0.1},
    "Flu": {"mutation_rate": 0.1, "resistance_level": 0.3},
    "Pneumonia": {"mutation_rate": 0.15, "resistance_level": 0.5},
    "COVID-19": {"mutation_rate": 0.2, "resistance_level": 0.7},
    "Ebola": {"mutation_rate": 0.3, "resistance_level": 0.9}
}

# Let the user choose a sickness
print("Choose a sickness to simulate:")
for i, sickness in enumerate(sickness_types.keys()):
    print(f"{i+1}. {sickness}")

choice = int(input("Enter the number of your choice: ")) - 1
sickness_name = list(sickness_types.keys())[choice]
sickness = sickness_types[sickness_name]
print(f"Simulating {sickness_name} with mutation rate {sickness['mutation_rate']} and resistance level {sickness['resistance_level']}.")

# Initialize simulation parameters
generations = 100
virus = Virus(sickness_name, mutation_rate=sickness["mutation_rate"], resistance_level=sickness["resistance_level"])
immune_system = ImmuneSystem()

# Run the simulation
virus_resistance_history = []
immune_effectiveness_history = []

for generation in range(generations):
    virus.mutate()  # Mutate the virus

    # Immune system attempts to respond to the virus
    effectiveness = immune_system.respond_to_virus(virus)
    virus_resistance_history.append(virus.resistance_level)
    immune_effectiveness_history.append(effectiveness)

# Plot the results
plt.figure(figsize=(12, 6))
plt.plot(virus_resistance_history, label="Virus Resistance")
plt.plot(immune_effectiveness_history, label="Immune Effectiveness", alpha=0.7)
plt.title(f"Virus Resistance vs Immune Effectiveness over Generations ({sickness_name})")
plt.xlabel("Generations")
plt.ylabel("Effectiveness / Resistance")
plt.legend()
plt.show()
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




