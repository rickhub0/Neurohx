# Security Specification - Neurohx

## Data Invariants
1.  **User Ownership**: All user-specific data (chats, journals, habits, moods, assessments) must be owned by the authenticated user.
2.  **Relational Integrity**: Sub-collections (chats, journals, etc.) must reside under the correct `users/{userId}` path where `userId` matches the authenticated user's UID.
3.  **Schema Enforcement**: All writes must conform to the schema defined in `firebase-blueprint.json`.
4.  **Immutability**: Fields like `createdAt` and `userId` should not be changeable after initial document creation.
5.  **Role Integrity**: Plan levels (free, starter, pro, premium) can only be set to 'free' by a regular user during creation, or managed by an admin/system. (Note: The app seems to have a payment flow with Razorpay, so plan updates are likely handled via a backend or admin, but the current client-side code shows `updateDoc` for user profile).
6.  **Support Tickets**: Users can create tickets, but only admins can update the status or priority.

## The "Dirty Dozen" Payloads

1.  **Payload**: `updateDoc(doc(db, 'users', 'other-user-id'), { plan: 'premium' })`
    -   **Attack Type**: Privilege Escalation / Cross-User Write.
    -   **Expected Result**: PERMISSION_DENIED.
2.  **Payload**: `addDoc(collection(db, 'users', auth.currentUser.uid, 'chats'), { userId: 'other-user-id', messages: [] })`
    -   **Attack Type**: Relational Sync Failure (userId mismatch).
    -   **Expected Result**: PERMISSION_DENIED.
3.  **Payload**: `updateDoc(doc(db, 'users', auth.currentUser.uid), { plan: 'admin' })`
    -   **Attack Type**: Privilege Escalation (self-assigning admin role).
    -   **Expected Result**: PERMISSION_DENIED.
4.  **Payload**: `addDoc(collection(db, 'users', auth.currentUser.uid, 'journals'), { title: 'A'.repeat(500), content: 'Short' })`
    -   **Attack Type**: Resource Poisoning (overflowing title size).
    -   **Expected Result**: PERMISSION_DENIED.
5.  **Payload**: `updateDoc(doc(db, 'users', auth.currentUser.uid, 'habits', 'habit-id'), { streak: 999999, extraField: 'Hacked' })`
    -   **Attack Type**: Shadow Update / Value Poisoning.
    -   **Expected Result**: PERMISSION_DENIED.
6.  **Payload**: `deleteDoc(doc(db, 'support_tickets', 'any-ticket-id'))`
    -   **Attack Type**: Unauthorized Deletion by regular user.
    -   **Expected Result**: PERMISSION_DENIED.
7.  **Payload**: `addDoc(collection(db, 'users', auth.currentUser.uid, 'moods'), { value: 99, date: '2026-04-19' })`
    -   **Attack Type**: Out-of-range Value Poisoning.
    -   **Expected Result**: PERMISSION_DENIED.
8.  **Payload**: `getDoc(doc(db, 'users', 'other-user-id'))`
    -   **Attack Type**: Unauthorized Read of another user's profile.
    -   **Expected Result**: PERMISSION_DENIED.
9.  **Payload**: `updateDoc(doc(db, 'users', auth.currentUser.uid), { createdAt: serverTimestamp() })`
    -   **Attack Type**: Mutating Immutable Field.
    -   **Expected Result**: PERMISSION_DENIED.
10. **Payload**: `addDoc(collection(db, 'referrals'), { status: 'completed', rewardClaimed: true })`
    -   **Attack Type**: Self-Claiming Referral Reward during creation.
    -   **Expected Result**: PERMISSION_DENIED.
11. **Payload**: `getDocs(query(collection(db, 'support_tickets')))` (Without filtering by userId)
    -   **Attack Type**: Unauthorized List Query access.
    -   **Expected Result**: PERMISSION_DENIED.
12. **Payload**: `addDoc(collection(db, 'users', auth.currentUser.uid, 'journals'), { docId: 'too-long-id-'.repeat(20) })`
    -   **Attack Type**: ID Poisoning (Maliciously long custom ID).
    -   **Expected Result**: PERMISSION_DENIED.

## Test Runner (Draft)
A comprehensive test suite will be implemented using `@firebase/rules-unit-testing`.
