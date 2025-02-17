# how to test the code

## setup the environment

create extensions.json .vscode folder and add the following code

```json
// extensions.json
{
  "recommendations": [
    "ms-azuretools.vscode-azurefunctions",
    "ms-dotnettools.csharp"
  ]
}
```

```json

// launch.json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach to .NET Functions",
            "type": "coreclr",
            "request": "attach",
            "processId": "${command:azureFunctions.pickProcess}"
        }
    ]
}
```

```settings.json
{
    "azureFunctions.deploySubpath": "FirstHttpTrigger/bin/Release/netcoreapp3.1/publish",
    "azureFunctions.projectLanguage": "C#",
    "azureFunctions.projectRuntime": "~3",
    "debug.internalConsoleOptions": "neverOpen",
    "azureFunctions.preDeployTask": "publish (functions)"
}
```

```json

// tasks.json
{
    "version": "2.0.0",
    "tasks": [
      {
      "label": "clean (functions)",
      "command": "dotnet",
      "args": [
        "clean",
        "/property:GenerateFullPaths=true",
        "/consoleloggerparameters:NoSummary"
      ],
      "type": "process",
      "problemMatcher": "$msCompile",
      "options": {
        "cwd": "${workspaceFolder}/FirstHttpTrigger"
      }
      },
      {
      "label": "build (functions)",
      "command": "dotnet",
      "args": [
        "build",
        "/property:GenerateFullPaths=true",
        "/consoleloggerparameters:NoSummary"
      ],
      "type": "process",
      "dependsOn": "clean (functions)",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "problemMatcher": "$msCompile",
      "options": {
        "cwd": "${workspaceFolder}/FirstHttpTrigger"
      }
      },
      {
      "label": "clean release (functions)",
      "command": "dotnet",
      "args": [
        "clean",
        "--configuration",
        "Release",
        "/property:GenerateFullPaths=true",
        "/consoleloggerparameters:NoSummary"
      ],
      "type": "process",
      "problemMatcher": "$msCompile",
      "options": {
        "cwd": "${workspaceFolder}/FirstHttpTrigger"
      }
      },
      {
      "label": "publish (functions)",
      "command": "dotnet",
      "args": [
        "publish",
        "--configuration",
        "Release",
        "/property:GenerateFullPaths=true",
        "/consoleloggerparameters:NoSummary"
      ],
      "type": "process",
      "dependsOn": "clean release (functions)",
      "problemMatcher": "$msCompile",
      "options": {
        "cwd": "${workspaceFolder}/FirstHttpTrigger"
      }
      },
      {
      "label": "func: host start",
      "type": "func",
      "dependsOn": "build (functions)",
      "options": {
        "cwd": "${workspaceFolder}/FirstHttpTrigger/bin/Debug/net8.0"
      },
      "command": "host start",
      "isBackground": true,
      "problemMatcher": "$func-dotnet-watch"
      }
    ]
}
```

## run the function

open the terminal and run the following command using powershell

```powershell
func host start –verbose
```
