# TMAP

![TMAP generated from ~4000 molecules from HMDB](https://github.com/afloresep/HMDB-clustering/blob/master/tmap/Screenshot%202023-09-12%20at%2017.59.40.png)

This repository aims to visualize and cluster a highly diverse dataset obtained from the Human Metabolite Data Base. The input dataset comprises approximately 4,000 molecules. To use the script with a different input dataset than the one provided, please adhere to the following format in a CSV file:

```
idnumber,smiles
your_molecule,molecule_smiles
```

For example:

```
idnumber,smiles
HMDB0000058,Nc1ncnc2c1ncn2C1OC2COP(=O)(O)OC2C1O
```

**Execution:**
The visualization and clustering are achieved by calling a single script, `create_tmap.py`, which, in turn, utilizes other scripts to produce the desired outcome. In essence, this script follows several steps:

### 1. Data Preparation

By invoking `data_preparation.py`, the script prepares the data. This script takes SMILES and id_name as input and calculates various parameters:

- Desalted Smiles
- Heavy atom count (hac)
- Molecular weight (mw)
- Estimated lipophilicity (alogp)
- Number of hydrogen bond acceptors (hba)
- Number of hydrogen bond donors (hbd)
- Polar surface area (psa)
- Number of rotatable bonds (rotb)
- Number of aromatic rings (arom)
- Number of structural alerts (alerts)

Additionally, it generates other files in pickle (pkl) format that will be used in subsequent steps.

### 2. Create TMAP

TMAP, which stands for Topological Mapping, is a data visualization method designed to address the challenge of representing large datasets with millions of data points and high dimensionality in a two-dimensional format. It aims to visualize data while preserving both global and local features, providing sufficient detail for human inspection and interpretation.

[Reference](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-020-0416-x)

In essence, TMAP accomplishes these four steps:

1. LSH Forest indexing
2. Build c-approximate k-NN
3. Calculate MST of the k-NN
4. Lay the tree on the Euclidean plane using the OGDF modular C++ library

### 3. Find the Centroid

TMAP generates a tree structure, making it impossible to directly identify cluster centers. To overcome this limitation, the tree's topology is leveraged to create a DataFrame representing the hierarchy of the tree. This enables the selection of fragments that represent specific branches or portions.

To find the centroid of the tree, a node is identified where each branch contains fewer nodes than half the total number of nodes/fragments (n) in the tree. This process involves using a Python script (centroid.py), similar to the one used in OpenGenus IQ17 for centroid decomposition. The ID of the centroid node is saved to a text file (centroid.txt).

### 4. Hierarchy

The script identifies the children of the centroid node and adds them to a new column in the DataFrame. It also repeats this process for children of children while avoiding the addition of nodes that are already present in the DataFrame. The final DataFrame is converted to CSV for visualization purposes.



## Usage


## Setting up the Conda Environment

To ensure that you have the required dependencies to run this project, you can create a Conda environment based on the provided `tmap_environment.yml` file. Follow these steps:

1. **Clone the Repository**: First, clone this repository to your local machine:

   ```
   git clone https://github.com/yourusername/yourproject.git
   ```

2. **Navigate to the Project Directory**: Change your current directory to the project's root directory:

   ```
   cd yourproject
   ```

3. **Create a New Conda Environment**: Create a new Conda environment using the provided YAML file. You can choose to use the same environment name (`tmap`) or specify a different one:

   ```
   conda create env --name tmap --file tmap_environment.yml
   ```

4. **Activate the Environment**: Activate the newly created environment:

   ```
   conda activate tmap
   ```

Now, you have set up the Conda environment with all the required packages. You can run the project within this environment.

To deactivate the environment when you're done, simply run:

```
conda deactivate
```

### Running the Script

To run the script, open your terminal, go to tmap folder and execute the following command:

```
python create_tmap.py HMDB-smiles 
```

### Command-Line Options

The script supports several command-line options that allow you to customize the TMAP creation process:

- `--LSH_dim`: LSH dimension (default: 1024)
- `--prefix_trees`: Prefix trees (default: 128)
- `--cfg_k`: k for layout configuration (default: 50)
- `--cfg_kc`: kc for layout configuration (default: 50)
- `--cfg_mmm`: mmm_repeats for layout configuration (default: 2)
- `--cfg_node`: node_size for layout configuration (default: 1/70)
- `--cfg_sl_steps`: sl_extra_scaling_steps for layout configuration (default: 10)
- `--cfg_sl_repeats`: sl_repeats for layout configuration (default: 2)
- `--map4_dim`: MAP4 dimension (default: 1024)

Example:

```
python create_tmap.py HMDB-smiles --map4_dim 512
```

This command runs the script using `HMDB-smiles.pkl` as input data and modifies the `map4_dim` option to 512, overriding the default value.

### Output

The script will create a TMAP visualization and save it as `.html` and `.js` files in the same directory as the script. It will also move these files and any files starting with the input file name into a new folder with the same name as the input file.


### Images

![TMAP generated from ~4000 molecules from HMDB](https://github.com/afloresep/HMDB-clustering/blob/master/tmap/Screenshot%202023-09-12%20at%2017.59.40.png)



