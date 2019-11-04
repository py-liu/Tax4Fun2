# Tax4Fun2 v1.1.4

Welcome to the homepage of Tax4Fun2.
Older versions are also available under https://sourceforge.net/projects/tax4fun2/

**Tax4Fun2 requirements**

BLAST+ (ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)

R packages seqinr, ape

diamond v0.9.24 is needed for functional annotation (binaries for Windows and Linux are downloaded as part of the reference data), Mac users need to compile diamond. Please see the wiki for instructions.

## Install the Tax4Fun2 package, build the default reference data and install all dependencies
**Download and install Tax4Fun2**

1) Click [here](https://github.com/bwemheu/Tax4Fun2/releases/download/1.1.4/Tax4Fun2_1.1.4.tar.gz) to download the latest R package

2) Install the package in R using the command below
```
install.packages(pkgs = "Tax4Fun2_1.1.4.tar.gz", repos = NULL)
```

**Load the Tax4Fun2 library and create a new folder for the installation**

```
# Load the Tax4Fun2 library
library(Tax4Fun2)

# Create a new folder
dir.create(path = "Tax4Fun2")

# Change working directory to the newly created folder
setwd("Tax4Fun2")
```

**Build the default reference database**

In order to provide a straight-forward solution, we implemented a function in Tax4Fun2 v1.1 which will download and build the reference database. This buildReferenceData() command will download and build the default Tax4Fun2 reference database. In addition, it will install the R packages ape and seqinr if requested. Moreover, it will test for the presence of blastn in PATH.

Options:
  path_to_working_directory = "."      Path to the folder for Tax4Fun2 installation (Default: Build database in current working directory)
  use_force = FALSE                    Overwrite folder if exists (Default is FALSE)
  install_suggested_packages = TRUE    Install suggested R packages ape and seqinr (Default is TRUE)

```
buildReferenceData(path_to_working_directory = ".", use_force = FALSE, install_suggested_packages = TRUE)
```

We noticed some issues with the unzip command in Windows. Basically, the data was download but couldn't be unzipped. In case you have issues here, click [here](https://cloudstor.aarnet.edu.au/plus/s/PL7ieXPOP6mp1hA/download) to download the data.

**Install dependencies**

This command will download the currently latest version of blast (v2.9.0) and will place the binaries in the Tax4Fun2 reference folder. In addition, it will test for the presence of the R packages ape and seqinr.

Options:
  path_to_reference_data = "."         Path to the folder Tax4Fun2 containing the reference data (Default: Build database in current working directory)
  install_suggested_packages = TRUE    Install suggested R packages ape and seqinr (Default is TRUE)

```
buildReferenceData(path_to_working_directory = ".", use_force = FALSE, install_suggested_packages = TRUE)
```

Those who wish to install blast on their own or make it available to all users, please check 
[here](ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)
for the latest package and download the appropiate version for your system. Note that Mac users might have issues opening a ftp site in Safari.

For Mac users. Please run in terminal:
```
# Use curl to list the content of the ftp site
curl ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/

# Select the dmg file (e.g. ncbi-blast-2.9.0+.dmg) and use wget to download the file
wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.9.0+.dmg
```

**Download the test data**

This command will automatically download and build the Tax4Fun2 test data.

Options:
  path_to_working_directory = "."      Path to the folder for Tax4Fun2 installation (Default: Build database in current working directory(
  use_force = FALSE                    Overwrite folder if exists (Default is FALSE)

```
getExampleData(path_to_working_directory = ".", use_force = F)
```
Alternativly, check [here](https://cloudstor.aarnet.edu.au/plus/s/4htgFinDIpzOuKK/download) to download the data.

***
## Step 2: Generate your own reference datasets

**1. Extracting SSU seqeunces (16S rRNA and 18S rRNA)**

Either select one single genome (see first command below) or select a folder with several genomes or MAGs (see second command below). In order to use the second command, genomes must be placed in one folder (one file per genome) and all end with the same file extension (default is 'fasta')

```
# Option A) Extracting SSU sequences from a single genome
extractSSU(genome_file = "OneProkaryoticGenome.fasta", file_extension = "fasta", path_to_refernce_data = "Tax4Fun2_ReferenceData_v1.1")

# Option B) Extracting SSU sequences from multiple genomes
extractSSU(genome_folder = "MoreProkaryoticGenomes", file_extension = "fasta", path_to_refernce_data = "Tax4Fun2_ReferenceData_v1.1")
```
Note that genomes must have at least a single 16S or 18S rRNA gene sequences in thier genome. Check output after running the command to remove 'empty' genomes (those where the file size is 0)

**2. Assigning functions to prokayotic genomes**

*Linux and Windows users*
```
# Option A) Assigning function to a single genome
assignFunction(genome_file = "OneProkaryoticGenome.fasta", file_extension = "fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", num_of_threads = 1, fast = T)

# Option B) Assigning function to multiple genomes
assignFunction(genome_folder = "MoreProkaryoticGenomes/", file_extension = "fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", num_of_threads = 1, fast = T)
```

*Mac users*

Mac users need to compile diamond first (see wiki for help) and have to provide the path to the compiled diamond binary
```
# Option A) Assigning function to a single genome
assignFunction(genome_file = "OneProkaryoticGenome.fasta", file_extension = "fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", num_of_threads = 1, fast = T, path_to_diamond_binary_mac = "./diamond")

# Option B) Assigning function to multiple genomes
assignFunction(genome_folder = "MoreProkaryoticGenomes/", file_extension = "fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", num_of_threads = 1, fast = T, path_to_diamond_binary_mac = "./diamond")
```

**3. Generate the reference data**

After extraction of SSU sequences and functional assignments, you will have at least two files for each genome: one with the SSU seqeunces and one with the functional profile
In order to generate the final dataset, select the folder where these files and provide the correct file extensions (removing the respective file extensions will result in the same filename)
```
# 1) Generate user-defined reference data without uclust
generateUserData(path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_user_data = "MoreProkaryoticGenomes", name_of_user_data = "TEST1", fasta_extension = "_16SrRNA.ffn", uproc_file_extension = "_funPro.txt")

# 2) Generate user-defined reference data without uclust
generateUserDataByClustering(path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_user_data = "MoreProkaryoticGenomes", name_of_user_data = "TEST2", fasta_extension = "_16SrRNA.ffn", uproc_file_extension = "_funPro.txt", diamond_file_extension = "", use_force = T, path_to_usearch_bin = "usearch.exe")
```
I recommend to use the second command which includes a uclust clustering step and thus removes redundancy in your data
A 32-bit version of usearch can be downloaded [here](https://www.drive5.com/usearch/). Note that some windows user might need to install the 32bit version of the Microsoft Visual C++ Redistributable to run usearch.

## Step 3: Making functional predictions

**1. Making functional predictions using the default reference data only**
```
# 1. Run the reference blast
runRefBlast(path_to_otus = "KELP_otus.fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR", database_mode = "Ref99NR", use_force = T, num_threads = 6)
# 2) Predicting functional profiles
makeFunctionalPrediction(path_to_otu_table = "KELP_otu_table.txt", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR", database_mode = "Ref99NR", normalize_by_copy_number = TRUE, min_identity_to_reference = 0.97, normalize_pathways = FALSE)
```
Note: normalize_pathways = FALSE will affiliate the rel. abundance of each KO to each pathway it belongs to. By setting it to true, the rel. abundance is equally distributed to all pathways it was assigned to.)

**2. Making functional predictions using the default database without and with a user-gernated one***
```
# 1) Making functional predictions using the default database
runRefBlast(path_to_otus = "KELP_otus.fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR", database_mode = "Ref99NR", use_force = T, num_threads = 6)

# 2) Predicting functional profiles
makeFunctionalPrediction(path_to_otu_table = "KELP_otu_table.txt", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR", database_mode = "Ref99NR", normalize_by_copy_number = T, min_identity_to_reference = 0.97, normalize_pathways = F)
```

**2) Making functional predictions using the default database and a user-generated database (unclustered)**
```
# 1. Generate user data (specify the path to the user data [here: KELP_UserData]); the database will be generated in the folder with your data
generateUserData(path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_user_data = "KELP_UserData", name_of_user_data = "KELP1", fasta_extension = ".ffn", uproc_file_extension = ".txt")

# 2. Run the reference blast with include_user_data = TRUE and specifiy the path to the user data [here: KELP_UserData/KELP1]
runRefBlast(path_to_otus = "KELP_otus.fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR_withUser1", database_mode = "Ref99NR", use_force = T, num_threads = 6, include_user_data = T, path_to_user_data = "KELP_UserData/KELP1")

# 3. Making the prediction with your data included
makeFunctionalPrediction(path_to_otu_table = "KELP_otu_table.txt", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR_withUser1", database_mode = "Ref99NR", normalize_by_copy_number = T, min_identity_to_reference = 0.97, normalize_pathways = F, include_user_data = T, path_to_user_data = "KELP_UserData/KELP1")
```

**3) Making functional predictions using the default database and a user-generated database (clustered with ucsearch)**
```
# 1. Generate user data (specify the path to the user data [here: KELP_UserData]); the database will be generated in the folder with your data
generateUserDataByClustering(path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_user_data = "KELP_UserData", name_of_user_data = "KELP2", fasta_extension = ".ffn", uproc_file_extension = ".txt", path_to_usearch_bin = "usearch.exe", similarity_threshold = 0.99)

# 2. Run the reference blast with include_user_data = TRUE and specifiy the path to the user data [here: KELP_UserData/KELP1]
runRefBlast(path_to_otus = "KELP_otus.fasta", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR_withUser2", database_mode = "Ref99NR", use_force = T, num_threads = 6, include_user_data = T, path_to_user_data = "KELP_UserData/KELP2")

# 3. Making the prediction with your data included
makeFunctionalPrediction(path_to_otu_table = "KELP_otu_table.txt", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Kelp_Ref99NR_withUser2", database_mode = "Ref99NR", normalize_by_copy_number = T, min_identity_to_reference = 0.97, normalize_pathways = F, include_user_data = T, path_to_user_data = "KELP_UserData/KELP2")
```

## Step 4: Calculating (multi-)functional redundancy indices (experimental)

calculates phylogentic distributions of KEGG functions (High FRI -> high redundancy, low FRI -> function is less redundant and might get lost with community change)
```
# 1. Run the reference blast
runRefBlast(path_to_otus = "Water_otus.fna", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Water_Ref99NR", database_mode = "Ref99NR", use_force = T, num_threads = 6)

# 2. Calculating FRIs
calculateFunctionalRedundancy(path_to_otu_table = "Water_otu_table.txt", path_to_reference_data = "Tax4Fun2_ReferenceData_v1.1", path_to_temp_folder = "Water_Ref99NR", database_mode = "Ref99NR", min_identity_to_reference = 0.97)
```

Have Fun!
