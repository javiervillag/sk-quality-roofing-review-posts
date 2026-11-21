# Finding the newest reviews in a browser

These steps work with any browser tool: Claude in Chrome, Cowork's browser, or ChatGPT's agent browser.

General rules:
- Work in a new tab, and close it when you're done.
- **Read only.** Never click Reply, Like, Share, Report, Flag, or "Write a review".
- Prefer `get_page_text`, `find` and `read_page` over screenshots. Use `javascript_tool` only to read the page, never to change it.
- Collect about the 10 newest reviews from each source, then stop.
- **Verification check** ("Just a moment…", "Press and hold", a CAPTCHA): wait 10 seconds once. If it's still there, **stop**: never try to solve or get around it. Ask the person to open the page and paste the reviews instead.
- From each review, record the name, date, star rating, full text, and any photos attached.

## Finding the links (when the person doesn't have them)
- `brand_kit.py` lists the review links it found on the website (`review_and_social_links`).
- **Google:** search Google Maps for the business name plus city. The link is the business's Maps page.
- **Yelp, BBB, Trustpilot, Houzz, Angi:** use the site's own search box with the business name plus city.
- Confirm the address or phone number matches before using a page. Many businesses have similar names.

## Google Maps
1. Open the link. If a consent banner shows, choose **Reject all**.
2. Click the **Reviews** tab. `find` "Reviews for" locates it.
3. Click **Sort**, then **Newest**.
4. **Scroll the left panel**, not the map, about 5 ticks with the pointer inside it. Review cards only load after you scroll.
5. Click every **More** button to expand long reviews.
6. Read the text with `get_page_text`, or `javascript_tool` on `document.querySelector('[role="main"]').innerText`. Each card reads in this order:
   - the name
   - "N reviews" (the reviewer's own count, so ignore it)
   - the relative date ("3 weeks ago")
   - the review text
7. **Stars:** the card text doesn't include them. Use `read_page` on the card (look for "5 stars"), or zoom a screenshot on the star row.
8. **Photos:** thumbnails under the text. Click one and take the largest version: the image URL with `=w2400` in place of the size suffix.
9. Leave out "Response from the owner". If the text says "Translated by Google", use the original text or skip the review.

## Yelp
1. Open the business page and scroll to **Reviews**. Pick **Sort by: Newest first**.
2. Read with `get_page_text`. The star rating is in each card's `aria-label` (for example "5 star rating"). Use `read_page` or `find` "5 star rating".
3. Click "Read more" where it appears.
4. Leave "Not Recommended" reviews alone.

## BBB
1. Open the profile, then **Customer Reviews**. Pick **Most recent**.
2. Read with `get_page_text`. Each review shows "Review from", the name, "Date: MM/DD/YYYY", then "5 stars" and the text.
3. Drop any leading "?????": that's emoji stars, not words.
4. **Hidden names:** BBB shows staff names as "REMOVED". Keep it exactly as written in `quote`, and handle it with `edits` (see SKILL.md).
5. Complaints live on a separate tab. Ignore them.

## Facebook
1. Open the page, then click **Reviews**. Facebook shows **"recommends"** or **"doesn't recommend"**, not stars.
2. Only use "recommends" posts, and only if the person agreed to include Facebook. Set `"source": "Facebook"`.
3. If you hit a login wall, ask the person to paste the reviews instead.

## Trustpilot
1. Open the business page and sort by **Most recent**, or use the 5-star filter.
2. Each card has the stars in an `img` `alt` ("Rated 5 out of 5 stars"), a title, and the text. Use the text, and leave the title out unless it's a full sentence.

## Houzz, Angi, Nextdoor, HomeAdvisor, Thumbtack, Tripadvisor
Same pattern as the others:
1. Find the reviews list and sort by newest if you can.
2. Expand "More".
3. Read the name, date, rating and full text.

The stars are usually in an `aria-label` or `alt` text.

## Testimonials on the business's own website
- Often a review widget (Trustindex, Elfsight, EmbedSocial) that **copies Google reviews**. If so, use `"source": "Google"` and check for duplicates against Google.
- **Stars are often images.** Count the filled ones. For example, Trustindex uses `/star/f.svg` for filled and `/star/e.svg` for empty, so a 1-star review still shows five star shapes.
- **Image testimonials** (a screenshot or card with text): read the text from the image, check it word for word, and use the source the image shows.
- A testimonial with no stars (a plain quote) counts only if the person says so. Use `"source": "Website"`.
