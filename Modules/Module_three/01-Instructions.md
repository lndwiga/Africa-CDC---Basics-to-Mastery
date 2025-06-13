# 🧪 Module 3: Organizing FASTA and FASTQ Files

This guide will help you organize your downloaded `.fasta` and `.fastq.gz` files into separate folders using basic Linux commands.

---

## 📁 Step-by-Step Instructions

### 1. Create a working directory and move into it
```bash
mkdir module_three
cd module_three
```

---

### 2. Create two subfolders: one for FASTA files, one for FASTQ files
```bash
mkdir rawFastq fasta_files
```

---

### 3. Move all FASTA files into the `fasta_files` folder
```bash
mv ../*.fasta fasta_files/
```

---

### 4. Move all FASTQ files into the `rawFastq` folder
```bash
mv ../*.fastq.gz rawFastq/
```

---

## ✅ Check your folder structure
Use the following commands to verify everything is in the right place:

```bash
pwd
ls fasta_files/
ls rawFastq/
```

---

📘 *Tip: Use `ls -lh` to view file sizes, and `tab` for autocomplete.*
