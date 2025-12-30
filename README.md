<h1>Apply filters to SQL queries</h1>

<h2>Project description</h2>
My organization is working to make its systems more secure. I ensure the system is safe, investigate potential security incidents, and coordinate updates for employee machines. The steps below show how I used SQL filters to perform security‑related tasks: detect after‑hours suspicious activity, isolate activity on specific dates, exclude activity from a country, and target employees by department and office for updates.
<br />

<h2>Retrieve after hours failed login attempts :</h2>
There was a potential security incident that occurred after business hours (after 18:00). All after‑hours failed login attempts needed to be investigated.
The following query filters for failed login attempts that occurred after business hours:

<p align="center">
<br/>
<img src="https://github.com/user-attachments/assets/14c1ec76-7e31-4dbd-be63-3e3e58e3daa8" height="80%" width="80%" alt="SQL Query and Results"/>
<br/>
</p>

<p>
The first part of the screenshot shows the query, and the second part shows a portion of the output. This query selects all columns from <b>log_in_attempts</b>, then uses a <b>WHERE</b> clause with an <b>AND</b> operator to return only rows where the <b>login_time</b> is strictly greater than 18:00 and <b>success</b> equals FALSE (stored as 0).
<br/><br/>
<b>Result:</b> 19 failed login attempts after 18:00.
<br/>
<b>Action:</b> Prioritize investigation of these accounts (check IPs, repeated attempts, consider temporary lock or MFA).
</p>


<br />
<hr />
<br />

<h2>Retrieve login attempts on specific dates :</h2>
A suspicious event occurred on 2022-05-09. Any login activity on 2022-05-09 or the previous day (2022-05-08) needed review.
The following query filters for login attempts that occurred on those specific dates:
<br /><br />
SELECT *
<br />
FROM log_in_attempts
<br />
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08'; 

<p align="center">
<br/>
<img src="https://i.imgur.com/MHTpDuh.png" height="80%" width="80%" />
<br/>
</p>

Key result: 75 login attempts across those two days.

<p align="center">
<br/>
<img src="https://i.imgur.com/BsinTqq.png" height="80%" width="80%" />
<br/>
</p>

The screenshot would show the query and a sample of results. This query uses OR to return rows where login_date equals either 2022-05-09 or 2022-05-08. Result: 75 login attempts across those two days. Action: correlate these records with IP addresses and user accounts to identify potential compromise patterns.
<br/><br/>
<b>Key result:</b> 75 login attempts across those two days.
<br/>
<b>Action:</b> Correlate these records with IP addresses and user accounts to identify potential compromise patterns.
</p>
</p>


<br />
<hr />
<br />

<h2>Retrieve login attempts outside of Mexico :</h2>
Investigation suggested focusing on login attempts originating outside Mexico. The country field can contain 'MEX' or 'MEXICO', so a pattern match is required.
The following query excludes countries starting with "MEX":
<br /><br />
SELECT *
<br />
FROM log_in_attempts
<br />
WHERE NOT country LIKE 'MEX%';

<p align="center">
<br/>
<img src="https://i.imgur.com/mf40Mbj.png" height="80%" width="80%" />
<br/>
</p>

Key result: 144 attempts outside Mexico.
Action: use this set to analyze geographic distribution and flag unusual foreign activity.

<p align="center">
<br/>
<img src="https://i.imgur.com/Xjc8ws1.png" height="80%" width="80%" />
<br/>
</p>

The screenshot would show the query and example output. This query selects all rows where the country does not match the pattern 'MEX%' (the % wildcard matches any trailing characters). Result: 144 attempts outside Mexico. Action: use this set to analyze geographic distribution and flag unusual foreign activity.


<br />
<hr />
<br />

<h2>Retrieve employees in Marketing :</h2>
The team needed to update machines for employees in the Marketing department located in East building offices.
The following query selects Marketing employees in East building offices: 
<br /><br />
SELECT *
<br />
FROM employees
<br />
WHERE department = 'Marketing' AND office LIKE 'East-%';

<p align="center">
<br/>
<img src="https://i.imgur.com/lZRRx89.png" height="80%" width="80%" />
<br/>
</p>

Key result: first returned username: elarson.
The screenshot would show the query and a sample of returned rows. This query filters employees to those whose department equals 'Marketing' and whose office begins with 'East-' using LIKE with a wildcard. First returned username: elarson. Action: schedule on‑site updates for these offices.


<br />
<hr />
<br />

<h2>Retrieve employees in Finance or Sales:</h2>
A different security update was required for Finance and Sales. I needed records for employees in either department.
The following query retrieves those employees:
<br /><br />
SELECT *
<br />
FROM employees
<br />
WHERE department = 'Finance' OR department = 'Sales';

<p align="center">
<br/>
<img src="https://i.imgur.com/d0wcc6y.png" height="80%" width="80%" />
<br/>
</p>

Key result: first returned Sales username: lrodriqu.
The screenshot would show the query and sample results. This query uses OR to include employees from either department. First returned Sales username: lrodriqu. Action: prepare update bundles and communication targeted to Finance and Sales teams.


<br />
<hr />
<br />

<h2>Retrieve all employees not in IT:</h2>
IT machines were already updated; I needed to obtain records for all employees who are not in the Information Technology department.
The following query retrieves non‑IT employees:
<br /><br />
SELECT *
<br />
FROM employees
<br />
WHERE NOT department = 'Information Technology';

<p align="center">
<br/>
<img src="https://i.imgur.com/xHyDbUG.png" height="80%" width="80%" />
<br/>
</p>

Key result: 161 employees not in Information Technology

<p align="center">
<br/>
<img src="https://i.imgur.com/Y3qNXNv.png" height="80%" width="80%" />
<br/>
</p>

The screenshot would show the query and a portion of results. This query excludes rows where department equals 'Information Technology'. Result: 161 employees not in IT. Action: plan and track updates for these remaining users.
<br/>
</p>

The screenshot would show the query and a portion of results. This query excludes rows where department equals 'Information Technology'. Result: 161 employees not in IT. Action: plan and track updates for these remaining users.
