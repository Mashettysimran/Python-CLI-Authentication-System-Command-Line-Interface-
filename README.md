# Python CLI Authentication 
A simple command-line authentication system written in Python. Users can register with an email and password, log in, reset a forgotten password, and view the stored user records. Data is saved in a local text file, and passwords are stored as SHA-256 hashes rather than plain text.

The project was originally built in Google Colab and is a good beginner exercise in regular expressions, file handling, and hashing.

## Features

- **Register**: create an account with a validated email and password
- **Login**: authenticate against the stored hashed password
- **Forgot Password**: reset the password for a registered email
- **Retrieve Registered Users**: print the stored emails and password hashes
- **Email validation** using a regular expression
- **Password rules** enforced on registration and reset
- **Hashed storage** using `hashlib.sha256`

## Requirements

- Python 3.6 or higher
- No third-party packages (uses only `re`, `os`, and `hashlib` from the standard library)

## Getting Started

1. Save the script as `auth.py` (or any name you prefer).
2. Run it from a terminal:

   ```bash
   python auth.py
   ```

3. On first run, a `users.txt` file is created automatically in the same folder.

## Usage

The main menu looks like this:

```
💡 Welcome to the System!
1️⃣ Register
2️⃣ Login
3️⃣ Forgot Password
4️⃣ Retrieve Registered Users
5️⃣ Exit
Enter your choice (1/2/3/4/5):
```

| Option | What it does |
|--------|--------------|
| 1 | Asks for an email and password, validates both, and saves the user |
| 2 | Asks for email and password and checks them against the stored hash |
| 3 | Asks for a registered email and a new password, then updates the record |
| 4 | Prints every line in `users.txt` (email and hashed password) |
| 5 | Exits the program |

### Validation Rules

**Email**: must match the pattern `name@domain.tld`, using letters, digits, and the characters `_ . + -` before the `@`.

**Password**:
- At least 6 characters long
- At least one uppercase letter
- At least one special character from `! @ # $ % ^ & * ( ) _ +`

Example of a valid password: `Hello@123`

## How It Works

1. **Storage**: each user is saved as one line in `users.txt` in the format `email,hashed_password`.
2. **Hashing**: when a password is entered, it is converted with SHA-256 and only the hash is stored or compared.
3. **Login**: the entered password is hashed and compared with the stored hash for that email.
4. **Password reset**: the file is read into memory, the matching user's hash is replaced, and the file is rewritten.

## Project Structure

```
.
├── auth.py      # Main script
├── users.txt    # Auto-generated user database (created on first run)
└── README.md
```

## Function Reference

| Function | Purpose |
|----------|---------|
| `hash_password(password)` | Returns the SHA-256 hex digest of a password |
| `is_valid_email(email)` | Checks email format with a regex |
| `is_valid_password(password)` | Checks length, uppercase, and special-character rules |
| `email_exists(email)` | Checks whether an email is already registered |
| `get_stored_password(email)` | Returns the stored hash for an email, or `None` |
| `register_user()` | Handles the registration flow |
| `login_user()` | Handles the login flow |
| `forgot_password()` | Handles the password reset flow |
| `retrieve_raw_data()` | Prints all stored records |
| `main()` | Runs the menu loop |

## Known Limitations

This project is intended for learning, not production use.

- **Unsalted SHA-256**: fast, unsalted hashes are vulnerable to rainbow-table and brute-force attacks. Real systems should use `bcrypt`, `scrypt`, or `argon2`.
- **No identity check on password reset**: anyone who knows a registered email can reset its password. A real system would send a verification code or link.
- **Plain text file storage**: `users.txt` is not encrypted and is readable by anyone with file access.
- **Option 4 exposes all hashes**: the "Retrieve Registered Users" option should be removed or restricted in any real application.
- **Passwords are echoed on screen**: consider using `getpass.getpass()` to hide input.
- **Commas in emails**: since `,` is the field separator, the file format would break if one were stored (the email regex currently prevents this).

## Possible Improvements

- Use `getpass` to hide password input
- Switch to `bcrypt` or `argon2` with per-user salts
- Add email-based verification for password reset
- Limit failed login attempts
- Store users in SQLite or JSON instead of a text file
- Add unit tests for the validation functions

## License

This project is provided for educational purposes. Add a license of your choice (for example, MIT) before sharing publicly.
