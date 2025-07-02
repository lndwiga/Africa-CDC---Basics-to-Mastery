# 🧪 Module 03 Assignment: Text Viewing, Navigation & Editing in Linux

Welcome to the Module 03 assignment! This challenge will test your ability to work with biological data using basic Linux commands for text viewing, file manipulation, and sequence extraction.

---

## ✅ Practice Challenge Questions & Solutions

### 🔹 File Viewing & Navigation

1) **Create a directory called `fasta_practice` and move your `16s_rRNA.fasta`, `Bacillus.fasta` & `Pseudomonas.fasta` files into it.**

<details>
  <summary>Show Answer</summary>

  ```bash
  mkdir fasta_practice
  mv *fasta fasta_practice/
  ```
</details>

2) **Use `head` to view the first 10 lines of the `16s_rRNA.fasta` file.**

<details>
  <summary>Show Answer</summary>

  ```bash
  head 16s_rRNA.fasta
  ```
</details>

3) **Use `tail` to view the last 15 lines of the `16s_rRNA.fasta`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  tail -n 15 16s_rRNA.fasta
  ```
</details>

4) **Use `less` to scroll through and search for the word `Vibrionaceae` in `16s_rRNA.fasta`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  less 16s_rRNA.fasta
  ```
  Then type:
  ```
  /Vibrionaceae
  ```
</details>

5) **Use `cat` to combine `Bacillus.fasta` & `Pseudomonas.fasta` and save as `bacillus_pseudomonas.fasta`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  cat Bacillus.fasta Pseudomonas.fasta > bacillus_pseudomonas.fasta
  ```
</details>

---

### 🔹 Sequence Header Analysis

1) **Use `grep` to extract all headers (lines beginning with `>`) in the `16s_rRNA.fasta` into `headers.txt`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  grep "^>" 16s_rRNA.fasta > headers.txt
  ```
</details>

2) **Count how many headers belong to the class `Bacilli` (case-insensitive) in `headers.txt`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  grep -i "Bacilli" headers.txt | wc -l
  ```
</details>

3) **Count how many headers contain the word `unidentified`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  grep -i "unidentified" headers.txt | wc -l
  ```
</details>

4) **Extract the headers of `16s_rRNA.fasta`, then save headers 15 to 21 into a new file called `subset_headers.fasta`.**

<details>
  <summary>Show Answer</summary>

  ```bash
  grep ">" 16s_rRNA.fasta | head -n 21 | tail -n 7 > subset_headers.fasta
  ```
</details>

---

### 🔹 Redirection & Editing



### 1️⃣ Edit `headers.txt` using `nano`, change one header, and save.

<details>
  <summary>Show Answer</summary>

  ```bash
  nano headers.txt
  ```
  (Edit one line manually and press `Ctrl+O`, `Enter` to save, and `Ctrl+X` to exit)
</details>

---

### 2️⃣ Use `>` to write the first 20 lines of `headers.txt` into `top20.txt`.

<details>
  <summary>Show Answer</summary>

  ```bash
  head -n 20 headers.txt > top20.txt
  ```
</details>

---

### 3️⃣ Use `>>` to append the last 5 lines of `headers.txt` to `top20.txt`.

<details>
  <summary>Show Answer</summary>

  ```bash
  tail -n 5 headers.txt >> top20.txt
  ```
</details>

---

### 4️⃣ Use `tee` to display and save all headers (lines starting with `>`) to `visible_headers.txt`.

<details>
  <summary>Show Answer</summary>

  ```bash
  grep "^>" headers.txt | tee visible_headers.txt
  ```
</details>

---
