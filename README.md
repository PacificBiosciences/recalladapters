<h1 align="center">Recall Adapters</h1>
<p align="center">A tool to recall adapters for PacBio data</p>

***

## Availability
Latest version can be installed via bioconda package `recalladapters`.

Please refer to our [official pbbioconda page](https://github.com/PacificBiosciences/pbbioconda)
for information on Installation, Support, License, Copyright, and Disclaimer.

## Motivation
`recalladapters` provides a way to change the adapter calls in a PacBio BAM file. It accepts a `subreads.bam` and a `scraps.bam`, and emits a `subreads.bam` and `scraps.bam`. Basecalls may be moved between files (e.g. as new adapters are found and moved to scraps), and BAM records may be split or concatenated (e.g. two subreads will be concatenated if the adapter between them is ignored).

See our File Format Primer for more information on the relationship between PacBio SMRTBells, subreads and adapters:
https://pacbiofileformats.readthedocs.io/en/5.1/Primer.html

This tool allows users to modify the adapter sequences used in adapter finding, for instance if an incorrect set of adapters was specified on-instrument. It can also be used to modify adapter finding parameters, for instance to increase the importance of the flank alignment when filtering out false positive adapter calls.

## Usage
### Helptext
```$ recalladapters -h
Usage: -s subreadset.xml -o outputPrefix [options]

Version:9.0.0.da2e8977c

recalladapters operates on BAM files in one convention (subreads+scraps or
hqregions+scraps), allows reprocessing adapter calls then outputs the resulting
BAM files as subreads plus scraps.

"Scraps" BAM files are always required to reconstitute the ZMW reads internally.
Conversely, "scraps" BAM files will be output.

ZMW reads are not allowed as input, due to the missing HQ-region annotations!

Input read convention is determined from the READTYPE annotation in the @RG::DS
tags of the input BAM files.A subreadset *must* be used as input instead of the
individual BAM files.

Options:
  -h, --help            show this help message and exit
  --version             show program's version number and exit

  Mandatory parameters:
    -o STRING           Prefix of output filenames
    -s STRING, --subreadset=STRING
                        Input subreadset.xml

  Optional parameters:
    -j INT, --nProcs=INT
                        Number of threads for parallel ZMW processing
    -b INT              Number of threads for parallel BAM compression, can only
                        be set when not generating pbindex inline with --inlinePbi
    --inlinePbi         Generate pbindex inline with BAM writing
    --silent            No progress output.

  Adapter finding parameters:
    --disableAdapterFinding
    --adapters=adapterSequences.fasta
    --globalAlnFlanking
    --flankLength=INT
    --minSoftAccuracy=FLOAT
    --minHardAccuracy=FLOAT
    --minFlankingScore=FLOAT
    --disableAdapterCorrection
    --adpqc

  Fine tuning:
    --minAdapterScore=int
                        Minimal score for an adapter
    --minSubLength=INT  Minimal subread length. Default: 50
    --minSnr=FLOAT      Minimal SNR across channels. Default: 3.75

  White list:
    --whitelistZmwNum=RANGES
                        Only process given ZMW NUMBERs

Example: recalladapters -s in.subreadset.xml -o out --adapters adapters.fasta
```

### Examples
Provide the `subreadset.xml` as input, together with an output
`projectName` and the FASTA file containing the new adapter(s):

    recalladapters -o projectName --adapters newAdapters.fasta -s input.subreadset.xml

Adapter finding can put more emphasis on flank size if the adapter sequence is very similar to a sequence found in the rest of the sample:

    $ recalladapters --flankLength=500 --minFlankingScore=200 --adapter adapters.fasta -o movieName.newAdapters -s input.subreadset.xml

## Full Changelog
 * **9.0.0.da2e8977c**:
   * New CLI UX
   * Add `--minSnr`
 * 7.1.0.425709f:
   * Initial release

## DISCLAIMER
THIS WEBSITE AND CONTENT AND ALL SITE-RELATED SERVICES, INCLUDING ANY DATA, ARE PROVIDED "AS IS," WITH ALL FAULTS, WITH NO REPRESENTATIONS OR WARRANTIES OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, ANY WARRANTIES OF MERCHANTABILITY, SATISFACTORY QUALITY, NON-INFRINGEMENT OR FITNESS FOR A PARTICULAR PURPOSE. YOU ASSUME TOTAL RESPONSIBILITY AND RISK FOR YOUR USE OF THIS SITE, ALL SITE-RELATED SERVICES, AND ANY THIRD PARTY WEBSITES OR APPLICATIONS. NO ORAL OR WRITTEN INFORMATION OR ADVICE SHALL CREATE A WARRANTY OF ANY KIND. ANY REFERENCES TO SPECIFIC PRODUCTS OR SERVICES ON THE WEBSITES DO NOT CONSTITUTE OR IMPLY A RECOMMENDATION OR ENDORSEMENT BY PACIFIC BIOSCIENCES.
