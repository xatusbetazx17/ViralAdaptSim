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

# Define simulation parameters
population_size = 20        # Number of organisms in the population
genome_length = 10          # Length of each genome (number of bits)
mutation_rate = 0.1         # Mutation probability per bit
generations = 50            # Number of generations for the simulation
target_genome = np.ones(genome_length)  # Target genome (array of ones)

# Initialize population with random genomes
population = np.random.randint(0, 2, (population_size, genome_length))

# Fitness function: counts the matching bits with the target genome
def fitness(genome):
    return np.sum(genome == target_genome)

# Function to create a new generation through selection and mutation
def create_next_generation(population):
    # Calculate fitness for each organism
    fitness_scores = np.array([fitness(genome) for genome in population])

    # Select top 50% of organisms based on fitness
    selection_count = population_size // 2
    selected_indices = np.argsort(fitness_scores)[-selection_count:]
    selected_population = population[selected_indices]

    # Generate new population by crossover and mutation
    new_population = []
    for _ in range(population_size):
        # Select two parents randomly from the selected population
        parents = selected_population[np.random.choice(selection_count, 2, replace=False)]

        # Perform crossover
        crossover_point = np.random.randint(1, genome_length - 1)
        child_genome = np.concatenate((parents[0][:crossover_point], parents[1][crossover_point:]))

        # Apply mutation
        mutation_mask = np.random.rand(genome_length) < mutation_rate
        child_genome = np.where(mutation_mask, 1 - child_genome, child_genome)

        new_population.append(child_genome)

    return np.array(new_population)

# Run the simulation
fitness_history = []
for generation in range(generations):
    # Calculate and store the average fitness of the current generation
    avg_fitness = np.mean([fitness(genome) for genome in population])
    fitness_history.append(avg_fitness)

    # Generate the next generation
    population = create_next_generation(population)

# Display the final fitness history to observe evolution
plt.plot(fitness_history)
plt.title("Evolution of Average Fitness over Generations")
plt.xlabel("Generation")
plt.ylabel("Average Fitness")
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




