# How To

## Evaluate a Custom DPP model with another test set

Assume you are creating a new test set named `entity` in a project, 

1. Create the test set file named `entity.tsv` under the `test` subfolder of the project 

2. Add test cases into `entity.tsv` and use following command to upload the file.
`cdpp push test -n entity`

3. Use following command to start an evaluation on the `entity.tsv` file:
`cdpp eval -n entity`


You may use the `-f` switch in `cdpp push test` command to specify what features should be applied to the test set.


## Terminate an evaluation

An evaluation might take too long time to execute. To abort an evaluation on the default test set, use following command:

> `cdpp delete eval`

To abort an evaluation on another test set (say `entity`), use following command:

> `cdpp delete eval -n entity`


## Observe the default behaviors of DPP service

To observe the default behaviors of DPP service, 

1. Create a new project with empty model files.

2. Upload the empty model files to Microsoft Speech server by `cdpp push model`.

3. Edit the `test/test.tsv`, add the test cases you want to test on the default behaviors.

4. Upload the test file by `cdpp push test`.

5. Start an evaluation by `cdpp eval`.

6. Use `cdpp get eval -t` to get the logs.

The downloaded logs has the output of each test case from DPP service without custom models.