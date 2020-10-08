## 1. BLASTing from a container

 \-\-
 | [TOC](README.md) |
 [Episode 2 \>\>](2.containers.md)
______


### Objectives

This episode will introduce containers for bioinformatics by showcasing a practical real use case.

Do not worry at this stage if the commands you are running are unclear, they will be discussed in detail in
[Episode 2](2.containers.md).


---
### Getting ready

It is assumed that all commands are executed in a Unix-like terminal.

All the materials for this workshop, including tutorials and scripts, are available on GitHub. Run the following command to get them in the current directory:

    git clone https://github.com/PawseySC/bio-workshop-18.git

In this first Episode we'll be running a [BLAST](https://blast.ncbi.nlm.nih.gov) (Basic Local Alignment Search Tool) example
with a container from [BioContainers](https://biocontainers.pro).

To begin, we'll move to the directory:

```
cd bio-workshop-18/episode1_blast
```

There is a script, `blast-demo.sh`, that will run all the commands if you want.  Otherwise, you
can follow along below.


---
### Setting up the container

To start we'll pull the BLAST container (this will take up to several minutes):

```
docker pull biocontainers/blast:v2.2.31_cv2
```

The output will look like:

```
v2.2.31_cv2: Pulling from biocontainers/blast
22dc81ace0ea: Pull complete
1a8b3c87dba3: Pull complete
...
Status: Downloaded newer image for biocontainers/blast:v2.2.31_cv2
```

That's it! BLAST is ready to run.


---
### A quick check

We can run a simple command to verify the container works:

```
docker run biocontainers/blast:v2.2.31_cv2 blastp -help

USAGE
  blastp [-h] [-help] [-import_search_strategy filename]
...
 -use_sw_tback
   Compute locally optimal Smith-Waterman alignments?

```


---
### Running BLAST on a sample dataset

Let's copy some data over to start working with:

```
cp -p ../data_files/P04156.fasta .
cp -p ../data_files/zebrafish.1.protein.faa.gz .
gunzip zebrafish.1.protein.faa.gz
```

This is a human prion FASTA sequence along with a reference database we'll blast against.  We need to prepare the zebrafish database with `makeblastdb`.
Do not worry about all of the flags we are using, we will explain them in detail in
[Episode 2](2.containers.md):

```
docker run --rm -v $(pwd):/data/ -w /data biocontainers/blast:v2.2.31_cv2 makeblastdb -in zebrafish.1.protein.faa -dbtype prot
```

We can now run BLAST and compare our data against the database:

```
docker run --rm -v $(pwd):/data/ -w /data biocontainers/blast:v2.2.31_cv2 blastp -query P04156.fasta -db zebrafish.1.protein.faa -out results.txt
```

The final results are stored in `results.txt`:

```
less results.txt
```

This will display:

```
BLASTP 2.2.31+


Reference: Stephen F. Altschul, Thomas L. Madden, Alejandro A.
Schaffer, Jinghui Zhang, Zheng Zhang, Webb Miller, and David J.
Lipman (1997), "Gapped BLAST and PSI-BLAST: a new generation of
protein database search programs", Nucleic Acids Res. 25:3389-3402.

...
                                                                     Score     E
Sequences producing significant alignments:                          (Bits)  Value

 XP_017207509.1 protein piccolo isoform X2 [Danio rerio]             43.9    2e-04
 XP_017207511.1 mucin-16 isoform X4 [Danio rerio]                    43.9    2e-04
 XP_021323434.1 protein piccolo isoform X5 [Danio rerio]             43.5    3e-04
 XP_017207510.1 protein piccolo isoform X3 [Danio rerio]             43.5    3e-04
 XP_021323433.1 protein piccolo isoform X1 [Danio rerio]             43.5    3e-04
 XP_009291733.1 protein piccolo isoform X1 [Danio rerio]             43.5    3e-04
 NP_001268391.1 chromodomain-helicase-DNA-binding protein 2 [Dan...  35.8    0.072
...
```

You can press the `space bar` to go forward with displaying the contents of the file,
and then `q` to exit and return to the shell prompt.


---
### Conclusion

We have installed BLAST and run it on a sample dataset with just three commands, corresponding to:
1. Pulling (download) the container;
2. Preparing the database using the container;
3. Running the alignment query using the container.


______
 \-\-
 | [TOC](README.md) |
 [Episode 2 \>\>](2.containers.md)