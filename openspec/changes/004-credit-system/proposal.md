# Change 004: Credit System + Stripe

**Status:** Draft stub
**Priority:** P0
**Phase:** 1 (MVP)

## What

Credit-based billing system. Users purchase credit bundles or subscribe to membership tiers. Credits are consumed per operation. Stripe handles all payments.

## Credit Model (Locked)

**Anchor:** 50 credits = $5.00

### Pay-as-you-go bundles

| Bundle | Credits | Price | Rate |
|---|---|---|---|
| Free tier | 10/mo | $0 | — |
| Small | 50 | $5.00 | $0.10/cr |
| Medium | 200 | $16.00 | $0.08/cr |
| Large | 500 | $35.00 | $0.07/cr |

### Membership tiers (ElevenLabs model)

| Plan | Credits/mo | Price | Overage | Advanced features |
|---|---|---|---|---|
| Creator | 100 | $8/mo | $0.08/cr | ✅ |
| Pro | 300 | $20/mo | $0.07/cr | ✅ |
| Studio | 1,000 | $50/mo | $0.06/cr | ✅ |

### Credit costs by operation

| Operation | Credits | $ at base |
|---|---|---|
| Chart generation (DSP) | 0 | free |
| AI summary (Haiku) | 5 | $0.50 |
| Deep analysis (Sonnet) | 20 | $2.00 |
| Expert analysis (Opus) | 50 | $5.00 |
| Separation 2-stem | 10 | $1.00 |
| Separation 4-stem | 20 | $2.00 |
| Separation 6-stem | 30 | $3.00 |
| Fine-tuned separation (+ft) | +15 | +$1.50 |

**Advanced features (membership-gated):** Demucs source separation, comparison view, guide mode, priority queue, stem downloads.

## Stripe Integration

- Pay-as-you-go: Stripe Payment Intent (one-time)
- Memberships: Stripe Subscription (recurring)
- Webhook events: `payment_intent.succeeded`, `customer.subscription.updated`, `invoice.payment_succeeded`
- Credit top-up: credited to account on webhook confirmation

## Open Questions

- Stripe account setup: Alice action item
- Credit rollover: do unused monthly credits roll over?
- Free tier refresh: automatic on calendar month, or rolling 30 days?
- Overage billing: automatic charge or manual top-up required?

## Acceptance Criteria

- [ ] Credit balance visible at all times for authenticated users
- [ ] Free tier: 10 credits granted monthly (auto-refresh)
- [ ] Bundle purchase via Stripe → credits credited immediately on webhook
- [ ] Membership subscription via Stripe → credits granted monthly
- [ ] Credit deduction on AI operations (not on chart generation)
- [ ] Insufficient credits → clear error + prompt to purchase
- [ ] Membership gates advanced features correctly
