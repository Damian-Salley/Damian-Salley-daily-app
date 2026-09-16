# QuickNotes Signup Documentation Practice

## Task 1 — Code Comments

```javascript
function signup(email, password) {
    if (!email.includes('@')) return { error: 'invalid' };
    if (password.length < 8) return { error: 'weak' };

    // Use a bcrypt cost of 10 to balance password security and hashing performance.
    const hash = bcrypt.hashSync(password, 10);

    const existing = db.users.find(u => u.email === email);
    if (existing) return { error: 'exists' };

    // Keep new accounts unverified until the user confirms their email.
    const user = db.users.insert({ email, hash, verified: false });
    sendEmail(user.email, 'confirm-token-' + user.id);

    return { id: user.id };
}

