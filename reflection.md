# AI-Assisted Debugging Reflection — Playlist Chaos Project

## 1. Issue We Chose to Fix

The main issue I worked on was inconsistent and incorrect playlist behavior in the classification and statistics logic. Specifically, songs were sometimes misclassified into the wrong categories (Hype, Chill, Mixed), and playlist stats were not correctly aggregating across all songs.

### What I observed in the app
- Playlist stats were inconsistent (total count and average energy did not match expectations).
- Some songs were being incorrectly categorized as "Hype" even when they had low energy.
- The “lucky_pick” feature was not considering all playlists equally in its default behavior.

### Where I looked in the code
I traced the issue to `playlist_logic.py`, focusing on:
- `classify_song()` → where category assignment logic happens
- `compute_playlist_stats()` → where totals and averages were calculated
- `lucky_pick()` → where songs were selected based on mode

### Steps to the final fix
1. Identified that stats were incorrectly using only the "Hype" playlist instead of all songs.
2. Fixed aggregation logic to consistently use `all_songs`.
3. Expanded `lucky_pick()` to include the "Mixed" playlist.
4. Refined classification logic to prevent low-energy songs from being forced into "Hype" due to genre matching.

---

## 2. Using the AI Coding Assistant

### How I decided what to ask
I used the AI assistant in a step-by-step debugging way:
- First, I asked it to interpret each diff hunk during staging.
- Then I asked for commit message suggestions to better understand the intent of each change.
- Finally, I asked clarification questions when logic felt inconsistent or unintuitive.

### Did the AI understand the context?
Mostly yes, especially when I provided full diffs. It was good at identifying:
- Whether a change fixed a bug or just refactored logic
- Whether a function behavior became safer or more consistent

However, I still had to:
- Confirm assumptions (especially around classification rules)
- Double-check suggestions that seemed too generic or slightly misaligned with the spec

### Moments where I didn’t fully trust the AI
- When it interpreted logic changes too broadly (e.g., assuming intent without referencing the spec)
- When multiple fixes were involved, it sometimes oversimplified the impact

In those cases, I relied on my own understanding of the code and assignment spec before staging changes.

---

## 3. Refactor Discussion

### Did the AI suggest a good structure?
Yes, in most cases the AI helped identify cleaner structure, especially:
- Consistently using `all_songs` for stats
- Making playlist selection logic more complete in `lucky_pick`

### Did I reject or edit suggestions?
Yes:
- I did not blindly accept all classification logic interpretations
- I ensured that changes matched the intended spec rather than just “making sense logically”

### How I confirmed the refactor was safe
- I reviewed each diff hunk before staging
- I checked that changes were consistent across functions
- I ensured no playlist category was unintentionally excluded
- I mentally traced edge cases (empty lists, missing categories, low-energy songs)

---

## 4. Testing Approach

### What I tested
- Checked playlist stats before and after changes
- Verified classification behavior for edge cases (low energy vs high energy songs)
- Ensured `lucky_pick()` still returned valid songs across all modes

### New issues that appeared
- Initially, classification logic felt inconsistent due to overlapping conditions
- Needed to ensure “Mixed” playlist inclusion did not unintentionally dominate selection

Overall, no major breaking issues were introduced after fixes.

---

## 5. Group Insight About AI Debugging

One key insight we would share:

> AI is most effective when used for *incremental validation of changes*, not for fully trusting high-level conclusions.

In practice, the best workflow was:
- We broke problems into small diffs
- Asked the AI to interpret each change
- Used it to sanity-check behavior
- But always verified against the spec and real code logic ourselves

The biggest takeaway is that AI is a strong “second reviewer,” but not a replacement for understanding the system and its requirements.