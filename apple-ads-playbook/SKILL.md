---
name: apple-ads-playbook
description: Operating playbook for running Apple Ads (Apple Search Ads, ASA) for an app — account setup, campaign structure, keywords and match types, bids and breakeven, negatives, brand defense, custom product pages, placements, reading results, incrementality, scaling, and audits. Use whenever the user mentions ASA, Apple Search Ads, Apple Ads or search ads, or asks to set up, change, review or scale App Store ad campaigns, or asks whether to use Maximize Conversions, the Today tab or other Apple recommendations. Built on Thomas Petit's (@Thomasbcn) public Apple Ads work, 2016–2026.
---

# Apple Ads playbook

You are running Apple Ads for an app developer. This playbook tells you what to do, in what
order, and how to judge the recommendations you get. It is built on ten years of public talks, guides and posts by
Thomas Petit (@Thomasbcn); sources are in `references/sources.md`. Old advice you may find
online is listed in `references/outdated-advice.md`.

Platform facts are dated (checked November 2026). Apple changes Apple Ads often; recheck a
dated fact before acting on it.

## How to work

- **Read before you write.** Pull the current state (campaigns, ad groups, keywords,
  negatives, last 30 days of reports) before proposing anything.
- **The developer approves every change that can spend money**: new keywords, bids,
  budgets, status, targeting, placements. You recommend, with numbers; they decide.
- **Keep a changelog** (date, what, why, expected effect, when to review). Apple's own
  change history is not enough to explain later why something moved.
- **One change round per keyword per day at most.** Bid, pause and status changes take
  hours to reach delivery; intraday data misleads.
- **After a change, re-read the changed entities** and confirm the state.

## 1. Before any spend

- **Attribution must work first.** Apple Ads attribution (AdServices) is free, keyword-level
  and does not need ATT consent. Wire it through RevenueCat, an MMP, or directly. Without it
  you can see installs but not trials, payers or revenue per keyword. Check that the app
  actually sends the token (an SDK call), not only that the dashboard integration is on.
- **Check unit economics.** Know the money a new user brings after Apple's fee and VAT
  (proceeds) per trial start or per payer, by country. If that value is tiny (for example
  under ~$1 per US install for a subscription app), paid acquisition will not work; fix
  monetization first. If reliable cohort proceeds are not yet available, use the capped
  learning workflow A1 below; do not invent an LTV or claim profitable acquisition.
- **Check the store page.** The listing's conversion rate sets what each install costs you.
  A weak page makes every bid expensive; fix the page before buying traffic.
- **Never use Apple Ads Basic.** It hides search terms, which are the most valuable data the
  channel gives. Use Advanced.

## 2. Account structure

Campaigns per country (or per bundle of similar small countries):

| Campaign / ad group | Match type | Purpose |
|---|---|---|
| Brand | Exact | Your own app name and its variants. Cheap, defensive, reported separately. |
| Competitor | Exact | Names of apps people search for instead of yours. Often the largest volume and the highest price. |
| Generic | Exact | Category terms ("budget tracker", "driving test"). |
| Discovery | Broad + Search Match | Find new terms at a low bid. The only place Broad or Search Match is allowed. |

- **Brand and non-brand are two channels.** Never put them in the same goal, CPA or ROAS
  figure. Brand looks excellent because most of those users would have installed anyway;
  blending it hides weak non-brand results.
- **Mirror rule:** every Exact keyword you bid on gets an Exact negative in Discovery,
  brand terms first. Otherwise Discovery bids against your own keywords.
- **One home per keyword.** A keyword lives in exactly one ad group.
- **Split countries** with different prices or monetization. Never bundle a high-price and a
  low-price country. Keep your most important country on its own.
- **Ad groups by intent:** group keywords that share an intent; give high-spend keywords
  their own ad group; keep a catch-all ad group for low-volume long-tail terms until data
  says which deserve their own home.
- **No age, gender or "new vs returning users" targeting to save money.** Targeting applies
  only to users with personalized ads on (about 22%), so it removes most of the audience.
  Apple's "new users only" still lets some existing users through.

## 3. Keywords and search terms

- **Research order, best to worst:** your own brainstorm (plus an LLM), competitors' names
  and keywords, ASO tools, Discovery campaign search terms, Apple's keyword suggestions,
  web keyword tools. Add competitor review mining: users' own words are good keywords.
- **Intent beats volume.** A popular term with the wrong intent wastes money. Check what a
  searcher of that term actually wants.
- **Set the match type explicitly on every keyword.** New keywords default to Broad, and a
  Broad mistake can spend hundreds of dollars in hours. Re-read match types after any bulk
  add.
- **Exact is not exact.** Apple serves close variants, plurals, translations and terms with
  special characters (which are read as spaces and cannot be negated). Read the search-terms
  report for Exact ad groups too, over long date ranges.
- **A different intent gets its own ad group.** If an Exact keyword serves a search term
  with a different intent, move that term to its own ad group and add cross-negatives
  (each one negative in the other's group). Typos and plurals can stay together.
- **Promote discovered terms fast.** A converting Discovery or Broad term moves to Exact
  (plus a Discovery negative) as soon as it appears; only Exact gives clean per-term data.
- **Search Match shows how Apple classifies your app.** Off-topic matches are a metadata
  problem to fix in ASO, not only a negative to add.
- **Estimate volume from delivery,** not from Search Popularity (unreliable since late
  2025): for a term you already serve with ≥ ~10 impressions/day,
  searches ≈ impressions ÷ impression share (all-slots share, because impressions count
  every slot).
- **Cheap low-intent terms are noise.** Low cost per install often means users who never
  pay. Buy them only if they pass the breakeven test below.
- **Go deep on the long tail before adding countries.** Big advertisers skip it. Also bid
  on the second language people search in within a country (for example Spanish in the US).

## 4. Bids and breakeven

- **Bid cost per tap (CPT) on Exact ad groups.** Start near the expected CPT, not at the
  maximum.
- **Breakeven chain** (all money after Apple's fee and VAT, in one currency):
  - Breakeven per trial start = proceeds per trial start from matured cohorts (cohorts old
    enough to have renewed at least once; young cohorts understate value).
  - Breakeven CPT per keyword = breakeven per start × (trial starts ÷ taps) for that keyword.
  - Compare it with the keyword's **actual average CPT** (spend ÷ taps), not with the bid.
    Apple usually charges less than the bid.
  - For apps without a trial, use proceeds per payer × (payers ÷ taps) instead. State the
    payback horizon; do not assume future renewals for a new subscription cohort.
  - Keep a margin: target well below breakeven while data is young.
- **It is not a second-price auction.** A higher bid can buy the same impressions at a
  higher price. When a keyword holds ≥ 80% first-slot impression share, test a lower bid
  (one step of about 20%, held a week) before anything else.
- **Use impression share to find headroom:** low share on a profitable keyword means a
  higher bid can buy volume; ~80–90%+ means it is covered.
- **CPA cap only on Discovery ad groups with volume,** paired with a higher CPT. On stable
  Exact ad groups use plain CPT.
- **Ignore Apple's bid, budget and target-CPA recommendations** as a reason to raise. Apple
  optimizes taps and installs; it cannot see your trials or revenue.
- **Small samples prove nothing.** One or two trial starts on a keyword is not a verdict.
  Pause a keyword with zero starts only once its spend reaches about 2× the breakeven per
  start; below that, judge tap-through rate and tap→install rate, or judge similar keywords
  together. Allow for the funnel's conversion delay and reporting lag before treating
  recent taps as zero-start or zero-purchase failures. The approved loss limit still wins:
  pause spend at the limit and review late conversions afterwards.
- **CPT spikes in December and around Black Friday** are often giant advertisers buying
  broadly. Check impression share and wait before raising bids.

## 5. Maximize Conversions and other Apple recommendations

Apple's sales advice to new advertisers often reads: bid high, try all placements, use
Maximize Conversions. Answer each one this way:

- **Maximize Conversions** (2025) is Search Match with an average CPA target. It optimizes
  installs, cannot see what a user is worth after the install, and does not show which
  search term produced each conversion. Treat it like Broad: a discovery source only, with
  your brand and Exact keywords as negatives, and move what it finds to Exact.
- **"Bid high":** bid by each keyword's breakeven; lower bids where share is already high.
- **"Try all placements":** see section 6.
- **"See the full impact of your ad spend" (view-through):** do not count it; see section 8.

## 6. Placements

- **Search results: yes.** This is where performance lives, on generic and competitor terms.
- **Product page ("You might also like"): optional.** Decent users, very low volume.
- **Today tab and Search tab: no, for a performance budget.** They are awareness placements
  bought by very large brands; direct returns are poor. If a large app tests them, judge on
  install rate and on brand-search lift during on/off periods, never on last click.
- **Second search ad slot** (added March 2026): you cannot bid on it separately. Do not
  compute its share as all-slots minus first-slot share; that metric is wrong.

## 7. Brand defense and the store listing under your ad

- **Always bid on your brand,** at a low bid, in its own ad group. Otherwise a competitor's
  ad sits above your organic result. Brand data also shows how other marketing drives
  searches for your name.
- **Screens 4–6 are your organic listing.** When your ad and your #1 organic result show
  in the same search, Apple shows screenshots 1–3 in the ad and screenshots 4–6 in the
  organic listing (from the default page or from the ad's CPP). So:
  - Have at least 6 portrait screenshots.
  - Make screens 4–6 work alone: no panorama that crosses from screen 3 to 4; an app preview
    video takes a slot.
  - Screens 7+ are almost never seen; put the design effort into 1–6.
- **Use a CPP on the brand ad and design its screens 4–6 for the organic slot.** In one
  public case, a brand CPP with a lower ad tap rate was the real winner: its high-contrast
  screen 4 sent more taps to the free organic listing. It only showed up when taps were
  compared with view-through installs (see section 8). Brand only: on other terms a lower
  ad tap rate weakens your auction position.
- **Keep an in-app event live.** For users who already have the app, the organic result
  shows no screenshots without one, and a competitor's second ad moves above you.
- **Portrait screenshots for most apps.** Landscape takes less height and pulls the next
  results up; it suits games and kids' apps.

## 8. Reading results

- **Tap-through only.** Apple credits two kinds of installs: tap-through (the user tapped
  the ad) and view-through (the user saw the ad, did not tap, installed later). Since 2024
  view-through is included in "total installs", and since 2026 it is the default view in
  Apple's UI. Many view-through users would have installed anyway, especially on brand.
  - Apple Ads API report fields: `tapInstalls` (use), `viewInstalls`, `totalInstalls`.
  - RevenueCat: the attribute `$claimType` is `Click` (tap) or `Impression` (view).
    Exclude `Impression` customers from paid results.
  - If results improved suddenly without a change you made, check this first.
- **The single exception:** brand CPPs (section 7). When your ad and your organic #1 show
  together, taps that move from the ad to the organic listing appear as view-through, so
  look at view-through next to taps when you compare brand CPPs. Never use it for cost or
  verdicts.
- **Compare non-brand CPPs on tap-through only,** and never compare a CPP directly with the
  default page (they serve different audiences); compare CPPs with each other or before
  vs after.
- **Judge on trial starts, payers and proceeds,** not installs. Name the unit every time:
  "cost per tap-through trial start, USD" is not "cost per install".
- **Watch the match rate:** installs your attribution tool assigns to Apple Ads ÷ Apple's
  tap-through installs. 60–90% is normal. A sudden change means measurement changed, not
  performance; explain it before any verdict.
- **Installs greater than taps on a row** means view-through or attribution-window
  effects; read a longer period.
- **Storefront country is not the user's country.** Apple targets by App Store storefront.
  A share of "US" installs can come from other countries at US prices; check revenue by the
  user's real country.
- **Never compare Apple Ads results directly with other ad networks** (they are measured
  differently), and never with brand blended in.

## 9. Incrementality

- **Cannibalization = paying for installs you would have had anyway.** You do not need zero;
  you need to know the size.
- **Test it with pauses:** pause a campaign (brand and non-brand separately) for a week and
  compare total App Store search installs and trial starts with the week before.
- **Apple Ads does not lift organic keyword rank** in any useful way, and does not hurt it.
  An organic ranking problem is an ASO and ratings problem.

## 10. Scaling, in this order

1. Remove self-imposed limits: demographic targeting, very low caps.
2. Raise share of voice on short-tail terms that are under breakeven.
3. Add long-tail and second-language keywords.
4. Add countries where organic users already pay; check the price of the inventory there.
5. Raise bids, keyword by keyword, inside breakeven.
6. Improve relevance: a better page and intent-matched CPPs raise tap-through rate, which
   lowers what each tap costs.

Search volume is finite: duplicating campaigns or raising the budget does not create more
searches. At scale, Apple Ads is usually 5–20% of an app's marketing spend.

## Workflows

### A. Set up a new app
For a tiny budget or insufficient revenue history, use A1 instead of launching every
non-brand campaign category at once.

1. Confirm attribution, unit economics and store page (section 1).
2. Build the keyword list by intent: brand, competitors, generic short tail, long tail.
3. One country first. Create Brand, Competitor, Generic (Exact) and Discovery (Broad +
   Search Match, low bid). Mirror negatives. No demographic targeting. Search results
   placement only. A tight daily budget.
4. First weeks: read tap-through rate and tap→install per keyword; fix the page and title
   from what you learn. No keyword profitability verdicts until starts accumulate.

### A1. Tiny budget or no reliable cohort proceeds
1. Agree a total learning-spend limit, an end date and the event to measure (trial start
   or direct purchase). Confirm attribution and the store page first. With unknown LTV,
   treat bids as experimental costs, not proven breakeven bids; do not scale on that basis.
2. In one country, narrow to roughly 3–10 extremely relevant Exact keywords, often
   medium- or low-volume. Research intent and competitors before spending. Keep brand
   defense separate; this is a narrower non-brand test, not a requirement to fund every
   campaign category in workflow A.
3. Optionally add very cheap Discovery within the same total limit, with brand variants
   and existing Exact keywords excluded. Skip it if it would spread the budget too thin.
4. Read tap-through rate, tap→install and downstream events. Wait for the chosen event's
   conversion delay and reporting lag before judging recent cohorts. Stop spending when
   the approved limit is reached, even if the evidence is still incomplete.
5. Report one of: enough evidence to propose another bounded test, evidence to stop, or
   **insufficient evidence**. A spent credit or one purchase does not establish profitability.
   Do not apply revenue-based stop thresholds when the underlying proceeds are unknown.

Thomas's [small-budget replies](https://x.com/Thomasbcn/status/2120625086034255927) motivate
this narrower start. His [illustrative conversion chain](https://x.com/Thomasbcn/status/2120625558170058838)
was 800 clicks → 400 installs → 60 trials → 20 sales, potentially several hundred US dollars
**per keyword**, before confident measurement. These are not required sample sizes or
benchmarks. The spend limit, delayed-conversion handling and outcome labels above are
operating safeguards, not additional claims attributed to Thomas.

### B. Weekly loop
1. Spend and cost per tap-through start per campaign, brand and non-brand separately.
2. Search terms: Discovery (promote to Exact + negate, or negate) and Exact (split
   different intents).
3. Bids: toward each keyword's breakeven CPT; test lower bids at ≥ 80% first-slot share.
4. Check the match rate and the tap-through setting.
5. Log every change and its review date.

### C. Audit an existing account
Flag each item that is true:
- [ ] Brand blended with non-brand in any goal or report
- [ ] View-through counted in results ("total installs")
- [ ] No working attribution, or match rate unknown or moved without explanation
- [ ] Broad or Search Match outside Discovery; keywords left on Broad by mistake
- [ ] Exact keywords not negated in Discovery
- [ ] Search terms never reviewed on Exact ad groups
- [ ] Maximize Conversions used as a main campaign, or without brand negatives
- [ ] Bids compared with breakeven instead of actual average CPT, or no breakeven at all
- [ ] Bids never tested lower on keywords at ≥ 80% share
- [ ] Demographic or user-type targeting limiting reach
- [ ] Today tab or Search tab spend judged on direct return
- [ ] Fewer than 6 screenshots, or screens 4–6 that do not work alone
- [ ] No in-app event; no brand campaign
- [ ] Apple Ads results compared directly with other networks
- [ ] Verdicts on keywords with too little data
- [ ] No pause test in the last few months

### D. "Should we try X?"
Yes, test it: testing is how an account learns. Maximize Conversions, a new placement,
a new creative format, an Apple recommendation: run it as a proper test so the result means
something.
1. Write the hypothesis and what result would make you keep it.
2. Give it its own campaign or ad group and budget, so it cannot take spend from proven
   keywords, and add your brand and Exact keywords as negatives where match types overlap.
3. Spend enough, for long enough, to measure. A tiny share of the budget for a few days
   proves nothing.
4. Measure beyond last click where the format needs it (trial starts, payers, brand-search
   volume during on and off periods), on tap-through installs.
5. Set an end date, decide keep or stop, and log the result.

For AI bid automation: let it recommend; letting it change every bid on its own needs more
data than a small account produces.

## Recheck these dated facts

- Apple plans to replace the CPA cap with an average CPA (expected around early 2027).
- iOS 27.2 removes the ad background colour in search results (expected December 2026).
  Record tap-through rate, CPT and impression share per keyword before it ships.
- Apple's July 2026 terms allow Apple Ads outside Apple's own apps; no such inventory is
  live yet.
