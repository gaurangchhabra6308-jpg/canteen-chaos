# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

Delete the example before you submit.



## Example — delete this

### CC-99 — "The cart total is wrong"

**Reproduced:** Added 2 dosas at Rs. 60 each. The cart showed
Rs. 119.99999 instead of Rs. 130. Happened every time, on any dish with
a price ending in .50.

**Cause:** The total was being added up with plain floating point and
never rounded, so 0.1 + 0.2 style errors showed up on screen. The
rounding helper existed but this one place was not using it.

**Fix:** Ran the total through the existing rounding helper instead of
adding a new one, so every price on screen goes through the same path.

**Checked:** Cart, checkout and the order screen all show Rs. 130 now.
Prices without decimals still show without a trailing .00.

**Time:** about 40 minutes, most of it working out that the cart and the
order screen round in different places.



## CC-0X — "<the complaint, in short>"

**Reproduced:**

**Cause:**

**Fix:**

**Checked:**

**Time:**



## Could not fix

For anything you investigated but did not solve. Say what you tried and
where you got to. This is worth marks — leaving it blank when you got
stuck is not.

### CC-0X — "<the complaint>"

**What I tried:**

**Where I got to:**

**What I would try next:**



## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.

## CC-01 — Search suggestions are behind everything

### Reproduction

1. Started the application locally using `npm start`.
2. Opened `http://localhost:3000`.
3. Entered `chi` in the search field so that multiple suggestions appeared.
4. Observed that the lower part of the suggestion dropdown was covered by the category tabs.
5. The covered suggestions could not be interacted with correctly.

### Expected Behavior

The complete search suggestion dropdown should appear above the category tabs, with all suggestions visible and clickable.

### Actual Behavior

The suggestion dropdown was partially covered by the category tabs. The suggestions were present, but the lower portion of the dropdown was obscured.

### Root Cause

The `.search-wrap` element had `z-index: 1`, while the category tabs had a higher stacking level.

Although `.suggest-box` had `z-index: 100`, it was inside the stacking context created by `.search-wrap`. Therefore, the suggestion box could not appear above the category tabs.

### Fix

Changed the `z-index` of `.search-wrap` from `1` to `50` in `frontend/style.css`.

This places the search wrapper above the category tabs while keeping the existing `z-index: 100` of the suggestion box.

### Verification

After the change:

- Searched for `chi`.
- Confirmed that the complete suggestion dropdown appeared above the category tabs.
- Tested multiple suggestions.
- Confirmed that the suggestions were visible and clickable.

### Files Changed

- `frontend/style.css`
- `LOG.md`

## CC-02 — Can't read anything in dark mode

### Reproduction

1. Started the application locally.
2. Opened the menu page.
3. Switched the application to dark mode.
4. Observed that dish names and prices on the menu cards became very difficult to read because they were displayed in a dark color against the dark card background.
5. Switched back to light mode and confirmed that the text was readable.

### Expected Behavior

Dish names and prices should remain clearly readable in both light mode and dark mode.

### Actual Behavior

In dark mode, the dish names and prices used a dark brown text color, which had very low contrast against the dark card background.

### Root Cause

The `.dish-body` element had a hard-coded text color:

    color: #2b2118;

The application already uses the `--ink` CSS custom property for theme-dependent text colors. The light theme defines `--ink` as `#2b2118`, while the dark theme defines it as `#f2e8df`.

The dish name button uses `color: inherit`, and `.dish-name` does not define its own color, so the dish name inherited the hard-coded color from `.dish-body`. Other text inside the card was affected by the same parent color.

### Fix

Changed the `.dish-body` color from the hard-coded `#2b2118` value to:

    color: var(--ink);

This allows the component to use the appropriate text color for the active theme instead of always using the light-theme color.

### Verification

After the change:

- Checked the menu in dark mode.
- Confirmed that dish names were clearly visible.
- Confirmed that dish prices were clearly visible.
- Switched to light mode and confirmed that the existing light-mode appearance remained readable.
- Switched between light and dark modes to verify that the theme-dependent text color changed correctly.

### Files Changed

- `frontend/style.css`
- `LOG.md`