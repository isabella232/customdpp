
# Get start with Custom DPP CLI

Custom DPP CLI is a command-line utility that you can upload, evaluate and deploy your custom display format rules which applied to the Microsoft Speech recognition results. This article helps you download Custom DPP CLI, and then go through a typical customization workflow step by step.

## Download Custom DPP CLI

First, download the Custom DPP CLI executable file (`cdpp`) to any directory on your computer. Custom DPP CLI is just an executable file, so there's nothing to install.

* Windows 64-bit (zip)
* Windows 32-bit (zip)
* Linux x86-64 (tar)
* macOS (zip)

These files are compressed as a zip file (Windows and Mac) or a tar file (Linux). To download and decompress the tar file on Linux, see the documentation for your Linux distribution.

## Run Custom DPP CLI

For convenience, consider adding the directory location of the Custom DPP CLI executable to your system path for ease of use. That way you can type `cdpp` from any directory on your system.

If you choose not to add the Custom DPP CLI directory to your path, you'll have to change directories to the location of your Custom DPP CLI executable and type `cdpp` or `.\cdpp` in Windows PowerShell command prompts.


https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10

## Get command help

To see a list of commands, type `cdpp -h` and then press the ENTER key.

To learn about a specific command, just include the name of the command (For example: `cdpp push -h`).

## Create a project

To get started, you need an Azure subscription key and region identifier (for example, eastus, westus). Since Custom DPP service works together with [Custom Speech](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/custom-speech-overview) service, you should use the same Azure subscription key and region identifier for both services.

Content like display format rule data, models and tests are organized into projects in the Custom DPP. Each project is specific to a domain and locale. For example, you might create a project for call centers that use English in the United States.

To create a project, run the following command:

> `cdpp init <name>`

Then, follow the prompt instructions to type in the locale name, region identifier and the Azure subscription key. 
If the command is succeeded, a project folder is created at the current directory you are running the command, and in the project folder, scaffolding rule files and test files are created.

## Prepare your data for Custom DPP model

Custom DPP supports three kinds of rules for display format customization, *rewrite*, *profanity* and *ITN*. The collection of the rule data files form a Custom DPP model. The scaffolding rule files of a project are in the `data` subfolder of the project folder. 

* `data/itn.pat` - The pattern rules to build a custom *ITN* model.
* `data/rewrite.tsv` - The key value pair rules about how to *rewrite* one phrase to another.
* `data/profanity.tsv` - The collection of *profanity* words.

Please see [Concepts](CONCEPTS.md) for more details about how to add/edit the custom rules, and [VS Code](https://code.visualstudio.com/) is recommended as the editor of the rule data files. 

> Note 
> VS Code might convert the TAB char which required in *rewrite* rule file into spaces automatically. Please check the TAB setting in VS Code before editing the rule files, make sure the TAB char will NOT be converted. You may run `cdpp check` to check the grammars of the rule files and find this kind of issue if has any.

Once the rule files of a Custom DPP model are ready, run following command to upload them to Microsoft Speech server.

> `cdpp push model`

## Evaluate your Custom DPP model

The `test/test.tsv` file is the default test set file for a project. You map put all your test cases into this file, or create another test set file under the `test` subfolder of a project as well.

## Deploy your Custom DPP model

To deploy the model to another region, you can use `cdpp config` to change the region and subscription key settings of the project, make the project points to another region, and then do the deployment for that region.

## Other useful commands

To list all models, test sets, evaluation and deployment operations in the subscription, run the following command:

> `cdpp list [ model | test | eval | deploy ]`

## Next steps

* Learn the basic [Concepts](CONCEPTS.md) in Custom DPP



Inline help

List of commands
The following table lists all AzCopy v10 commands. Each command links to a reference article.

Command	Description
azcopy bench	Runs a performance benchmark by uploading or downloading test data to or from a specified location.
azcopy copy	Copies source data to a destination location
azcopy doc	Generates documentation for the tool in Markdown format.
azcopy env	Shows the environment variables that can configure AzCopy's behavior.
azcopy jobs	Subcommands related to managing jobs.
azcopy jobs clean	Remove all log and plan files for all jobs.
azcopy jobs list	Displays information on all jobs.
azcopy jobs remove	Remove all files associated with the given job ID.
azcopy jobs resume	Resumes the existing job with the given job ID.
azcopy jobs show	Shows detailed information for the given job ID.
azcopy load	Subcommands related to transferring data in specific formats.
azcopy load clfs	Transfers local data into a Container and stores it in Microsoft's Avere Cloud FileSystem (CLFS) format.
azcopy list	Lists the entities in a given resource.
azcopy login	Logs in to Azure Active Directory to access Azure Storage resources.
azcopy logout	Logs the user out and terminates access to Azure Storage resources.
azcopy make	Creates a container or file share.
azcopy remove	Delete blobs or files from an Azure storage account.
azcopy sync	Replicates the source location to the destination location.
 Note

AzCopy does not have a command to rename files.

Use in a script
Obtain a static download link
Over time, the AzCopy download link will point to new versions of AzCopy. If your script downloads AzCopy, the script might stop working if a newer version of AzCopy modifies features that your script depends upon.

To avoid these issues, obtain a static (unchanging) link to the current version of AzCopy. That way, your script downloads the same exact version of AzCopy each time that it runs.

To obtain the link, run this command:

Operating system	Command
Linux	curl -s -D- https://aka.ms/downloadazcopy-v10-linux | grep ^Location
Windows	(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).headers.location
 Note

For Linux, --strip-components=1 on the tar command removes the top-level folder that contains the version name, and instead extracts the binary directly into the current folder. This allows the script to be updated with a new version of azcopy by only updating the wget URL.

The URL appears in the output of this command. Your script can then download AzCopy by using that URL.

Operating system	Command
Linux	wget -O azcopy_v10.tar.gz https://aka.ms/downloadazcopy-v10-linux && tar -xf azcopy_v10.tar.gz --strip-components=1
Windows	Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>
Escape special characters in SAS tokens
In batch files that have the .cmd extension, you'll have to escape the % characters that appear in SAS tokens. You can do that by adding an additional % character next to existing % characters in the SAS token string.

Run scripts by using Jenkins
If you plan to use Jenkins to run scripts, make sure to place the following command at the beginning of the script.


Copy
/usr/bin/keyctl new_session
Use in Azure Storage Explorer
Storage Explorer uses AzCopy to perform all of its data transfer operations. You can use Storage Explorer if you want to leverage the performance advantages of AzCopy, but you prefer to use a graphical user interface rather than the command line to interact with your files.

Storage Explorer uses your account key to perform operations, so after you sign into Storage Explorer, you won't need to provide additional authorization credentials.


Configure, optimize, and fix
See any of the following resources:

AzCopy configuration settings

Optimize the performance of AzCopy

Troubleshoot AzCopy V10 issues in Azure Storage by using log files

Use a previous version
If you need to use the previous version of AzCopy, see either of the following links:

AzCopy on Windows (v8)

AzCopy on Linux (v7)