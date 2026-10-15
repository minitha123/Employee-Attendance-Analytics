REVIEW.md

List every defect found in the starter "app/main.py" (helpers and endpoints).

#| Where (function / line)| What is wrong| How you'd notice it (test, input, or symptom)| How you fixed it
1| "compute_late_minutes"| The grace-period calculation must use elapsed seconds before calculating whole late minutes.| Test a 09:30 shift with punch-in at 09:40:01; expected 10 minutes.| Compare elapsed seconds against 600 and then return whole minutes.
2| "compute_late_minutes"| The starter helper does not handle overnight shifts.| Test a 05:50 punch-in for a 22:00–06:00 shift.| Added overnight-shift handling and tested the early-morning case.
3| "compute_work_hours"| Built-in "round()" does not guarantee the required half-up rounding.| Test a duration that falls exactly on a half-cent boundary.| Use decimal arithmetic with "ROUND_HALF_UP".
4| "compute_overtime"| The starter helper does not account for overnight shift-end dates or the required 30-minute threshold.| Test a punch-out after midnight and overtime below 30 minutes.| Calculate the correct shift-end instant and apply the threshold.
5| "health"| It returns success without checking whether MongoDB is available.| Stop MongoDB and call "/health".| Ping MongoDB and return an error response when the database is unavailable.
6| "create_employee"| Checking for duplicates before insertion is not race-safe by itself.| Attempt concurrent creation requests with the same employee code.| Add a unique database index and handle duplicate-key errors.
7| "create_employee"| "created_at" is stored as a naive datetime rather than the required timestamp representation.| Inspect the stored employee document.| Store the timestamp in the required format.
8| "list_employees"| Pagination uses "page * page_size" as the skip value.| Request page 1 with page size 20.| Use "(page - 1) * page_size".
9| "list_employees"| The total count ignores the department filter.| Filter by department and compare the total with the filtered results.| Count documents using the same filter as the query.
10| "list_employees"| Results are not sorted by employee code, and pagination inputs are not fully validated.| Request multiple pages or invalid pagination values.| Add stable sorting and pagination validation.
11| "punch_in"| A nonexistent employee is not handled before accessing employee shift information.| Submit a punch-in request with an unknown employee code.| Validate the employee and return 404 if not found.
12| "punch_in"| Duplicate attendance prevention relies on a separate check before insertion.| Submit simultaneous punch-in requests for the same employee and date.| Enforce a unique compound index and handle duplicate-key errors.
13| "punch_in"| The starter does not fully validate status values, timestamps, or IST handling.| Submit invalid statuses and timestamps or test a punch near midnight.| Validate inputs, truncate instants to whole seconds, and apply the IST and overnight-shift rules.
14| "punch_in"| The response exposes the MongoDB document ID using a custom "id" field.| Inspect the response body against the API contract.| Return only fields required by the API contract.
15| "list_attendance"| It loads all matching records into Python and sorts them there, which is inefficient for large collections.| Test with a large attendance collection.| Apply filtering, sorting, pagination, and counting in MongoDB.
16| "list_attendance"| The total is calculated from all loaded records rather than a separate filtered count query.| Compare pagination totals against filtered records.| Use "count_documents" with the same query filter.
17| "list_attendance"| Sorting only by date does not provide a deterministic order when records share a date.| Create multiple records with the same date and repeat a paginated request.| Add a stable secondary sort field.
18| "list_attendance"| Pagination and date-filter inputs are not sufficiently validated.| Submit invalid dates, page values, or page sizes.| Validate query parameters and date ranges.

Also Checked and Not Defects

- Punch-out, regularization, analytics, and explain endpoints were explicitly listed as features for the candidate to implement. Their absence from the starter file is not itself a starter-code defect.
- The final fixes should be checked against the completed implementation and tested before submission.