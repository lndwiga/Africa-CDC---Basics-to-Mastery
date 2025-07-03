
# `cleanseq_v1.sh` — Training Guide

Version: **v1.0**  
Author: Leo  
Date: 2025-07-04  

This guide explains how the `cleanseq_v1.sh` Bash script works step by step. It performs quality control, trimming, and reporting on FASTQ files using **FastQC**, **fastp**, and **MultiQC**.

---

## Step 1: Script Header

```bash
###############################################
# Script: cleanseq_v1.sh
# Version: v1.0
# Description: Quality control and Trimming
# Author: Leo
# Date: 2025-07-04
###############################################
```

This is just a **comment block** that documents:
- Script name and version
- What it does
- Author and date

This is good practice for documentation, but does not affect execution.

---

## Step 2: Assigning Paths and Creating Directories

```bash
set -eu

RAW_DIR="./rawFastq"
TRIMMED_DIR="./trimmed"
FASTQC_RAW_DIR="./fastqc_raw"
FASTP_REPORTS="./fastp_reports"
MULTIQC_DIR="./multiqc"

mkdir -p "$TRIMMED_DIR" "$FASTQC_RAW_DIR" "$FASTP_REPORTS" "$MULTIQC_DIR"

START_TIME=$(date +%s)
echo "Pipeline started at: $(date)"

echo "-----------------------------------"
```

### Explanation:
- `set -eu`:  
  - `-e` exits the script on any error.  
  - `-u` exits if an undefined variable is used.
- Defines relative paths for input/output directories.
- `mkdir -p` creates these directories if they don't already exist.
- Logs the start time for runtime tracking.

---

## Step 3: FastQC on Raw Reads

```bash
echo "Running FastQC on raw FASTQ files..."

for R1 in "$RAW_DIR"/*_R1_001.fastq.gz
do
    BASENAME=$(basename "$R1")
    sample=$(echo "$BASENAME" | cut -d'_' -f2)
    R2="${R1/_R1_/_R2_}"

    echo "Sample: $sample"
    echo "    R1: $R1"
    echo "    R2: $R2"

    fastqc "$R1" "$R2" -o "$FASTQC_RAW_DIR"
done

echo "Finished FastQC on raw reads."
echo "-----------------------------------"
```

### Explanation:
#### Filename Parsing:
- `basename "$R1"` extracts just the file name (without path).
- `cut -d'_' -f2` extracts the sample ID (e.g., `PT0747`) from the file name.
- `R2="${R1/_R1_/_R2_}"` substitutes `_R1_` with `_R2_` to find the paired read.

#### FastQC:
- Runs **FastQC** on each paired-end read.
- Outputs reports to `$FASTQC_RAW_DIR`.

---

## Step 4: MultiQC on Raw FastQC Reports

```bash
echo "Running MultiQC on raw FastQC outputs..."
multiqc "$FASTQC_RAW_DIR"   -o "$MULTIQC_DIR/initial_multiqc"
echo "MultiQC report on raw reads generated: $MULTIQC_DIR/initial_multiqc"
echo "-----------------------------------"
```

### Explanation:
- Runs **MultiQC** on the FastQC results from Step 3.
- Aggregates individual FastQC reports into one summary in `$MULTIQC_DIR/initial_multiqc`.

---

## Step 5: Trimming with fastp

```bash
echo "Trimming reads with fastp..."

for R1 in "$RAW_DIR"/*_R1_001.fastq.gz
do

    BASENAME=$(basename "$R1")
    sample=$(echo "$BASENAME" | cut -d'_' -f2)
    R2="${R1/_R1_/_R2_}"

    echo "Trimming Sample: $sample"

    fastp -i "$R1" -I "$R2" \
        -o "$TRIMMED_DIR/${sample}_trimmed_R1.fastq.gz" \
        -O "$TRIMMED_DIR/${sample}_trimmed_R2.fastq.gz" \
        --thread 4 \
        --length_required 50 \
        --trim_tail1 1 \
        --trim_tail2 1 \
        --n_base_limit 0 \
        -h "$FASTP_REPORTS/${sample}_fastp.html" \
        -j "$FASTP_REPORTS/${sample}_fastp.json" \
done

echo "Trimming complete."
echo "-----------------------------------"

```

### Explanation:
#### fastp flags used:
- `-i` / `-I`: Input FASTQ files for R1 and R2.
- `-o` / `-O`: Output trimmed FASTQ files.
- `--thread 4`: Use 4 CPU threads - this depends on your computing power.
- `--length_required 50`: Discard reads shorter than 50 bases after trimming.
- `--trim_tail1 1` / `--trim_tail2 1`: Trim 1 base from the end of each read to remove low-quality tails.
- `--n_base_limit 0`: Discard reads containing any `N` base.
- `-h` / `-j`: Generate per-sample HTML and JSON reports.

fastp outputs go to:
- Trimmed FASTQs in `$TRIMMED_DIR`
- Reports in `$FASTP_REPORTS`

---

## Step 6: Final MultiQC on fastp Reports

```bash
echo "Running MultiQC on fastp reports..."
multiqc "$FASTP_REPORTS"   -o "$MULTIQC_DIR/final_multiqc"
echo "MultiQC report on trimmed reads generated: $MULTIQC_DIR/final_multiqc"

echo "-----------------------------------"
END_TIME=$(date +%s)
ELAPSED_TIME=$((END_TIME - START_TIME))
echo "Pipeline completed successfully at: $(date) and has taken $((ELAPSED_TIME)) seconds."
```

### Explanation:
- Runs **MultiQC** on the fastp reports to summarize trimming results and quality improvements.
- Outputs final summary to `$MULTIQC_DIR/final_multiqc`.
- Logs the total pipeline run time.

---

For any questions or improvements, refer to the script comments or the GitHub repository README.
