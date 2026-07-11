# Zoosite product contract

Verified against the draft source on 2026-07-11 CT. The main evidence is site-config.json, variables.json, planes/i18n/es.json, route page configs, and current public assets.

## Identity and offer

- Public brand: zoositioweb. Do not reintroduce the superseded short name.
- Canonical origin: https://zoositioweb.com.mx.
- Primary product: commercial, measurable websites for businesses, with first-party interaction data and human support.
- WhatsApp is the primary contact path. Contact and legal routes remain available.
- Public claims must stay within verified scope. Current copy supports a first test round in 5 days when base materials are complete, prices before IVA, and a 100% invoiceable service.

## Plans

The current Spanish source publishes:

| Plan | Current reference price | Position |
| --- | --- | --- |
| Presencia | $3,000 MXN before IVA | Professional base and first contacts |
| Clientes | $6,000 MXN before IVA | More service detail and better-qualified conversations |
| Crecimiento | From $10,000 MXN before IVA | Expandable routes, campaigns, sectors, and data |

Copy must remain outcome-led and must not expose internal cost structure. If prices change, update the runtime dictionary first and then this summary.

## Routes and page personalities

Commercial routes are home, services, plans, websites for small businesses, platform, about, FAQ, contact, privacy, terms, and six sector routes. site-config.json is authoritative for the complete route list, including auth, account, admin, and blog routes.

- Home: commercial storefront.
- Services: process and workflow.
- Plans: comparison and pricing.
- Platform: technical foundation without unsupported claims.
- Contact: action-focused intake.
- FAQ: centralized help center; do not duplicate full FAQ sections on every page.
- Sector pages: distinct visual and content personalities for clinics, law firms, real estate, local services, construction, and agencies.

Prefer draft configuration before app code. Record a platform gap when the draft cannot express a requirement and app changes are outside scope.

## Brand and theme

- Dark theme uses #25d366 as live-data accent and #128c7e as the darker WhatsApp action color.
- WhatsApp actions use the success palette with its paired readable foreground color.
- The provisional square Z brand asset and social image are served from the public Zoolanding assets host; never use private source photos or local paths in runtime config.
- Team positioning: Alec Montaño and Hector Coronado cover software, cloud architecture, data engineering, and AI; Oswaldo García covers web development and Google Ads; Pamela Betancourt is the marketing specialist.

## SEO, analytics, and legal

- Canonical URLs use the .com.mx origin; configured aliases should not force a redirect when measurement on the alias is intentional.
- Keep titles concise, descriptions specific, hydrated structured data, unknown routes as noindex/follow, and accessible draft markup.
- Privacy and terms are draft-local footer routes.
- First-party analytics may describe pages viewed, section attention, important clicks, language choice, and WhatsApp actions. Do not claim sensitive-data collection or integrations that are not implemented.
- GA4 is the current analytics direction. Do not add Google Ads tags, conversion labels, GTM, or Search Console values without verified real configuration.

## Preview-host status

Use the shared preview:

https://test.zoolandingpage.com.mx/?draftDomain=zoositioweb.com.mx

The dedicated test.zoositioweb.com.mx host timed out in the 2026-07-11 audit and is not an approved healthy target. site-config.json still contains that legacy alias, so treat it as known configuration drift until it is explicitly removed or repaired and reapproved.
