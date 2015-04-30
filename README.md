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

## PCR cloning technique

Following [Addgene's documentation on plasmid cloning by PCR](http://www.addgene.org/plasmid-protocols/pcr-cloning/). I'd like to clone into [pUC21-Notl](http://www.addgene.org/51659/).

### restriction enzymes

Cutters (from [Addgene analyzer](http://www.addgene.org/tools/analyze/0bfaeda1a36d2cdf330810bfdc8219ea1fc94172/default/)).
I simply pasted r2dm.fa into the analyzer, so this can be done again if the
link is down.

```text
==> dm6.r2dm.X_23220092-23223699.cutters <==
NdeI    65
EcoRV   310
SacI    528
NarI    697
MscI    1373
BglII   1601
KpnI    1743
NotI    1908
ApaI    2563
PstI    2982
DraI    3310

==> pUC21-Notl.cutters <==
StuI    29
XhoI    31
HindIII 67
PstI    83
SalI    85
BamHI   91
XmaI    96
SmaI    98
PacI    107
AscI    112
BglII   119
SacI    137
EcoRI   139
XbaI    184
```

Finding ones in the plasmid but not our target.

```sh
 % for cutter in $(cat pUC21-Notl.cutters | cut -f 1); do echo $cutter $(grep $cutter dm6.r2dm.X_23220092-23223699.cutters); done \
    | grep -v ' ' | parallel 'grep {} pUC21-Notl.cutters'
StuI    29
XhoI    31
HindIII 67
SalI    85
BamHI   91
XmaI    96
SmaI    98
PacI    107
AscI    112
EcoRI   139
XbaI    184
```

These are in order on [pUC21-NotI](http://www.addgene.org/51659/). Using BamHI
(5') and PacI (3') would:

* give a bit of space between the two restriction sites,
* put the inserted sequence in the middle of the multiple restriction locus,
* and would only drop XmaI and SmaI from the plasmid.

### primers for PCR cloning into pUC21-Notl
