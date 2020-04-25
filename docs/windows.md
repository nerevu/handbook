# Windows System Setup

NOTE: The following has not been fully tested!

## Build tools

MISSING

## Required software

### Python 3

#### Installation

MISSING

#### Configuration

Add the `python3` directory to your `PATH` so your system has access to the Python commands

MISSING

### Git (distributed version control software)

#### Installation

MISSING

#### Configuration

Set your name (replace `Your Name` with your first and last name, e.g., `Reuben Cummings`)

```bash
git config --global user.name "Your Name"
```

Set your commit email address (replace `your@email.address` with your email address, e.g., `reubano@gmail.com`).

Note: This should be the same email address you used to signup for your GitHub account

```bash
git config --global user.email "your@email.address"
```

Check that both configurations have been set correctly

```bash
git config --global --list
```

### PostgreSQL (relational database)

#### Installation

MISSING

### Node Version Manager (NVM)

#### Installation

MISSING

#### Configuration

MISSING

## Encrypted Volume w VeraCrypt

### Installation

- [VeraCrypt](https://www.veracrypt.fr/en/Downloads.html)
- follow the setup wizard instructions (use the defaults)
- if VeraCrypt is not working for you, try using one of the other free services found [here](https://www.makeuseof.com/tag/syskey-encryption-alternatives-windows-10/)

### Usage

Use Veracrypt to create an Encrypted Volume

- follow the beginners tutorial [here](https://www.veracrypt.fr/en/Beginner%27s%20Tutorial.html)
- add the password to your personal Keepass Database so you don't forget it
    ![Add a password in Keepass](https://user.fm/files/v2-3212126b246b39151303f4333b83b43b/add_password.PNG)
