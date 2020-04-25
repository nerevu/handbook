# Getting Started

This section contains a several topics to help a new user get started during their first week at Nerevu.

## System Setup

### Initial setup

- [Mac OS](mac-os)
- [Windows](windows)
- [Linux](linux)

### Python environment setup

Install `virtualenv` to manage your Python environments

```bash
pip install virtualenv
```

Create and activate a virtual environment

```bash
cd REPO_NAME
virtualenv --no-site-packages --python=python3.x venv
source venv/bin/activate
```

Install required Python libraries

```bash
pip install -r requirements.txt -r dev-requirements.txt
```

### Text Editor setup

* enable [Newline at End of File](https://github.com/editorconfig/editorconfig/wiki/Newline-at-End-of-File-Support) ([reason](https://www.reddit.com/r/Python/comments/1zjugg/question_why_does_pep8_recommend_leaving_a_blank/)).

* enable Unix-style line endings (`View menu -> Line Endings -> …` in Sublime Text).

* enable [Trim trailing spaces](https://github.com/editorconfig/editorconfig/wiki/Property-research%3A-Trim-trailing-spaces) ([reason](https://softwareengineering.stackexchange.com/a/121560)).

* [VS Code configuration](vs-code)

## Fastmail

### Access your new account

1. [Login to Fastmail](https://www.fastmail.com/login) with the following information

- username: [first initial][last name]@nerevu.com, e.g., `rcummings@nerevu.com`
- password: separately emailed to you

2. [Change your password](https://www.fastmail.com/help/account/password.html)

### Create App password to connect Fastmail to Cloze

Fastmail comes under `Other Email` type in Cloze, and therefore giving access to a 3rd-party application, like Cloze, would need an app password that will be used by that application.

1. From your Fastmail dashboard, open the dropdown menu at the top-left corner.
2. Click on `Settings` and then on `Passwords & Security`.
3. Look for `App Passwords` in this page and click on `Manage →`.
4. Enter your password at the top of this page and click on `Unlock`.
5. Click on `New App Password` button.
6. Open the dropdown adjacent to `Name`, select `Custom` and enter `Cloze` in the input box.
7. Make sure that the `Access` field is set to `Mails, Contacts & Calendars`.
8. Click on `Generate Password` and make a temporary note of it, as it will not be displayed again once you click on `Done`.
9. Use this password when you add this account in Cloze (Refer to [Linking various Accounts and Services to Cloze](#linking-various-accounts-and-services-to-cloze)).

### Setup Fastmail Calendar Sync

Nerevu Group uses Fastmail Calendar to keep everyone up to date on events that are happening. This section will show you how to sync your existing calendars so that they update Fastmail. In general, there are two options for syncing with your teammates:

1. Show them your events with the titles and all details (this is good for a "Work" calendar)
2. Or show them an event block that just indicates that you are busy during that block (no details are shared - this is good for time blocks when you are busy but don't need to share details about why)

We will go over how to setup both of these syncs.

#### From Google Calendars

See short video [here](https://user.fm/files/v2-2be5127a4a80f34fcf67ef8bfd5198f3/sync%20google%20calendar%20in%20fastmail.mp4)

## KeePass and Encrypted Volumes

KeePass is the Password Manager that Nerevu Group uses to keep usernames and passwords secure. Encrypted Volumes help us keep important files secure. Please follow the instructions to download both so that you can get access to the Nerevu Group KeePass Database.

### Installation

- [KeePassXC](https://keepassxc.org/)

### Configuration

- open preferences/settings
- click `Security`
- check `Lock Databases after inactivity of` and set seconds to `43200`. This is the maximum time a database can be open before automatically closing. Adding this setting helps us keep our data more secure.
- uncheck `Clear search query after`

### Encrypted Volume Configuration

- [Mac OS](mac-os#encrypted-volume)
- [Windows](windows#encrypted-volume)
- [Linux](linux#encrypted-volume)

### KeePass Setup

1. Add keyfile to your [encrypted volume](#encrypted-volume-configuration)

  - download `.key` keyfile (link will be given to you)
  - cut and paste the downloaded keyfile to your encrypted volume
  - make sure the keyfile does not exist anywhere else on your computer except in the encrypted volume (for security reasons)

2. Open Nerevu Group KeePass Database

  - Open the KeePass app
  - Press `Cancel` if it tries to log into your personal database
  - In the menu bar, navigate to `Database > Open database...`
  - Navigate to the `.kdbx` password database in dropbox (link will be given to you) and press `Open`
  - Enter the password provided to you
  - Click `Browse` to navigate the keyfile stored in your encrypted volume
  - Click Ok

3. You did it! You accessed the Nerevu Group KeePass Database! Now enjoy a few minutes of you time. You deserve it ;)

## Ngrok

`Ngrok` is a service that allows you to forward localhost to the public web through https. This comes in handy when you're testing code that interacts with an API that requires a valid https URL (like QuickBooks in the [Comissioner API](https://github.com/nerevu/commissioner-api)).

You can see how to use it by looking at the [`commissioner-api` README](https://github.com/nerevu/commissioner-api#ngrok).

Visit [Ngrok's website](https://ngrok.com/) to learn more about it.

## Linking various Accounts and Services to Cloze

See [this article for pictures](https://help.cloze.com/help/how-do-i-connect-dropbox-to-cloze)

1. Log in to you Cloze account.
2. Click on `More` at the bottom left corner of the dashboard and then click on `Settings`.
3. Under `Accounts and Services`, click on `Connect Accounts` to expand it.
4. Click on the `Add` button.
5. Select the respective service that you want to connect to Cloze.

- For Fastmail, select `Other Email` under `Other Mail and Calendar`.
- For Google account, select `Google or G Suite`.
- For Dropbox, go to `Notes and Messaging` and select `Dropbox`.

6. This will take you to the sign-in page for the respective service.
7. Sign in to the service using Nerevu credentials.
8. Review the permissions and click on `Allow`, when prompted.

## Google Sheets API setup

### Enable Google Cloud Platform

1. Go to gSuite [Additional Google services](https://admin.google.com/u/2/ac/appslist/additional)
2. Search for `Google Cloud Platform` and click the checkbox
3. Click `on` in the blue bar that appears above

### Enable Google Drive API

1. Go to [Google Developers Console](https://console.developers.google.com).
2. If you see `Google Drive API` in the list of APIs at the bottom of the screen, click it and skip the remaining steps.
3. Otherwise, click on `ENABLE APIS AND SERVICES`.
3. Search for `Google Drive API` and click on it.
4. Click on `Enable`.

### Create API Credentials

1. Click on `CREATE CREDENTIALS`
2. Select the options as shown below:

![credentials](https://user.fm/files/v2-b7c2674a972bc45d82aa29babbb54349/google-sheets-credentials.png)

3. Click on `What credentials do I need?`.
4. Enter the details similar to shown below (name the service account after the client):

![service_account](https://user.fm/files/v2-92432356bd687da43b4a051ed907cf1a/create-service-account.png)

5. Click `Continue` and save the downloaded JSON key file.

### Access Google Sheets workbook

1. Open the Google Sheets workbook that you need to access through APIs.
2. Open the downloaded JSON key file and look for `client_email`.
3. Share the workbook with the `client_email` address and grant `Edit` permission.
4. Uncheck `Notify People` since this email address is not handled by a human.
5. Follow the respective API/library documentation for Google Sheets, for authentication and usage.

## Other Useful Information

### Time Management

- Don't dwell on something too long. Just move to a different project.

