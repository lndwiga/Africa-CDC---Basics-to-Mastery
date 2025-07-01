
# 🧪 Module 04 Practice Questions: Linux Text Processing

📁 Dataset: `monkeypox.csv`

This assessment tests your ability to use core Linux text-processing tools:  
**egrep, grep, cut, sort, wc, awk, uniq, sed**, and **command piping**.



### 1️⃣ `egrep` – Search with Multiple Patterns  
**Task:**  
Find all rows that contain either **"AFRO"** or **"EURO"** in the WHO region column.

**Hint:** Use the `egrep` command.

<details>
<summary>💡 Show Answer</summary>

```bash
egrep 'AFRO|EURO' monkeypox.csv
```

</details>

---

### 2️⃣ `grep` – Filter by Month  
**Task:**  
Show all lines where the **lab-confirmed month** is **"Jan-24"**.

**Hint:** Use the `grep` command.

<details>
<summary>💡 Show Answer</summary>

```bash
grep 'Jan-24' monkeypox.csv
```

</details>

---

### 3️⃣ `cut` – Extract Columns  
**Task:**  
Extract only the **Country** and **Cases** columns.

**Hint:** Use the `cut` command.

<details>
<summary>💡 Show Answer</summary>

```bash
cut -d',' -f1,6 monkeypox.csv
```

</details>

---

### 4️⃣ `sort` – Organize by WHO Region  
**Task:**  
Sort the dataset alphabetically by the **WHO region** (column 3).

**Hint:** Use the `sort` command.

<details>
<summary>💡 Show Answer</summary>

```bash
sort -t',' -k3,3 monkeypox.csv
```

</details>

---

### 5️⃣ `wc` – Count Rows  
**Task:**  
Count the number of **data rows** in the file (excluding the header).

**Hint:** Use the `wc` command.

<details>
<summary>💡 Show Answer</summary>

```bash
tail -n +2 monkeypox.csv | wc -l
```

</details>

---

### 6️⃣ `awk` – Conditional Filtering  
**Task:**  
Using `awk`, print all **countries** where **Cases > 0** and **Region is AFRO**.

**Hint:** Use the `awk` command with a field separator and conditional expressions (`$3`, `$6`).

<details>
<summary>💡 Show Answer</summary>

```bash
awk -F',' 'NR > 1 && $3 == "AFRO" && $6 > 0 {print $1}' monkeypox.csv
```

</details>

---

### 7️⃣ `cut` + `sort` + `uniq` – Unique Country List  
**Task:**  
Create a list of **unique country names**, sorted alphabetically.

**Hint:** Use the `cut`, `sort`, and `uniq` commands together in a pipeline.

<details>
<summary>💡 Show Answer</summary>

```bash
cut -d',' -f1 monkeypox.csv | tail -n +2 | sort | uniq
```

</details>

---

### 8️⃣ 💥 Bonus Pipeline – Summary by Region  
**Task:**  
Write a command that:
- Removes the header,
- Extracts the **WHO region**,
- Counts how many times each region appears.

**Hint:** Use a pipeline combining `tail`, `cut`, `sort`, and `uniq -c`.

<details>
<summary>💡 Show Answer</summary>

```bash
tail -n +2 monkeypox.csv | cut -d',' -f3 | sort | uniq -c
```

</details>

---

### 9️⃣ `sed` – Text Replacement  
**Task:**  
Replace all occurrences of **"AFRO"** with **"AFRICA"** in the dataset and show only the first 5 modified lines.

**Hint:** Use the `sed` command and combine it with `head`.

<details>
<summary>💡 Show Answer</summary>

```bash
sed 's/AFRO/AFRICA/g' monkeypox.csv | head -n 5
```

</details>

---

### 📤 Submission Instructions:
- Submit your answers as a `.md` or `.txt` file.
- Paste the command you used and the resulting output under each task.
- Include screenshots if requested by the instructor.
