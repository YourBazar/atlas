# Revenue and Payments

## Planned Payment Categories

Product docs describe separation of:

- platform fees;
- provider charges;
- partner charges;
- property charges;
- deposits;
- recurring service charges;
- refunds and cancellations.

## Current Implementation

Centaurus contains a `Transactions` model with:

- amount;
- mode;
- payment ID;
- paid by;
- paid to;
- status.

No payment routes or external payment gateway integration were observed.
