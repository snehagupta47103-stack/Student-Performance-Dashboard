# Student Performance Dashboard – Academic & Behavioral Insights

An interactive **Power BI** dashboard that analyzes student academic performance, attendance, and behavior across classes, sections, subjects, and terms.

> Tool: Power BI Desktop

---

## Objective

Build an interactive dashboard that helps teachers and administrators:

- Track academic performance across subjects, classes, and terms
- Monitor attendance
- Understand behavior patterns
- Drill into an individual student's profile

---

## Dataset

Four CSV files were used:

| File | Columns |
|---|---|
| `Students.csv` | StudentID, Name, Gender, Class, Section |
| `Scores.csv` | StudentID, Subject, ExamType, Score, MaxScore, Term |
| `Attendance.csv` | StudentID, Date, Status (Present/Absent), Reason |
| `Behavior.csv` | StudentID, Date, BehaviorType, Notes |

---

## Data Modeling & Cleaning

**Power Query cleaning**

- Promoted headers and set correct data types (Text, Number, Date)
- Trimmed extra spaces in text columns
- Converted `StudentID` to uppercase and trimmed it in all tables so keys match
- Removed duplicate students from the `Students` table
- Replaced null values in `Reason` with `Not Applicable`

**Relationships (Star schema)**

`Students` is the dimension table and connects to the other tables through `StudentID`:

- Students (1) → Scores (many)
- Students (1) → Attendance (many)
- Students (1) → Behavior (many)

All relationships are single-direction, one-to-many.

---

## DAX Measures

```DAX
% Score = DIVIDE(SUM('Scores csv'[Score]), SUM('Scores csv'[MaxScore]))

Average Score per Subject = AVERAGE('Scores csv'[Score])

Attendance % =
DIVIDE(
    CALCULATE(COUNTROWS('Attendance csv'), 'Attendance csv'[Status] = "Present"),
    COUNTROWS('Attendance csv')
)

Behavior Count per Type = COUNTROWS('Behavior csv')

Performance Category =
SWITCH(
    TRUE(),
    [% Score] >= 0.8, "High",
    [% Score] >= 0.5, "Medium",
    "Low"
)
```

---

## Dashboard Pages

### 1. Main Dashboard (Page 1)

- **Cards:** Total Students, Average Attendance, Average Score
- **Bar chart:** Average score by Subject and Class
- **Line chart:** Performance trend by Term
- **Donut chart:** Behavior types distribution
- **Table:** Student-wise scores with conditional formatting
  - Green for scores above 80%
  - Red for scores below 40%
- **Key Insights** text panel and Red & White branding

### 2. Student Profile (Drillthrough page)

Right-click any student on the main page and choose **Drill through → Student Profile** to see:

- Student name
- Student's % Score and Attendance %
- Subject-wise score chart
- Behavior log (Date, BehaviorType, Notes)

### 3. Tooltip page

A custom report-page tooltip showing a mini subject-wise score chart when hovering over the line chart.

---

## Interactivity

- **Slicers:** Class, Section, Subject, Term
- **Drillthrough:** Individual student profile
- **Tooltips:** Mini chart on hover
- **Bookmarks and buttons:** Switch between **Academic** and **Behavioral** views

---

## Key Insights

- Average score is **49.87%**, with most students in the Low/Medium category.
- Attendance is healthy at **90.05%**, but scores are still low.
- Performance is nearly the same across all subjects and terms.
- Behavior is evenly split across 5 types (~20% each).

---

## How to Open

1. Download or clone this repository.
2. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free).
3. Open `Student Performance Dashboard.pbix`.
4. If prompted, update the CSV file paths under **Home → Transform data → Data source settings**.

> Buttons in Power BI Desktop work with **Ctrl + Click**.

---

## Repository Structure

```
├── Student Performance Dashboard.pbix
├── data/
│   ├── Students.csv
│   ├── Scores.csv
│   ├── Attendance.csv
│   └── Behavior.csv
├── screenshots/
│   └── dashboard.png
└── README.md
```

---

## Dashboard Preview

<img width="1202" height="677" alt="Final Dashboard" src="https://github.com/user-attachments/assets/bbcc80c5-4076-4ffc-9616-9ec2a6210fca" />

---

## Tools Used

- Power BI Desktop
- Power Query (data cleaning)
- DAX (measures)

---

## Author

**Sneha Gupta**
**Data Analyst**
Red & White Skill Education
