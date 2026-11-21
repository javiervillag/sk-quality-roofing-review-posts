---
name: sk-quality-roofing-review-posts
description: Turns SK Quality Roofing's newest 5-star reviews (Google and Yelp) into ready-to-post Instagram images (1080x1350) in SK's brand, using SK's own drone photos, with captions and an approval sheet. Already set up with SK's logo, colors, fonts, photos and review links, so there are no setup questions. Use when someone says "make review posts", "new review posts", "turn our reviews into Instagram posts", "post our 5-star reviews", "new reviews for SK", or pastes a review and wants a post. It never posts anything itself.
---

# SK Quality Roofing review posts

You're helping someone on the SK Quality Roofing team, most likely marketing, who is probably **not technical**. Keep messages short and friendly. Never show code, JSON or file paths unless they ask, and do the technical work quietly.

This skill **never posts, publishes or replies** to anything online. The person approves the images and posts them themselves.

Everything is already set up for SK: logo, brand colors (navy and the pink from the logo's heart), fonts (Saira, which matches the logo lettering, and Noto Sans, from the website), background photos (SK's own drone shots) and review links. **Don't ask setup questions.**

## 0. Get the SK folder ready (first time only)

- The tools sit next to this file: `scripts/make_posts.py` and `scripts/fonts/`.
- The ready-made business folder is `business/`. It holds `settings.json`, the logo, `fonts/`, `photos/`, and `seen.json` (the reviews already turned into posts, so they aren't repeated).
- If there's no `SK Quality Roofing Review Posts/` folder in the working folder yet, **copy `business/` there under that name** and use the copy from now on. In Cowork that's the folder the person picked; it's kept for next time.
- If the person attaches a `seen.json` or a `settings.json` from a previous run, put it in that folder first.

Tell the person once, in one line: **"You can change the background photos any time. Just send me new drone or roof photos and I'll swap them in."**

## 1. Get the newest 5-star reviews

Use the links in `settings.json` (`review_links`): Google and Yelp.

**If you have a browser** (Claude in Chrome, or Cowork's browser), open each one and follow `reference/find-reviews.md`. Read only: never click Reply, Like, Report or Write a review. On Google, take about the 20 newest reviews.

**If you can't browse, or a page is blocked:** ask the person to paste the reviews. Say: "Open SK Quality Roofing on Google Maps, click Reviews, sort by Newest, and paste the last few 5-star reviews here."

Rules:
- **5-star only.** Skip 4-star and lower.
- **Skip reviews with no real content**: a few words only, a review written before the job ("haven't had my appointment yet"), or text that is really SK's own reply.
- **Same review in two places:** the same customer sometimes posts on Google and Yelp. Make **one** post, from the Google copy.
- **Photos with reviews:** if the reviewer attached a photo of the work, download the largest version into `photos/from-reviews/` and set `"photo"` on that review. It becomes that post's background. Don't use it if it has text, labels, a watermark or a house number on it (before/after collages usually do); then use the normal rotation and say so in the hand-over.
- Reviews already in `seen.json` are skipped automatically. Say how many were new.

## 2. Write `reviews.json` (you do this; the person never sees it)

The format is at the top of `scripts/make_posts.py`.

- **`quote`** is the full review, word for word. Never fix spelling or grammar, and keep the company name exactly as the reviewer wrote it. If a review misspells the company name (for example "SL Quality Roofing"), skip it rather than print it.
- **`name`** is first name plus last initial ("Sarah M."), capitalized. Skip reviewers whose name is only initials or a nickname, unless the review is especially good.
- **`date`** is "Mon YYYY". It goes in the approval sheet only, not on the image.
- **`excerpt`** (for long reviews) is the 1 to 3 strongest whole sentences, copied word for word.
- **`hl`** is one short phrase shown in pink, copied word for word.
- **`caption`**: under 80 words, warm and plain, family-owned South Florida voice.
  - Thank the reviewer by first name.
  - Mention one specific thing from the review (and the SK team member they named).
  - End with `Free roof estimate: (561) 276-8040` and the hashtags in `settings.json`.
  - Never invent details that aren't in the review.

## 3. Make the images

```bash
pip install pillow   # only if "Pillow is missing" appears
python3 scripts/make_posts.py --folder "SK Quality Roofing Review Posts" reviews.json
```

Layout is automatic:
- Short reviews get the big-quote look.
- Medium reviews get the review-card look ("Client Review").
- Reviews of 70+ words become a 2-slide carousel: best line first, full review second.

If they ask for a different look, add `--template hero|bubble|auto` or `--carousel off|long-only|always`.

**Look at every image before showing it.** Check for cut-off text or an awkward photo crop. To fix a crop, set `photo_focus` for that photo in `settings.json`, then run again.

## 4. Hand over

1. Show the images grouped per post, with each caption underneath.
2. Give them the `posts/<date>` folder to download, zipped if possible. It includes `approval.csv` (opens in Excel or Google Sheets) and `captions.md`.
3. Tell them in plain words:
   - How many posts you made, and how many are carousels
   - Anything that was skipped
   - That nothing was posted, so they review and post themselves
   - The next step: "Next week just say 'make review posts' and I'll grab the new ones."
