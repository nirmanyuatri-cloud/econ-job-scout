# Gate test cases

These are hand-written posting snippets with the outcome `skills/scan/SKILL.md` should give for two profiles. Use them after editing the gates. Ask Claude to "apply the scan skill's Step 2 gates to tests/gate-cases.md for profiles A and B, without opening any websites", then compare its answers with the expected column.

**Profile A:**
- finance only (all tracks);
- `noMasters: true`;
- non-EU student permit, zoekjaar true;
- Dutch A1;
- internships from 2027-01-01, part-time OK;
- jobs from 2027-07-01;
- graduates 2027-06-30, enrolment may run to 2027-08-31;
- no extra cities.

**Profile B:**
- policy + data; tracks "Central banking & supervision", "Economic research" and "Business analytics";
- `noMasters: false`;
- EU citizen;
- Dutch B2;
- internships from 2027-02-01, jobs from 2027-09-01;
- graduates 2027-06-30, enrolment ends 2027-06-30;
- extra cities Brussels, Frankfurt, Luxembourg;
- French B1.

| # | Posting snippet | Expected A | Expected B |
|---|---|---|---|
| 1 | "M&A Intern, Amsterdam. Start Feb 2027, 5 months. Master's students in Finance." | Reject: master's | Not relevant (M&A isn't in B's tracks or adjacent lists) |
| 2 | "Junior Economist, De Nederlandsche Bank, Amsterdam. Recent graduates (BSc or MSc). Fluent Dutch required. Start Sept 2027." | Reject: language (A1) | Reject: language ("fluent" is above B2) |
| 3 | "Research Assistant, CPB, The Hague. Bachelor's or Master's students. Good command of Dutch. Start March 2027, 6 months, must be enrolled." | Reject: not relevant (economic research isn't a finance track or adjacent role) | Reject: timing (ends Aug 2027, after ENROL_END = GRAD = 30 Jun) |
| 4 | "Treasury Intern, Unilever Rotterdam. Students in Finance or Economics. English. January–June 2027, enrolled throughout." | Pass: core (FP&A / Treasury), degree "Any degree" with "Degree level not stated" note | Reject: not relevant |
| 4b | "Corporate Finance Intern, Rotterdam. Bachelor's or Master's students. English. Start February 2027, 6 months, must be enrolled." | Pass with caveat: ends Jul–Aug 2027, so priority one lower (at most Medium) plus the enrolment note; degree "BSc OK" | Reject: not relevant |
| 5 | "Data Analyst Intern, Booking.com Amsterdam. BSc/MSc in Economics, Econometrics or similar. Start Feb 2027. Part-time possible." | Reject: not relevant (general data analytics isn't in finance's tracks; its adjacent list only has finance data/BI) | Pass: core (Business analytics), degree "BSc OK" |
| 6 | "Economic Analyst Trainee, European Central Bank, Frankfurt. Master's degree required. EU nationality required. Start Oct 2027." | Reject: location (Frankfurt isn't an extra city for A) | Pass: MSc required, priority one lower, note added; the permit gate is off (EU) |
| 7 | "Policy Intern, EU think tank, Brussels. French required (C1). Start March 2027." | Reject: location | Reject: language (French C1 required; B has French B1) |
| 8 | "Graduate Risk Analyst, ING Amsterdam. Bachelor's or Master's. EU passport holders only. Start Sept 2027." | Reject: permit (EU only, with no student-permit or zoekjaar route) | Reject: not relevant (risk isn't in policy or data) |
| 8b | "Graduate Business Analyst, ING Amsterdam. Bachelor's or Master's. EU passport holders only. Start Sept 2027." | Reject: permit | Pass: core (Business analytics). Same employer as case 8, but relevance follows the role |
| 9 | "Afstudeerstage Finance, Rabobank Utrecht." | Reject: email pre-filter (thesis) | Reject: email pre-filter (thesis) |
| 10 | "Senior Risk Manager, NN Group, The Hague. 5+ years' experience." | Reject: email pre-filter (senior) | Reject: email pre-filter (senior) |

Notes:
- Case 3A is deliberately a "not relevant" for a finance-only student.
- Cases 8 and 8b test that relevance is judged on the role itself, not on the employer.
- When a run disagrees with this table, fix the gate wording in `skills/scan/SKILL.md` rather than the table, unless the table is wrong.
