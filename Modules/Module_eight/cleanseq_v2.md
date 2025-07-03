
# `cleanseq_v2.sh` 

Version: **v2.0**  
Author: Leo  
Date: 2025-07-04  

---

##  Script Header and Setup

```bash
#!/bin/bash

set -eu
```

- `#!/bin/bash`: Shebang line specifies the script should run using Bash.
- `set -eu`:
  - `-e`: Exit if any command fails.
  - `-u`: Exit if any variable is used without being defined.

---

##  Directory Setup

- Defines the paths used for inputs and outputs.

```bash
RAW_DIR="./rawFastq"
TRIMMED_DIR="./trimmed"
FASTQC_RAW_DIR="./fastqc_raw"
FASTP_REPORTS="./fastp_reports"
MULTIQC_DIR="./multiqc"

mkdir -p "$TRIMMED_DIR" "$FASTQC_RAW_DIR" "$FASTP_REPORTS" "$MULTIQC_DIR"
```

---

##  Start Time Logging

```bash
START_TIME=$(date +%s)
echo "Pipeline started at: $(date)"
```

Records the time the pipeline starts for later calculating total runtime.

---

##  Function: `run_fastqc_raw`

```bash
run_fastqc_raw() {
    echo "Running FastQC on raw FASTQ files..."
    for R1 in "$RAW_DIR"/*_R1_001.fastq.gz
    do
        BASENAME=$(basename "$R1")
        sample=$(echo "$BASENAME" | cut -d'_' -f2)
        R2="${R1/_R1_/_R2_}"
        echo "Sample: $sample"
        fastqc "$R1" "$R2" -o "$FASTQC_RAW_DIR"
    done
    echo "Finished FastQC on raw reads."
}
```

- Loops through all R1 files.
- Extracts sample name- using the function basename - which as mentioned will remove the paths
- Identifies matching R2 file - by substituting
- Runs `fastqc` 

---

##  Function: `run_multiqc_raw`

```bash
run_multiqc_raw() {
    echo "Running MultiQC on raw FastQC outputs..."
    multiqc "$FASTQC_RAW_DIR" -o "$MULTIQC_DIR/initial_multiqc"
}
```

- Compiles all .html files from FastQC using MultiQC.
- multiqc will automatically create the output directory

---

##  Function: `run_fastp`

```bash
run_fastp() {
    echo "Trimming reads with fastp..."
    for R1 in "$RAW_DIR"/*_R1_001.fastq.gz
    do
        BASENAME=$(basename "$R1")
        sample=$(echo "$BASENAME" | cut -d'_' -f2)
        R2="${R1/_R1_/_R2_}"
        fastp \
            -i "$R1" -I "$R2" \
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
}
```


- Outputs cleaned FASTQs and HTML/JSON reports which can directly be used by multiqc

---

##  Function: `run_multiqc_fastp`

```bash
run_multiqc_fastp() {
    echo "Running MultiQC on fastp reports..."
    multiqc "$FASTP_REPORTS" -o "$MULTIQC_DIR/final_multiqc"
}
```

---

##  Interactive Mode

```bash
if [[ $# -eq 0 ]]

then

echo "No step specified — please select one or more steps to run (enter numbers separated by spaces):"
echo "1) fastqc_raw"
echo "2) multiqc_raw"
echo "3) fastp"
echo "4) multiqc_fastp"
echo "5) all"
read  -p "Selection: " selection
args=($selection) # splits the string into an array

else
    args=("$@")
fi
```

- If no arguments provided, the user will be prompted to enter one (e.g., `1 2 3 4 5`).
- `args=($selection)` splits user input into an array.
- `args=("$@")` grabs command-line arguments into an array.

---

##  Function Selector

```bash
for choice in "${args[@]}"
do
    case "$choice" in
        1) run_fastqc_raw;;
        2) run_multiqc_raw;;
        3) run_fastp;;
        4) run_multiqc_fastp;;
        5)
            run_fastqc_raw
            run_multiqc_raw
            run_fastp
            run_multiqc_fastp
            ;;
        *) 
            echo "Invalid selection: $choice"
            echo "Valid options: 1 2 3 4 5"
            ;;
    esac
done
```

- Iterates through each user-selected step.
- Matches `choice` to a function using `case`.
- If `5`, runs the full pipeline.
- `*` is a fallback for invalid entries.

---

##  End Time

```bash
END_TIME=$(date +%s)
ELAPSED_TIME=$((END_TIME - START_TIME))
echo "Pipeline completed at: $(date) and took $ELAPSED_TIME seconds."
```

Logs the end time and calculates total pipeline runtime.

---

