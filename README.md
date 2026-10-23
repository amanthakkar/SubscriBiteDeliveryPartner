# SubscriBite — Delivery Partner

The delivery-partner companion app for **[SubscriBite](https://github.com/amanthakkar/SubscriBite)**, built as part of a UC Irvine Master of Computer Science capstone (2022–23).

SubscriBite was a subscription grocery-delivery concept. The main app was the customer side; this is the partner side — the app a delivery partner uses to see what's going out and to tell the customer when it has arrived.

## What it does

One screen, deliberately:

- **Upcoming deliveries** — fetches upcoming orders for a partner from the SubscriBite backend (`POST /subscriptions/upcoming_orders`), flattens the line items, and groups them by **delivery date + time slot**. Each group renders as a card: date, slot description, item names with quantities, and the slot's total cost.
- **"Delivered" tap** — tapping a delivery card sends the customer an Expo push notification: *"Check your doorstep! 📬 — Your order has been delivered! $X will be deducted from your account."*
- **Push plumbing** — on launch the app registers for Expo push notifications on a physical device, keeps its push token, and installs notification/response listeners (the delivery-confirmation loop was exercised partner-phone → customer-phone during the capstone).

## How it works

Everything lives in a single `App.js` — a capstone prototype, not a layered codebase:

1. `fetchData()` posts the partner's user id to the backend and gets orders back keyed by date.
2. Line items are flattened, then regrouped under a `date__timeslot` key with a running `totalCost`.
3. A `FlatList` renders one pressable card per group; the press handler is the delivery confirmation (push via the Expo push service, `exp.host`).

| Layer | Tech |
|---|---|
| Runtime | Expo SDK 48 · React Native 0.71 · React 18 |
| Networking | axios (orders) · fetch (Expo push send) |
| Notifications | expo-notifications · expo-device · expo-permissions |

## Run it

```bash
npm install
npx expo start        # then open on a physical device — push tokens need real hardware
```

The app points at the capstone's hosted dev backend, which is no longer guaranteed to be running. To use it against your own deployment of the SubscriBite backend, change the endpoint URL in `App.js` (it's a literal at the top of `fetchData`).

## Honest status & limitations

This is a **capstone prototype kept for the record**, not a maintained product:

- **Hardcoded demo configuration** — the partner user id, the backend URL, and the push-notification target are literals in `App.js`. There is no settings screen, env config, or auth; that's what "prototype" meant on a capstone timeline.
- **No tests**, one file, scaffold names (`Test2`) still in `app.json`/`package.json`.
- The delivery flow it served — SubscriBite's subscription backend — was a course deployment, so end-to-end runs today require standing that backend up again.

## Related

- **[SubscriBite](https://github.com/amanthakkar/SubscriBite)** — the customer-facing app this one partnered with
- More of my work: [@amanthakkar](https://github.com/amanthakkar)
