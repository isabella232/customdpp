
The latest binary for AzCopy along with installation instructions may be found here.

https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10

First, download the AzCopy V10 executable file to any directory on your computer. AzCopy V10 is just an executable file, so there's nothing to install.

Windows 64-bit (zip)
Windows 32-bit (zip)
Linux x86-64 (tar)
macOS (zip)
These files are compressed as a zip file (Windows and Mac) or a tar file (Linux). To download and decompress the tar file on Linux, see the documentation for your Linux distribution.

 Note

If you want to copy data to and from your Azure Table storage service, then install AzCopy version 7.3.

Run AzCopy
For convenience, consider adding the directory location of the AzCopy executable to your system path for ease of use. That way you can type azcopy from any directory on your system.

If you choose not to add the AzCopy directory to your path, you'll have to change directories to the location of your AzCopy executable and type azcopy or .\azcopy in Windows PowerShell command prompts.

As an owner of your Azure Storage account, you aren't automatically assigned permissions to access data. Before you can do anything meaningful with AzCopy, you need to decide how you'll provide authorization credentials to the storage service.