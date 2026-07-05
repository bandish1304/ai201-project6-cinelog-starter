# PR Response Doc - CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end - how you used AI tools during this project -->

## Comment 1 - Rename
**What I did:**
Renamed `save_to_watchlist()` to `add_to_watchlist()` in the watchlist service to match CineLog's established `verb_to_noun` naming convention. Updated all call sites in the watchlist route import and POST handler.

**How I verified:**
Ran a project-wide search for both names to confirm there were no leftover `save_to_watchlist` references. Then ran the test suite (`pytest tests/`) and confirmed all existing tests passed.

## Comment 2 - Deduplication
**What I did:**

**How I verified:**

## Comment 3 - Missing test
**What I did:**

**How I verified:**

## Comment 4 - Default visibility
**My position:**

**Reasoning:**

**Tradeoff acknowledged:**

## Comment 5 - Sort order
**My position:**

**Reasoning:**

**Engagement with reviewer's point:**

## Comment 6 - Rebase
**What conflicted:**

**How I resolved it:**

**How I verified no conflict remains:**

## PR Description
<!-- Written at the end - feature overview, design decisions, manual testing steps -->
