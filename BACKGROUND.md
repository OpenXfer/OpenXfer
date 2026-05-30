# Background and how OpenXfer came about

This document gives the context for grant reviewers and anyone else who wants to understand who is behind OpenXfer and where the project comes from. It is not part of the funded deliverables.

## About the steward

OpenXfer is authored and maintained by **Winpis j.d.o.o.** (Croatia, set up in 2012). The standard itself is intended to be open and adopted across the EU — Winpis is the company that originated it and serves as its steward.

The lead author is **Davor Geci**, founder of Winpis. For more than 25 years I have been building ERP systems and production-management software for Croatian companies. Invoicing and fiscalisation have been part of that work all along, so I have followed the Croatian e-invoicing rollout closely from the practical side.

- LinkedIn: https://www.linkedin.com/in/davor-geci/
- GitHub: https://github.com/dageci · https://github.com/winpis

## How the need for OpenXfer came up

### 2018 — early proposal

In February 2018 I wrote to the Croatian Ministry of Finance with a proposal. The idea was simple: extend the existing fiscalisation system to cover e-invoicing, rather than build something new on the side.

The Ministry forwarded the letter to the Tax Authority, who invited me to a meeting at their headquarters in Zagreb. We went through the proposal in detail with senior representatives of the Tax Authority — including the Deputy Director-General and an Assistant Director-General — and with technical staff from APIS-IT (the IT company that operates Croatian fiscalisation).

The proposal has been part of the public record since then:
https://www.facebook.com/davor.geci/posts/pfbid02gS5puUsoTfXHC7ndt4SCFQpn8B1zm8PYeiWGA6WpnzmwXpwUF47JgKSGVvyhA7mgl

The general direction proposed in 2018 became, in the end, the basis of "Fiskalizacija 2.0" — except for one piece. The ERP-to-intermediary connection was left out of the official standard. OpenXfer is the proposal for that missing piece.

### 2025 — analysis of the remaining gap

A couple of months before Fiskalizacija 2.0 went live, I publicly analysed the remaining gap and shared the analysis with the developer and accounting community. ERP vendors, accountants, business owners and even one of the intermediaries themselves agreed in the discussion that the missing portability layer is a real problem in practice. I also notified the Ministry of Finance again at that point.

## What this looks like in practice

A few concrete points that make this a real problem rather than a theoretical one:

**E-archives are not handled the same way everywhere.** Some intermediaries include the e-archive in the basic package. Others charge extra for it as a separate service. In early 2025, one of the larger Croatian intermediaries actually announced that it would permanently delete the e-invoice archives of customers who had not signed up for its paid archiving service. Customers had to download whatever they could before a deadline, with a note from the provider that it had "no legal obligation to keep the data indefinitely."

**Even when you can download, you only get the XML files.** The status history is not part of the export. One of the leading Croatian intermediaries puts it like this in its own documentation:

> "The statuses of e-invoices are visible when documents are viewed and searched within the e-archive through the online interface. However, when an e-invoice is downloaded (for example as PDF or XML), the statuses are not automatically included as separate metadata next to the downloaded documents."

So once a business switches intermediary, the new provider has the XML documents but does not know which ones were rejected, which were fiscalised, which were paid, what the timeline of events was, or which XML belongs to which underlying e-invoice. Each intermediary assigns its own internal ID to each XML it receives, and several XMLs (resubmissions, corrections, original + replacement) can belong to the same underlying invoice. Without the metadata, that grouping is lost too.

**Doing it by hand is not a serious answer.** Reconstructing all of this manually for hundreds or thousands of past invoices is impractical and error-prone. Keeping the archive yourself on a USB stick or a local disk is not professional for a business that has to keep accounting records for years and may need them during a tax audit.

**The legal background.** The EU Data Act (Regulation (EU) 2023/2854), adopted in 2023 and in force since September 2025, gives users the right to access their data and move it to another service provider within a reasonable time and without unreasonable cost. E-invoicing intermediaries are exactly the kind of cloud service the Data Act covers. The OpenXfer Transfer API is one concrete way to implement that obligation in this market, instead of leaving each provider to handle it on its own.

## Where OpenXfer applies

Croatia is the first place where OpenXfer can be built and tested in practice — live since January 2026, with 34 licensed intermediaries each running their own API. Other EU countries with a decentralised setup share parts of the same problem, but not all to the same degree:

- **Croatia** — accredited information intermediaries with parallel fiscalisation reporting. No standard for either the ERP-to-intermediary connection or for data portability between intermediaries.
- **Germany** — decentralised market exchange (receipt mandatory since January 2025; sending phased in over 2027–2028). No licensing regime and no standardisation of the ERP-to-service-provider connection or of portability between providers.
- **Belgium** — Peppol-based exchange (mandatory since January 2026). Peppol BIS covers the document and the AP-to-AP transport, but portability of the full archive between Access Points is not standardised.
- **France** — accredited private platforms after the October 2024 change that removed the role of the central exchange platform. The ERP-to-platform API is already standardised nationally (AFNOR XP Z12-013), but data portability between platforms is not addressed.

What none of them has solved is the portability layer — moving the archive (invoices, statuses, signed tax-authority receipts) from one intermediary to another without losing anything. That is the piece OpenXfer leads with.

Crucially, the Transfer API treats statuses, identifiers, receipts, rejection codes, timeline events and anything else attached to an invoice as **metadata that travels with the document**. The structure of that metadata is defined by each country (or by each intermediary within a country); the Transfer API just preserves and carries it without imposing a schema. A Croatian intermediary using OpenXfer's reference EDID and three-dimensional status model talks to another Croatian intermediary using the same. A French platform using AFNOR XP Z12-013's 31-state model and its national identifier talks to another French platform using the same. That is how a single open standard can serve very different national e-invoicing systems without forcing any of them to change.

For the country classifications, the source is the European Commission's "eInvoicing in [Country]" pages on https://ec.europa.eu/digital-building-blocks/.

## Why this is not a fresh idea

OpenXfer is not an idea that came up overnight. It is the answer to a question I have been carrying for more than seven years and that I have analysed publicly twice. The choices in OpenXfer — the focus on data portability, the parallel with France's accredited platforms, and a Transfer API that works the same in any country — come from that work.
