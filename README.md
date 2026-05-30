# OpenXfer — an open transfer standard for portable e-invoicing data

[![Status](https://img.shields.io/badge/status-v0.1--draft-orange)](https://openxfer.org)
[![Licence (funded)](https://img.shields.io/badge/licence-Apache--2.0-lightgrey)](https://www.apache.org/licenses/LICENSE-2.0)
[![EU ViDA](https://img.shields.io/badge/EU-ViDA_aligned-blue)](https://taxation-customs.ec.europa.eu/taxation/vat/vat-digital-age-vida_en)
[![Website](https://img.shields.io/badge/website-openxfer.org-2980b9)](https://openxfer.org)
[![Stars](https://img.shields.io/github/stars/OpenXfer/OpenXfer?style=social)](https://github.com/OpenXfer/OpenXfer)

An open standard for moving e-invoice archives, statuses and tax-authority receipts between intermediaries without losing anything.

**Links:**
- 🌐 Website · [openxfer.org](https://openxfer.org)
- 💬 Discussions · [github.com/OpenXfer/OpenXfer/discussions](https://github.com/OpenXfer/OpenXfer/discussions)
- 🐛 Issues · [github.com/OpenXfer/OpenXfer/issues](https://github.com/OpenXfer/OpenXfer/issues)
- 📖 Background · [BACKGROUND.md](./BACKGROUND.md)
- 🤖 GenAI policy · [GENAI.md](./GENAI.md)

> **Status:** v0.1-draft · pre-funding landing repository
> **Steward:** Winpis j.d.o.o. (Croatia) — the company that authored and maintains the standard.
> **Licence:** © 2026 Winpis j.d.o.o., all rights reserved. Funded work will be released under the Apache License 2.0 as milestones are completed. See [LICENSE](./LICENSE).

## The problem

EU rules (ViDA) will make B2B e-invoicing mandatory across the EU by 2030. Several member states have already gone live with a decentralised setup: invoices are sent through competing private intermediaries rather than one central government platform. Croatia (live since January 2026), France, Belgium and Germany all work this way. More countries will follow.

All of these countries share one specific problem: when a business wants to move from one intermediary to another, the delivery and status history of past invoices is left behind. The new intermediary can be given the XML files of past invoices, but it does not get the statuses, the signed tax-authority receipts, the rejection reasons, or the timeline of events. It does not even know which of several XML files belong to the same underlying invoice — each intermediary assigns its own internal IDs, and resubmissions or corrections produce multiple XMLs for one invoice. Doing all of this by hand is impractical for any business with more than a handful of invoices. That is vendor lock-in, and it goes against the EU Data Act, which gives users the right to take their data with them. ([BACKGROUND.md](./BACKGROUND.md) describes the practical side in more detail.)

On the layer above — how ERP software talks to the intermediary in the first place — the picture varies. France has standardised this at the national level (AFNOR XP Z12-013). Belgium uses Peppol BIS, which covers part of it. Croatia and Germany leave it open, so each intermediary builds its own API. OpenXfer proposes an open EU-wide standard for both pieces, but it leads with the portability layer because that is where every country has the same unsolved gap.

## What OpenXfer proposes

The core of OpenXfer is the **Transfer API** — a protocol for moving an e-invoice together with **all the metadata that travels with it** (document identifiers, statuses, signed tax-authority receipts, rejection reasons, timeline of events, vendor-specific extensions — anything the originating intermediary attached) from one intermediary to another, automatically and without manual download-and-upload workarounds.

The **structure of that metadata varies** from country to country, and often from intermediary to intermediary within a country. The Transfer API is deliberately **agnostic** about it:

- **Document identifiers** are metadata. Some countries use national formats; others define per-intermediary IDs. The Transfer API carries whatever it finds.
- **Status models** are metadata. Some intermediaries use a few states; France's XP Z12-013, for example, defines 31. The Transfer API moves status history regardless of the structure or number of states.
- **Receipts, rejection codes, timeline events, vendor-specific extensions** are all metadata. The Transfer API preserves them without imposing a schema.

Alongside the Transfer API, OpenXfer offers two **reference proposals** that countries without an existing scheme can adopt as their metadata structure:

1. **EDID** — a portable document identifier (UUID v4) that travels with the invoice and survives a change of intermediary.
2. **Three-dimensional status model** — keeps delivery, fiscalisation and business-process state separate so they can be tracked clearly.

Countries that already have national standards (France's AFNOR XP Z12-013, Peppol BIS conventions, etc.) keep their definitions; the Transfer API treats those definitions as the country's chosen metadata structure and carries the data unchanged.

The detailed specification, the reference implementation and the conformance tests are the work the grant would fund (see Roadmap below). They will be opened under Apache 2.0 as each milestone is delivered.

## Where this applies

Croatia is the first place where OpenXfer can be built and tested in practice. The market is live, the rules are in place, and there are 34 licensed intermediaries to work with. But nothing in OpenXfer is specific to Croatia. The same design works for France, Belgium, Germany and any other EU country that ends up with a similar setup as ViDA rolls out.

## Roadmap (what the grant would fund)

- **M1** — Technical specification, written in English: scope, threat model, EDID, the status model, and the lifecycle of the Transfer API. Compatible with the EN16931 e-invoice standard.
- **M2** — Reference implementation of EDID generation/validation and the status model.
- **M3** — Test scenarios and conformance criteria. Defining what an automated test suite would need to check to verify that a given intermediary's software actually follows the OpenXfer standard correctly.
- **M4** — Documentation, examples and an outreach package for ERP vendors and intermediaries.

## Steward and author

**Winpis j.d.o.o.** (Croatia, established 2012) is the company that authored OpenXfer and maintains the standard. **Davor Geci** is the founder and lead author — an ERP developer with 25+ years of experience in Croatian business software. Background and how this project came about are in [BACKGROUND.md](./BACKGROUND.md).

## Status of this repository

This is the public landing page for the project. The specification, code and tests are still in private development and will be opened progressively under Apache 2.0 as milestones are delivered.

For questions, open a discussion on this repository or contact the steward through the profile linked in BACKGROUND.md.
