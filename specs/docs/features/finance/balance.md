> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Account Balance & Transparent Fees

> Monitor your Polar balance without hidden fees

You can see your available balance for payout at any time under your `Finance` page.

<img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/finance/balance/overview.light.jpeg?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=603040c0ac9331aa9f71061ce20401fe" width="2700" height="1655" data-path="assets/features/finance/balance/overview.light.jpeg" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/finance/balance/overview.dark.jpeg?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=b4ac9458eabafcf793d75e6f3ed1fba5" width="2700" height="1655" data-path="assets/features/finance/balance/overview.dark.jpeg" />

Your balance is all the earnings minus:

1. Any VAT we've captured for remittance, i.e balance is excluding VAT
2. Our revenue share (varies by plan — see [fees](/docs/merchant-of-record/fees))

All historic transactions are available in chronological order along with their associated fees that have been deducted.

Note: Upon [payout (withdrawal)](/docs/features/finance/payouts), Stripe incurs additional fees that will be deducted before the final payout of the balance.

## Multiple payment currencies orders and settlement

When customers purchase your products in currencies other than USD, Polar automatically converts these amounts to USD (the settlement currency) for your account balance. This conversion ensures that all transactions are consolidated into a single currency for easier financial management and payout processing.

The conversion process uses current exchange rates at the time of the transaction, and the converted USD amount is what appears in your account balance and is available for payout.

## Payouts in ISK, HUF, TWD, or UGX

For accounts using Icelandic króna (ISK), Hungarian forint (HUF), New Taiwan dollar (TWD), or Ugandan shilling (UGX), Stripe requires payout amounts to be in whole currency units. This means any fractional amount (less than 1 ISK/HUF/TWD/UGX) will remain in your balance and be included in your next payout.
