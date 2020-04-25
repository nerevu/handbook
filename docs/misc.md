# Misc

## Common Issues

### Forgot the Environment File - API Connection Issues

Every project should have a `.env` file associated with it. You can find these files stored [as described here](new-projects#create-an-env-file). If you don't have a `.env` file, you will get confusing errors when trying to connect to an API because you will be missing your `client_id` or other important information. To get the `.env` file in your project, check the [`Nerevu Group Dropbox`](new-projects#create-an-env-file) for one and create a symbolic link to it. If you don't find one in Dropbox, [create one and add it to Dropbox](new-projects#create-an-env-file).

## Retainer Invoicing

- Create a non project linked invoice billed to "Retainer Fees" ([example](https://invoicing.xero.com/view/fa1810de-26a7-476b-b347-da3f9b087132))
- Create a project linked credit note paid from "Retainer Fees" ([example](https://go.xero.com/AccountsReceivable/ViewCreditNote.aspx?creditNoteID=b9581d93-7e82-41a5-a639-aa30c6e8eecb))
- Once paid, reconcile the bank transfer
- Apply credits to future retainer based work
