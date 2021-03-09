# Monaco HOT - Exercise Six

## Delete Configuration
Configuration which is not needed anymore can also be deleted in automated fashion. This tool is looking for delete.yaml file located in projects root folder and deletes all configurations defined in this file after finishing deployment. In delete.yaml you have to specify the ***name of the configuration how it exists in Dynatrace** (not id) of configuration to be deleted.

Here is one example of delete.yaml with multiple configurations:
```yaml
delete:
  - "auto-tag/app-one"
  - "auto-tag/app-two"
  - "management-zone/app-one"    
  - "calculated-metrics-service/simplenode.staging" 
```
Warning: if the same name is used for the new config and config defined in delete.yaml, then config will be deleted right after deployment.

During this exercise, we will run monaco command line to delete selected configuraiton (auto tag from exercise-one)

# Prerequisites

You have successfully completed exercise-one


## Step One - create a delete.yaml file


1. create new `delete.yaml `file under `monaco-hot/exercises/1_exercise-one/projects`
2. add the following to delete.yaml file
    ```yaml
    delete:
      - "auto-tag/Owner"
    ```
3. Commit your changes

## Step Two - Verify the target object ("owner" tagging rule) exist
1. Open the Dynatrace UI and navigate to settings
2. Open tags -> automatically applied tags
3. verify tagging rule "owner" exist

    ![Owner Tag](Resources/Ownertagui.png)

## Step Three - Execute Monaco command
1. Go to the exercise one folder
    ```bash
    cd monaco-hot/exercises/1_exercise-one
    ```
2. Run Monaco 

    ```bash
    $ monaco -v -e=environments.yaml projects/
    ```
    Monaco should execute and you should not see any errors

    ![Owner git pull yaml](Resources/delete_console.png)

3. Check your Dynatrace environment make sure `Owner` tagging rule is deleted.



# ***Congratulations on completing Exercise-six!***




