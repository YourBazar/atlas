# Feature Inventory

## Existing Production-Oriented Features

| Feature | Evidence | Status |
| --- | --- | --- |
| Signup | Centaurus `/users/signup`, Cygnus Signup page | Implemented |
| Signin | Centaurus `/users/signin`, Cygnus Signin page | Implemented |
| Auth token helpers | Centaurus `auth_helper.py` | Implemented |
| Payload checksum validation | Centaurus `payload_decorators.py`, Cygnus checksum helper | Implemented |
| Role portals | Centaurus portal routes/models/tests | Implemented |
| Businesses | Centaurus business routes/models/tests | Implemented |
| Business profiles | Centaurus business profile routes/models/tests | Implemented |
| Role landing pages | Cygnus investor/partner/provider home pages | Implemented as content pages |

## Partial Features

| Feature | Evidence | Status |
| --- | --- | --- |
| Investment tracking | `Investments` model | Model only |
| Expenses | `Expenses` model | Model only |
| Earnings | `Earnings` model | Model only |
| Transactions | `Transactions` model | Model only |
| Business investors | `BusinessInvestors` model | Model only |
| Business partners | `BusinessPartners` model | Model only |
| Business providers | `ProviderPortals` model | Model only |

## Planned Features

Planned features are primarily evidenced by `services/MarkDowns` and `services/ApiSpecs/BusinessInvestor.yaml`.

- Business options.
- Business setup plans.
- Property selection.
- Partner/provider assignment.
- Milestones.
- Payments.
- Portfolio.
- Support tickets.
- Chat.
- Admin.
