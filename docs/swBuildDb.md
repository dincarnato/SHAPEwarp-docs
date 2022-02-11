The ``swBuildDb`` module allows generating SHAPEwarp-compliant databases starting from RNA reactivity profiles. Reactivity profiles must be provided in the [__RNAframework__](https://rnaframework-docs.readthedocs.io/en/latest/rf-norm/)'s XML format<br/><br/>

# Usage
To list the required parameters, simply type:

```bash
$ swBuildDb --help
```

Parameter         | Type | Description
----------------: | :--: |:------------
__-o__ *or* __--output__ | string | Output database folder (Default: __sw_db/__)
__-ow__ *or* __--overwrite__ | | Overwrites output database folder if already existing
__--threads__ | int | Number of processors to use (Default: __1__)
__--blockSize__ | int | Size (in nt) of the blocks for shuffling (Default: __10__)
__--inBlockShuffle__ | int | Besides shuffling blocks, residues within each block will be shuffled as well
__--chunkSize__ | int | For each shuffling, only a chunk of this size will be extracted and used to build the shuffled database (Default: __1000__)<br/>__Note:__ this setting works fine for short queries (<1000 nt). If you plan to search longer queries, then it is advisable to increase the value of ``chunkSize``
__--shufflings__ | int | Number of shufflings to perform for each database entry (Default: __100__)
__--foldDb__ | | Provided SHAPE profiles are first used to calculate base-pairing probability profiles, that are then used to generate the database<br/>__Note:__ query searches must be performed with the ``foldQuery`` option of [``SHAPEwarp``](https://shapewarp-docs.readthedocs.io/en/latest/SHAPEwarp/)
 | | Probability profile database construction options
__--maxBPspan__ | int | Maximum allowed base-pairing distance (Default: __600__)
__--noLonelyPairs__ | | Disallows lonely pairs (helices of 1 bp)
__--noClosingGU__ | | Dissalows G:U wobbles at the end of helices
__--slope__ | float | Slope for SHAPE reactivities conversion into pseudo-free energy contributions (Default: __1.8__)
__--intercept__ | float | Intercept for SHAPE reactivities conversion into pseudo-free energy contributions (Default: __-0.6__)
__--temperature__ | float | Folding temperature (Default: __37.0__)
__--winSize__ | int | Size (in nt) of the sliding window for partition function calculation (Default: __800__)
__--offset__ | int | Offset (in nt) for partition function window sliding (Default: __200__)
__--winTrim__ | int | Number of bases to trim from both ends of partition function windows to avoid terminal biases (Default: __50__)

!!! note "Note"
    Shuffling SHAPE data in 10 nucleotide-long blocks (``blockSize`` = 10) yields more realistic profiles, as it preserves the relationship between neighboring residues. Although enabling ``inBlockShuffle`` might produce hits with lower E-values, hence increasing the chance to recover more *distal* matches, it also increases the chances of recovering more false positive matches.
    