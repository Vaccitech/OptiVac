#OptiVac - Designing String-of-beads with optimal spacers

Authors: Benjamin Schubert and Oliver Kohlbacher   
Date: June 2015   
Version: 1.1  
License: This software is under a three-clause BSD license  


Introduction:
-------------
The software is a novel approach to construct epitope-based string-of-beads
vaccines in optimal order and with sequence-optimized spacers of flexible length
such that the recovery of contained epitopes is maximized and immunogenicity of 
arising neo-epitopes is reduced. 

## Installing this fork

This fork is maintained to make it easier to install optivac.
Create a conda environment using `conda env create -f optivac_conda.yml`.
The key packages this installs are python=2.7, fred2, coincbc, mhcflurry, and mhcnuggets.

Next install the LKH algorithm, the code below installs it in the optivac conda env `bin` folder.
```
wget -P ~/miniconda3/envs/optivac/bin/ http://akira.ruc.dk/~keld/research/LKH/LKH-2.0.9.tgz
cd ~/miniconda3/envs/optivac/bin/
tar xvfz ~/miniconda3/envs/optivac/bin/LKH-2.0.9.tgz
cd LKH-2.0.9
make
cd ..
rm LKH-2.0.9.tgz
```
To check the LKH install run a test problem `path-to-LKH/LKH pr2392.par`.

Next clone the OptiVac repository, I have chosen to put it in the optivac
conda environment's `lib/python2.7/site-packages` folder.
```
cd ../lib/python2.7/site-packages
git clone https://github.com/Vaccitech/OptiVac.git
```
You must activate the optivac conda env and
when running OptiVac the LKH directory must be on $PATH.
Can be temporarily added with
```
conda activate optivac
PATH=$PATH:/home/dwells/miniconda3/envs/optivac/bin/LKH-2.0.9/
```
You must also refer to the optivac script directly when calling optivac.py.
For this reason it can be helpful to define the path to optivac.

```
optivac_path=/home/dwells/miniconda3/envs/optivac/lib/OptiVac

python ${optivac_path}/OptiVac.py \
 -i epitopes.txt \
 -a ${optivac_path}/example/allele_probabilities_europe.csv \
 -o assembled_epitopes.txt \
 --ips-solver cbc
``` 

Be aware that some sequence viewers will wrap long sequences across multiple lines.
OptiVac will interpret these as seperate beads.

Requirement:
-------------
Spacer Design uses the following software and libraries:

 1.  Python 2.7 (https://www.python.org/)
 2.  Fred2 (https://github.com/FRED-2/Fred2)
 3.  Cplex >= 12.5 (www.ilog.com) or other solveres supported by Pyomo (Version 1.1)
 4.  LKH TSP-Approximation >= 2.0.7 (http://www.akira.ruc.dk/~keld/research/LKH/)

Please make sure you have installed said software/libraries
and their dependencies.


Installation:
-------------
First install all required software and libraries. CPLEX/LKH should be globally executable
via command line. 


Usage:
-------------
```
usage: OptiVac.py [-h] -i INPUT -a ALLELES [-k MAX_LENGTH] [-al ALPHA]
                   [-be BETA] [-cp CLEAVAGE_PREDICTION]
                   [-ep EPITOPE_PREDICTION] [-thr THRESHOLD] -o OUTPUT
                   [-t THREADS]
```

The software is a novel approach to construct epitope-based string-of-beads
vaccines in optimal order and with sequence-optimized spacers of flexible
length such that the recovery of contained epitopes is maximized and
immunogenicity of arising neo-epitopes is reduced.
```
Arguments:
  -h, --help            show this help message and exit
  -i INPUT, --input INPUT
                        File containing epitopes (one peptide per line)
  -a ALLELES, --alleles ALLELES
                        Specifies file containing HLA alleles with
                        corresponding HLA probabilities (one HLA per line)
  -k MAX_LENGTH, --max_length MAX_LENGTH
                        Specifies the max. length of the spacers (default 6)
  -al ALPHA, --alpha ALPHA
                        Specifies the first-order preference of the user in
                        the model [0,1] (default 0.99)
  -be BETA, --beta BETA
                        Specifies the second-order preference of the user in
                        the model [0,1] (default 0).
  -cp CLEAVAGE_PREDICTION, --cleavage_prediction CLEAVAGE_PREDICTION
                        Specifies the used cleavage prediction method (default
                        PCM) [available: PCM, ProteaSMMConsecutive, ProteaSMMImmuno]
  -ep EPITOPE_PREDICTION, --epitope_prediction EPITOPE_PREDICTION
                        Specifies the used epitope prediction method (default
                        Syfpeithi) [available: Syfpeithi, BIMAS, SMM, SMMPMBEC]
  -thr THRESHOLD, --threshold THRESHOLD
                        Specifies epitope prediction threshold for SYFPEITHI
                        (default 20).
  -o OUTPUT, --output OUTPUT
                        Specifies the output file.
  -t THREADS, --threads THREADS
                        Specifies number of threads. If not specified all
                        available logical cpus are used.
  --seed
                        Seed for random ordering of string-of-beads polypeptide.
                        Default 1
```
Example
------
```
python OptiVac.py -i example/epitope_list.csv -a example/allele_probabilities_europe.csv -o example/out.txt
```

To create a randomly ordered string-of-beads polypeptide with optimal spacer
sequences. Here we use the cbc IPS solver for an optimal solution:

```bash
python OptiVac.py \
    -i example/epitope_list.csv \
    -a example/allele_probabilities_europe.csv \
    -o example/out.txt \
    --ips-solver cbc \
    --tsp-solution optimal \
    --random-order
```
```
Generating a randomly ordered polypeptide

Resulting String-of-Beads:  ALGENSEVV-MW-YLAHAIHQV-MWYWNY-KIPEQSVLL-MNW-RIIGMRTQL
```

Citation
-------

Please cite:

[Schubert, B., & Kohlbacher, O. (2016). Designing string-of-beads vaccines with optimal spacers. Genome medicine, 8(1), 1.](http://genomemedicine.biomedcentral.com/articles/10.1186/s13073-016-0263-6)


Contacts:
---------

Benjamin Schubert   
schubert@informatik.uni-tuebingen.de   
University of Tübingen, Applied Bioinformatics,   
Center for Bioinformatics, Quantitative Biology Center,   
and Dept. of Computer Science,   
Sand 14, 72076 Tübingen, Germany
