# Linux System Setup

NOTE: The following has not been fully tested!

## Required software

### Python 3

#### Installation

```bash
apt-get install python3
```

#### Configuration

Add the `python3` directory to your `PATH` so your system has access to the Python commands

```bash
echo "export PATH=\"/usr/local/opt/python3/libexec/bin:$PATH\"" >> ~/.profile
```

### Git (distributed version control software)

#### Installation

```bash
apt-get install git
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
apt-get install postgresql@10
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
