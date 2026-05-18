# Change 013: Membership Tiers

**Status:** Draft stub
**Priority:** P1
**Phase:** 1/2

## What

Three membership tiers (Creator/Pro/Studio) with monthly credits, discounted overage rates, and access to advanced features. Stripe Subscription handles billing.

## Tiers (Locked)

| Plan | Credits/mo | Price | Overage |
|---|---|---|---|
| Creator | 100 | $8/mo | $0.08/cr |
| Pro | 300 | $20/mo | $0.07/cr |
| Studio | 1,000 | $50/mo | $0.06/cr |

## Advanced Feature Gates (membership-only)

- Demucs source separation
- Comparison view
- AI guide mode
- Priority job queue
- Stem downloads

## Acceptance Criteria (stub)

- [ ] Subscription purchase via Stripe
- [ ] Monthly credit grant on renewal
- [ ] Overage auto-charged at tier rate
- [ ] Advanced features unlocked for active subscribers
- [ ] Cancellation: access until end of billing period
- [ ] Upgrade/downgrade between tiers
