### Evaluate a Custom DPP model with another test set (put into how to)

To create another test set file under the `test` subfolder of a project as well. 

You may use the `-f` switch to specify what features should be applied to a test set. For example, if a test file (let's say `test/itn_test.tsv`) only has *ITN* related test cases, ????

To upload another test file except the default `test/test.tsv`, put the file into the `test` subfolder of the project, then run following command:

### terminate a evaluation (put how to)
 

> `cdpp push test -n <test_file_name_without_extension>`


You may use the `-n` switch to specify an alternative test set for evaluation.

> `cdpp eval -n <test_file_name_with_extension>`
