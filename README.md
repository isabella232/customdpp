# Custom Display Post Processing CLI

## Overview

The Custom Display Post Processing (DPP) command-line utility presents easy-to-use commands for display format customization of Microsoft Speech service.

Supported on Windows, macOS and several Linux flavors.

## Features and capabilities

The Custom DPP CLI enable the customers to rewrite the display text output by Microsoft Speech service. Moreover, it provides the Invert Text Normalization (ITN) rules and profanity words customizations for many domain specific scenarios.


Supported locales:

:white_check_mark: en-us :white_check_mark: pt-br :white_check_mark: pt-pt :white_check_mark: it-it :white_check_mark: es-es 

:white_check_mark: es-mx :white_check_mark: fr-fr :white_check_mark: fr-ca :white_check_mark: zh-cn :white_check_mark: ja-jp 

Supported service region: 

:white_check_mark: westus :white_check_mark: westus2 :white_check_mark: eastus :white_check_mark: eastus2 :white_check_mark: japaneast

:white_check_mark: centralindia :white_check_mark: northeurope :white_check_mark: westeurope :white_check_mark: eastasia

## Download

The latest binary for cdpp along with installation instructions may be found [here](https://github.com/microsoft/customdpp/download).

## Supported Operations

The general format of Custom DPP CLI commands is: `cdpp [command] [arguments] --[flag-name] [flag-value]`

Available Commands:
* `init` - Create and init a project, scaffolding it with template files
* `push` - Push the custom model files or test set to Microsoft Speech server
* `eval` - Evaluate the latest model pushed to the Microsoft Speech server
* `deploy` - Deploy a custom DPP model and bind to a custom speech model
* `get` - Get model, test set, evaluation or deployment detail information from Microsoft Speech server
* `check` - Check the format/grammar of the custom rule dataset rewrite, profanity and ITN
* `config` - Config subscription key or service region of a project, or get the current project settings
* `delete` - Delete a model, test, eval or deploy from Microsoft Speech server
* `fetch` - Fetch model files or test set from Microsoft Speech server
* `list` - List models, test sets, evaluation or deployment summary information of the subscription from Microsoft Speech server
* `version` - Show version information and supported regions, locales
* `help` - Help about any command

## Find help from your command prompt

For convenience, consider adding the AzCopy directory location to your system path for ease of use. That way you can type `azcopy` from any directory on your system.

To see a list of commands, type `azcopy -h` and then press the ENTER key.

To learn about a specific command, just include the name of the command (For example: `azcopy list -h`).

![AzCopy command help example](readme-command-prompt.png)

If you choose not to add AzCopy to your path, you'll have to change directories to the location of your AzCopy executable and type `azcopy` or `.\azcopy` in Windows PowerShell command prompts.

## Frequently asked questions

### How to observe the default behaviors of the display post processing in Microsoft Speech service?

* The `copy` command is a simple transferring operation. It scans/enumerates the source and attempts to transfer every single file/blob present on the source to the destination.
  The supported source/destination pairs are listed in the help message of the tool.

* On the other hand, `sync` scans/enumerates both the source, and the destination to find the incremental change.
  It makes sure that whatever is present in the source will be replicated to the destination. For `sync`,

* If your goal is to simply move some files, then `copy` is definitely the right command, since it offers much better performance.
  If the use case is to incrementally transfer data (files present only on source) then `sync` is the better choice, since only the modified/missing files will be transferred.
  Since `sync` enumerates both source and destination to find the incremental change, it is relatively slower as compared to `copy`

### Do we support Bring Your Own Storage?