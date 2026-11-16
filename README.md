# Supply Chain Risk Control Tower

A small control tower that watches two outside risks to a European import supply chain and emails me when they get worse:

- **Currency risk:** the live EUR/USD rate (a weaker euro makes imports bought in USD more expensive)
- **Port risk:** wind speed at the Port of Rotterdam (strong wind slows or stops crane operations)

Both feed into one composite risk score. The score goes to a dashboard, and if it crosses a threshold, an alert email goes out automatically.

## Why I built it

Most supply chain dashboards show what already happened. I wanted something that looks at outside signals and warns early, the way a control tower should. It also connects my IT background (integrations, APIs) with supply chain work.

## How it works

```
EUR/USD API ─┐
             ├─► n8n workflow ─► risk score (Python) ─► Google Sheets / SQL ─► Tableau dashboard
Wind API ────┘                                    └─► Gmail alert if score > threshold
```

1. An n8n workflow (running in Docker) pulls both data sources on a schedule.
2. A Python step normalises each signal and combines them into one risk score.
3. Results are stored (Google Sheets / SQL) and shown in Tableau.
4. If the score is above the threshold, the Gmail API sends an alert.

## Tools

n8n · Docker · Python · SQL · Google Sheets · Tableau · Gmail API

## Run it yourself

1. Install Docker and start n8n: `docker run -it --rm -p 5678:5678 n8nio/n8n`
2. Import `workflow/control_tower.json` into n8n.
3. Add your own API keys and Gmail credentials in n8n (never commit them).
4. Open the Tableau workbook in `dashboard/`.

## What I would add next

- More signals: fuel prices, port congestion data, supplier news
- Weights based on real shipment volumes per lane
- A short weekly summary instead of single alerts

## Screenshots

_Add: workflow screenshot, dashboard screenshot, sample alert email._
