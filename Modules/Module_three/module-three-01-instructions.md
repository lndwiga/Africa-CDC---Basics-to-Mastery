# 🧪 Module 3: Organizing FASTA and FASTQ Files from a Zipped Archive

This guide walks you through unzipping a downloaded file, organizing its contents into proper subdirectories, and verifying that everything has been moved correctly.

---

## 📦 Step 1: Move the ZIP file to your working location

Choose where you want to work (e.g., inside your `Documents` or `Bioinformatics` folder), then move the ZIP file there:

```bash
mv ~/Downloads/Module-3-20250701T091352Z-1-001.zip ~/Documents/
cd ~/Documents/
```

---

## 📂 Step 2: Unzip the downloaded file

Unzip the file using:

```bash
unzip Module-3-20250701T091352Z-1-001.zip
```

---

## 🧹 Step 3: Delete the original ZIP file to save space

```bash
rm Module-3-20250701T091352Z-1-001.zip
```

---

## ✏️ Step 4: Rename the extracted folder

The extracted folder might be named `Module-3`, rename it to `module_three`:

```bash
mv Module-3 module_three
```

---

## 🗂️ Step 5: Create two subfolders inside `module_three`

One folder will store `.fasta` files and the other `.fastq.gz` files:

```bash
mkdir -p module_three/{fasta_files,rawFastq}
```

---

## 🚚 Step 6: Move files into their respective folders

Move all `.fasta` files into `fasta_files`:

```bash
mv module_three/*.fasta module_three/fasta_files/
```

Move all `.fastq.gz` files into `rawFastq`:

```bash
mv module_three/*.fastq.gz module_three/rawFastq/
```

---

## ✅ Step 7: Check your folder structure

Run the following commands to confirm the files are in the correct places:

```bash
ls module_three/
ls module_three/fasta_files/
ls module_three/rawFastq/
```

You should see:
- All `.fasta` files under `fasta_files/`
- All `.fastq.gz` files under `rawFastq/`

---

📘 *Tip: Use `ls -lh` to view file sizes and `tab` to autocomplete file and folder names.*
