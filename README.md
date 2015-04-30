# R2Dm

## a target primed reverse transcription transposon

Reference sequence is 3607bp long.

```sh
 % oligotm $(samtools faidx r2dm.fa r2dm:0-3607 | tail -n+2 | perl -pe 's/\n//')                                                                                                                 ~/r2dm (master) oog
101.095285
```

### Putative primers.

Forward: 

```sh
samtools faidx r2dm.fa r2dm:1-27 | tail -1 >primer.5p
# TTGGGGATCATGGGGTATTTGAGAGCA
```

Reverse:

```sh
# chops off polyA tail
#samtools faidx r2dm.fa r2dm:$(echo "3607 - 49" | bc)-$(echo "(3607 - 49) + 26" | bc)
./revcomp $(samtools faidx r2dm.fa r2dm:3558-3584 | tail -1) >primer.3p
# ATCGCGGAGGTATGGAAATCTTCGAAA
```

Tm are within 5C. Oligtm is in `primer3`.

```sh
oligotm $(cat primer.5p)
# 72.337151
oligotm $(cat primer.3p)
# 70.646787
```

Secondary primers, somewhere in the middle?
