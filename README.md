# TravelLog MCP server

TravelLog is a trip planner and travel log. Plan trips day by day, log where you've been, and keep a list of places
you want to go.

**This is a hosted MCP server, not open source.** TravelLog runs it for TravelLog users. This repository holds
documentation only: there is no server code here to install or run. You connect your AI assistant to the hosted
address below and sign in with your TravelLog account.

```
https://travellogapp.web.app/mcp
```

## Contents

- [What it does](#what-it-does)
- [Set up your assistant](#set-up-your-assistant)
  - [Claude](#claude)
  - [ChatGPT](#chatgpt)
  - [Grok](#grok)
  - [Other MCP clients](#other-mcp-clients)
  - [Your own bot: access keys](#your-own-bot-access-keys)
- [Sign-in](#sign-in)
- [Permissions](#permissions)
- [Tools](#tools)
- [Limits](#limits)
- [Undo and disconnect](#undo-and-disconnect)
- [Privacy](#privacy)
- [Tested with](#tested-with)
- [Support](#support)

## What it does

Once connected, your assistant can read your TravelLog trips: each trip's itinerary, trip and day notes, expenses,
a travel history summary (past trips, days away, countries and cities) and your Been and Want to go places lists.
If you allow it, it can also create trips, add its ideas to a trip as suggestions you add or dismiss, add bookings,
entries, expenses, day notes and places you ask for, import past trips in bulk, and edit or remove itinerary
entries.

Its own ideas never go straight into your plan: they arrive as suggestions labelled "From <assistant name>", and
nothing becomes a real entry until you add it (in the app, or by asking the assistant to). The assistant is told to
add things directly only when you asked for that exact thing, and to ask you before it edits or removes anything.

Example: ask "What's on my Lisbon trip tomorrow?" and the assistant reads that day of the trip's itinerary.

The full tool reference is on the TravelLog website: <https://travellogapp.web.app/mcp-docs>.
Setup help for people (not developers) is at <https://travellogapp.web.app/connect-ai>.

## Set up your assistant

Menus change from time to time, and custom connectors may need a paid plan. If a step doesn't match, look for
"connectors" or "MCP" in your assistant's settings.

### Claude

1. In Claude, open **Settings › Connectors** and choose **Add custom connector**.
2. Paste the address and choose **Add**: `https://travellogapp.web.app/mcp`
3. Claude opens the TravelLog sign-in page. Sign in and allow access.

### ChatGPT

1. In ChatGPT, open **Settings › Connectors** and turn on **developer mode**.
2. Add a custom MCP server with the address: `https://travellogapp.web.app/mcp`
3. ChatGPT opens the TravelLog sign-in page. Sign in and allow access.

Developer mode depends on your ChatGPT plan.

### Grok

1. On grok.com, add a custom connector.
2. Paste the address: `https://travellogapp.web.app/mcp`
3. Grok opens the TravelLog sign-in page. Sign in and allow access.

The exact menu names on grok.com have not been checked by the TravelLog team.

### Other MCP clients

| Setting   | Value |
|-----------|-------|
| Address   | `https://travellogapp.web.app/mcp` |
| Transport | Streamable HTTP |
| Sign-in   | OAuth 2.1 with PKCE. Your client registers itself (dynamic client registration or a client ID metadata document), so you don't need a client ID or secret. Or use an [access key](#your-own-bot-access-keys). |
| Metadata  | `https://travellogapp.web.app/.well-known/oauth-protected-resource/mcp` |
| Protocol versions | `2026-07-28`, `2025-11-25`, `2025-06-18` |

### Your own bot: access keys

For a tool that can't open a sign-in page (for example a bot you build on the xAI Grok API), create a personal
access key at <https://travellogapp.web.app/account> or in the TravelLog app under **Settings › Connected apps ›
Create access key**:

- Name it, choose **Read only** or **Read & write**, and choose when it expires: 30 days, 90 days (the default),
  1 year or never.
- The key starts with `tlpat_` and is shown once. Copy it then; TravelLog stores only a one-way hash of it.
- Give your tool the address above and send the key as a bearer token (`Authorization: Bearer tlpat_…`), or paste
  it where the tool asks for an API key or token.
- You can have up to 5 active keys and revoke any of them at once. Whoever holds a key can use your TravelLog as it
  allows, so keep it secret.

### No TravelLog account? Let your assistant create one

Add this address instead, with no sign-in:

```
https://travellogapp.web.app/mcp?signup=1
```

Your assistant asks you for the same things TravelLog asks at sign-up (your email, the name others see, a unique
@handle, and whether you accept the Terms of Service and Privacy Policy and are 18 or older) and may answer yes only
if you told it to. TravelLog then emails you a verification link: tapping it is the only thing you do. The assistant
then receives an access key for your new account (Read & write, expires after 90 days, listed under your access keys
where you can revoke it) and the same chat is signed in.

Until you tap the link, the sign-up connection lists only the three sign-up tools; any other tool answers that it
needs a TravelLog account and does nothing. Once the account is verified, the connection tells the assistant its
tool list changed, and the assistant is told to list the tools again. The sign-up expires
after 24 hours, and if nobody taps the link within 3 days, TravelLog deletes the account and frees the @handle.

Afterwards, use the normal address above for everyday use. You can also create an account yourself at
<https://travellogapp.web.app/account>, or on the sign-in page your assistant opens. The app is optional; install it
later and everything is there.

## Sign-in

TravelLog uses OAuth. When you connect, your assistant opens TravelLog's connect page, where you:

1. Sign in with your TravelLog account (Google, Apple or email), or create one on the same page.
2. See which assistant is asking, which website it will return you to, and what it wants to do (see
   [Permissions](#permissions)). You can switch on **Read only**.
3. Tap **Allow**.

Nothing is shared until you tap Allow. The assistant gets an access key that works for 1 hour and renews it with a
renewal key that lasts 60 days and is extended every time it is used, so a connection the assistant hasn't renewed
for 60 days ends on its own.

An account made on the website must verify its email before an assistant can change anything; until then the
assistant can only read. Accounts made in the TravelLog app are not affected.

## Permissions

The connect page lists what the assistant asked for. You can accept it all or switch on **Read only**, which keeps
only `trips:read`. Access keys are either Read only (`trips:read`) or Read & write (all four).

| Scope | In plain words |
|-------|----------------|
| `trips:read` | See your trips, itineraries, notes, expenses, travel history and saved places |
| `trips:write` | Create and import trips, add the entries, expenses, notes and places you ask for, and edit or remove entries in trips you can edit |
| `suggestions:write` | Add suggestions to your trips, and dismiss them |
| `bookings:write` | Add your own bookings to your trips |

## Tools

21 tools once you are signed in, plus 3 sign-up tools on the sign-up address. Server version 1.5.0. "Read" tools
change nothing. Every change tool takes a `clientRequestId`, so a retried request happens only once.

### Read (`trips:read`)

| Tool | Read or change | What it does |
|------|----------------|--------------|
| `list_trips` | Read | Your trips (your own and ones shared with you): name, dates, destinations, your role and how many entries each has. Newest first, paged. |
| `get_itinerary` | Read | One trip's itinerary, optionally for a range of days: each entry's title, category, place, place-local times and time zone, and notes; day notes; and pending suggestions. |
| `list_suggestions` | Read | The pending suggestions on a trip that you can see. |
| `get_wishlist` | Read | Your Been and Want to go places lists, with your notes. |
| `get_travel_history` | Read | Where you have travelled, worked out from your trips: past trips, trip count, days away, countries and cities, and upcoming trips. |
| `get_trip_notes` | Read | A trip's trip-level notes and day notes. |
| `get_trip_expenses` | Read | A trip's expenses (title, amount, currency, category, date, who paid, the split) and totals per currency. Only on trips you own or can edit. |
| `request_account_deletion` | Change (sends an email only) | Asks TravelLog to email you a link to delete your account. It deletes nothing: only you can, from that link. At most one link a minute, 3 a day. |

### Trips (`trips:write`)

| Tool | Read or change | What it does |
|------|----------------|--------------|
| `create_trip` | Change | Creates a new, empty trip from a name, destinations and start and end dates. |
| `add_entry` | Change | Adds one stop or place you asked for to a day of a trip, with its time, notes and whether you visited it. |
| `add_expense` | Change | Adds one expense you asked for, with who paid and how it is split between people already on the trip. |
| `add_day_note` | Change | Adds to a day's note. Your existing note is kept; the new text goes at the end. |
| `add_to_wishlist` | Change | Adds one place to your Want to go list, with an optional note. Never edits or removes a saved place. |
| `add_been_place` | Change | Adds one place to your Been list. Never edits or removes a saved place. |
| `import_trips` | Change | Imports past trips you asked to bring in: up to 20 per call, each with its entries and day notes. Each trip is added whole or not at all; a trip you already have with the same name and dates is left alone. |
| `accept_suggestion` | Change | Turns pending suggestions into real entries, as tapping Add in the app does. |
| `update_entry` | Change | Changes an entry on a trip you own or can edit: only the fields given. The assistant must say what will change and get your OK. |
| `remove_entry` | Change | Removes an entry from a trip you own or can edit, on every device and for everyone on the trip. The assistant must name it and get your OK. |

### Suggestions (`suggestions:write`)

| Tool | Read or change | What it does |
|------|----------------|--------------|
| `add_suggestions` | Change | Files the assistant's ideas for a trip as one batch of suggestions labelled "From <assistant name>", up to 40 at a time. Nothing becomes an entry until you add it. On a shared trip only you see them. |
| `dismiss_suggestion` | Change | Removes pending suggestions without adding them, as tapping Dismiss in the app does. Entries already on the trip are never touched. |

### Bookings (`bookings:write`)

| Tool | Read or change | What it does |
|------|----------------|--------------|
| `add_booking` | Change | Adds a booking you actually made (flight, stay, restaurant, event or transit) as a real entry, like pasting a confirmation in the app. A booking already on the trip is not added twice. |

### Sign-up (only on `https://travellogapp.web.app/mcp?signup=1`, no sign-in)

| Tool | Read or change | What it does |
|------|----------------|--------------|
| `get_started` | Read | Explains how to create a TravelLog account from the chat. |
| `create_account` | Change | Creates a TravelLog account after you agree, and emails you a verification link. |
| `check_account_status` | Change | Shows whether you have tapped the link yet; once you have, it hands the assistant its access key and the same chat is signed in. |

Arguments and full descriptions for every tool: <https://travellogapp.web.app/mcp-docs>.

## Limits

Per assistant you connect, and per access key, on your account:

- 60 calls an hour (20 of them changes) and 600 calls a day.
- Imports have their own limit for your account (every assistant and key together): 10 import calls an hour and
  200 trips a day, 20 trips a call, 80 entries and 15 day notes a trip. Each import call still counts toward the
  60 an hour and 600 a day, but not toward the 20 changes.
- Up to 40 suggestions a call (`add_suggestions`, `accept_suggestion`, `dismiss_suggestion`).
- Places lists hold up to 500 places.
- Up to 5 active access keys.
- Account deletion links: 1 a minute, 3 a day. Each link expires after about an hour.
- Assistant sign-up: 3 a minute and 10 new accounts a day per network, 3 a day per email address.

Connecting an assistant, and every tool, is free with a free TravelLog account; no tool needs TravelLog Pro. No
TravelLog MCP tool runs AI on TravelLog's side.

## Undo and disconnect

Suggestions an assistant adds are labelled with its name and stay suggestions until you add or dismiss them.
Everything else it adds or changes behaves like your own edits: change or remove it in the app or ask the
assistant to. There is no one-step undo of an assistant's changes.

In the TravelLog app, go to **Settings › Connected apps**, or on the website go to
<https://travellogapp.web.app/account>:

- **Disconnect** (an assistant) or **Revoke** (an access key) stops its access at once and deletes TravelLog's
  request records for it. Anything it already added stays in your trips. It does not delete your account.
- **Sign out of all assistants and keys** disconnects every assistant and revokes every key in one step.

You can also disconnect from inside the assistant where it offers that, but disconnect in TravelLog to be sure.
Deleting your TravelLog account disconnects every assistant and revokes every key.

## Privacy

Full policy: <https://travellogapp.web.app/privacy> (Section 3a covers AI assistants).

- **What it can see:** only what you allowed: your trips' names, dates and destinations (your own and trips shared
  with you); each trip's itinerary entries (title, category, place, local start and end times, notes); trip and day
  notes; the suggestions assistants have added for you; your Been and Want to go lists with your notes; a travel
  history summary worked out when asked; and, on trips you own or can edit, expenses (title, amount, currency,
  category, date, who paid and the split) and totals.
- **What it never sees:** location tracking data of any kind (location points, detected stops, distances or times
  recorded by tracking), your private entries and notes, other members' private entries, settlement payments,
  receipts, photos, confirmation codes, other people's profiles, comments, email addresses or invite links. On a
  trip shared with you as a viewer it cannot see expenses and can only read.
- **What it can't do:** delete trips, share trips, change your account, or see, change or remove another member's
  private entries or anyone's own visit records. Health-care places are not accepted as suggestions or in your places lists.
- **What TravelLog keeps:** only what you asked the assistant to store; a record of each request (which assistant,
  which action, whether it worked and when) with no trip content, for 90 days; access keys in hashed form only;
  and, when an assistant adds a place to your lists, a short note of that addition (name and coordinates) for
  30 days so a phone that hasn't synced yet doesn't drop it.
- **The assistant's company:** what your assistant reads goes to the company that runs it, under that company's
  own privacy policy, and it may keep it after you disconnect. TravelLog doesn't sell this data or use what an
  assistant sends it for advertising, profiling or AI training.
- **Deletion:** an assistant can't delete your account. It can only ask TravelLog to email you a link that opens a
  page showing what will be deleted, where you confirm with one tap. Deleting your account, in the app, on the
  website or from that link, deletes your data, disconnects every assistant and revokes every key.

## Tested with

These were run by the TravelLog team against the TravelLog server code:

- **MCP Inspector CLI 2.10.1:** `tools/list` on the current and older protocol versions and a `list_trips` call
  (on the first release of the server).
- **The MCP conformance suite** (`@modelcontextprotocol/conformance` 0.2.0-alpha.12) on protocol versions
  `2026-07-28` and `2025-11-25` (on the first release of the server).
- **A scripted Streamable HTTP client** against local test servers: the full OAuth flow (client registration,
  sign-in, consent, code exchange, key renewal, revoke), permission checks, every tool listed above, access keys,
  the limits, Disconnect, the assistant sign-up flow from `create_account` to a signed-in session, and account
  deletion.

The Claude, ChatGPT and Grok steps above are the ones on the TravelLog website; they are not yet listed here as
tested.

## Support

Email us at [armgardtj@outlook.com](mailto:armgardtj@outlook.com). Privacy questions: see the [privacy policy](https://travellogapp.web.app/privacy).
Setup help: <https://travellogapp.web.app/connect-ai>. Tool reference: <https://travellogapp.web.app/mcp-docs>.
