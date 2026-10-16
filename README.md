# 📞 Call Center Performance Dashboard 

An interactive Power BI dashboard that analyzes **5,000 customer calls over 3 months** (January – March) for a call center. It tracks call volume, answer and resolution rates, customer satisfaction, speed of answer, and agent and topic performance, then turns them into actionable recommendations.

<img width="1058" height="582" alt="Image" src="https://github.com/user-attachments/assets/95a90091-e2a0-416b-817c-8c3b841c613d" />


---

## 📌 Table of Contents
- [Project Overview](#-project-overview)
- [Business Questions](#-business-questions)
- [Data Preparation](#-data-preparation)
- [Data Modelling](#-data-modelling)
- [Data Analysis (DAX)](#-data-analysis-dax)
- [Data Visualization Dashboard](#-data-visualization-dashboard)
- [Key KPIs](#-key-kpis)
- [Key Insights](#-key-insights)
- [Recommendations](#-recommendations)


---

## 🎯 Project Overview

Call centers generate large volumes of data, but raw numbers rarely explain *why* customers are unhappy. This project builds a multi-page Power BI report that connects operational metrics (answer rate, ASA, resolution) with customer experience (satisfaction) to find the real drivers behind performance gaps.

## ❓ Business Questions

1. How many calls are answered vs. unanswered, and is the trend improving?
2. What is the true resolution rate?
3. How far is customer satisfaction from the target, and why?
4. Are there performance differences between agents?
5. Which topics generate the most calls and the most unresolved issues?
6. How does call volume change across days and months?

---

## 🧹 Data Preparation

Data was loaded and cleaned in **Power Query** before modelling:

- Imported the raw call center dataset (5,000 call records).
- Checked data types (dates, numbers, text) and corrected them where needed.
- Handled missing and blank values, especially for unanswered calls where there is no satisfaction rating.
- Standardized the Yes/No flags for **Answered** and **Resolved** calls.
- Extracted **Month** and **Day** from the call date to support time-based analysis.
- Removed duplicates and verified record counts against the source.

## 🧩 Data Modelling
And then dataset was cleaned and transformed, it was ready to the data modeled.
The model follows a star schema: one fact table, one date dimension, and a dedicated measures table.
- Added a dedicated **Date table** to enable monthly and daily analysis.
- Kept all calculations in a separate **Measures** table for a cleaner, easier-to-maintain model.
<img width="1178" height="622" alt="Image" src="https://github.com/user-attachments/assets/9a4f16c4-1478-4a2b-832f-e475f0f361f3" />

## 🧮 Data Analysis (DAX)

KPIs were built as reusable DAX measures:

```DAX
Total Calls = COUNT('Fact Calls'[Call Id])

Total Agents = DISTINCTCOUNT('Fact Calls'[Agent])

Answered Calls = CALCULATE(COUNT('Fact Calls'[Answered (Y/N)]),'Fact Calls'[Answered (Y/N)]="Y")

Unanswered Calls = CALCULATE(COUNT('Fact Calls'[Answered (Y/N)]),'Fact Calls'[Answered (Y/N)]="N")
Answer Rate = DIVIDE([Answered Calls], [Total Calls])

Answer Rate = DIVIDE(
    CALCULATE([Total Calls],'Fact Calls'[Answered (Y/N)]="Y"),[Total Calls],0)

Resolved Calls = CALCULATE(COUNT('Fact Calls'[Resolved]),'Fact Calls'[Resolved]="Y")
Unresolved Calls = [Answered Calls] - [Resolved Calls]

Unresolved Calls = CALCULATE(COUNT('Fact Calls'[Resolved]),'Fact Calls'[Resolved]="N")

Resolution Rate = DIVIDE(CALCULATE([Total Calls],'Fact Calls'[Resolved]="Y"),
CALCULATE([Total Calls],'Fact Calls'[Answered (Y/N)]="Y"),0)


AVG Satisfaction Rating = AVERAGE('Fact Calls'[Satisfaction rating])

ASA = AVERAGE('Fact Calls'[Speed of answer in seconds])

AVG Talk Duration = AVERAGE('Fact Calls'[AVG_TalkDuration Minutes])




```

## 📊 Data Visualization Dashboard

Shows visualizations from Call Center Performance Dashboard

<img width="1158" height="630" alt="Image" src="https://github.com/user-attachments/assets/d0f5ef94-02b4-469f-9ba6-42a39ae54036" />

<img width="1154" height="633" alt="Image" src="https://github.com/user-attachments/assets/a9c76b25-c8d1-4647-b1bc-52d0300d9403" />

<img width="1155" height="627" alt="Image" src="https://github.com/user-attachments/assets/0ac7a2f9-8981-4cdd-903d-858332dd9477" />
---

## 📈 Key KPIs

| KPI | Value |
|-----|-------|
| Total Calls | 5,000 |
| Answered Calls | 4,054 (81.08%) |
| Unanswered Calls | 946 (18.92%) |
| Resolved Calls | 3,646 |
| Unresolved Calls | 1,354 |
| Resolution Rate (of answered calls) | 90% |
| Avg. Satisfaction Rating | 3.4 (target: 4.4) |
| Avg. Speed of Answer (ASA) | 67.5 |
| Avg. Talk Duration | 3.7 |
| Agents | 8 |

## 💡 Key Insights

- **Answer rate is 81%.** 946 calls went unanswered, and the number stays almost constant each month (≈315), which points to a structural issue rather than a seasonal one.
- **Resolution rate is 90% of answered calls but only 73% of all calls.**
- **Satisfaction is 3.4 vs. a target of 4.4.** A full-point gap exists despite a high resolution rate, which suggests the problem is the customer experience before resolution (waiting time, unanswered calls).
- **ASA is 67.5 across all agents (65–71).** Delays are system-wide, so the issue lies in operations and staffing rather than in individual agents.
- **Agent performance is closely matched.** Jim leads with 485 resolved calls and Stewart has the fewest with 424.
- **Topics are evenly distributed** (976–1,022 calls each). Streaming has the highest volume, and Technical Support has the most unresolved calls (283).
- **Call volume peaks on day 11 (230 calls)** and drops towards the end of the month.

## ✅ Recommendations

1. **Reduce unanswered calls** by optimizing shift scheduling during peak hours.
2. **Lower ASA** to improve customer experience and raise satisfaction.
3. **Investigate the monthly decline** in satisfaction and link it to waiting times.
4. **Focus on Streaming and Technical Support**, the topics with the highest volume and the most unresolved calls.
