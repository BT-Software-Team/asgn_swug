# Short Tandem Repeats

Repetitive DNA sequences (short motifs repeated in tandem) sized by read-based analysis.

**Applicable genes:** CFTR, FMR1

## FMR1 CGG/AGG

The FMR1 gene contains a CGG repeat in the 5′ UTR of exon 1, central to fragile X syndrome and related disorders. Repeat length determines pathogenicity: unaffected is 5–44 repeats, full mutation is >200 (see [Gene-Specific Details](gene-specific-details.md#fmr1) for the intermediate/premutation ranges and sizing precision).

**Method** (3 steps):

1. **Read identification** — sample reads are aligned to anchor sequences upstream and downstream of the CGG STR region, accounting for alignment orientation, to identify reads usable for sizing.
2. **CGG sizing** — a histogram of repeat sizes (from step 1's reads) is built, and peaks are identified with an algorithm that accounts for coverage expectations as a function of allele size. Peak heights are normalized to the tallest peak (ratio 0–1); peaks below a 0.05 ratio are dropped. Reads within a ±3 repeat window of a peak are grouped together and output as an observed CGG allele.
3. **AGG interrupt detection** — for each CGG peak cluster, synthetic contigs are built and AGG interrupts are assessed by aligning the cluster's reads; interrupt location (if present) is determined via signal processing of the AGG peak.

## CFTR Poly-T/TG

Sizes the poly-T/TG region in CFTR Exon 10.

**Method:** Using fully spanning reads aligned to the CFTR Exon 10 amplicon, the region is iteratively sized in each read, and the frequency of each resulting size is summarized. The most frequent size is called as the first allele. If a second size group meets a frequency threshold relative to the most frequent group (0.5), it's called as the second allele; otherwise the call is homozygous for the first allele's size.

## What's next

- [SNVs/Indels](snvs-indels.md), [Copy Number Variants](copy-number-variants.md), [Structural Variants](structural-variants.md) — the other variant classes.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
