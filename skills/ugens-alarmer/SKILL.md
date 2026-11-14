---
name: ugens-alarmer
description: Create an overview of the last 7 days of alarms in MilePro grouped by vehicle, with what needs action first. Use for "ugens alarmer", "hvilke alarmer har vi haft", "alarmer sidste uge", or "hvad skal jeg reagere på". Only for administrators, managers and owners. Read-only.
---

# Ugens alarmer

Tool names may have a client prefix (for example, `mcp__milepro__list_alarms`); use the names shown in your tool list.

Input: `dage` (optional, default 7): how many days back the overview covers.

## Steps

1. Call `list_alarms` with from = today minus `dage` days and to = today (limit 200). If the tool refuses, tell the user in Danish that alarms are only available to ledere og administratorer, and stop.
2. If the answer says more alarms exist than were returned, say so and offer to narrow it with `kind` (vehicle or device).
3. Call `find_vehicles` (limit 200) to match the alarms to vehicles and see each vehicle's status. Alarms without a vehicle are listed under "Uden køretøj".
4. Group the alarms by vehicle. For each vehicle show: number of alarms, the types or reasons, the time of the latest one, and how many are not yet acknowledged.
5. Write the overview in Danish:
   - First "Kræver handling": vehicles with unacknowledged alarms, the vehicle with the most or the newest first.
   - Then "Kvitteret": vehicles where all alarms are acknowledged.
   - End with a short summary: total number of alarms, how many are unacknowledged, and any pattern, such as the same vehicle or the same alarm type repeating.
6. Offer to look closer at a vehicle with `get_vehicle`, or at where it is now with `list_live_positions`.

## Rules

- The workflow is read-only. Alarms are acknowledged in MilePro, not here; say so if the user asks.
- Vehicle alarms are for ledere og administratorer; tracker alarms (such as a disconnected tracker) only for administratorer og ejere. If tracker alarms are missing, say it may be due to the user's role.
- Never guess a cause for an alarm. Use only the reason and type the tool returns.
- If the user has several companies, call `list_organizations` and say which company the overview is for.
