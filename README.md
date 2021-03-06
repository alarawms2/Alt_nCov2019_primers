# Alternative primers for the ARTIC Network's nCov2019 multiplex PCR

![GIF](https://raw.githubusercontent.com/ItokawaK/Alt_nCov2019_primers/master/nCoV_coverage.gif)

Here we provide some resources regarding the alternative primers for the [ARTIC Network's mupltiplex PCR for SARS-CoV-2](https://github.com/artic-network/artic-ncov2019).

See below article for detail of the modifications:

[**A proposal of alternative primers for the ARTIC Network's multiplex PCR to improve coverage of SARS-CoV-2 genome sequencing**](https://www.biorxiv.org/content/10.1101/2020.03.10.985150v3) (ver. 3)

 Kentaro Itokawa, Tsuyoshi Sekizuka, Masanori Hashino, Rina Tanaka, Makoto Kuroda

doi: https://doi.org/10.1101/2020.03.10.985150

https://www.biorxiv.org/content/10.1101/2020.03.10.985150v3

### Tools
-------
 Those tools are still experimental and have no warranty to work properly.

- tools/plot_depth.py

   Generates depth plots in sigle PDF file from multiple BAM files to briefly check coverages.

   Requires:

     - samtools in $PATH
     - python3
     - matplotlib
     - numpy
     - pandas

  ```
  Usage: plot_depth.py [-h] [-i [BAMS [BAMS ...]]] [-p PRIMER] [-o OUT]

      -i [BAMS [BAMS ...]], --bams [BAMS [BAMS ...]]
                      Paths for input BAMs
      -p PRIMER, --primer PRIMER
                      primer_region in BED format
      -o OUT, --out OUT     Output PDF file name
      -r REF_FA, --ref_fa REF_FA
                      Reference fasta file [optional]
      -t THREADS, --threads THREADS
                          Num tasks to process concurrently [optional]
  ```
    If `-r` option is set, mismatches found on >80% reads (parsed from *mpileup*'s output) will be highlighted. This, however, takes additional time.

    Output image

![plot_depth_out_image_2](https://user-images.githubusercontent.com/38896687/78553244-e8142e00-7843-11ea-8c40-e27a0f2066a6.png)

- tools/trim_primers/trim_primer_parts.py

    Trim primer parts of paired-end reads obtained from illumina machines.

    This program works as:

 1. Looks alignments of mapped fragments from paired reads.
 1. Finds fragment ends *contained* in a primer region.
 1. Trims sequence overlapping the primer region.

    Note that this program work conservatively, it throw away all soft masked parts and reads not mapped in proper pair (non 0x2 bit flag).

![trimming_image](https://user-images.githubusercontent.com/38896687/78016726-2a41f900-7386-11ea-8dfd-a3960ee3283f.PNG)

 Send the bwa mem output or name sorted SAM to this script *via* PIPE.
 ```
   Usage:

      bwa mem nCov_bwadb untrimmed_R1.fq untrimmed_R2.fq |
         trim_primer_parts.py [--gziped] primer.bed trimmed_R1 trimmed_R2
  ```

  Then remap the reads.
  ```
  bwa mem nCov_bwadb trimmed_R1.fq trimmed_R2.fq > ...

  ```

![trimming_image](https://user-images.githubusercontent.com/38896687/77902160-b89d7880-72bb-11ea-9ef6-9beaa33310bb.png)
