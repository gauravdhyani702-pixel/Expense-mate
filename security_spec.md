# Security Specification - ExpenseMate Expense Tracker

## 1. Data Invariants
- An expense must always have a valid `userId` matching the authenticated user's UID.
- Amounts must be positive numbers.
- Timestamps (`createdAt`, `updatedAt`) must be server-generated or validated against request time.
- Users can only read, update, or delete their own data.

## 2. The "Dirty Dozen" Payloads (Denial Expected)
1. **Identity Spoofing**: Creating an expense with someone else's `userId`.
2. **Resource Poisoning**: Using a 1MB string as a category or title.
3. **Negative Amount**: Setting `amount: -100`.
4. **Illegal ID**: Attempting to create a document with ID `../../secrets`.
5. **PII Leak**: Querying for all users' email addresses.
6. **Ghost Fields**: Adding `isVerified: true` to a user profile.
7. **Bypassing Auth**: Attempting to write without a token.
8. **Shadow Update**: Changing `userId` on an existing expense.
9. **Timestamp Manipulation**: Setting `createdAt` to a date in 1999.
10. **Orphaned Writes**: Creating an expense with an invalid user reference.
11. **Denial of Wallet**: Infinite recursion or too many `get()` calls in a rule (minimized here).
12. **Status Skipping**: Modifying immutable fields like `createdAt`.

## 3. Test Runner (Mock Logic)
Verification will be performed via rules deployment and manual testing as standard for this environment.
