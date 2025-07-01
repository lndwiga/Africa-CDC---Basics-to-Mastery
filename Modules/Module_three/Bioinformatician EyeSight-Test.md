# 👁️ Bioinformatics Eye-Sight Test (Command Understanding)

This test is based on what was taught on Tuesday: `sed`, `echo`, `cat`, `head`, `tail`, `wc`, and regular expressions.

📝 For each command:
- Read what it's supposed to do
- Decide: Will it **Work ✅** or **Error ❌**?

### 1. What it's meant to do:
> Concatenate the contents of two files.

```bash
cat file1.txt file2.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Concatenates file1.txt and file2.txt if both exist.**  
🧠 *Explanation:* This is valid syntax. It displays the contents of file1.txt followed by file2.txt.
</details>

### 2. What it's meant to do:
> Write the phrase 'Hello World' into a new file called greetings.txt.

```bash
echo Hello World > greetings.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Writes 'Hello World' into greetings.txt.**  
🧠 *Explanation:* This is a valid use of echo and output redirection.
</details>

### 3. What it's meant to do:
> Concatenate using input redirection and an extra file argument.

```bash
cat < file1.txt file2.txt
```
<details>
<summary>🧪 Show Answer</summary>

**❌ Error — Unexpected second file.**  
🧠 *Explanation:* `cat < file1.txt` is valid, but the second filename is ignored or causes an error depending on the shell.
</details>

### 4. What it's meant to do:
> Display the first 5 lines of a file.

```bash
head -n5 file.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Displays the first 5 lines of file.txt.**  
🧠 *Explanation:* Correct use of head with `-n` flag.
</details>

### 5. What it's meant to do:
> Show the last 5 lines of a file.

```bash
tail file.txt -n 5
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Displays the last 5 lines of file.txt.**  
🧠 *Explanation:* GNU tail accepts options before or after the filename.
</details>

### 6. What it's meant to do:
> Count lines in a file, placing the file name first.

```bash
wc file.txt -l
```
<details>
<summary>🧪 Show Answer</summary>

**❌ Error — Incorrect flag order.**  
🧠 *Explanation:* `wc -l file.txt` is correct; flags must come before the filename.
</details>

### 7. What it's meant to do:
> Find lines that start with the codon 'ATG' in a file.

```bash
grep '^ATG' sequences.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Finds lines starting with 'ATG'.**  
🧠 *Explanation:* The regex `^ATG` matches any line starting with ATG.
</details>

### 8. What it's meant to do:
> Replace 'ATG' with 'TAA' in each line of a file.

```bash
sed s/ATG/TAA/ sequences.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Replaces first 'ATG' with 'TAA' in each line.**  
🧠 *Explanation:* Basic sed syntax without the delimiter escaping is valid in this form.
</details>

### 9. What it's meant to do:
> Replace digits with 'NUM' using regex.

```bash
sed 's/[0-9]+/NUM/' data.txt
```
<details>
<summary>🧪 Show Answer</summary>

**❌ Error — Invalid regex syntax for sed.**  
🧠 *Explanation:* Sed does not support `+` without extended regex (`-E` or `-r`).
</details>

### 10. What it's meant to do:
> Pipe 'Hello' into head to show 2 lines.

```bash
echo Hello | head -n 2
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Prints 'Hello' and then exits (less than 2 lines).**  
🧠 *Explanation:* Even though the input has one line, head works without error.
</details>

### 11. What it's meant to do:
> Use shorthand to get first 5 lines of a file.

```bash
head -5 file.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Displays the first 5 lines (shorthand for -n 5).**  
🧠 *Explanation:* `-5` is a valid shorthand for `-n 5` with GNU head.
</details>


### 13. What it's meant to do:
> Count words in a file using wc.

```bash
wc -w input.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Counts all words in input.txt.**  
🧠 *Explanation:* `-w` is the correct flag to count words using wc.
</details>

### 14. What it's meant to do:
> Replace all instances of 'GAT' with 'TAG' using global substitution.

```bash
sed 's/GAT/TAG/g' input.txt
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Replaces all GAT with TAG in each line.**  
🧠 *Explanation:* The `g` at the end of the sed command makes the replacement global for each line.
</details>

### 15. What it's meant to do:
> Display the last 3 lines of two files combined.

```bash
cat file1.txt file2.txt | tail -n 3
```
<details>
<summary>🧪 Show Answer</summary>

**✅ Works — Shows last 3 lines after combining both files.**  
🧠 *Explanation:* This pipes the output of cat into tail, which works as expected.
</details>

