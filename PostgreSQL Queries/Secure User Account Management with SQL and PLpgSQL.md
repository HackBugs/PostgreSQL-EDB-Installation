## Conditions --

- A user should be locked after more than 5 failed attempts.  
- Passwords should expire after 180 days.  
- The previous 5 passwords cannot be reused.  
- If an account is inactive for 120 days, it should be automatically deactivated.  
- The password length should be at least 8 characters.  
- The password should follow complexity rules.

> # Your SQL script is mostly correct, but there are a few issues that need to be addressed:

1. **Password Complexity Constraint**: The `password_hash` column is intended to store hashed passwords, which are typically long strings of characters that do not directly reflect the complexity of the original password. Therefore, the complexity check should be applied to the plaintext password before it is hashed, not to the hash itself. This constraint should be handled at the application level, not in the database.

2. **Trigger on `users` Table for Password Reuse**: The `prevent_password_reuse` function is designed to prevent the reuse of the last 5 passwords, but the trigger is set to fire on `INSERT OR UPDATE` on the `users` table. This means it will fire even when non-password fields are updated, which is unnecessary. The trigger should only fire when the `password_hash` is updated.

3. **Trigger on `users` Table for Password History**: The `update_password_history` function is designed to insert a new record into the `password_history` table whenever the `password_hash` is updated. However, the trigger is set to fire on `AFTER INSERT OR UPDATE`, which means it will also fire when non-password fields are updated. This should be restricted to only fire when the `password_hash` is updated.

4. **Deactivate Inactive Accounts**: The `deactivate_inactive_accounts` function is designed to deactivate accounts that have been inactive for 120 days. However, the trigger is set to fire on `BEFORE UPDATE`, which means it will only deactivate accounts when an update occurs. This might not be sufficient if you want to deactivate accounts automatically after 120 days of inactivity. You might need a scheduled job or cron job to handle this.

Here is the corrected version of your script:

```sql
-- Step 1: Create the Users Table
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    failed_attempts INT DEFAULT 0,
    last_failed_attempt TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Step 2: Create Password History Table
CREATE TABLE password_history (
    password_id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(user_id) ON DELETE CASCADE,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Step 3: Enforce Password Complexity (Min 8 Length, Must Include Uppercase, Lowercase, Number, Special Char)
-- This should be handled at the application level, not in the database.

-- Step 4: Lock Account After 5 Failed Attempts
CREATE OR REPLACE FUNCTION lock_account_after_failed_attempts()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.failed_attempts >= 5 THEN
        NEW.is_active := FALSE;
        NEW.last_failed_attempt := CURRENT_TIMESTAMP;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_lock_account
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION lock_account_after_failed_attempts();

-- Step 5: Deactivate Account If Inactive for 120 Days
CREATE OR REPLACE FUNCTION deactivate_inactive_accounts()
RETURNS TRIGGER AS $$
BEGIN
    IF (NEW.is_active = TRUE AND (NOW() - NEW.updated_at) > INTERVAL '120 days') THEN
        NEW.is_active := FALSE;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_deactivate_inactive_accounts
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION deactivate_inactive_accounts();

-- Step 6: Prevent Last 5 Passwords Reuse
CREATE OR REPLACE FUNCTION prevent_password_reuse()
RETURNS TRIGGER AS $$
DECLARE
    recent_passwords TEXT[];
BEGIN
    SELECT ARRAY(
        SELECT password_hash
        FROM password_history
        WHERE user_id = NEW.user_id
        ORDER BY created_at DESC
        LIMIT 5
    ) INTO recent_passwords;

    IF NEW.password_hash = ANY(recent_passwords) THEN
        RAISE EXCEPTION 'Cannot reuse the last 5 passwords';
    END IF;

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_prevent_password_reuse
BEFORE UPDATE OF password_hash ON users
FOR EACH ROW
EXECUTE FUNCTION prevent_password_reuse();

-- Step 7: Update Password History on Change
CREATE OR REPLACE FUNCTION update_password_history()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO password_history (user_id, password_hash)
    VALUES (NEW.user_id, NEW.password_hash);

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_update_password_history
AFTER UPDATE OF password_hash ON users
FOR EACH ROW
EXECUTE FUNCTION update_password_history();

-- Step 8: Insert New User
INSERT INTO users (username, password_hash) VALUES ('john_doe', 'Password1!');

-- Step 9: Update Password (If Changing Password)
UPDATE users SET password_hash = 'NewPassword1!' WHERE user_id = 1;

-- Step 10: Record Failed Login Attempt
UPDATE users SET failed_attempts = failed_attempts + 1 WHERE user_id = 1;
```

### Key Changes:
1. **Password Complexity**: Removed the constraint from the database. This should be handled in the application layer.
2. **Triggers**: Modified the triggers to only fire when the `password_hash` is updated, not on every update.
3. **Deactivate Inactive Accounts**: Consider adding a scheduled job to handle deactivation of inactive accounts if needed.

<hr>

Agar aap yeh check karna chahte hain ki aapka SQL script successfully work kar raha hai ya nahi, toh aap kuch test cases run kar sakte hain. 
### Step-by-Step Testing:

#### 1. **Naya User Create Karna (Insert)**
```sql
INSERT INTO users (username, password_hash) VALUES ('afsar', 'Password1!');
```
- **Check**: `users` table mein naya entry ban gaya hai ya nahi.
```sql
SELECT * FROM users WHERE username = 'afsar';
```

#### 2. **Password Update Karna**
```sql
UPDATE users SET password_hash = 'NewPassword1!' WHERE username = 'afsar';
```
- **Check**: `password_history` table mein naya entry add hua hai ya nahi.
```sql
SELECT * FROM password_history WHERE user_id = (SELECT user_id FROM users WHERE username = 'afsar');
```

#### 3. **Password Reuse Check Karna**
- Same password dobara use karne ki koshish karen:
```sql
UPDATE users SET password_hash = 'NewPassword1!' WHERE username = 'afsar';
```
- **Expected Result**: Error aana chahiye - `Cannot reuse the last 5 passwords`.

#### 4. **Failed Login Attempts Check Karna**
- Failed attempts badhane ke liye:
```sql
UPDATE users SET failed_attempts = failed_attempts + 1 WHERE username = 'afsar';
```
- **Check**: `failed_attempts` ki value badh gayi hai ya nahi.
```sql
SELECT failed_attempts FROM users WHERE username = 'afsar';
```

- 5 failed attempts ke baad account lock ho jana chahiye:
```sql
UPDATE users SET failed_attempts = 5 WHERE username = 'afsar';
```
- **Check**: `is_active` column `FALSE` ho gaya hai ya nahi.
```sql
SELECT is_active FROM users WHERE username = 'afsar';
```

#### 5. **Inactive Account Check Karna**
- `updated_at` column ko 120 days purana date set karen:
```sql
UPDATE users SET updated_at = NOW() - INTERVAL '121 days' WHERE username = 'afsar';
```
- Fir account update karen:
```sql
UPDATE users SET is_active = TRUE WHERE username = 'afsar';
```
- **Check**: `is_active` column `FALSE` ho gaya hai ya nahi (trigger ne account ko deactivate kar diya hai).
```sql
SELECT is_active FROM users WHERE username = 'afsar';
```

#### 6. **Password History Check Karna**
- Multiple password changes karke check karen:
```sql
UPDATE users SET password_hash = 'Password2!' WHERE username = 'afsar';
UPDATE users SET password_hash = 'Password3!' WHERE username = 'afsar';
UPDATE users SET password_hash = 'Password4!' WHERE username = 'afsar';
UPDATE users SET password_hash = 'Password5!' WHERE username = 'afsar';
UPDATE users SET password_hash = 'Password6!' WHERE username = 'afsar';
```
- **Check**: `password_history` table mein 5 entries hain ya nahi.
```sql
SELECT * FROM password_history WHERE user_id = (SELECT user_id FROM users WHERE username = 'afsar');
```

#### 7. **Last 5 Passwords Reuse Check Karna**
- Kisi bhi last 5 passwords mein se password reuse karne ki koshish karen:
```sql
UPDATE users SET password_hash = 'Password2!' WHERE username = 'afsar';
```
- **Expected Result**: Error aana chahiye - `Cannot reuse the last 5 passwords`.

---

### Expected Results:
1. **User Creation**: `users` table mein naya entry add hona chahiye.
2. **Password Update**: `password_history` table mein naya entry add hona chahiye.
3. **Password Reuse**: Last 5 passwords reuse karne par error aana chahiye.
4. **Failed Attempts**: 5 failed attempts ke baad account lock hona chahiye.
5. **Inactive Account**: 120 days ke baad account deactivate hona chahiye.
6. **Password History**: `password_history` table mein sahi entries add honi chahiye.

Agar sab test cases successfully pass ho jaate hain, toh aapka script sahi se kaam kar raha hai. Agar koi error aata hai, toh us error ke hisab se debugging karen.

<!--
```
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    failed_attempts INT DEFAULT 0,
    last_failed_attempt TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP  -- ⚠️ No extra comma here
);
```
-->
