# Feature Checklist

Original-app status reflects the Phase 1 source audit; runtime behavior remains unverified where documented. Migration implementation status is tracked separately and remains **Not started** unless otherwise stated.

| Feature area | Required items to audit | Original app | Migration status |
|---|---|---|---|
| Product catalog | Products, categories, search, images, prices, availability, stock indicators | Present in source; runtime blocked by deployed API 404 | Not started |
| Product customization | Sizes, add-ons, defaults, product-specific options, customization pricing | Present in source | Not started |
| Cart and orders | Add/remove items, quantities, edit customization, subtotal, discounts, VAT/tax, total, hold/restore/cancel | Partial: cart, cancel, calculations present; Hold Order is inert; held-order restore absent | Not started |
| Checkout and payments | Checkout, payment selection, cash/card/wallet if supported, status, transaction records, validation | Present in source; card/wallet status is simulated; delivery/payment runtime not verified | Not started |
| Receipts | Preview, printing, 80mm layout, order/items/payment/tax/business details | 80mm browser receipt/print source present; native printer not found | Not started |
| Order management | History, details, status, search, filtering | GET API exists; no history/detail UI found | Not started |
| Inventory | Stock, updates, movements, low-stock indicators, synchronization | Stock display and sale decrement present; adjustments/movements/sync absent | Not started |
| Offline operation | Local catalog, supported orders, eligible cash sales, persistent queue, retry, status, conflict behavior | No frontend persistence, queue, retry, or sync found | Not started |
| Authentication and authorization | Login, manager-only role requested, permissions, one-business initial scope, registered devices | No login/roles/auth found; new migration scope | Manager-only role, one business initially, and device enrollment are owner-approved; login/enrollment details to define | Not started |
| Hardware | Thermal printer and barcode/QR scanner workflows | Browser print CSS only; native printer/scanner integration absent | Not started |
| Owner-defined scope | Refunds, voids, shifts, cash drawer, end-of-day reports, customer records | No such workflows found in inspected app | Refunds, voids, shifts, cash drawer, and end-of-day reporting are in scope; details to define |

## Phase 1 audit checklist

- [ ] Record each feature as present, absent, or unclear in the original app.
- [ ] Document current behavior and calculations with source evidence.
- [ ] Create a behavior parity checklist.
- [ ] Capture reference screenshots of every original screen in `docs/reference/`.
- [ ] Identify login, roles, multi-business, and device functionality as existing or new.
- [ ] Do not mark any feature implemented, tested, or verified before evidence exists.
