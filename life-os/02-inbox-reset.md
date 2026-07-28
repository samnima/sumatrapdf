# 02 — Inbox Reset

**Time: ~45 minutes, once. Payoff: your inbox becomes a signal channel again.**

Do this before anything else in this system.

---

## The principle

Stop sorting mail by *topic*. Sort it by **whether it needs you**.

Topic labels (`Travel`, `Personal`, `Car Leasing`) failed for you — all sitting at
0–2 messages — because topic is something you have to think about. The only taxonomy
that survives volume is one where a filter can decide without you.

Four lanes. That's it.

| Label | Means | Rule |
|---|---|---|
| `1-ACTION` | A human is waiting on you | Never auto-applied. You put it here. |
| `2-WAITING` | You're waiting on someone else | You put it here when you send the ask |
| `3-REF` | Might need it later, nobody's waiting | Auto-applied by filter |
| `4-NOISE` | Marketing. Skip inbox entirely. | Auto-applied, **skips inbox** |

Anything in the inbox with no label is unprocessed. **Inbox = unprocessed queue**,
not storage. That single reframe is most of the win.

---

## Step 1 — Kill the noise (30 min, biggest win)

These are the senders actually flooding you, pulled from your real mail. Create one
Gmail filter per block below, with **Skip the Inbox** + **Apply label `4-NOISE`** +
**Never mark as important**.

### Travel & points marketing
```
from:(roame.travel OR airbusinessclass.com OR ceoflights.com OR flyluxury.com OR
justflyemail.com OR email.point.me OR thepointsguy.com OR frequentmiler.net OR
email.exoticca.com OR email.autoslash.com OR hello.turo.com OR emails.rakuten.com)
```

### Car & EV marketing
```
from:(newsletter@carbuzz.com OR shared1.ccsend.com OR autoninjas.com OR
theleasewiz.com OR autoleasedirectory.com)
```

### Tech / AI newsletters
```
from:(techradar@smartbrief.com OR newsletter.theneurondaily.com OR
info@theinformation.com)
```

### Job alerts — go turn this off at the source ✅
```
from:mailer@jobleads.com
```
> **Your daughter got the job — VA hospital.** The search is over. These alerts have
> been firing 2–3× a day for a goal that's already achieved.
>
> **Don't filter these. Go delete the saved search in the JobLeads account and
> unsubscribe.** A filter would hide a machine that's still running; you want the
> machine off.

This is the single best example in your whole inbox of what's actually wrong:

> **The goal closed. The machinery kept running.**

Nobody ever goes back to turn things off. That's the **Drop** step from the README,
and it's the one everyone skips — including the version of you that set this alert up
months ago, correctly, for a good reason.

When you do the reset, ask this of every recurring sender: *is the thing this was for
still true?* You'll find several where the answer is no. Every one of those is pure
recovered attention at zero cost.

### Receipts
```
from:anthropic.com subject:receipt
```
Label `3-REF`, skip inbox. You have 136 of these already labeled. They're records,
not mail.

> **Better than filtering: unsubscribe.** For any sender above you genuinely don't
> read, hit unsubscribe instead. A filter hides the symptom; unsubscribing removes it.
> Budget 15 minutes for this and be ruthless — you can always resubscribe.

---

## Step 2 — Clear the backlog (5 min)

Do not read 7,647 messages. Declare bankruptcy on anything old:

```
in:inbox older_than:60d
```

Select all → apply `3-REF` → Archive. It's all still searchable forever. Nothing is
deleted. If something from 60+ days ago mattered, the person has already followed up.

That takes your inbox from 7,647 to roughly the last two months.

Then run the noise filters against existing mail (Gmail offers "Also apply filter to
matching conversations" when you create one — say yes).

---

## Step 3 — Split the ME TO ME lane (10 min)

The `ME TO ME` label stays as your capture inbox. But it needs two exits, or it
refills into a landfill.

Create two labels:

- **`ME-DECIDE`** — this note implies a decision or commitment. Goes to Open Loops.
- **`ME-READ`** — interesting link, nobody's waiting. Reference only.

**Right now**, do this to the 267 unread: don't read them. Sort them by subject and
bulk-move. Based on what's actually in there:

| Search | Action |
|---|---|
| `label:"ME TO ME" (leasehackr OR lyriq OR cadillac OR edmunds OR kbb OR caredge OR autotrader OR broker)` | → `ME-READ`, mark read. **This is ~70% of the pile.** It is superseded by one decision (see Open Loops). |
| `label:"ME TO ME" (claude OR anthropic OR prompt OR "ai ")` | → `ME-READ`, mark read |
| `label:"ME TO ME" (business class OR zipair OR emirates OR trustpilot)` | → `ME-READ`, mark read |
| `label:"ME TO ME" (pinhome OR belitung OR rumah)` | → `ME-DECIDE` — this is the home build, a real open loop |
| `label:"ME TO ME" (BBB OR "lemon law" OR GM)` | → `ME-DECIDE` — **money attached, do not lose this one** |

Whatever's left after those five — probably 30–50 messages — read those. That's your
actual backlog, and it's an evening's work, not a month's.

---

## Step 4 — The new capture rule

Keep emailing yourself. One change only:

> **Put a verb in the subject line.**

- ❌ `https://forum.leasehackr.com/t/...`
- ✅ `DECIDE: Lyriq vs Polestar by Aug 15`
- ✅ `CALL: Everest re 193 Market bid`
- ✅ `READ: lease broker list`

`DECIDE:` and `CALL:` go to `ME-DECIDE`. `READ:` goes to `ME-READ`. Now your weekly
review takes 10 minutes instead of two hours, because past-you already sorted it.

A bare URL with no verb is a bookmark. Bookmarks are not commitments and should never
have been in the same pile as commitments — that conflation is the actual reason you
can't tell what's outstanding.

---

## Want me to do it?

I can create the label structure (`1-ACTION`, `2-WAITING`, `3-REF`, `4-NOISE`,
`ME-DECIDE`, `ME-READ`) in your account right now — that's additive and reversible.

I can't create Gmail *filters* through the connector, so Step 1 is copy-paste by you.
Bulk archiving in Step 2 I'd want you to pull the trigger on, since it touches 7,000
messages — but say the word and I'll walk you through it live.
