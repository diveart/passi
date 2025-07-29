# passi

## How to install

```bash
sudo wget -O /usr/local/bin/passi https://raw.githubusercontent.com/diveart/passi/main/passi && sudo chmod +x /usr/local/bin/passi
```

## How to uninstall

```bash
sudo rm /usr/local/bin/passi
```

## Manual

```
NAME
    passi is a simple yet powerful and convenient password manager.

COMPLETE MODERN PASSWORD MANAGEMENT LIFECYCLE
Passi provides a full-featured password management workflow covering every aspect of modern credential handling. Generate secure passwords with passi genp, store them with structured metadata using passi add, and retrieve credentials instantly via passi show or passi clip, or browse interactively with passi i. For two-factor authentication, scan QR codes from authenticator setup screens with passi qr screenshot.png to extract TOTP secrets, then generate time-based one-time passwords on demand with passi totp. From initial password creation to daily authentication workflows, passi handles the complete security lifecycle without requiring multiple tools or complex integrations.

SYNOPSIS
    passi [file <path>] <command> [args]

FILE SELECTION:
    passi <command>               Use default /home/r/.passi/passistore.asc
    passi file <path> <command>   Use specific file

COMMANDS:
    init
        Initialize new password storage file with demo records.

    interactive
        Interactive mode. Browse password entries and perform actions.
        Aliases: i

    list
        List all password IDs in the password storage file.
        Aliases: ls, l

    show <password_id>
        Show password record for the specified password_id.
        
    show all
        Show entire decrypted password storage file.
        
    clip <password_id>
        Copy password to clipboard for the specified password_id. Clears after 45 seconds.
        Aliases: c, clipp, cp
        
    clipu <password_id>
        Copy username to clipboard for the specified password_id. Clears after 45 seconds.
        Aliases: cu
        
    add
        Add new password entry using editor with pre-generated password.
        Aliases: a, new, n
        
    edit <password_id>
        Edit existing password entry with the specified password_id.
        Aliases: e
        
    edit all
        Edit entire password storage file in editor.
        
    open
        Alias for 'edit all' - edit entire password storage file.
        Aliases: o
        
    clipt <password_id>
        Copy TOTP code to clipboard for the specified password_id. Clears after 45 seconds.
        Aliases: ct, c2fa
        
    totp <password_id>
        Show TOTP authentication code for the specified password_id.
        Aliases: t, authenticate, 2fa

    qr <image_file>
        Extract TOTP secret from QR code image for 2FA setup.
        Outputs totp_secret and totp_secret_details lines to copy into records.
            Aliases: scan, authqr, qr2fa, totpqr, 2faqr

    genp [length]
        Generate secure password. Default length: 30 characters.
        Aliases: generate, gp, g
        
    git <git_command>
        Execute git commands in password store directory.
        Use 'passi git init' to initialize git repository.
        
    version
        Show version information.
        
    help
        Show short help with examples.
        
    man
        Show this detailed help.

ENVIRONMENT VARIABLES:
    PASSI_DIR              Password store directory (default: /home/r/.passi)
    PASSI_CLIP_TIME        Clipboard timeout (default: 45 seconds)
    X_SELECTION            X11 clipboard selection (default: clipboard)
    EDITOR                 Text editor for editing records

PASSWORD STORAGE FILE FORMAT:
    Records use rec format with fields like password_id, username, password, url, notes.
    Multiline fields use '+' continuation lines.
    See https://www.gnu.org/software/recutils/ for more details.

EXIT CODES:
    0    Success
    1    General error
    130  Interrupted (Ctrl+C)

DEPENDENCIES:
    - gpg (encryption)
    - fzf (interactive selection)
    - xclip or wl-copy (clipboard)
    - oathtool (TOTP codes)
    - zbar-tools (QR code scanning)
    - git (optional versioning)
    - diff (for detecting changes during editing)

SEE ALSO:
    gpg(1), fzf(1), oathtool(1), zbarimg(1), recutils(7)

COPYING
This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.

MAIN DIFFERENTIATION POINTS PASSI VS PASS
- Single encrypted ASCII file, zero metadata leakage
- Typo-proof symmetric encryption
- Ability to structure records and easily get needed values
- 2FA support: built-in QR scanning and TOTP generation
- Easy switching of multiple password storage files

REAL LIFE WORKFLOWS

INITIAL SETUP
passi init                         # Initialize your default password storage file
passi open                         # Add passwords in bulk by editing the entire password storage file

INTERACTIVE MODE
passi i                            # Browse entries visually, then choose actions

ADDING NEW PASSWORD RECORDS
passi genp 30                      # Generate a secure 30-character password
passi add                          # Add new record (pre-filled with generated password)
passi qr ~/Downloads/setup-qr.png  # Extract TOTP secret from authenticator QR code
passi edit gmail                   # Add the TOTP secret to your existing record

DAILY USAGE
passi clipu work/vpn               # Copy username for work VPN
passi clip personal/gmail          # Copy password to clipboard for login
passi totp work/gmail              # Show 2FA code for Gmail login
passi clipt bank                   # Copy 2FA code to clipboard for banking

BROWSING AND FINDING
passi list                         # List all password IDs
passi show work/wifi               # Show record for work Wi-Fi
passi clip work/email              # Copy work email password to clipboard
passi show all                     # Show all password storage file records

MAINTENANCE AND UPDATES
passi edit gmail                   # Edit specific record directly
passi show gmail | grep url        # Find URL for specific service

MANAGING MULTIPLE CONTEXTS
passi file work.asc init           # Create separate work password storage file
passi file work.asc add            # Add work-specific password
passi file personal.asc clip gmail # Access personal Gmail from personal password storage file
passi file clients.asc show        # Browse client access credentials

EMERGENCY WORKFLOWS
gpg -d ~/.passi/passistore.asc     # Direct GPG decryption w/o passi installed
gpg -d vault.asc | grep -A5 gmail  # Find specific entry without passi

ADVANCED WORKFLOWS
# Extract specific data for scripts:
passi show aws | grep username     # Get AWS username
passi totp work/vpn > /tmp/code    # Save 2FA code for script use

# Bulk operations:
passi open                         # Edit multiple records at once
passi show all | grep -A5 "work/"  # Find all work-related records

# Integration with other tools:
passi list | dmenu | passi clip -  # GUI password selection
passi clip $(passi list | fzf)     # Alternative interactive selection

VERSION CONTROL
passi git init                     # Enable version control for your password history
passi git log                      # View change history
passi git diff HEAD~1              # See what changed in last edit
passi git branch backup            # Create backup branch before major changes

TERMINOLOGY & CONCEPTS
🔐 CORE CONCEPTS:
- Password Storage File   Single encrypted file containing all your data
- Master Password         Single password that encrypts your entire password storage file
- Record                  A complete set of login, password, and any other related information
- Password ID             Unique identifier for each record

📝 DEFAULT RECORD FORMAT:
- password_id             Record identifier
- url                     Website or service URL
- username                Login username
- password                Login password
- totp_secret             Base32 secret for 2FA/TOTP codes
- totp_secret_details     Full otpauth:// URL from QR codes, includes totp_secret
- notes                   Free-form notes

🔧 TECHNICAL TERMS:
- GPG                     GNU Privacy Guard - encryption software
- AES256                  Advanced Encryption Standard with 256-bit key
- TOTP                    Time-based One-Time Password (6-digit 2FA codes)
- TOTP Secret             Secret key used to generate 6-digit codes
- QR Code                 Square barcode containing TOTP setup information
- QR Code URL             otpauth:// URL embedded in QR codes for 2FA setup
- Rec Format              GNU standard for human readable, plain text databases

💡 EXAMPLES:
- Password IDs:           gmail personal, Work VPN, bank_main, github/dev
- Complete Record:        password_id: work/gmail
                          username: john.doe@example.com
                          password: MyStr0ngP@ssw0rd!
                          url: https://mail.google.com
                          totp_secret: JBSWY3DPEHPK3PXP
                          custom_field: custom contents
                          2fa_email: personal_email@example.com
                          notes: Work email account
                          + Recovery phone: +1-555-0123

🎯 DESIGN TRADE-OFFS
Passi prioritizes:
- Simple code over extensive functionality
- But simple data format over simple code
- Human-readable data over format performance optimization
- Single file over complex directory structure
- Metadata privacy over filesystem convenience
- Symmetric encryption over asymmetric key sharing
- Master password in your head over managing keys and certificates   

NOTABLE ADVANTAGES

1. TYPO-PROOF SYMMETRIC ENCRYPTION
Passi prevents accidental password changes by validating your master password against the existing password storage file before allowing any modifications. When you enter the password for encryption, passi first tests that this same password can successfully decrypt your existing data - eliminating the risk of typos creating an inaccessible password storage file.
So this is impossible with passi even though you use symmetric encryption:
"I was editing my password database at 2 AM, made a typo during the re-encryption, and now my 10 years of passwords are locked away forever because I can't figure out what I mistyped."

2. ULTIMATE BACKUP FLEXIBILITY
Passi's single-ASCII-file design means you can backup your entire password storage file using any method imaginable. Print the encrypted file directly (cat vault.asc | lpr) for fireproof storage in a safe, or print the decrypted contents (gpg -d vault.asc | lpr) for emergency access without computers. Email the encrypted file to yourself, paste it into secure notes in your favorite messenger, or simply copy-paste the text content anywhere you trust. Unlike directory-based password managers that require special backup procedures, passi works with every communication method humans have invented.

3. MULTIPLE PASSWORD STORAGE FILES
Passi makes it trivial to maintain separate encrypted password storage files for different contexts using the `file` command. Keep personal passwords in `personal.asc`, work credentials in `work.asc`, and client access in `clients.asc` - each with their own master password, switched instantly with `passi file work.asc show`.

4. SIMPLE YET POWERFUL SECURITY MODEL
Passi uses straightforward "one master password" symmetric AES encryption, avoiding complex key management while being more quantum-resistant than RSA/ECC systems. At ~800 lines of readable bash code, it's simple enough to audit yet strong enough to hide everything in a single encrypted blob without revealing any metadata.

5. MODERN INTERFACE
Passi provides a sleek interactive experience with fzf-powered selection menus instead of clunky tab completion, letting you visually browse and search your passwords. Smart field extraction means you can copy passwords, usernames, or 2FA codes to clipboard with simple commands like passi clip, passi clipu, or passi clipt. The flexible data structure lets you introduce any fields you need - from recovery emails to security questions - structuring your private data exactly how you want it.

```
