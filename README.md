# MilePro for Claude

MilePro is a fleet and GPS platform for Danish companies: vehicles and assets with trackers, trips, the mileage log (kørebog), alarms and Circle K fuel cards. This plugin connects Claude to your MilePro account and teaches Claude two everyday MilePro workflows, so you can ask in plain language (Danish or English) for the month's mileage log or the week's alarms.

The plugin contains:

- **The MilePro connector**, a remote MCP server at `https://api.milepro.dk/mcp`. It gives Claude read-only tools for vehicles, live positions, trips, mileage summaries, alarms, employees and fuel purchases. Nothing in MilePro can be changed through it.
- **Two skills** that tell Claude the steps, defaults and checks for common tasks.

## Skills

| Skill | What it does | Who can use it |
| --- | --- | --- |
| `koerebog-for-maaneden` | The mileage log for a month: km for business, private and unclassified trips per vehicle or per driver, with a control total and a list of trips that still need a purpose. Ready for SKAT or the accountant. | Everyone. A driver (role Bruger) sees only their own trips. |
| `ugens-alarmer` | The last 7 days of alarms grouped by vehicle, with what needs action first. | Managers, administrators and owners. Tracker alarms only for administrators and owners. |

Both skills are read-only. Claude never estimates km and never guesses the cause of an alarm.

## Requirements

- A MilePro account in an active company. There is no plan requirement.
- Claude sees the same data you see in MilePro: an administrator sees the fleet, a driver only their own vehicles and trips.
- If you belong to more than one MilePro company, Claude asks which company you mean.

## Install

### In Claude (claude.ai, desktop, mobile and Cowork)

The fastest way is from MilePro itself: open **Connect > Claude & ChatGPT** in [MilePro](https://app.milepro.dk/marketplace/claude) and click **Forbind MilePro til Claude**. Sign in with your normal MilePro login on `idp.rs-systemer.dk`, the Rasmussen Solutions sign-in service, and approve access.

To get the workflows as skills, download them on the same page and upload them under **Customize > Skills** in Claude. Or add this repository as a marketplace: **Customize > Plugins > Add > Add marketplace**, and enter `rasmussen-solutions/milepro-claude`.

### In Claude Code

```
/plugin marketplace add rasmussen-solutions/milepro-claude
/plugin install milepro@milepro
```

The first time Claude uses a MilePro tool, Claude Code opens your browser to sign in. Run `/mcp` to see the connection state. In Claude Code the tools are named with the plugin prefix, for example `mcp__plugin_milepro_milepro__find_trips`; the skills work with either form.

## Data and privacy

The plugin itself stores nothing and runs no code on your computer. It contains only Markdown skills, this README, an icon and the connector address.

When you use it, Claude sends your requests (periods, vehicle and employee names or ids) to the MilePro MCP server at `https://api.milepro.dk/mcp`, operated by Rasmussen Solutions ApS. Sign-in uses OAuth through `https://idp.rs-systemer.dk`. MilePro returns only the data you have access to, such as vehicles, positions, trips with start and end address, km, alarms and employee names. No data is sent anywhere else. You can revoke the access at any time under **Forbundne apps** in your RsConnect account.

Read the [MilePro privacy policy](https://milepro.dk/pages/privacy.html) and [terms](https://milepro.dk/pages/terms.html).

## Support

Email kontakt@milepro.dk or use the [MilePro contact page](https://milepro.dk/kontakt/).

## Dansk

MilePro-pluginet forbinder Claude med din MilePro-konto, så du kan spørge på dansk efter månedens kørebog (km erhverv og privat pr. bil eller chauffør, klar til SKAT eller bogholderen) eller ugens alarmer (hvad der kræver handling). Claude ser det samme som du gør i MilePro og kan ikke ændre dine data.

Krav: en MilePro-bruger i en aktiv virksomhed. Ugens alarmer er for ledere, administratorer og ejere. Pluginet gemmer intet selv; dine forespørgsler sendes kun til MilePro på `https://api.milepro.dk/mcp`. Se [privatlivspolitikken](https://milepro.dk/pages/privacy.html). Spørgsmål: kontakt@milepro.dk.

## License

MIT. See [LICENSE](LICENSE).
