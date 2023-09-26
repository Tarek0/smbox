# smbox

`smbox`:**Sequential Model-Based Optimization eXpress** is a lightweight Python library for Hyperparmeter Optomisimation.

## Table of Contents

- [Introduction](#Introduction)
- [Installation](#Installation)
- [License](#License)
- [Contact](#Contact)
- [Example](#Example)
- [Cite](#Cite)

## 📖 Introduction
`smbox` provides a cutting-edge implementation of Sequential Model-Based Optimization (SMBO), tailored for tuning ML algorithm parameters. At its core, SMBO is designed to optimally explore expensive and noisy black-box functions, making it particularly suitable for hyperparameter optimization. With smbox, users can achieve peak performance in their ML models, bypassing the need for exhaustive and time-consuming search methods.

### Features:
- 🚂 **State-of-the-art HPO**: Achieve state-of-the-art hyperparameter optimization performance with minimal effort.


- 🎯 **Default Parameter Space**: Use our default parameter search space to simply plug and play with the most common ML algorithms.
Making it simpler than defining grid search ranges or complicated parameter search distributions.


- 🤖️ **Bring your own Parameter Space**: Provide your own parameter search space to better suit your needs, or update our default search spaces with ease.


- 🤖️ **Bring your own Objective Function**: Tune any ML algorithm using the performance metric to suit your needs with. (Coming soon)

## 🛠 Installation
For Users (Non-Developers):
If you just want to use smbox, you can install it directly from the repository:
```
pip install git+https://github.com/Tarek0/smbox.git
```
For Developers:
If you're a developer and intend to contribute or make changes to smbox, you'll want to clone the repository and install in "editable" mode. This ensures that changes you make are immediately reflected in the version of smbox that's used in your Python environment.

1. Clone the repository:
```git clone https://github.com/Tarek0/smbox.git```
2. Navigate to the cloned directory:
```cd smbox```
3. Install in editable mode:
```pip install -e . ```


## 📄 License
smbox is released under the MIT License. See the LICENSE file for more details.

## 📮 Contact
For questions or feedback, please join our [Slack channel.](https://join.slack.com/t/slack-4aw5037/shared_invite/zt-22maoikro-_v_cxHvh7L_nMo7oqPvIvg)

## 🚀 Example
```
import pandas as pd
import openml
from smbox.utils import Logger
from smbox.optimise import Optimise
from smbox.smbox_config import smbox_params
from smbox.paramspace import rf_default_param_space

# Fetch a classification dataset from OpenML
dataset = openml.datasets.get_dataset(38)
target_name ='target'

X, y, categorical_indicator, attribute_names = dataset.get_data(
    dataset_format="array", target=dataset.default_target_attribute
)
df = pd.DataFrame(X, columns=attribute_names)
df[target_name] = y

# Basic data preprocesing 
y = df[target_name]
X = df.drop(target_name, axis=1)
X.fillna(0, inplace=True)

# Configuration settings for the experiment.
# This dictionary holds key details for the setup, including dataset details, algorithm choice, search strategy, etc.
# Some keys enhance the clarity of logs and outputs, ensuring reproducibility and transparency in experiments.
global config
config = {
    'dataset_source': 'openml',               # Dataset's source platform; 'openml' in this instance.
    'dataset': 38,                            # Unique identifier for the dataset on OpenML.
    'algorithm': 'rf',                        # Chosen algorithm: Random Forest (denoted as 'rf').
    'search_strategy': 'smbox',               # Optimization/search strategy, specified as 'smbox'.
    'search_strategy_config': smbox_params,   # Configuration specifics for 'smbox'. Assumes `smbox_params` is predefined.
    'wallclock': 600,                         # Maximum time allotted for the task (600 seconds or 10 minutes).
    'output_root': './output/'                # Directory for saving output/results.
}

# Create a dictionary with training data. This format is needed for the SMBOX optimizer.
data = {"X_train": X, "y_train":y}

# Use our default hyperparameter search space for a Random Forest algorithm.
cfg_schema = rf_default_param_space

logger = Logger()
logger.log(f'-------------Starting SMBOX')
optimiser = Optimise(config, random_seed=42)
best_parameters, best_perf = optimiser.SMBOXOptimise(data, cfg_schema)
```
## Cite
BibTex
TBC
