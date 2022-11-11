# Using `gspread`: Google spreadsheets in Python

## Preparations

1. Head to [Google Developers Console](https://console.developers.google.com/) and create a new project (or select the one you already have).
2. In the box labeled “Search for APIs and Services”, search for “Google Drive API” and enable it.
3. In the box labeled “Search for APIs and Services”, search for “Google Sheets API” and enable it.

4. Go to “[APIs & Services > Credentials](https://console.cloud.google.com/apis/credentials)” and choose “Create credentials > Service account key”.
5. Fill out the form
6. Click “Create” and “Done”.
7. Press “Manage service accounts” above Service Accounts.
8. Press on **⋮** near recently created service account and select “Manage keys” and then click on “ADD KEY > Create new key”.
9. Select JSON key type and press “Create”.

You will automatically download a JSON file with credentials. It may look like this:

```json
{
    "type": "service_account",
    "project_id": "api-project-XXX",
    "private_key_id": "2cd … ba4",
    "private_key": "-----BEGIN PRIVATE KEY-----**\n**NrDyLw … jINQh/9**\n**-----END PRIVATE KEY-----**\n**",
    "client_email": "473000000000-yoursisdifferent@developer.gserviceaccount.com",
    "client_id": "473 … hd.apps.googleusercontent.com",
    ...
}
```

## Usage

For the bot to access an already created file, it needs to be shared with the `client_email` (e.g. `473000000000-yoursisdifferent@developer.gserviceaccount.com`)

After that, the document can be opened like this:

```python
import gspread

gc = gspread.service_account("path-to-json-credentials")

# Open a sheet from a spreadsheet in one go
wks = gc.open("breast_cancer_meds").sheet1
```

Alternatively, the bot can create a new spreadsheet and share it with another user like so:

```python
sh = gc.create('A new spreadsheet')
sh.share('my-google-user@gmail.com', perm_type='user', role='writer')
```

To delete the spreadsheet:

```python
sh = gc.open("A new spreadsheet")
gc.del_spreadsheet(sh.id)
```