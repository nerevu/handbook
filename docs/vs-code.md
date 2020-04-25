# Debugging in VS Code

In VS Code, you can create a `launch.json` file that allows you to add breakpoints and step through your API code. Here are the steps:

1. Open your project in VS Code.
2. Click the debug menu on the sidebar.
3. Click `create a launch.json file`

![Set up launch.json](https://user.fm/files/v2-0e899c0b656c0e0cb343623b2d5c3377/debug.png)

4. Choose `Python` as the language.
5. I typically choose `Python File` as the template, and then just add the following to the configuration.

```json
{
    "configurations": [
        {
            "name": "Dev Ngrok",
            "type": "python",
            "request": "launch",
            "module": "flask",
            "env": {
                "FLASK_APP": "app:create_app('Ngrok')",
                "FLASK_ENV": "development",
                "FLASK_DEBUG": "0"
            },
            "args": [
                "run",
                "--no-debugger",
                "--no-reload"
            ]
        }
    ]
}

```

6. The debug configuration should now show up in the debug menu on the sidebar. Now you can add break points to your code and click the Green Triangle button to run your flask application.

![Run the debug configuration](https://user.fm/files/v2-32b756482dce2e659a3cbdfa86a1402d/run-debug.png)

If you want to run the application with a different configuration mode, you can replace the `Ngrok` in `app:create_app('Ngrok')` with any other config mode.

You can learn more about the VS Code debugger [here](https://code.visualstudio.com/docs/editor/debugging) and more about debugging Python applications [here](https://code.visualstudio.com/docs/python/debugging)
