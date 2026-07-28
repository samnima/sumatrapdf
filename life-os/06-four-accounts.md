# 06 — Four Gmails, One River

> "I have four Gmail which need to organize... I am juggling between those,
> those need to flow like a river so smooth."

This is the most important file in the system, and it should probably be done
alongside the inbox reset.

---

## The actual problem

You don't have an organization problem. You have **five identities and no hub**:

| Account | Purpose (you confirm) | Currently |
|---|---|---|
| `sam.icc@gmail.com` | Primary personal | Connected here. 7,647 msgs, 845 unread. |
| `s3mamin@gmail.com` | ? Personal / family | Appears in your sent mail |
| `docmajan@gmail.com` | ? Medical / doctor-related | Likely source of the Palliative Care Physician alerts |
| `s3mallc@gmail.com` | ? Your LLC / business entity | Legal + financial exposure — treat carefully |
| `samin@hudsonmeridian.com` | Work (Hudson Meridian) | Outlook. Not connected. |

Every time you check mail, you're paying a **context-switch tax** five times over.
That's not a discipline failure — five inboxes cannot be kept in your head, by anyone.
And critically: with five inboxes, "nothing falls through the cracks" is *impossible*
by construction, because there is no single place where "everything" exists.

**A river has one channel.** Right now you have five streams and you're the dam.

---

## The design: hub and spokes

**Hub: `sam.icc@gmail.com`** — it's your primary, it's already connected to this
system, and after the inbox reset it'll be clean.

The other three Gmails become **spokes**. They keep existing (never delete them —
they hold logins, 2FA recovery, and legal history). They just stop being places you
*visit*.

```
  s3mamin  ─┐
  docmajan ─┼──▶  sam.icc  (HUB — the only inbox you open)
  s3mallc  ─┘         ▲
                      │
  hudsonmeridian ─────┘  (once IT connects it)
```

### Step 1 — Forward each spoke into the hub

In **each** of `s3mamin`, `docmajan`, `s3mallc`:

`Settings → Forwarding and POP/IMAP → Add a forwarding address` → `sam.icc@gmail.com`
→ confirm the verification email in the hub → select **"Keep Gmail's copy in the Inbox."**

> Keep the copy. That preserves each account as its own system of record — important
> for `s3mallc` if it's a real entity with tax or legal records.

### Step 2 — Label on arrival in the hub

In the hub, one filter per spoke. This is what keeps the river from becoming a swamp:

| Filter | Apply label |
|---|---|
| `deliveredto:s3mamin@gmail.com` | `@FAMILY` |
| `deliveredto:docmajan@gmail.com` | `@DOC` |
| `deliveredto:s3mallc@gmail.com` | `@LLC` |
| `deliveredto:sam.icc@gmail.com` | `@PERSONAL` |

Now one inbox, and every message is visually tagged with which life it belongs to.
You can read all of it at once, or click `@LLC` and see only business.

> `deliveredto:` is the reliable operator here — it matches the envelope recipient,
> so it still works after forwarding, where `to:` often doesn't.

### Step 3 — Send from the right identity (this is the part people skip)

A hub is useless if replying from it makes you look wrong to the recipient.

In the hub: `Settings → Accounts and Import → Send mail as → Add another email address`.
Add all three spokes. Gmail sends a verification code to each — grab it from the hub
since mail is already forwarding.

Then set — and this is the critical setting:

> **When replying to a message: _Reply from the same address the message was sent to_**

Now mail to `s3mallc` is answered *as* `s3mallc`, automatically, without you thinking
about it. That single radio button is what turns five accounts into one workflow.

### Step 4 — Work email, when IT comes through

Two options once `samin@hudsonmeridian.com` is on your machine:

- **Keep it separate.** Recommended. Work mail stays in Outlook — most employers
  require it, and mixing corporate mail into a personal Gmail is usually a policy
  violation and always a bad idea during litigation or if you leave.
- **Bridge it, don't merge it.** The daily brief covers both by *reading* each, rather
  than by funneling work mail into a personal account.

So the river has one personal channel plus one work channel. **Two, not five.** That's
the realistic target, and it's a 60% reduction in switching.

---

## What to do about `docmajan` — search closed ✅

`docmajan` is the medical account, and the JobLeads alerts were your daughter's
physician job search. **She got the job — VA hospital.** Loop closed.

So the action is: **go into JobLeads and delete the saved searches, then unsubscribe.**
Don't filter them. Filtering hides a machine that's still running; you want it off.

Then decide what `docmajan` is *for* now:

- **Her ongoing professional account** → she owns it, you stop monitoring it, it
  doesn't forward to your hub at all. Most likely right answer now that she's placed.
- **Dormant** → forward to hub, archive everything, revisit in a year.

Either way it should stop consuming a slot in your daily attention. It was a live
workstream; it isn't anymore, and the system should reflect that.

### The general rule this teaches

> **When a goal closes, go turn off everything that was serving it.**

Alerts, saved searches, newsletters, shared spreadsheets, recurring meetings. This is
the highest-return five minutes in the entire system, because you're removing
*permanent* recurring load for a one-time cost — and unlike everything else here, it
requires no ongoing discipline at all.

Run this check on the car search the day you sign: five broker spreadsheets and a dozen
lease newsletters all become dead weight the moment that decision closes. Kill them the
same afternoon, or they'll still be arriving next spring.

---

## "Do you need access to all four?"

**No.** That's a feature of this design, not a coincidence.

Once forwarding is on, everything lands in the hub — and the hub is already connected.
One authorization covers all four accounts. Without forwarding, you'd need three more
separate connections to get the same visibility.

| | With forwarding | Without |
|---|---|---|
| Accounts to authorize | 1 (already done) | 4 |
| Inboxes you open | 1 | 4 |
| Places a commitment can hide | 1 | 4 |

Three caveats, so you're deciding with full information:

1. **I can't configure the spokes.** Forwarding, send-as, and filters are Gmail
   *settings*, not mail data — the connector can't touch them. You do those by hand in
   a browser, about 10 minutes per account. One-time.
2. **Forwarding means I can read all of it.** Mail from `s3mallc` and `docmajan`
   becomes visible to this system once it lands in the hub. That's the trade for the
   single-inbox view. Consider it deliberately for the LLC account especially.
3. **Sending is still from the hub.** Drafts I create originate in `sam.icc`. The
   send-as setting in Step 3 is what makes them go out under the right identity — set
   it up or replies will come from the wrong address.

If you'd rather keep `s3mallc` fully separate, that's completely reasonable: forward
the other two, keep the LLC standalone, and check it deliberately once a week. Four
inboxes becomes two. Still an 80% reduction in switching.

---

## The one rule for the river

> **Open one inbox. Ever.**

If you find yourself logging into a spoke account directly, something's misconfigured —
fix the forwarding rather than making a habit of the visit. The instant you're checking
two inboxes "just to be safe," you're back to being the dam.

---

## Sequencing

Do this **at the same time as** [`02-inbox-reset.md`](02-inbox-reset.md), not after.
Merging three accounts into an inbox that's still 50:1 noise makes things worse, not
better. Kill the noise first, then open the channels.

1. Inbox reset on the hub (noise filters + 60-day bankruptcy)
2. Forwarding on all three spokes
3. `deliveredto:` labels in the hub
4. Send-as + "reply from same address"
5. Delete every other Gmail bookmark and app account from your phone except the hub
