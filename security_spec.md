# KHALAB Security Rules Specification

## 1. Data Invariants
- **Products & Promo Codes Info**: Only Administrative Users (verified with Admin Auth configuration or credential checking) can read/write promos, catalogs, product states, or edit global web settings. Non-admins have read-only access to products, catalogs, active themes, and active promos.
- **Order Isolation**: Customers can read and create their own orders. They cannot read or modify other customers' orders.
- **Reviews**: Anyone can read reviews. Signed-in or identified customers can write reviews for products.
- **Administrators**: Admin holds full read and write clearance over all collections.
- **PII Protection**: Mobile numbers, delivery addresses, and customer metadata must be secure and isolated from third-party client queries.

## 2. The Dirty Dozen Payloads (Targeting Exploits)
1. **Privilege Escalation**: Attempting to create or edit products/categories as a standard user.
2. **Global Config Highjacking**: Attempting to rewrite the brand logo, theme colors, or layout properties from a standard client environment.
3. **Ghost Order Creation**: Submitting an order with a mismatched owner ID to make another customer pay or look fake.
4. **Order Status Manipulation**: Attempting to transition an order status from "Pending" to "Shipped" or "Delivered" without admin clearance.
5. **Admin Access Spoofing**: Injecting customized login flags to bypass standard authentication processes.
6. **Denial-of-Wallet ID Poisoning**: Trying to write a massive 2MB string as a Document ID or brand title to inflate cloud Firestore billings.
7. **Promo Creation Hack**: Creating standard user accounts and writing active promos with a 100% discount rate.
8. **Negative Value Pricing**: Attempting to write a product with a price equal to -500BDT.
9. **Spamming Fake Reviews**: Flooding product feedback lists as an anonymous non-registered user targeting negative brand ratings.
10. **Fake Customer Tagging**: Attempting to toggle the `isFake` flag on other users' customer documents.
11. **Order Total tampering**: Setting the total price of elements within an order to 0 BDT while keeping high-value items in the cart array.
12. **Tampering with Timestamps**: Sending a custom retroactive date to force expired promos or bypass order processing times.

## 3. Threat Matrix & Remediation
All validation patterns are implemented inside `firestore.rules` utilizing dedicated helpers to ensure strict schema enforcement, length restrictions, and role-based permissions.
