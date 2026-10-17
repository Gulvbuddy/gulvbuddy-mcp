# Gulvbuddy MCP server: wood floor prices in Denmark

Price estimates for new wood floors and floor sanding in Denmark, a coverage check by postcode, and a booking link that opens [Gulvbuddy's configurator](https://www.gulvbuddy.dk/konfigurator) with the customer's own answers filled in.

[Gulvbuddy](https://www.gulvbuddy.dk) installs new wood floors (nyt trægulv) and sands existing floors (gulvafslibning) in Denmark. This remote MCP server lets an AI assistant answer "what will my floor cost?" with the same price engine as [gulvbuddy.dk](https://www.gulvbuddy.dk), instead of guessing.

- **Server URL:** `https://www.gulvbuddy.dk/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** none
- **Access:** free and read-only
- **Documentation:** [gulvbuddy.dk/mcp-info](https://www.gulvbuddy.dk/mcp-info)
- **Registry:** `dk.gulvbuddy/prisestimat` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=dk.gulvbuddy)

This repository holds the server's public metadata and setup instructions. The server itself runs on gulvbuddy.dk.

## Tools

| Tool | What it does |
|---|---|
| `estimer_traegulv` | Estimate a new wood floor (plank, herringbone/sildeben or chevron) from area and the choices the user has made. Choices left open are covered by a from-to range. |
| `estimer_slibning` | Estimate floor sanding with oil, lacquer or lye and soap. |
| `daekning` | Check whether a postcode or municipality is inside the service area. |
| `forklar_prisen` | Explain how the price works: what moves it, waste, duration, and why the subfloor is assessed on site. |
| `valgmuligheder` | List the valid choices and ids for the estimate and booking tools. |
| `ydelser` | List the services and the process from estimate to fixed price. |
| `start_booking` | Build a link to the configurator with the user's own answers filled in. |
| `soeg_svar` | Search Gulvbuddy's questions, answers and guides, with the URL to cite. |
| `hent_guide` | Get a full guide as markdown with its canonical URL. |
| `prisindeks` | Get the open [price index](https://www.gulvbuddy.dk/traegulv/prisindeks) (CC BY 4.0) with edition and source URLs. |
| `kontakt` | Get phone, email, opening hours and contact links. |

All prices are in DKK including VAT and are estimates, not quotes. The subfloor is never included in the total; it is assessed at a free, no-obligation on-site measurement, after which the customer receives a fixed price. Durations come from measured installation times, never a fixed number of days. Answers are in Danish.

The server never creates an enquiry, a booking or a case, and it stores no personal data. See [how the price is calculated](https://www.gulvbuddy.dk/traegulv/saadan-regner-vi-prisen).

## Setup

### Claude (claude.ai and Claude Desktop)

Settings → Connectors → Add custom connector, and paste:

```
https://www.gulvbuddy.dk/mcp
```

### ChatGPT

Turn on Developer mode, then add an app with the URL above and "No authentication".

### Cursor

`.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "gulvbuddy": {
      "url": "https://www.gulvbuddy.dk/mcp"
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "gulvbuddy": {
      "type": "http",
      "url": "https://www.gulvbuddy.dk/mcp"
    }
  }
}
```

### Clients without remote support

```json
{
  "mcpServers": {
    "gulvbuddy": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://www.gulvbuddy.dk/mcp"]
    }
  }
}
```

## Example prompts

1. "Hvad koster 60 m² nyt sildebensgulv i Hellerup?"
2. "Hvad koster det at slibe og lakere 45 m² fyrregulv i Aarhus?"
3. "Dækker Gulvbuddy postnummer 4000?"
4. "Hvorfor er undergulvet ikke med i prisen?"
5. "Giv mig et link til at bestille 80 m² plankegulv."

## Price index

Gulvbuddy publishes its price index for new wood floors as open data under CC BY 4.0:

- Page: [gulvbuddy.dk/traegulv/prisindeks](https://www.gulvbuddy.dk/traegulv/prisindeks)
- CSV: [prisindeks.csv](https://www.gulvbuddy.dk/traegulv/prisindeks.csv) · JSON: [prisindeks.json](https://www.gulvbuddy.dk/traegulv/prisindeks.json)
- Archived edition with DOI: [10.5281/zenodo.23431835](https://doi.org/10.5281/zenodo.23431835)

Cite as: Gulvbuddys prisindeks, september 2026 (CC BY 4.0), https://www.gulvbuddy.dk/traegulv/prisindeks

## Company

- Gulvbuddy ApS, CVR 46838726
- Website: [www.gulvbuddy.dk](https://www.gulvbuddy.dk)
- Support: [hej@gulvbuddy.dk](mailto:hej@gulvbuddy.dk)
- [Privacy policy](https://www.gulvbuddy.dk/privatlivspolitik) · [Terms](https://www.gulvbuddy.dk/handelsbetingelser)

## Data handling

The server receives only the fields the tool call contains (area, choices, postcode or address). It does not store them, does not ask for name, email or phone, and sets no cookies. Requests are logged without personal data (tool name, duration, outcome) for operations and abuse protection.

## License

The files in this repository are MIT licensed. The price index is CC BY 4.0. The Gulvbuddy name and logo are not covered by either license.
