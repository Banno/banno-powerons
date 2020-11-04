## Error Codes

If a request is not successful for any number of reasons, the poweron should respond with the following structure:

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "TRANPERFORM Error:+TRANERROR"
}
```

## Error codes by Poweron

- ### CERTIFICATE RENEWAL

> | Code | Description                                                     |
> | ---- | --------------------------------------------------------------- |
> | 500  | Generic Error                                                   |
> | 501  | Configuration file error (error opening, reading or processing) |
> | 502  | Invalid account or share (based on account type/share warning)  |
> | 503  | Maturity selection previously made - contact CU                 |
> | 504  | Processing Error                                                |
> | 505  | Cross account access attempt                                    |

- ### OPEN SUBSHARE POWERON

  > | Code | Description        |
  > | ---- | ------------------ |
  > | 500  | Generic Error      |
  > | 501  | Try again          |
  > | 502  | Missing address    |
  > | 503  | Invalid loan/share |
  > | 504  | Reg D Limit        |

- ### SKIP PAYMENT POWERON, LOAN PAYOFF POWERON

  > N/A

- ### ODT OPT-IN POWERON

  > Error Codes // Individual error codes are for ease of researching issues.
  > // All error codes except for '503' will be returned to the UX as '500'
  > | Code | Description |
  > | ---- | ------------------------------------------------ |
  > | 501 | Config file read error |
  > | 502 | Config file validation error |
  > | 503 | No eligible shares |
  > | 504 | Ineligible account type |
  > | 505 | Account warning found |
  > | 506 | Error attempting to update share tracking record |
  > | 507 | Error updating source code and auth & fee fields |

- ### WITHDRAW BY CHECK POWERON

  > | Code | Description             |
  > | ---- | ----------------------- |
  > | 500  | CFG file error          |
  > | 501  | Insufficient Funds      |
  > | 502  | Invalid address         |
  > | 503  | Invalid Acct/loan/share |
  > | 504  | Reg D Limit             |
  > | 505  | X-Acct Error            |

## Standardized Error codes

| Code   | Event                                              | Description                                      | Default Error Message in UI                                                                                                                           |
| ------ | -------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **0**  | `Generic Error`                                    | Generic Error                                    | Oops, something went wrong on our end. Please try again.                                                                                              |
| **1**  | `Config file read error`                           | error opening, reading or processing             | Oops, something went wrong on our end.                                                                                                                |
| **2**  | `Config file validation error`                     | validation errors                                |
| **3**  | `Invalid address`                                  | user address on file is invalid                  | There was a problem getting your mailing address. Please review your address in your profile settings and try again.                                  |
| **4**  | `Invalid acct/loan/share type`                     | based on account type/share warning              |
| **5**  | `Insufficient Funds`                               | not enough money                                 |
| **6**  | `No eligible shares`                               | No eligible shares.                              |
| **7**  | `Maturity selection previously made`               |                                                  | A Maturity selection was previously made for this certificate. You'll need our assistance to make changes to this account. Contact us to get started! |
| **8**  | `Account warning found`                            | Account warning found                            |
| **9**  | `Cross account access attempt`                     | Cross Account Error                              | You must be an account owner in order to manage this \${account type}                                                                                 |
| **10** | `Reg D Limit`                                      | Reg D Limit                                      |
| **11** | `Error attempting to update share tracking record` | Error attempting to update share tracking record |
| **12** | `Processing Error`                                 | Processing Error                                 |

---
