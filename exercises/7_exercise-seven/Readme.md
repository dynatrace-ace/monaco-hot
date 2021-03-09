# Monaco HOT - Exercise Seven - Linking configs

In this exercise we will show how we can link multiple configurations together without having to manually figure out the IDs of the configurations.
We will reference them using Monaco config instances and let Monaco figure out the IDs, dependencies and priorities!

## Step 1 - Take a look at the project
Navigate to `monaco-hot/exercises/7_exercise-seven`.
You will find a standard monaco setup:
```bash
├── Readme.md
├── environments.yaml
└── projects
    └── perform
        ├── alerting-profile
        │   ├── profile.json
        │   └── profile.yaml
        ├── management-zone
        │   ├── management-zone.json
        │   └── zone.yaml
        └── notification
            ├── notification.json
            └── notification.yaml
```

We have an `alerting-profile` that references a `management-zone` and a `notification` that references an `alerting-profile`.
It is up to us to complete the configurations so that Monaco can create all configurations in one go.

## Step 2 - Link the management-zone to the alerting-profile
Open up `exercises/7_exercise-seven/projects/management-zone/zone.yaml`: 
```yaml
config:
    - management-zone-app-one: "management-zone.json"
    - management-zone-app-two: "management-zone.json"

management-zone-app-one:
    - environment: "app-one"
    - name: "app-one"

management-zone-app-two:
    - environment: "app-two"
    - name: "app-two"
```
We can see that there are two named configurations, called `management-zone-app-one` and `management-zone-app-two`. We will use those references in our `alerting-profile`.

Open up `exercises/7_exercise-seven/projects/alerting-profile/profile.json`. Slightly dedacted, it contains at the bottom two variables that point to the management-zone ids:
```json
"managementZoneId": "{{.mzId}}",
"mzId": "{{.mzId}}",
"eventTypeFilters": []
```

No changes are needed in this file.

In the `exercises/7_exercise-seven/projects/alerting-profile/profile.yaml`, we have already the definition of these variables created:
```yaml
config:
    - alerting-profile-app-one: "profile.json"
    - alerting-profile-app-two: "profile.json"

alerting-profile-app-one:
    - name: "profile-app-one"
    - mzId: ""

alerting-profile-app-two:
    - name: "profile-app-one"
    - mzId: ""
```

For the two `mzId` fields, we can now reference the management zones by monaco ref. The structure of the contents is as follows:
```
/[PROJECT]/[CONFIG_TYPE]/[CONFIG_INSTANCE_NAME].[id|name]
```
Where the fields mean the following:
- `PROJECT`: the name of the Monaco project, for us that would be `perform`
- `CONFIG_TYPE`: the type of configuration you want to reference, for us that would be `management-zone`
- `CONFIG_INSTANCE_NAME`: the name of the configuration instance (**NOT the name of the configuration as it is known in Dynatrace**), for us that would be `management-zone-app-one`
- `id|name`: which field of that configuration - as it is known in Dynatrace - do you want to use, for us that would be `id`.

So to summarize, for us `exercises/7_exercise-seven/projects/alerting-profile/profile.yaml` should look like this:

```yaml
config:
    - alerting-profile-app-one: "profile.json"
    - alerting-profile-app-two: "profile.json"

alerting-profile-app-one:
    - name: "profile-app-one"
    - mzId: "/perform/management-zone/management-zone-app-one.id"

alerting-profile-app-two:
    - name: "profile-app-two"
    - mzId: "/perform/management-zone/management-zone-app-two.id"
```

Commit these changes in gitea

## Step 3 - Modify the notification

Similarly as in step 2, modify the `notification` to link to the `alerting-profile` 

## Step 4 - Run Monaco

1. Ensure your `DT_API_TOKEN` and `DT_TENANT_URL` environment variables are set
2. On the command line, navigate to the `monaco-hot/exercises/7_exercise-seven` folder.
3. Within the folder, execute the following command to do a dry-run of all projects
    ```bash
    $ monaco -v -dry-run -e=environments.yaml projects/
    ```
4. Once validated, remove the dry-run flag and run again:
    ```bash
    $ monaco -v -e=environments.yaml projects/
    ```

## Step 5 - View results in Dynatrace

In Dynatrace, you should now have all configurations created and interlinked

## This concludes the lab!
