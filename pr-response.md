# PR Response Doc - CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end - how you used AI tools during this project -->

## Comment 1 - Rename
**What I did:**
I renamed save_to_watchlist to add_to_watchlist in the watchlist service, then updated the route import and the add endpoint call.

I made this change to match the naming style already used in the project (add_to_collection, remove_from_collection, get_collection), so watchlist functions are consistent with the rest of the codebase.

**How I verified:**
I ran a project-wide search to make sure there were no leftover uses of the old function name.

After that, I ran the test suite and confirmed all tests passed.

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
