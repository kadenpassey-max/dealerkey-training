# DealerKey Auto — Arbitrage Engine Guide

## What it is

The Arbitrage Engine is a scoring system that tells you whether a car at auction is worth bidding on, exactly how much to bid, and why. It pulls from the Manheim condition report, the MMR, seller history, and local market demand to output a ranked score and a dual bid number — a best-case and worst-case max bid.

---

## How to start a session

Open Claude Code and type:

```
/arbitrage-engine
```

It will ask which city you're buying for. Always answer with the client's metro area — the engine applies city-specific pricing and demand data. Pre-built profiles exist for Salt Lake City, Houston, Dallas, Miami, Denver, Phoenix, Seattle, Atlanta, Las Vegas, and Portland. For any other city it builds one on the fly.

---

## The three ways you'll use it

---

### 1. Screening a batch from Manheim (most common)

When you export a CSV from Manheim after setting your search parameters, you can drop the whole file in and get a ranked buy list.

**How:**
1. Export the CSV from Manheim (make, model, year, mileage, MMR, grade, seller name)
2. Start a session: `/arbitrage-engine`
3. Tell it the city, then paste or attach the CSV
4. It outputs a ranked table sorted by Elite Score with tier labels: **A / B / C / Pass**

**What to look for:**
- **A-tier** = pull the condition report PDF before bidding — these are your targets
- **B-tier** = worth watching, bid conservatively
- **C-tier / Pass** = skip unless you have a specific client reason

The engine flags any unit where recon was estimated from the grade (no PDF) — you'll see a note saying "pull PDF before bidding." For any A-tier unit you intend to actually bid on, always get the PDF.

---

### 2. Evaluating a specific car for a client

When a client comes in with a specific make/model/year in mind, you can score individual listings from Manheim directly.

**How:**
1. Pull up the listing in Manheim
2. Start a session: `/arbitrage-engine`
3. Paste the listing details (year, make, model, trim, mileage, MMR, grade, seller name, condition items)
4. Attach the condition report PDF if you have it

**What you get back:**
- Best-case and worst-case max bid numbers
- The estimated gross if you win at each bid level
- A confidence score (80%+ = A-tier, 65–79% = B-tier, below 65% = C or Pass)
- A plain-English summary of the risk factors and what's driving the score

This is also what you're doing during the Section 3 demo with the client — you can run this live and narrate the output.

---

### 3. Scoring a condition report PDF

Once you've identified A-tier units from the batch screen, the next step is to get the actual PDF from Manheim and run it through the engine. This replaces the grade-estimated recon with real line-item costs.

**How:**
1. Download the condition report PDF from the Manheim listing
2. In your session: attach the PDF and say "score this"
3. The engine extracts: condition items and severity, WIP status, title state, key/fob count, tire tread, equipment options, seller announcements (Green Light, warranty remaining, invoice price)

**Why it matters:**
- WIP (Work in Progress) items on the report mean the repair wasn't finished at inspection — the engine applies a risk premium and gives you a higher worst-case bid to protect you
- A Canadian import triggers a $2,500 retail ceiling penalty and usually becomes an auto-Pass
- Structural damage items = automatic Pass, inspect in person before bidding
- Equipment like heated seats, panoramic roof, or remaining HMA warranty adds to the retail ceiling

---

## Understanding the bid output

Every score gives you two numbers, never one:

| | What it assumes |
|---|---|
| **Best-case max bid** | WIP items resolved, low-end recon costs |
| **Worst-case max bid** | WIP risk premiums added, high-end recon costs |

If the gap between best and worst is more than $1,500, the engine flags it and tells you to call the Manheim floor to verify condition before bidding above worst-case.

The default target gross is **$3,500** — which maps directly to DealerKey's guarantee of saving clients at least $3,000 after your $1,500 fee. You can tell the engine to adjust this target if needed.

---

## Seller names to know

The engine automatically classifies sellers. Here's what matters day-to-day:

| Seller | What it means |
|---|---|
| **Hyundai Motor America / co Car** | Best tier — 2 keys included, warranty info in listing, $0 buffer if clean |
| **Hertz Corporation** | Always budget $350 for a missing key/fob. Ask if Green Light is announced. |
| **LDS Church Fleet (Element/CPB)** | Institutional fleet — very clean mechanically, high trust |
| **Driveway Finance / credit unions** | Final sale, no recourse — higher buffer, require PDF |
| **Independent dealers** | Most variable — always get the PDF before bidding |

---

## Connecting it to the client conversation

In **Section 3** of the advisory meeting, when you pull up the website and point to the arbitrage number, the engine is what's behind that number. You can run it live:

> *"Let me actually score this one right now — I'll show you exactly how we'd evaluate it before placing a bid."*

In **Section 5**, when you explain the bidding strategy and ceiling bid, the worst-case max bid is your ceiling. That's the number you're setting with the client before the auction.

The engine's default assumptions (transport $300, flooring $35/day, target gross $3,500) line up with DealerKey's cost structure. You shouldn't need to adjust them.

---

## Quick reference

| Task | What to say |
|---|---|
| Screen a batch | `/arbitrage-engine` → paste CSV |
| Score a single car | `/arbitrage-engine` → paste listing details |
| Analyze a condition report | Attach PDF → "score this" |
| Ask what sells well in a city | `/arbitrage-engine` → "what SUVs rank well in Phoenix?" |
| Get a max bid for a client call | `/arbitrage-engine` → give VIN, MMR, grade, seller |
