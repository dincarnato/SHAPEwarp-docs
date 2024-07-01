Here, we provide a simple application case to demonstrate how to use __SHAPEwarp__:<br/>
<br/>
__1.__ Download and decompress the sample [SHAPE reactivity profiles](https://www.incarnatolab.com/downloads/datasets/SHAPEwarp_Morandi_2022/XML.tar.gz) and the sample [query files](https://www.incarnatolab.com/downloads/datasets/SHAPEwarp_Morandi_2022/Queries.tar.gz):

```bash
# Download & decompress the SHAPE reactivity profiles
$ wget https://www.incarnatolab.com/downloads/datasets/SHAPEwarp_Morandi_2022/XML.tar.gz
$ tar -xzvf XML.tar.gz 

# Download & decompress the queries
$ wget https://www.incarnatolab.com/downloads/datasets/SHAPEwarp_Morandi_2022/Queries.tar.gz
$ tar -xzvf Queries.tar.gz 
```
<br/>
__2.__ Prepare the SHAPEwarp database (and shuffled database) for *B. subtilis* ribosomal RNAs:

```bash
$ SHAPEwarp --database XML/Bsubtilis/ --dump-db Bsubtilis_rRNA.db --dump-shuffled-db Bsubtilis_rRNA.shuffled.db --db-shufflings 10
```
<br/>

__3.__ Search the *E. coli* queries (200 nt-long, tiling the 16S rRNA) against the *B. subtilis* database:

```bash
$ SHAPEwarp --output bs_out/ --query Queries/Ecoli_16S_invivo.txt --database Bsubtilis_rRNA.db --shuffled-db Bsubtilis_rRNA.shuffled.db --report-alignment fasta --report-reactivity --match-kmer-gc-content --threads <n>
```
Take care to replace the argument of the ``--threads`` parameter with the number of processors on your machine.<br/><br/>

__4.__ Visualize the output:

```bash
$ ls -l bs_out/

drwxrwxr-x  2 danny danny 4096 Jun 30 16:57 alignments/
-rw-rw-r--  1 danny danny 1048 Jun 30 16:57 params.out
drwxrwxr-x  2 danny danny 4096 Jun 30 16:57 reactivities/
-rw-rw-r--  1 danny danny 1833 Jun 30 16:57 results.out
```

The file "*params.out*" stores the parameters used for the database search:

```text
db-shufflings = 100
db-block-size = 10
db-in-block-shuffle = false
max-reactivity = 1.0
max-align-overlap = 0.5
null-hsgs = 10000
inclusion-evalue = 0.01
report-evalue = 0.1
report-alignment = "Fasta"
report-reactivity = true
min-kmers = 2
max-kmer-dist = 30
kmer-len = 15
kmer-offset = 1
match-kmer-gc-content = true
match-kmer-seq = false
kmer-min-complexity = 0.30000001192092896
kmer-max-match-every-nt = 200
align-match-score = "-0.5,2"
align-mismatch-score = "-6,-0.5"
align-gap-open-penalty = -14.0
align-gap-ext-penalty = -5.0
align-max-drop-off-rate = 0.800000011920929
align-max-drop-off-bases = 8
align-len-tolerance = 0.10000000149011612
align-score-seq = false
align-seq-match-score = 0.5
align-seq-mismatch-score = -2.0
eval-align-fold = false
shufflings = 100
block-size = 3
in-block-shuffle = false
min-bp-support = 0.75
ribosum-scoring = false
slope = 1.7999999523162842
intercept = -0.6000000238418579
max-bp-span = 600
no-lonely-pairs = false
no-closing-gu = false
temperature = 37.0
```

The file "*results.out*" stores the results of the database search:


```text
Query     DB entry  Qstart  Qend   Dstart  Dend   Qseed    Dseed      Score    P-value    E-value
16S_600   16S       0       199    608     807    60-114   668-722    190.53   4.47e-11   6.39e-09  !
16S_500   16S       0       196    508     704    160-186  668-694    203.47   2.30e-10   3.06e-08  !
16S_200   16S       9       199    216     406    120-183  327-390    187.13   3.13e-10   3.41e-08  !
16S_700   16S       0       161    708     869    65-129   773-837    159.63   3.19e-09   5.07e-07  !
16S_1300  16S       0       180    1308    1488   10-112   1318-1420  199.02   3.94e-09   6.19e-07  !
16S_900   16S       7       170    916     1079   16-100   925-1009   173.76   2.23e-08   1.16e-06  !
16S_400   16S       19      199    427     607    97-144   505-552    159.77   1.09e-07   1.42e-05  !
16S_300   16S       4       194    311     501    20-83    327-390    146.14   2.70e-07   3.40e-05  !
16S_800   16S       0       199    808     1008   73-199   882-1008   137.66   5.85e-07   6.67e-05  !
16S_last  16S       0       138    1350    1488   0-70     1350-1420  128.85   6.24e-07   7.30e-05  !
16S_1100  16S       49      199    1157    1307   68-96    1176-1204  132.35   9.51e-07   9.70e-05  !
16S_0     16S       10      161    17      168    85-102   92-109     87.73    6.57e-05   4.66e-03  !
16S_1200  16S       0       199    1208    1407   110-199  1318-1407  131.31   3.52e-05   5.39e-03  !
16S_1100  16S       40      194    55      209    170-185  185-200    80.03    4.52e-04   0.05      ?
16S_400   23S       25      155    1252    1382   74-92    1301-1319  87.09    3.55e-04   0.05      ?
16S_800   23S       47      164    1608    1725   64-81    1625-1642  80.20    4.64e-04   0.05      ?
16S_800   16S       46      161    30      145    56-71    40-55      76.82    6.86e-04   0.08      ?
```
Significant matches at the specified E-value threshold (__0.01__ by default) are marked with a __!__, while non-significant matches are marked with a __?__. The "*alignments/*" and "*reactivities/*" folders respectively contain the FASTA-formatted alignments of the matched RNA sequences and the aligned SHAPE reactivity profiles in JSON format.