# DECISIONS.md  (about 150-200 words in total - plain language, your own words)

1. **Indexes.** Which indexes did you create, and which query or guarantee does each one serve? Which one did you consider and reject?
Answer:  I created a unique index on "employees.emp_code" to prevent duplicate employee codes. I created a compound index on "employees.department" and "employees.joined_on" to support department filtering and employee headcount calculations. For attendance, I created a unique compound index on "attendance_logs.emp_code" and "attendance_logs.date" to prevent duplicate daily records for an employee. I also created a compound index on "attendance_logs.date" and "attendance_logs.emp_code" to support date-based attendance queries and sorting.
2. **Punch-in race.** Two identical punch-in requests arrive at the same instant. Walk through exactly what happens in your code.
Answer: If two identical punch-in requests arrive simultaneously, both might pass an initial duplicate check. The unique compound index on employee code and date prevents both records from being inserted. One request succeeds, while the other receives a duplicate-key error that is handled by the API.
3. **Ties.** Two employees are tied on late minutes at the cutoff of `limit`. What does your leaderboard return, and why?
Answer: The late leaderboard uses ranking based on total late minutes. If multiple employees tie at the cutoff, the query includes all employees whose rank is within the requested limit. Results are then sorted by employee code to make the output order consistent.
4. **Headcount.** How does your department summary make sure employees with zero logs are counted?
Answer: The department summary starts with employee records and uses attendance data to calculate statistics. This allows employees with zero attendance logs to remain included in the headcount, rather than disappearing because they have no matching attendance records.
5. **One thing you would change** if this had to serve 100x the data.
Answer: I would introduce pre-aggregated daily or monthly summaries for frequently requested analytics. This would reduce repeated calculations over large attendance collections and improve response times. I would also monitor query execution plans and add indexes only when measurements show they are needed.
