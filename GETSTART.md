
# Get start with Custom DPP CLI

Custom DPP CLI is a command-line utility that you can upload, evaluate, and deploy your custom display format rules which applied to the Microsoft Speech recognition results. This article helps you download Custom DPP CLI, and then go through a typical customization workflow step by step.

## Download Custom DPP CLI

First, download the Custom DPP CLI executable file (`cdpp`) to any directory on your computer. Custom DPP CLI is just an executable file, so there's nothing to install.

* [Windows 64-bit (zip)](bins/cdpp_win_1.0.zip)
* [macOS (zip)](bins/cdpp_mac_1.0.zip)

These files are compressed as a zip file (Windows and Mac) or a tar file (Linux). To download and decompress the tar file on Linux, see the documentation for your Linux distribution.

## Run Custom DPP CLI

For convenience, consider adding the directory location of the Custom DPP CLI executable to your system path for ease of use. That way you can type `cdpp` from any directory on your system.

If you choose not to add the Custom DPP CLI directory to your path, you'll have to change directories to the location of your Custom DPP CLI executable and type `cdpp` or `.\cdpp` in Windows PowerShell command prompts.

## Get command help

To see a list of commands, type `cdpp -h` and then press the ENTER key.

To learn about a specific command, just include the name of the command (For example: `cdpp push -h`).

## Create a project

To get started, you need an Azure subscription key and region identifier (for example, eastus, westus). Since Custom DPP service works together with [Custom Speech](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/custom-speech-overview) service, you should use the same Azure subscription key and region identifier for both services.

Content like display format rule data, models and tests are organized into projects in the Custom DPP. Each project is specific to a domain and locale. For example, you might create a project for call centers that use English in the United States.

To create a project, run following command:

> `cdpp init <project_name>`

Then, follow the prompt instructions to type in the locale name, region identifier and the Azure subscription key. 
If the command is succeeded, a project folder is created at the current directory you are running the command, and in the project folder, scaffolding rule files and test files are created.

> Note <br/>
All the rest `cdpp` commands in this document should be run inside the project folder. You may use `cd <project_name>` to enter the project folder.

## Prepare your data for Custom DPP model

Custom DPP provides three kinds of features for display format customization, *rewrite*, *profanity* and *ITN*. The collection of feature's rule data files forms a Custom DPP model. The scaffolding rule file when a project was created is in the `data` subfolder of the project folder. 

* `data/itn.pat` - The pattern rules to build a custom *ITN* model.
* `data/rewrite.tsv` - The list of key-value pairs which defined how to *rewrite* one phrase to another.
* `data/profanity.tsv` - The collection of *profanity* words.

Please see [Concepts](CONCEPTS.md) for more details about how to add/edit the custom rules, and [VS Code](https://code.visualstudio.com/) is recommended as the editor of the rule data files. 

> Note <br/> VS Code might convert the TAB char which required in *rewrite* rule file into spaces automatically. Please check the TAB setting in VS Code before editing the rule files, make sure the TAB char will NOT be converted. You may run `cdpp check` to check the grammars of the rule files and find this kind of issue.

Once the rule files of a Custom DPP model are ready, run following command to upload them to Microsoft Speech server.

> `cdpp push model`

## Prepare your test set data for evaluation

The `test/test.tsv` file is the default test set file for a project. You may put all your test cases into this file.

A test case is composed by two parts, an input lexical phrase (without any digit, punctuation and symbols) and an expected output phrase.

Once the test set file is ready, run following command to upload it to Microsoft Speech server.

> `cdpp push test`

To add another test set, you can just put the file under the `test` subfolder of a project. Please see [How To](HOWTO.md) for more details about how to work with an alternative test set.

## Evaluate your Custom DPP model

To evaluate a Custom DPP model with the default `test/test.tsv` test set, after pushing the model and test set to Microsoft Speech server, run following command to start an evaluation job:

> `cdpp eval`

You may use the following command to check the status/progress of the evaluation:

> `cdpp get eval`

To download the detailed evaluation result, run following command:

> `cdpp get eval -t`

The downloaded file is saved in `/logs/eval/<test_name>/<yyyyMMddHHmmss>` folder. You can find the actual output for each test case in this file.


To terminate an evaluation if it takes too long time, see [Terminate an evaluation](Howto.md#terminate-an-evaluation).

## Deploy your Custom DPP model

Since a Custom DPP model needs to work with a Custom Speech model together in Microsoft Speech service, a Microsoft Custom Speech model identifier is required to deploy a Custom DPP model. 

To deploy a Custom DPP model, run following command:

> `cdpp deploy <custom_speech_model_id>`

You may use the following command to check the status/progress of the deployment:

> `cdpp get deploy <custom_speech_model_id>`

If the deployment succeeded, the Custom DPP model will be tied to the given Custom Speech model.

If the deployment fails, you may use following command to download the error logs:

> `cdpp get deploy <custom_speech_model_id> -d`

The downloaded error log files are saved in `/logs/deploy/<custom_speech_model_id>/<yyyyMMddHHmmss>` folder.

To deploy the Custom DPP model to another region, you can use `cdpp config` to change the region and subscription key settings, make the project setting points to another region, and then do the deployment for that region.

## Use your Custom DPP model

### Speech SDK

After the Custom DPP model is deployed, it can be consumed by Microsoft [Speech SDK](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-sdk). 

The following C# code shows how to consume a Custom DPP model via the Speech SDK.

```

var subscriptionKey = "<the same subscription key using to deploy the Custom DPP model>"
var region = "<the region identifier for the subscription key>"
var speechModelId = "<your Custom Speech model id that tied to the Custom DPP model>

var config = Microsoft.CognitiveServices.Speech.SpeechConfig.FromSubscription(subscriptionKey, region);
config.SetServiceProperty("postprocessing", speechModelId, ServicePropertyChannel.UriQueryParameter)

```

### REST API

To transcribe a large amount of audio in storage, you may want to use the [batch transcription REST API](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/batch-transcription).

To use a Custom DPP model with a Custom Speech model together in a batch transcription, you just need to deploy the Custom DPP model with the Custom Speech model identifier as the `cdpp deploy` command argument.

## Other useful commands

To list all models, test sets, evaluation and deployment operations in the subscription, run following command:

> `cdpp list [ model | test | eval | deploy ]`

To fetch the data files of a model or test set from the Microsoft Speech server, run following command:

> `cdpp fetch [ model | test ]`

To delete the data files of a model or test set from the Microsoft Speech server, run following command:

> `cdpp delete [ model | test ]`

To delete or abort an evaluation or deployment operation, run following command:

> `cdpp delete [ eval | deploy ]`

## Next steps

* Learn the basic [Concepts](CONCEPTS.md) in Custom DPP.

* See [How To](HOWTO.md) about more usages of the Custom DPP command CLI.