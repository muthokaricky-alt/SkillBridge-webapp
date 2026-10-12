# Partner Test Results — Week 7

This file documents the ring exchange: our contract (`openapi.yaml`) sent out, our ring partners' contracts received in return, and what running each side's tests against the other's real, live server found.

> **Before you submit:** the "all passed" results below are from your recollection, not from a log. Re-run your tests once (`npm test`) and add the dates, then delete this note and any remaining **[FILL IN]** markers.

## 1. Contracts exchanged

| Direction | Partner | Contract sent/received | Date |
|---|---|---|---|
| Sent | Farmers-AgroConnect | `openapi.yaml` (our downstream contract: experts, skills, consultation booking) | [FILL IN] |
| Received | Farmers-AgroConnect | [FILL IN: filename, or "none received"] | [FILL IN] |
| Received | Maji Website | Maji Platform — SkillsBridge API `openapi.yaml` (see section 4) | [FILL IN] |

## 2. Tests we wrote

**Against our own contract.** The automated suite in `__tests__/` (jest + supertest) covers every SkillBridge endpoint: happy path, validation rejection, not-found, and edge cases. `experts.test.js` and `skills.test.js` were written from `openapi.yaml` (the contract AgroConnect builds against) and assert its exact shape. Run it with `npm test`.

**Against Maji's contract (we are the consumer).** [FILL IN: name the test file, e.g. `__tests__/partner-maji.test.js`, or write "not written" if none exists. The repo as uploaded has no partner-specific test file.]

## 3. What AgroConnect's tests found, run against our real API



- [x] Nothing: all of their tests passed against our live server. This is a valid outcome, not a gap.
- [ ] Failures found.

How they reached our server (shared URL / their machine / ours): [FILL IN]

## 4. What our tests found, run against Maji's real API

**Contract we received from Maji.** Four read-only `GET` endpoints, all returning JSON arrays:

| Endpoint | Returns |
|---|---|
| `/orders/active/estates` | `[{ estate }]` |
| `/orders/active/schedules` | `[{ estate, deliveryTime }]` |
| `/vendors/top-rated` | `[{ vendorId, businessName, estate, rating, isVerified, isOnline, ownerName?, phone? }]` |
| `/vendors/locations` | `[{ vendorId, businessName, estate }]` |

**Contract note (from reading it against our code, not from a test run).** `server/services/majiClient.js` calls `/estates` and `/vendors/stations`, based on the Week 4 copy saved at `src/documentation/MAJI_OPENAPI_RECEIVED.yaml`. Those paths are not in the contract above, and the field names differ (our client expects `targetEstateId`, `stationName` and `deliveryStatus`; this contract has `estate`, `businessName` and `deliveryTime`). Our tests passed against Maji's live server, so the live server presumably still serves the Week 4 endpoints. We are flagging the mismatch to Maji as a contract ambiguity: their written contract and their running API should agree. [FILL IN: outcome of that conversation, if any.]

**Contract ambiguities to raise with Maji** (the contract defines no behaviour for these):

- No error responses are documented, only `200`. A test cannot assert what a failure looks like.
- No authentication is described.
- No query parameters are defined (for example, no way to filter vendors by estate).
- `deliveryTime` is a plain string (`"14:00"`) with no timezone or format rule.
- `/vendors/top-rated` documents `rating` as between 3 and 5 but does not say how many vendors are returned or in what order.

**Results of running our tests against Maji's live server.**

- [x] Nothing: all passed.
- [ ] Failures found.

Maji's live URL we tested against: [FILL IN]

## 5. How we classified failures

- **Real bug:** our implementation does not match the contract. We fix it.
- **Contract ambiguity:** a reasonable person could read the contract the way the failing test did, but we made a different assumption. We clarify the contract so there is only one reading.
- **Misunderstanding:** the test expects something the contract never promised. Not a bug on either side; we talk it through with the partner.