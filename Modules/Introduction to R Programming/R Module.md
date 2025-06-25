
# 🧬 Bioinformatics with Linux and Bash Scripting  
**📅 Date:** 30th June – 11th July 2025  
**👨🏾‍🏫 Trainers:** Leonard N.

---

## 📦 Required Libraries

```r
library(openxlsx)
library(dplyr)
library(janitor)
library(stringi)
library(ggplot2)
library(lubridate)
```

---

## 📁 Reading the Data

```r
fread(input = "PATH_TO_FILE/monkepox.csv")
fread("../Module7_Basic_Introduction_to_R/monkeypox.csv")
data_01_mpox <- fread("../Module7_Basic_Introduction_to_R/monkeypox.csv")
```

---

## 🔍 Exploring the Data

```r
# 1. Check structure
str(data_01_mpox)
# OR
glimpse(data_01_mpox)

# 2. View column names
names(data_01_mpox)
# OR
colnames(data_01_mpox)

# 3. Clean column names
data_02_mpox <- clean_names(data_01_mpox)
names(data_01_mpox)
names(data_02_mpox)

# 4. View first few rows
data_03_mpox <- head(data_01_mpox, 10)

# 5. View last few rows
data_04_mpox <- tail(data_01_mpox)

# 6. Selecting columns
data_01_subset <- select(data_02_mpox, country, who_region, month_start, cases)
data_02_subset <- select(data_02_mpox, -month_lab, -deaths, -iso3)

# 7. Filtering rows
Nigeria <- filter(data_02_mpox, country == "Nigeria")
Rwanda <- filter(data_02_mpox, country == "Rwanda")

# 8. Count samples by country
table(data_02_mpox$country)

# 9. Remove unnecessary objects
rm(data_02_mpox, data_03_mpox, data_04_mpox, Nigeria, Rwanda, data_01_subset, data_02_subset)
```

---

## 🧪 Pipe Operations (dplyr)

```r
data_02_mpox <- data_01_mpox %>%
  clean_names()

data_02_mpox <- data_01_mpox %>%
  clean_names()  %>%
  select(country, who_region, month_start, month_lab, cases)

data_02_mpox <- data_01_mpox %>%
  clean_names()  %>%
  select(country, who_region, month_start, month_lab, cases) %>%
  filter(country == "Uganda")
```

---

### 🌍 WHO Regions Reference

- **EURO** – European Region  
- **AMRO** – Region of the Americas  
- **WPRO** – Western Pacific Region  
- **AFRO** – African Region  
- **SEARO** – South-East Asian Region  
- **EMRO** – Eastern Mediterranean Region

---

## ❓ Practice Questions

1. **Total Deaths per WHO Region. Which region has the highest deaths?**

<details><summary>👉 Don't cheat</summary>

```r
data_01_mpox %>%
  clean_names() %>%
  group_by(who_region) %>%
  summarise(total_deaths = sum(deaths, na.rm = TRUE)) %>%
  arrange(desc(total_deaths))
```

</details>

---

2. **Total Cases per Country. Which country has the highest number of cases?**

<details><summary>👉 Don't cheat</summary>

```r
data_01_mpox %>%
  clean_names() %>%
  group_by(country) %>%
  summarise(total_cases = sum(cases, na.rm = TRUE)) %>%
  arrange(desc(total_cases))
```

</details>

---

3. **Filter Countries with More Than 20 Cases**

<details><summary>👉 Don't cheat</summary>

```r
data_01_mpox %>%
  clean_names() %>%
  group_by(country) %>%
  summarise(total_cases = sum(cases)) %>%
  filter(total_cases > 20)
```

</details>

---

4. **For the country with the highest number of cases, which month records the highest cases? And how many cases?**

<details><summary>👉 Don't cheat</summary>

```r
data_01_mpox %>%
  clean_names() %>%
  filter(country == "Viet Nam") %>%
  group_by(month_lab) %>%
  summarise(monthly_cases = sum(cases)) %>%
  arrange(desc(monthly_cases))
```

</details>

---

## 📊 Visualization

```r
data_03_mpox <- data_01_mpox %>%
  clean_names() %>%
  group_by(who_region, month_lab) %>%
  summarise(total_cases = sum(cases)) %>%
  mutate(
    month_lab_full = paste0("01-", month_lab),
    month_date = dmy(month_lab_full)
  ) %>%
  arrange(month_date)

ggplot(data_03_mpox, aes(x = month_date, y = total_cases, color = who_region)) +
  geom_line(size = 1.2) +
  geom_point(size = 2) +
  labs(
    x = "Month",
    y = "Total Cases",
    color = "WHO Region"
  ) +
  scale_x_date(date_labels = "%b-%y", date_breaks = "1 month") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```


---

## 🧠 Explanation of Visualization Code

```r
data_03_mpox <- data_01_mpox %>%
  clean_names()
```
- Cleans and standardizes column names to snake_case for easier handling.

```r
  %>%
  group_by(who_region, month_lab)
```
- Groups the data by WHO region and the reporting month.

```r
  %>%
  summarise(total_cases = sum(cases))
```
- Aggregates the data to calculate total cases for each region and month.

```r
  %>%
  mutate(
    month_lab_full = paste0("01-", month_lab),
```
- Adds "01-" to the start of each month (e.g., "Jan-24" → "01-Jan-24") to convert it into a valid date string.

```r
    month_date = dmy(month_lab_full)
  )
```
- Converts the full string into an actual `Date` object using `lubridate::dmy()`. This allows proper chronological ordering and plotting.

```r
  %>%
  arrange(month_date)
```
- Sorts the data frame in chronological order by the new `month_date` column.

```r
ggplot(data_03_mpox, aes(x = month_date, y = total_cases, color = who_region)) +
  geom_line(size = 1.2) +
  geom_point(size = 2) +
```
- Uses `ggplot2` to plot lines and points showing the trend of monkeypox cases over time for each WHO region.

```r
  labs(
    x = "Month",
    y = "Total Cases",
    color = "WHO Region"
  ) +
```
- Adds labels for the x-axis, y-axis, and color legend.

```r
  scale_x_date(date_labels = "%b-%y", date_breaks = "1 month") +
```
- Formats the x-axis labels to show abbreviated month and year (e.g., "Jan-24").

```r
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```
- Applies a clean theme and rotates x-axis labels for better readability.
