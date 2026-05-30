---
layout: default
title: "OpenXfer — Overview"
description: "Open standard for portable e-invoicing data between intermediaries."
---

# OpenXfer

**An open transfer standard for portable e-invoicing data.**

OpenXfer moves an e-invoice together with all the metadata that travels with it — document identifiers, statuses, signed tax-authority receipts, rejection reasons, timeline of events, vendor-specific extensions — from one intermediary to another, automatically and without manual download-and-upload workarounds.

> **Status:** v0.1-draft · pre-funding landing site
> **Steward:** [Winpis j.d.o.o.](https://www.winpis.com) (Croatia)
> **Licence:** Funded deliverables will be released under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
> **Source:** [github.com/OpenXfer/OpenXfer](https://github.com/OpenXfer/OpenXfer)

---

## The problem

EU rules (ViDA) will make B2B e-invoicing mandatory across the EU by 2030. Several member states have already gone live with a decentralised setup: invoices are sent through competing private intermediaries rather than one central government platform. Croatia (live since January 2026), France, Belgium and Germany all work this way. More countries will follow.

All of these countries share one specific problem: when a business wants to move from one intermediary to another, the delivery and status history of past invoices is left behind. The new intermediary can be given the XML files of past invoices, but it does not get the statuses, the signed tax-authority receipts, the rejection reasons, or the timeline of events. It does not even know which of several XML files belong to the same underlying invoice — each intermediary assigns its own internal IDs, and resubmissions or corrections produce multiple XMLs for one invoice. Reconstructing all of this by hand is impractical for any business with more than a handful of invoices. That is vendor lock-in, and it goes against the EU Data Act, which gives users the right to take their data with them.

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

## Where this applies

Croatia is the first place where OpenXfer can be built and tested in practice. The market is live, the rules are in place, and there are 34 licensed intermediaries to work with. But nothing in OpenXfer is specific to Croatia. The same design works for France, Belgium, Germany and any other EU country that ends up with a similar setup as ViDA rolls out.

For full context on how OpenXfer came about, see [Background]({{ site.baseurl }}/background).

## Status

This is the public landing site for the project. The detailed specification, code and tests are still in private development and will be opened progressively under Apache 2.0 as funded milestones are delivered.

For questions, [open a discussion on GitHub](https://github.com/OpenXfer/OpenXfer/discussions).
