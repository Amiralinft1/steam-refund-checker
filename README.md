# Steam Refund Check

A minimal GenLayer Intelligent Contract that evaluates a user-reported scenario against Steam's published standard refund criteria for ordinary games.

## Scope

The contract checks whether the submitted values meet the standard criteria stated on Steam's refund page:

- The purchase is within 14 days.
- The game has been played for less than 2 hours.

For this project, 14 days is represented as at most 336 hours. The playtime limit is strict: 120 minutes does not meet the "less than 2 hours" rule.

## Outcomes

- `MEETS_STANDARD_CRITERIA`
- `OUTSIDE_STANDARD_CRITERIA`
- `UNCLEAR`

The contract reads Steam's refund page during the assessment and uses GenLayer consensus for the outcome. Each assessment stores the outcome, submitted values, scope, criteria, and source URL.

## Contract

- Studio filename: `SteamRefundCheck.py`
- Python class: `SteamRefundChecker`
- Main method: `check_standard_game(purchase_age_hours, playtime_minutes)`
- Read methods: `get_record(record_id)` and `get_record_count()`

## Deployment

- Network: GenLayer StudioNet
- Contract address: `0xf0f940b90899b2C31396DFbb17900372D46e1032`
- Deployment transaction: [0x67a1a084c10ad595909970492edb3da2828b7e2aa296729dc697c9b2573111e5](https://explorer-studio.genlayer.com/tx/0x67a1a084c10ad595909970492edb3da2828b7e2aa296729dc697c9b2573111e5)

## Studio smoke tests

All listed transactions finalized with accepted consensus and successful execution.

| Purchase age (hours) | Playtime (minutes) | Outcome | Record ID | Transaction |
| ---: | ---: | --- | ---: | --- |
| 24 | 90 | `MEETS_STANDARD_CRITERIA` | 0 | [0x9deb426f...](https://explorer-studio.genlayer.com/tx/0x9deb426fad1c964482d8b922f9f04c1edc9cd0a7e0bcfeeb68086962679a28fd) |
| 337 | 90 | `OUTSIDE_STANDARD_CRITERIA` | 1 | [0xc0ca88c6...](https://explorer-studio.genlayer.com/tx/0xc0ca88c6e6691367c0e5f962cf4223fc3fb148077365b4a2a21d9ec9deaf9b9c) |
| 24 | 120 | `OUTSIDE_STANDARD_CRITERIA` | 2 | [0xdfc1161e...](https://explorer-studio.genlayer.com/tx/0xdfc1161ef964db92f46fcf72689ee9605b7ef65e7cf955f77fe02394faffaf4b) |
| 336 | 119 | `MEETS_STANDARD_CRITERIA` | 3 | [0x641b5c1f...](https://explorer-studio.genlayer.com/tx/0x641b5c1fe0aec6ad5e3493791952f1ddf5d5640d09b140f9ac9f5436b752ece8) |

Records 0 through 3 were read back in Studio and matched their assessment results.

## Source

[Steam Refunds](https://store.steampowered.com/steam_refunds/?l=english)

## Limitations

- Purchase age and playtime are user-reported; the contract does not verify them.
- This checks only the standard rule for an ordinary game purchased directly from Steam.
- It does not assess DLC, subscriptions, gifts, pre-orders, purchases outside Steam, regional consumer rights, or other exceptions.
- `MEETS_STANDARD_CRITERIA` is not a guarantee that Steam will approve a refund.
- The 336-hour interpretation is a simplifying convention used by this project.
- This contract is an informational prototype, not legal advice.
