# Data Quality & Corrections Log

This log documents substantive corrections made during a structured review of the
analysis notebooks — cases where a markdown finding misstated a number, mislabeled
a metric, or drew a conclusion the underlying data didn't actually support. It's
included here deliberately: catching and correcting these issues was part of the
analytical process, and the review methodology (re-deriving every claim from its
source figures before accepting it) is as much a part of this project as the
final findings themselves.

Each entry describes what the finding claimed, what the data actually showed, and
the correction made.

---

## 01_data_collection.ipynb

- **Stale cross-reference.** Cell 0 pointed readers to `03_analysis.ipynb` for the
  exploratory analysis and visualizations; corrected to `04_analysis.ipynb`, the
  notebook that actually contains them.
- **Heading level inconsistency.** Cell 10 ("Data Quality Checks") was set two
  levels below the title instead of one, breaking the outline structure used by
  sections 1–2 in the same notebook; corrected from `##` to `###`.
- **Title level inconsistency.** The notebook's main title (cell 0) was one level
  lower than the title style used in the other three notebooks; corrected from
  `##` to `#`.

## 02_cleaning.ipynb

- **Incomplete sentence.** Cell 14 ("Finding: Penetration Cross-Check") cut off
  mid-thought at "...while keeping," leaving the decision it was documenting
  unstated. Completed so the cell ends on a full sentence.

## 03_uae_data_collection_cleaning.ipynb

- **Row-count error.** Cell 17's summary stated "6 individual lines, 3 subtotals,
  1 grand total" as 14 rows — the three figures actually sum to 10. Corrected to
  "10 individual lines, 3 subtotals, 1 grand total."
- **Overstated validation claim.** Cell 13 ("Cross-Source Validation") concluded
  that cross-checking Alpen Capital and CBUAE against each other "confirms the
  UAE figures used throughout this project are consistent regardless of which
  source is referenced." The underlying check only compared *total* UAE market
  size (a 0.6% difference — genuinely close); it never checked the life/non-life
  split, which turned out to disagree substantially between the two sources (see
  below). Narrowed the claim to what was actually validated: "confirms the two
  sources agree closely on total UAE market size."
- **Unreported source discrepancy.** Added a new paragraph to cell 13 documenting
  a gap the original validation missed entirely: Alpen Capital reports UAE's life
  insurance share at 17.1% of total GWP, while recalculating the same ratio
  directly from CBUAE's line-of-business data gives 11.2% — a 6-percentage-point
  gap between two sources this project otherwise treats as mutually validating.
  The cause isn't identifiable from either source's public documentation, so the
  gap is now flagged rather than left unreconciled and unmentioned.

## 04_analysis.ipynb

**Chart accuracy**
- **Treemap data bug.** The UAE market composition treemap used `itertuples()`
  indexing that displayed 2024 values mislabeled as 2025. Fixed with `.iloc`-based
  access and added a verification assertion (cell 24) to catch a recurrence.

**Claims not supported by the data**
- **Motor vs. Fire.** Cell 30 claimed Motor & Transportation "overtook Fire as the
  largest non-life line starting in 2023." Motor was actually ahead of Fire from
  2021, the first year in the dataset — there was no overtaking event. Corrected
  to state Motor "has been the largest non-life line throughout the period, ahead
  of Fire in every year from 2021 onward."
- **Saudi Arabia growth claims.** Cells 8 and 37 asserted Saudi Arabia had "grown
  fastest since 2019" and cited a "markedly higher premium growth trajectory" —
  neither is supported by any time-series or CAGR data in the dataset, which only
  contains a single-year (2025) snapshot for Saudi Arabia. Both claims removed.
- **Bahrain's GDP per capita and volatility.** Cell 14 claimed Bahrain had "the
  smallest absolute GDP per capita of the six" (Oman's is actually lower — and
  this directly contradicted cell 17 elsewhere in the same notebook) and showed
  "minimal volatility" (Bahrain's data shows year-over-year swings of -10.3%,
  -10.7%, and -3.9%). Replaced with an accurate, verifiable comparison: UAE, Saudi
  Arabia, and Bahrain are the only three GCC markets that end 2025 net higher
  than their 2010 starting point.
- **Saudi Arabia's penetration/density ranking.** Cell 5 claimed Saudi Arabia
  "ranks near the bottom on both metrics." It actually ranks 3rd of 6 on both
  penetration (1.64%) and density (USD 582), ahead of half the region on each.
  Corrected to describe it accurately as mid-table, with the specific rankings
  shown.

**Direction and magnitude errors**
- **Life insurance's actual trend.** Cell 27 stated Life insurance "grew more
  modestly" than the other segments. Life insurance's total (from the
  USD-converted CBUAE data) actually declined 6.4%, from USD 2,442M to USD
  2,287M, driven by Annuities (-73.5%), Group Credit Life (-24%), and Individual
  Life (-12.4%), only partly offset by Group Life's growth (+73.9%). Corrected to
  state the decline directly, with the sub-line detail that explains it.
- **"More than doubling."** Cell 27 described Health insurance's growth (USD
  5.4B to USD 9.9B) as "more than doubling" — the actual increase is 83%.
  Reworded to state the correct figure.
- **Rounding.** Total market growth (USD 12,067.26M to USD 20,378.65M) is 68.9%,
  which rounds to 69%, not the 68% stated in cells 27 and 37. Corrected in both.
- **Group Life's base figure.** Cell 33 stated Group Life's 2021 value as USD
  228M; the precise figure is USD 227M, which also shifts the stated growth rate
  from +73% to +74% to stay consistent with the corrected base.

**Terminology and internal contradictions**
- **"Absolute growth" misapplied to a percentage.** Cell 37 described Motor as
  leading on "absolute growth (+113%)," but 113% is a percentage rate, not a
  dollar (absolute) figure — and if read literally as a dollar comparison, it's
  wrong: Health's actual dollar growth (USD 4,508M) is roughly 3x Motor's (USD
  1,455M). Reworded throughout to separate the two claims correctly: Health leads
  on absolute (dollar) growth and size; Motor leads on percentage growth and
  CAGR — the only line to lead on both rate measures.
- **Health/Motor CAGR contradiction in the Recommendation.** The Recommendation
  claimed Health and Motor "both" lead on CAGR, directly contradicting cell 36's
  own finding that Health's CAGR (16.4%) trails Motor (20.8%), Fire (19.8%), and
  Other (17.1%). Reworded to attribute size-leadership to Health and rate-and-CAGR
  leadership to Motor, consistent with cell 36.
- **Duplicated heading.** Cell 37 repeated the line "UAE Deep Dive (Section 3):"
  twice in a row; removed the duplicate.
- **Section numbering.** The notebook's own outline (cell 0) labels "UAE Deep
  Dive" as Section 3, but the "3." prefix had been placed on cell 22 ("UAE Market
  Composition, 2025") instead of cell 23 ("UAE Deep Dive — Product Line Trends"),
  the section the outline actually describes. Moved the numbering to match.
- **Broken cross-reference.** Cell 21's caveat about the life-share discrepancy
  pointed readers to the Cross-Source Validation section in
  `03_uae_data_collection_cleaning.ipynb` — but that section only validates total
  market size, not the life/non-life split, so the pointer led nowhere useful.
  Repointed to the Limitations section in cell 37, where the discrepancy is
  actually discussed.
- **Population growth figure.** The Limitations section (cell 37) stated UAE's
  most recent single-year population growth as "4.7% in 2024"; the precise value
  (4.79%) rounds to 4.8%. Corrected.

**New disclosures added**
- **Life-share cross-source discrepancy.** Added a caveat to cell 21 (and the
  Limitations section in cell 37) flagging that Alpen Capital's regional life
  insurance share for UAE (17.1%) doesn't reconcile with CBUAE's more granular
  UAE-specific data (~11.2%) — a gap that directly affects one of the two
  headline reasons given for prioritizing UAE ("most diversified product mix in
  the region"), and previously went unmentioned anywhere in the analysis.

---

## Presentation deck

The findings above were also used as source material for
`GCC_Insurance_Market_Analysis.pptx`. Every correction with a deck-facing
equivalent was propagated to the corresponding slide (market composition
figures, growth percentages, the Motor/Fire comparison, the Health/Motor
CAGR framing, and the life-share caveat), and the treemap visual was
re-exported at full resolution to reflect the corrected underlying data.
