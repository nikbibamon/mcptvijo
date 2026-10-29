The Tvijo Odoo MCP pilot explores a practical question: **can an assistant return a verifiable answer about sales and stock while staying within a strictly defined read-only scope?** The first prototype uses fabricated data. A real Odoo deployment and the released TvijoCode client still need separate acceptance.

Status: **experimental pilot, 29 October 2026**. [Browse the business integration catalogue](/wiki/mcp-business-integrations/).

## Who should evaluate it

Odoo integrators, distribution teams and analysts who need to reconcile source records before building larger automation. The initial offer is one scoped diagnostic or pilot, with a data owner and an agreed comparison report.

Example tasks:

- “Show confirmed orders for this period, separating currencies and stating whether the answer is complete.”
- “Read physical stock and reservations for the permitted company; explain what these quantities mean.”
- “Compare the permitted extract with our control report and list discrepancies for a specialist.”

The prototype supplies bounded source data. Cross-report reconciliation and explanations need additional verified processing; they are pilot objectives, not already proven capabilities.

## What exists and what remains

| Component | Readiness |
|---|---|
| Three MCP read tools and fabricated sales/stock fixture | Implemented; synthetic protocol and boundary checks |
| Odoo 19 JSON-2 adapter | Implemented prototype; live ERP acceptance pending |
| Odoo 17/18 adapter | Planned separately; not supported by this prototype |
| Built-in TvijoCode / extension Odoo preset | Planned; no released-client acceptance claimed |
| Demand forecasting, DWH and Ukrainian accounting localisation | Separate projects; not delivered by this MCP prototype |
| Hosted multi-tenant MCP service | Future design; not deployed |

## Synthetic demonstration

For the inclusive UTC period 1–29 October 2026, the fixture contains two permitted confirmed orders: **DEMO-S001: 1,200 UAH; DEMO-S002: 800 UAH**. These are invented demo amounts, not actual sales. A draft order and another company's order are excluded.

The first stock item has **50 physical units and 12 reserved units**. These remain separate fields. Their difference is not a complete available-to-promise calculation. Production units of measure need validation.

The tools are `odoo_connection_status`, `odoo_sales_read` and `odoo_inventory_read`. Status describes configuration; it does not prove a successful live connection. Reads return company, source model, fields, observation time, synthetic/live mode and a truncation flag. A single page must not be presented as the company's complete turnover.

The private prototype runs over stdio with the official MCP Python SDK. After access is agreed, a demonstration can run without ERP credentials. The public documentation contains the example; the implementation is not a public install package.

## Check compatibility first

The first live target is Odoo 19's JSON-2 API, using a dedicated read-only user and fixed permitted models. The [official Odoo documentation](https://www.odoo.com/documentation/19.0/developer/reference/external_api.html) describes version-specific API behaviour and permissions. For Odoo's applicable hosted offering, external API access is restricted to Custom plans; One App Free and Standard are not eligible targets for that path. Self-hosted deployments need their own check.

We must verify your actual version, edition, hosting, installed modules, database schema, company and record/field permissions. The initial adapter supports public HTTPS hostnames on port443; internal/VPN deployments require a separately reviewed network profile.

The pilot reads confirmed sales orders, including tax in `amount_total`; it does not claim posted accounting revenue. It reads stock quants and reservations, not stock valuation or a complete purchasing recommendation. The limit is 100 returned records per call and a sales period of at most366 days. Larger extracts need an independently verified pagination and reconciliation design.

## Data and action boundaries

The prototype exposes no ERP write, payment, invoice-posting, arbitrary-method or SQL-execution tool. The company and field allowlists are configured outside the conversation. ERP access rules remain mandatory. The returned company is checked again before results reach the client.

Local stdio does not guarantee local model processing: the client may send returned data to its chosen external model. Agree on the fields, provider and retention before using real records. Never paste credentials into a prompt, public issue or source repository. ERP text is untrusted data, not permission to run commands.

The current prototype is not a security certification. It still requires live access-rule tests, revocation tests, dependency/security review and exact-client installation tests. No production ERP was accessed to produce the synthetic demonstration.

## Ukrainian localisation and analytics

Odoo 19 includes an [official `l10n_ua` module](https://raw.githubusercontent.com/odoo/odoo/19.0/addons/l10n_ua/__manifest__.py) describing Ukraine accounting and a chart of accounts. Its existence does not prove complete coverage of your payroll, tax, reporting or document workflow. An accountant and Odoo specialist must assess the actual installed package and requirements. MCP does not supply that compliance.

Demand forecasting also needs its own evidence: a defined target and horizon, reliable history, stockout handling, a seasonal-naive baseline and rolling backtests. ARIMA/SARIMA, Prophet and gradient boosting are candidate methods, not an automatic benefit of connecting MCP. A DWH requires separately designed ETL, reconciliation and measured SQL performance.

## What a pilot must prove

1. The account sees only the agreed company, models and fields; forbidden reads fail.
2. Source records agree with the chosen control report at the same cutoff and definition.
3. The exact client discovers the tools and completes a real permitted read without leaking keys.
4. The user repeats a useful task; time, errors and support effort are recorded against a baseline.

A successful synthetic check satisfies none of the live-client requirements automatically. No time-saving percentage, revenue growth or forecast accuracy is promised.

## Request a scoped evaluation

[Open a public pilot enquiry](https://github.com/nikbibamon/mcptvijo/issues/new?template=odoo-pilot.yml) with a GitHub account. Include only a non-sensitive scenario, Odoo version, hosting type and your role. Request a private channel before sharing anything confidential. Do not attach exports, personal data or credentials.

We propose a diagnostic, then a two-week pilot for one agreed scenario. Scope, acceptance and price require written agreement. This page is not checkout, automatic enrolment or a promise of production compatibility.

## Sources and alternatives

Technical sources were automatically observed on 29 October 2026; human expert review is not claimed.

- [Odoo 19 external API](https://www.odoo.com/documentation/19.0/developer/reference/external_api.html).
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk).
- [MCP security guidance](https://modelcontextprotocol.io/docs/2025-11-25/tutorials/security/security_best_practices).
- Existing projects include [FGRibreau/mcp-odoo](https://github.com/FGRibreau/mcp-odoo) and [oconsole/odoo-mcp-server](https://github.com/oconsole/odoo-mcp-server). Their feature descriptions are developer claims; we have not independently tested or endorsed them.

Our proposed value is a verified workflow, installation and support for a named customer scenario. The existence of MCP itself is not a unique product advantage.
