# Fullbay API integration and MCP server: connect Fullbay to ChatGPT, Claude and any AI agent

**Fullbay API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your Fullbay account.** Fullbay has no official MCP server and no ChatGPT or Claude app; its open API key is part of the paid Fullbay Connect add-on (checked 26 Nov 2026). Cotlet.dev connects your own signed-in Fullbay account to any AI agent, so you ask in plain words and the AI helps you find unbilled overtime, parts that never arrived and fleet invoices that are overdue.

## What you can ask

> Look at this week's service orders. Which technicians worked after hours or over 8 hours in a day, and what overtime should I add as a separate line? Show the list before I change anything.

> Export my parts inventory, customers and units into separate sheets with one row per item, no broken lines.

> Take this month's Fullbay invoices and payments and put them in one sheet by customer and invoice number, so my bookkeeper can match them to QuickBooks.

> For last quarter, list every invoice with the customer, the tax charged and the customer's tax status, so I can compare it with what I filed.

> Which estimates did fleets not approve yet? Draft a short email to each contact. Send after my OK.

## What owners, service managers and parts managers of heavy-duty truck and trailer shops ask their AI to do in Fullbay

From owners, service managers and parts managers of heavy-duty truck and trailer shops' posts on Reddit, forums, review sites and the Fullbay help center (checked 26 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Overtime to bill | "Look at this week's service orders. Which technicians worked after hours or over 8 hours in a day, and what overtime should I add as a separate line? Show the list before I change anything." | Weekly |
| Sheets that open clean | "Export my parts inventory, customers and units into separate sheets with one row per item, no broken lines." | When you ask |
| Books for the accountant | "Take this month's Fullbay invoices and payments and put them in one sheet by customer and invoice number, so my bookkeeper can match them to QuickBooks." | Monthly |
| Sales tax check before an audit | "For last quarter, list every invoice with the customer, the tax charged and the customer's tax status, so I can compare it with what I filed." | Monthly |
| Estimates waiting for approval | "Which estimates did fleets not approve yet? Draft a short email to each contact. Send after my OK." | Daily |
| Units due for service | "Which customer units are due for preventive maintenance in the next 2 weeks? List unit, customer and what is due." | Weekly |
| Parts ordered but not received | "Which open service orders are waiting on parts that were ordered but not received? List the order, the vendor and how long it has waited." | Daily |
| Cores not returned | "Which cores have I not returned to the vendor yet, and how much credit is waiting on each?" | Monthly |
| Fleets that do not pay | "Which fleet customers have invoices unpaid for over 30 days? Draft a payment reminder for each. Send after my OK." | Weekly |
| Technician utilization | "Show each technician's hours billed against hours worked for last month and flag anyone under 70%." | Monthly |

Your AI asks you before it sends, books, replies or changes anything. Payments stay in your hands.

## Fullbay API

Fullbay's open API key is part of the paid Connect add-on, and Fullbay Next announced a developer portal with early access still to come, so a shop cannot yet get a key on its own (checked 26 Nov 2026). With Cotlet.dev you build your own Fullbay API: show your AI agent a job once in your signed-in Fullbay account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/fullbay

Endpoints you can build in a few minutes (none of them exists until you build it):

- `unapproved-estimates`: Estimates sent to fleets and not approved yet
- `overtime-hours`: Technician hours after hours or over 8 per day for a week
- `units-due-for-service`: Units due for preventive maintenance in the next days
- `parts-not-received`: Open service orders waiting on parts ordered but not received
- `unpaid-fleet-invoices`: Invoices unpaid over a number of days by customer
- `invoices-for-books`: Invoices and payments for a month by customer

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/unapproved-estimates`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Fullbay tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"fullbay":{"command":"uvx","args":["fullbay-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t fullbay-mcp .
docker run -i --rm -e COTLET_API_KEY fullbay-mcp
```

Tools: `fullbay_guide`, `fullbay_status`, `fullbay_open`, `fullbay_read`, `fullbay_click`, `fullbay_type`, `fullbay_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install fullbay-mcp        # or run it once: uvx fullbay-mcp connect
fullbay-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 fullbay_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 fullbay_agent.py status    # is Fullbay ready in your Cotlet.dev account
python3 fullbay_agent.py guide     # Fullbay screens and sign-in link
python3 fullbay_agent.py read      # open Fullbay, read the page, close the browser
python3 fullbay_agent.py tools     # list Cotlet.dev tools
python3 fullbay_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Fullbay users

**Does Fullbay have an API?**

Fullbay's open API key is part of the paid Connect add-on, and Fullbay Next announced a developer portal with early access still to come, so a shop cannot yet get a key on its own (checked 26 Nov 2026). With Cotlet.dev you build your own Fullbay API on your signed-in account and call it from n8n, Make or your own code: https://cotlet.dev/integrations/api/fullbay

**How do I connect Fullbay to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Fullbay once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Fullbay API key.

**Is there a way to use Claude with Fullbay?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Fullbay account.

**Is there a Fullbay MCP server?**

Fullbay has no official MCP server for your account. Cotlet.dev is a hosted MCP server that works in your own Fullbay account for you, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can AI work in my Fullbay account without a Fullbay API key?**

Yes. It works in your account in the browser on the screens you use, so you do not need the key from the paid Connect add-on. Writes and sends wait for your OK.

**Does Fullbay have an official MCP server?**

No (checked 26 Nov 2026). The MCP Registry has no Fullbay entry, and neither ChatGPT nor Claude lists a Fullbay connector. Cotlet.dev works in your own Fullbay account in a browser.

## Find it on

- [Fullbay on Cotlet.dev](https://cotlet.dev/integrations/mcp/fullbay?utm_source=github&utm_medium=readme&utm_campaign=fullbay&utm_content=find)
- [PyPI package fullbay-mcp](https://pypi.org/project/fullbay-mcp/)
- [Odel](https://odel.app/modules/cotlet/fullbay)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/fullbay/)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [Alto MCP](https://github.com/cotlet-connect/alto-mcp)
- [Rightmove Plus MCP](https://github.com/cotlet-connect/rightmove-plus-mcp)
- [TurboTenant MCP](https://github.com/cotlet-connect/turbotenant-mcp)
- [Innago MCP](https://github.com/cotlet-connect/innago-mcp)
- [Property Finder MCP](https://github.com/cotlet-connect/property-finder-mcp)
- [LoopNet MCP](https://github.com/cotlet-connect/loopnet-mcp)
- [Dotloop MCP](https://github.com/cotlet-connect/dotloop-mcp)
- [SkySlope MCP](https://github.com/cotlet-connect/skyslope-mcp)
- [ShowingTime MCP](https://github.com/cotlet-connect/showingtime-mcp)
- [connectMLS MCP](https://github.com/cotlet-connect/connectmls-mcp)
- [WebPT MCP](https://github.com/cotlet-connect/webpt-mcp)
- [Tebra MCP](https://github.com/cotlet-connect/tebra-mcp)
- [ChiroTouch MCP](https://github.com/cotlet-connect/chirotouch-mcp)
- [ChiroFusion MCP](https://github.com/cotlet-connect/chirofusion-mcp)
- [Semble MCP](https://github.com/cotlet-connect/semble-mcp)
- [Nookal MCP](https://github.com/cotlet-connect/nookal-mcp)
- [Cliniko MCP](https://github.com/cotlet-connect/cliniko-mcp)
- [Gentu MCP](https://github.com/cotlet-connect/gentu-mcp)
- [splose MCP](https://github.com/cotlet-connect/splose-mcp)
- [Physitrack MCP](https://github.com/cotlet-connect/physitrack-mcp)
- [ALLDATA MCP](https://github.com/cotlet-connect/alldata-mcp)
- [Mitchell 1 ProDemand MCP](https://github.com/cotlet-connect/mitchell1-mcp)
- [Identifix MCP](https://github.com/cotlet-connect/identifix-mcp)
- [Worldpac speedDIAL MCP](https://github.com/cotlet-connect/worldpac-mcp)
- [RepairLink MCP](https://github.com/cotlet-connect/repairlink-mcp)
- [PartsTech MCP](https://github.com/cotlet-connect/partstech-mcp)
- [Shopmonkey MCP](https://github.com/cotlet-connect/shopmonkey-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/fullbay?utm_source=github&utm_medium=readme&utm_campaign=fullbay

Start free: https://cotlet.dev/signup?app=fullbay&utm_source=github&utm_medium=readme&utm_campaign=fullbay
