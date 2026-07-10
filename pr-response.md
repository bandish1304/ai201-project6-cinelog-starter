# PR Response Doc - CineLog Watchlist Feature

## AI Usage
I used AI as a devil's advocate after writing my draft responses for Comments 4 and 5.

What I asked: what counterargument would a careful reviewer raise, and what tradeoff might I be underweighting.

What changed: I expanded both responses to acknowledge privacy and usability risks more directly, and I added a clearer mitigation/future option where appropriate.

## Comment 1 - Rename
**What I did:**
I renamed save_to_watchlist to add_to_watchlist in services/watchlist_service.py.

Then I updated the matching call sites in routes/watchlist/watchlist.py (both the import line and the add endpoint function call).

I made this change to match the naming style already used in the project (add_to_collection, remove_from_collection, get_collection), so watchlist functions are consistent with the rest of the codebase.

**How I verified:**
I used a project-wide search for save_to_watchlist and add_to_watchlist to confirm every reference was updated and there were no missed call sites.

After that, I ran the full test suite (pytest tests/ -v) and confirmed all tests passed.

## Comment 2 - Deduplication
**What I did:**
I added a duplicate check inside add_to_watchlist before creating a new row. The function now looks for an existing watchlist entry for the same user and film, and if it finds one, it raises AlreadyInWatchlistError instead of creating a second record.

I based this directly on the same pattern in add_to_collection inside services/collection_service.py: lookup first, raise a specific duplicate exception, then only insert when no prior entry exists.

**How I verified:**
I verified the logic path in code to make sure the duplicate lookup happens before db.session.add and db.session.commit.

I also ran pytest tests/ -v and confirmed the full suite still passed after the deduplication change.

## Comment 3 - Missing test
**What I did:**
I created tests/test_watchlist.py and added a watchlist version of the nonexistent film test.

I modeled it specifically after test_add_to_collection_nonexistent_film_raises in tests/test_collection.py, using the same fixture layout and the same pytest.raises(FilmNotFoundError) assertion style.

**How I verified:**
I ran pytest tests/test_watchlist.py -v and confirmed the new test passed.

I then ran pytest tests/ -v to confirm the new test integrates cleanly with the rest of the suite.

## Comment 4 - Default visibility
**My position:**
I support keeping the default as public=True for watchlist entries.

**Reasoning:**
The behavior I am optimizing for is low-friction sharing. In this app, watchlists are part of the social value: people discover films through each other, and most users who add a film are not trying to hide that action. If the default is private, shared watchlists stay mostly empty unless users actively change settings every time, which creates a lot of silent friction and lowers discoverability.

I also considered the interaction cost. Public-by-default means the common action is one click (add film), while private-by-default requires an extra choice on every add for users who want to participate socially. Since this is a community film tracking app, I think defaulting toward participation is the better product fit.

**Tradeoff acknowledged:**
The downside is that some users will expect watchlist actions to be private unless they opt in, and public-by-default can surprise privacy-sensitive users. Private-by-default would better protect that expectation. To balance this, I would pair public=True with clear UI/API messaging and an easy per-entry visibility toggle so users can switch to private immediately when needed.

## Comment 5 - Sort order
**My position:**
I want to keep alphabetical order as the default for now.

**Reasoning:**
The main watchlist behavior I am optimizing for is browseability when users return to a long list and ask, "What do I want to watch tonight?" In that moment, alphabetical order is predictable and makes scanning easier, especially when the user only remembers part of a title.

I also wanted to keep behavior stable for this PR instead of changing interaction patterns and naming in the same review cycle. Since this branch is already making watchlist service and test changes, I think preserving current list ordering reduces accidental product change risk.

**Engagement with reviewer's point:**
I agree with the maintainer's core point that date-added order better surfaces recent intent and can feel more "alive" as users add films. That is a valid product argument, and I think it would likely be better than alphabetical for users who treat watchlist as a short-term queue.

The tradeoff is that date-added can make refinding an older specific title harder in larger lists. My proposed follow-up is to support both orders with an explicit sort parameter (for example: title or date_added), while keeping one documented default. That gives us maintainable API behavior and lets us test which default users actually prefer.

## Comment 6 - Rebase
**What conflicted:**
After rebasing on origin/main, the watchlist feature had a UUID migration mismatch. Main had already moved film IDs to UUIDs, but my watchlist side still had pre-refactor assumptions in a few places. The biggest break was that WatchlistEntry was missing from models.py after replaying commits, which caused an import error in watchlist_service.

**How I resolved it:**
I rebased the branch onto origin/main, then resolved the watchlist/model mismatch by restoring WatchlistEntry in models.py with film_id as a String(36) foreign key to film.id.

I also updated watchlist references that still described film_id as an integer so the service and route docs match the UUID schema.

**How I verified no conflict remains:**
I ran the full test suite (pytest tests/ -v) and confirmed all tests passed.

I checked for merge commits with git log --merges --oneline and got no output, confirming the branch history is linear after rebase.

## PR Description
<!-- Written at the end - feature overview, design decisions, manual testing steps -->

## Commit History Screenshot
I ran git log --oneline and confirmed the branch has conventional commit messages, at least 4 separate commits, and no merge commits.

![Git log oneline history](../project6.png)
