# Mac OS System Setup

## Build tools

### Xcode Command Line Tools (gives your mac a C compiler)

#### Installation

```bash
xcode-select --install
```

### Homebrew (third-party macOS package manager)

#### Installation

1. In your terminal, type the following command:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. The script will explain what changes it will make and prompt you before the installation begins.

#### Configuration

Now you will update your `PATH` environment variable so your system has access to the Homebrew commands

1. Ensure you have `~/.profile` file by typing the following command in your terminal:

```bash
touch ~/.profile
```

2. Add the Homebrew directory to your `PATH` by typing the following command in your terminal

```bash
echo "export PATH=\"/usr/local/bin:/usr/local/sbin:$PATH\"" >> ~/.profile
```

## Required software

### Python 3

#### Installation

```bash
brew install python3
```

#### Configuration

Add the `python3` directory to your `PATH` so your system has access to the Python commands

```bash
echo "export PATH=\"/usr/local/opt/python3/libexec/bin:$PATH\"" >> ~/.profile
```

### Git (distributed version control software)

#### Installation

```bash
brew install git
```

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

```bash
brew install postgresql@10
```

#### Configuration

Create a PostgreSQL user

```bash
sudo createuser -U postgres YOUR_USERNAME
```

Start the server

```bash
sudo pg_ctl -D /usr/local/var/postgres start
```


### Node Version Manager (NVM)

#### Installation

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
```

Check successful installation using

```bash
nvm --version
```

#### Configuration

List the available versions of Node.js

```bash
nvm ls-remote
```

Install the version of Node.js that you require

```bash
nvm install 6.9.1
```

Switch between installed versions of Node.js

```bash
nvm use 6.9.1
```

## Encrypted Volume

### Usage

Create an encrypted `.dmg` image file on Mac (Source: [Apple](https://support.apple.com/guide/disk-utility/create-a-disk-image-dskutl11888/mac))

- In the `Disk Utility` app on your Mac, choose `File` > `New Image` > `Blank Image`.
- Enter a filename for the disk image, add tags if necessary, then choose where to save it. This is the name that appears in the Finder, where you save the disk image file before opening it.
- In the `Name` field, enter the name for the disk image. This is the name that appears on your desktop and in the `Finder` sidebar, after you open the disk image.
- In the `Size` field, enter a size for the disk image.
- Click the `Format` pop-up menu, then choose a format:
  - If you’re using the encrypted disk image with a Mac computer using macOS 10.13 or later, choose `APFS` or `APFS (Case-sensitive)`.
  - If you’re using the encrypted disk image with a Mac computer using macOS 10.12 or earlier, choose `Mac OS Extended (Journaled)` or `Mac OS Extended (Case-sensitive, Journaled)`.
- Click the `Encryption` pop-up menu, then choose an encryption option.
- Enter and re-enter a password to unlock the disk image, then click `Choose`. WARNING: If you forget this password, you won’t be able to open the disk image and view any of the files.
- Use the default settings for the rest of the options:
  - Click the `Partitions` pop-up menu, then choose `Single partition - GUID Partition Map`.
  - Click the `Image Format` pop-up menu, then choose `read/write` disk image.
- Click `Save`, then click `Done`.
- Disk Utility creates the disk image file where you saved it in the Finder and mounts its disk icon on your desktop and in the `Finder` sidebar.
- In the `Finder`, copy the documents you want to protect to the disk image.
- If you want to erase the original documents so they can’t be recovered, drag them to the `Trash`, then choose `Finder` > `Empty Trash`.
