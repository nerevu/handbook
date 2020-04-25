# New Projects

## Create a new Project

Start new projects by following the steps in the README of a [Nerevu Cookiecutter](https://github.com/nerevu?q=cookiecutter) repo.

## ENV file

A file for your environment variables.

1. Create an `env` file in your Dropbox folder

```bash
touch Dropbox/Nerevu/Security/<YOUR_USERNAME>/<PROJECT_NAME>-env
```

2. Navigate to your project directory

```bash
cd /PATH/TO/<PROJECT_NAME>
```

3. Create a symbolic link from the env file to your new project

```bash
ln -s Dropbox/Nerevu/Security/<YOUR_USERNAME>/<PROJECT_NAME>-env .env
```

### Example

Let's say you create a new project called CashFlowAPI and your username is `rcummings`. You should create an env file named `cashflow-api-env` in the `Nerevu Group Dropbox/Security/rcummings/` directory. You should then run `ln -s Nerevu Group Dropbox/Security/rcummings/cashflowapi_env .env` from the root folder of your new project (see [this page](https://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/) to learn more about symbolic links).

We use [python-dotenv](https://github.com/theskumar/python-dotenv#getting-started) to manage environment variables. Make sure you install this package into your new project so your environment variables get picked up by your application.

## Python Requirements

You should set your python projects up with 3 different requirements files that contain references to the packages needed by the program.

1. `base-requirements.txt`
   - Contains the necessary packages for most Nerevu APIs to run (you'll find examples of this in other repositories on GitHub).
2. `requirements.txt`
   - Contains an import statement for the `base-requirements.txt` file.
   - Contains other necessary packages for the current project to run.
3. `dev-requirements.txt`
   - Contains necessary and/or useful packages for developers working on the current project.
