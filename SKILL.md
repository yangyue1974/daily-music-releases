---
name: daily-music-releases
description: >
  Automated daily global new music release digest agent. Collects new album and single releases
  worldwide, filters by user-specified genres, translates into the user's preferred language,
  and delivers a structured email digest on a recurring schedule. Use when the user wants to
  track new music releases, set up a daily music newsletter, or monitor album drops across genres.
  Supports customizable genres, languages, and delivery schedules.
license: MIT
metadata:
  author: yangyue1974
  version: '1.0'
  tags: music releases albums newsletter automation email digest
---

# Daily Music Releases Digest

An automated agent skill that collects global new music releases daily, filters by genre, translates into the user's language, and delivers a digest via email.

## When to Use This Skill

Use this skill when the user asks to:

- Track or monitor new music/album/single releases
- Set up a daily or weekly music release newsletter
- Get notified about new albums in specific genres
- Build an automated music digest delivered by email
- Stay up to date with global music releases

## Configuration

Before setting up the recurring task, collect the following from the user. If not specified, use the defaults.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `GENRES` | List of genres to include | All genres (no filter) |
| `LANGUAGE` | Language for the digest content | User's primary language |
| `EMAIL` | Recipient email address | User's email from context |
| `SCHEDULE` | Delivery time and frequency | Daily at 12:00 noon, user's local timezone |
| `SOURCES` | Music data sources to search | Billboard, Pitchfork, NME, Spotify, Apple Music |

### Supported Genres

The following genres are recognized. Users may pick any subset:

- Alternative
- Rock
- Pop
- Gospel
- Metal
- Indie Folk
- Ambient
- Hip-Hop / Rap
- R&B / Soul
- Electronic / Dance
- Country
- Jazz
- Classical
- K-Pop
- J-Pop
- Latin
- Reggae / Dancehall
- Blues
- Punk
- World Music

Users can also specify custom genre keywords not on this list.

## Instructions

### Step 1: Gather User Preferences

Ask the user for their preferences using the configuration table above. At minimum, confirm:

1. **Genres**: Which genres to track (or all).
2. **Language**: What language the digest should be written in.
3. **Email**: Where to send the digest.
4. **Schedule**: When and how often (daily/weekly, what time, what timezone).

### Step 2: Calculate the Cron Schedule

Convert the user's desired delivery time to UTC for the cron expression.

```python
from datetime import datetime, timezone, timedelta

# Example: User wants noon delivery in Asia/Shanghai (UTC+8)
user_offset_hours = 8  # Adjust to user's timezone offset
user_hour = 12         # Desired local hour

utc_hour = (user_hour - user_offset_hours) % 24
cron_expression = f"0 {utc_hour} * * *"
# Result: "0 4 * * *"
```

Common timezone offsets:
- US Eastern (EST): UTC-5 / (EDT): UTC-4
- US Pacific (PST): UTC-8 / (PDT): UTC-7
- Central European (CET): UTC+1 / (CEST): UTC+2
- China Standard (CST): UTC+8
- Japan Standard (JST): UTC+9
- India Standard (IST): UTC+5:30

### Step 3: Create the Recurring Task

Use `schedule_cron` to create the automated task. Below is the task template — fill in the placeholders with user preferences.

```
schedule_cron(
  action="create",
  name="Daily Music Releases Digest",
  cron="{CRON_EXPRESSION}",
  exact=true,
  background=true,
  task="""<see Task Execution Prompt below>"""
)
```

### Task Execution Prompt

Use the following prompt as the `task` parameter. Replace `{GENRES}`, `{LANGUAGE}`, `{EMAIL}`, and `{SOURCES}` with the user's configuration.

---

You are a global new music release intelligence agent. Your job is to collect today's new album and single releases, filter by genre, translate into {LANGUAGE}, and email a digest.

**GENRE FILTER:**
Only include releases from these genres: {GENRES}.
If "All genres" is specified, include everything but still categorize by genre.
Discard any releases that do not match the specified genres.

**STEP 1 — Research new releases:**

Use search_web with multiple queries to maximize coverage:

- "new album releases this week {current_date}"
- "new music releases today {current_date}"
- "new {genre} album releases this week" (one query per genre)
- "Spotify new music friday"
- "Apple Music new releases this week"

Also search genre-specific and regional sources:
- For Rock/Metal/Punk: search Pitchfork, Metal Injection, Kerrang
- For Pop: search Billboard, Rolling Stone
- For Alternative/Indie: search Pitchfork, Stereogum, Bandcamp
- For Gospel: search Gospel Music Association, CCM Magazine
- For Ambient/Electronic: search Resident Advisor, Bandcamp ambient tag
- For Folk/Indie Folk: search Folk Radio UK, No Depression

Try to also use fetch_url on key new-release pages:
- https://pitchfork.com/reviews/albums/ (latest reviews often coincide with release dates)
- https://www.billboard.com/new-music/ (if available)

**STEP 2 — Compile and filter:**

For each qualifying release, collect:
- Artist name (original script + {LANGUAGE} translation/transliteration)
- Album or Single title (original + {LANGUAGE} translation)
- Genre (from the allowed list)
- Release date
- Brief description: 1-2 sentences in {LANGUAGE}
- Notable tracks (if found)

**STEP 3 — Organize by genre:**

Group releases under genre headings. Use these emoji labels:
- 🎸 Alternative
- 🎸 Rock
- 🎵 Pop
- ✝️ Gospel
- 🤘 Metal
- 🪕 Indie Folk
- 🌊 Ambient
- 🎤 Hip-Hop / Rap
- 🎶 R&B / Soul
- 🎧 Electronic / Dance
- 🤠 Country
- 🎷 Jazz
- 🎻 Classical
- 💜 K-Pop
- 🌸 J-Pop
- 💃 Latin
- 🌍 World Music
- 🎹 Blues
- ⚡ Punk
- 🎼 Reggae / Dancehall

Only include sections that have releases. Skip empty genre sections.

**STEP 4 — Format the email:**

Write the email body in PLAIN TEXT only (no HTML, no Markdown). Use line breaks, dashes, and spacing for visual structure. All descriptive text must be in {LANGUAGE}. Artist and album names should appear in both their original language and {LANGUAGE}.

Structure:
```
==============================
🎵 Daily Music Release Digest
   {date in localized format}
==============================

[Genre Emoji] [Genre Name]
------------------------------
1. Artist Name (translated name)
   Album/Single: "Title" (translated title)
   Release Date: YYYY-MM-DD
   Description: ...
   Notable Tracks: ...

2. ...

[Next Genre]
------------------------------
...

==============================
Total: X new releases today
==============================
```

**STEP 5 — Send the email:**

Use the send_email tool (source_id: "gcal") with:
- action: "send"
- to: ["{EMAIL}"]
- subject: "🎵 Daily Music Release Digest - {date}"
- body: the compiled digest
- in_reply_to: null
- user_prompt: null

---

### Step 4: Confirm to the User

After creating the cron job, confirm:
- The genres being tracked
- The delivery schedule (in the user's local time)
- The recipient email address
- That they can ask to modify genres, schedule, or stop the task at any time

## Examples

### Example 1: Chinese User, Selected Genres

**User says:** "帮我做一个每天自动搜集全球新唱片上市信息的智能体，只要 Alternative、Rock、Pop、Metal 这几个风格，翻译成中文，每天中午发送到我的邮箱。"

**Agent actions:**
1. Genres: Alternative, Rock, Pop, Metal
2. Language: Chinese (中文)
3. Email: (from user context)
4. Schedule: Daily at 12:00 noon, user's timezone
5. Calculate cron: `0 4 * * *` (for UTC+8)
6. Create cron with the task prompt, substituting parameters

### Example 2: English User, All Genres, Weekly

**User says:** "Set up a weekly new album digest every Monday morning at 9am, covering all genres."

**Agent actions:**
1. Genres: All genres
2. Language: English
3. Email: (from user context)
4. Schedule: Weekly on Mondays at 9:00 AM, user's timezone
5. Calculate cron: `0 14 * * 1` (for US Eastern, EDT)
6. Create cron with the task prompt

### Example 3: Minimal Request

**User says:** "I want to get notified about new indie folk and ambient releases."

**Agent actions:**
1. Confirm: genres = Indie Folk, Ambient
2. Ask: preferred language, email, schedule
3. Set up with confirmed parameters

## Modifying the Task

Users may request changes after setup:

- **Add/remove genres**: Update the cron task with the revised genre list
- **Change schedule**: Recalculate the cron expression and update
- **Change language**: Update the task prompt with the new language
- **Pause/stop**: Delete the cron job using `schedule_cron(action="delete", cron_id="...")`
- **Change email**: Update the task with the new recipient address

Always use `schedule_cron(action="update", ...)` to modify an existing task rather than deleting and recreating.

## Notes

- The skill relies on web search to find release information; coverage depends on search result quality.
- For best results, the task searches multiple authoritative music sources (Billboard, Pitchfork, NME, Spotify, Apple Music, genre-specific sites).
- Some niche genres (e.g., Ambient, Gospel) may have fewer results on any given day.
- The agent will only send an email if it finds at least one qualifying release.
- Email is sent via the user's connected Gmail (source_id: "gcal"). The user must have Gmail connected.
