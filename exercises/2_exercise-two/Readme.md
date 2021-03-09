# Monaco HOT - Exercise Two
During this exercise we will apply a large amount of configuration to our Dynatrace environment

Our configuration is divided into two monaco projects:
1. A **Global** project that contains cluster wide configurations:  
   - Auto tagging rules
   - Request attributes
   - Synthetic locations
2. A **Perform** project that contains the configuration specifically for our project:
    - Application definition
    - Application detection rules
    - Auto tagging rules
    - Calculated services metrics
    - Dashboards
    - Management zones
    - Synthetic monitors

## Step 1 - Explore configuration
### Folder structure
Explore the contents of the `exercises/2_exercise-two` folder. It looks like this:
```
|-- environments.yaml
`-- projects
    |-- global
    |   |-- auto-tag
    |   |   |-- auto-tag.json
    |   |   `-- auto-tag.yaml
    |   |-- request-attributes
    |   |   |-- request-attribute.json
    |   |   `-- request-attribute.yaml
    `-- perform
        |-- app-detection-rule
        |   |-- rule.json
        |   `-- rules.yaml
        |-- application
        |   |-- application.json
        |   `-- application.yaml
        |-- auto-tag
        |   |-- tagging.json
        |   `-- tagging.yaml
        |-- calculated-metrics-service
        |   |-- csm.json
        |   `-- csm.yaml
        |-- dashboard
        |   |-- dashboard.json
        |   `-- dashboard.yaml
        |-- management-zone
        |   |-- management-zone.json
        |   `-- zone.yaml
        `-- synthetic-monitor
            |-- health-check-monitor.json
            `-- synthetic-monitors.yaml
```
### Configurations
Navigate to the contents of `exercises/2_exercise-two`, and see which configurations will be applied automatically.



### The environments.yaml file
Open the environments file: `2_exercise-two/environments.yaml`
This file looks like this:
```yaml
perform:
  - name: "perform"
  - env-url: "{{ .Env.DT_TENANT_URL }}" 
  - env-token-name: "DT_API_TOKEN" 
```
We defined a Dynatrace environment for Monaco to handle called `perform`.
The attributes `env-url` and `env-token-name` contain the names of **environment variable** that contain the environment url and API token respectively. Note the difference in notation. Within Monaco it is possible to use environment variables anywhere, by using the format `{{ .Env.YOUR_ENV_VAR_NAME }}`. For the token, it is slightly different as this was historically always the name of an environment variable, so here you just put the format `YOUR_ENV_VAR_NAME` to load your token.

Monaco, when running, will load and replace all environment variables where it can. If an environment variable is not set, it will throw a validation error.

We have - in exercise-one - already created an environment variable for the Dynatrace API token. Execute the following command to see if it is still there:
```bash
$ echo $DT_API_TOKEN
```
> Note: In Windows 10, print an environment variable using `echo %DT_API_TOKEN%`

If the value is empty, you can re-set it as follows:
```bash
$ export DT_API_TOKEN=[your token here]
```
> Note: In Windows 10, create an environment variable using `setx <variable_name> "<variable_value>"`

We now also need to create an environment variable for `DT_TENANT_URL` using the commands above.
> Note: follow the following notation: `https://abc12345.live.dynatrace.com` or `https://my-managed-url.com/e/ENVIRONMENTID`. **Note that there is no trailing slash**



## Step 2 - Trigger Monaco
Once we are familiar with the project structure and the contents, it is time to trigger Monaco.

1. On the command line, navigate to the `monaco-hot/exercises/2_exercise_two` folder.
2. Within the folder, execute the following command to do a dry-run of only the **global** project:
    ```bash
    $ monaco -v -dry-run -e=environments.yaml -p=global projects/
    ```
3. Once validated, remove the dry-run flag and run again:
    ```bash
    $ monaco -v -e=environments.yaml -p=global projects/
    ```

4. Repeat steps 2 and 3 for the **perform** project. Alternatively, you can also remove the `-p` flag to apply all projects.

## Step 3 - View results in Dynatrace

As a last step, go to your Dynatrace environment and verify that Monaco created all the configurations as described in the two Monaco projects.

## Step 4 - Change configuration and re-apply

Take one of the configuration items that were created and make a change in the JSON file (**apart from the name!**). Re-run monaco and see the changes taking effect in Dynatrace.

That concludes this lab.