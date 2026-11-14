---
name: koerebog-for-maaneden
description: Create the mileage log (kørebog) for a month in MilePro, with km for business and private trips per vehicle or per driver, ready for SKAT or the accountant. Use for "kørebog for måneden", "kørselsregnskab", "km erhverv og privat", or "kørebog til bogholderen". Read-only; ask for the month and whether to group by vehicle or driver.
---

# Monthly mileage log

Tool names may have a client prefix (for example, `mcp__milepro__find_trips`); use the names shown in your tool list.

Input: `maaned` (optional, default: last month, for example `2026-10`) and `gruppering` (optional: `koeretoej` or `chauffoer`).

## Steps

1. If `maaned` is missing, use last month and say which month you use. If `gruppering` is missing, ask once, in Danish: "Skal kørebogen samles pr. køretøj eller pr. chauffør?" (default: per vehicle). Work out the first and last day of the month.
2. Call `get_mileage_summary` with from = first day and to = last day. This gives the total km and trips split by purpose (`erhverv`, `privat`, `uklassificeret`). Use it as the control total.
3. Per vehicle: call `find_vehicles` (limit 200), then `get_mileage_summary` with vehicleId for each vehicle that has trips (up to about 25 vehicles). Per driver: call `list_employees` for names and userIds, then `find_trips` with employeeId for each employee (limit 200).
4. Call `find_trips` for the period (limit 200) to find the trips whose purpose is `uklassificeret`. If the answer says more trips exist than were returned, repeat per week so none are missed. For the per-driver view, add up the `Km` of the trips per purpose.
5. Write the result in Danish as a table with one row per vehicle or driver. Danish column headings: Køretøj/Chauffør, Km erhverv, Km privat, Km uklassificeret, Km i alt. Add a total row and compare it with the control total from step 2.
6. Under the table, list the trips missing a purpose (`uklassificeret`): date, vehicle, driver and km. Tell the user they can classify them as business or private in MilePro before the log is handed to SKAT or the accountant.

## Rules

- The workflow is read-only: no tool changes data in MilePro.
- A user with the role `user` (a driver) sees only their own vehicles and trips. Say so in the text: the table then covers only their own trips, not the whole company.
- Never estimate or fill in km. If a vehicle has no tracker, `get_mileage_summary` gives zero for it; say so.
- If a tool says the period is too long or there are too many trips, ask for a shorter period or work week by week.
- If the user has several companies, call `list_organizations` and say which company the log is for.
