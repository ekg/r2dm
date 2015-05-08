# R2Dm

## a target primed reverse transcription transposon

Reference sequence is 3607bp long.

```sh
 % oligotm $(samtools faidx r2dm.fa r2dm:0-3607 | tail -n+2 | perl -pe 's/\n//')                                                                                                                 ~/r2dm (master) oog
101.095285
```

## TOPO cloning primers

### Putative primers.

Forward: 

```sh
samtools faidx r2dm.fa r2dm:1-22 | tail -1 >primer.5p
# TTGGGGATCATGGGGTATTTGA
```

Reverse:

```sh
# design chops off polyA tail and one C
./revcomp $(samtools faidx r2dm.fa r2dm:3558-3585 | tail -1 | tail -c 23 | head -c 21) >primer.3p
# GATCGCGGAGGTATGGAAAT
```

Secondary primers, somewhere in the middle (1632 bp):

```sh
echo AAGGCATTTGATTCTCTATCACATG >primer.mid.5p
./revcomp $(cat ./primer.mid.5p ) >primer.mid.3p
```

Tm are within 5C. Oligtm is in `primer3`.

```sh
oligotm $(cat primer.5p)
# 65.334608
oligotm $(cat primer.3p)
# 62.018127
oligotm $(cat primer.mid.5p)
# 60.706056
```

Note that the two-step PCR would yield intermediate products of 1632 and 1953 bp.
