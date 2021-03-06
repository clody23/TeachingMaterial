* How many reads map in total?
```bash
samtools view ERR315494.bam | wc -l
	# 707,960 /2 = 353,980
```

* How many reads map to each chromosome?
```bash
samtools view ERR315494.bam | awk '{print $3}' | sort | uniq -c | sort -nr > chr.txt
less chr.txt
```

By now, you should have notice something special with the data. What is it?


* How many different mapping qualities are represented in the BAM file. What do they represent?
```bash
samtools view ERR315494.bam | awk '{print $5}' | sort | uniq -c | sort -nr
```

  TopHat2 (and Bowtie) does not provide as much information encoded in the mapping quality as other software (e.g. BWA). Still, those are usually the values reported:
  * `50`: unique mapping (Note: for some previous versions or mapper it is `255`)
  * `3`: the read maps to 2 locations in the target
  * `2`: the read maps to 3 locations
  * `1`: the read maps to 4-9 locations
  * `0`: the read maps to 10 or more locations
  
* How many different alignment flags can you find in the BAM file?
```bash
samtools view ERR315494.bam | awk '{print $2}' | sort | uniq -c | sort -nr
```

* Try to print the unique CIGAR strings for the first 300 reads. What is their meaning?
```bash
samtools view ERR315494.bam | head -n 300 | awk '{print $6}' | sort -u
```
  * `10M1230N75M`- 10 mapped positions, followed by a gap of 1230 nucleotides, followed by 75 mapped positions
  * `1M1304N84M` - 1 mapped position, followed by a gap of 1304 nucleotides, followed by 84 mapped positions 
  * etc.
  * `85M` - 85 mapped positions

Note that `M` indicates that the position could be mapped, but it does not specify if it was an exact match or a mismatch.

[BAM - Ex3] (https://github.com/Functional-Genomics/TeachingMaterial/blob/Cancer-Genomics-07-2015/doc/21.bam.md#exercise-3)
