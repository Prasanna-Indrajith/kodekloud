# Jenkins Parameterized Builds

## Question

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

1. Create a parameterized job which should be named as `parameterized-job`

2. Add a **string parameter** named `Stage`; its default value should be `Build`.

3. Add a **choice parameter** named `env`; its choices should be `Development`, `Staging` and `Production`.

4. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).

5. Build the Jenkins job at least once with choice parameter value Production to make sure it passes.

-----

## Answer

## 🛠️ 2. Create and Configure `parameterized-job`

Follow these steps to create the new job and define its parameters and shell command.

### A. Create New Item

1.  Click **New Item** on the Jenkins dashboard.
2.  Enter the Item name: **`parameterized-job`**.
3.  Select **Freestyle project**.
4.  Click **OK**.

### B. Configure Job Parameters

1.  In the General tab of the job configuration, check the box for **This project is parameterized**.

2.  Click **Add Parameter** and add the following two parameters:

      * **String Parameter:**

          * **Name:** `Stage`
          * **Default Value:** `Build`
          * **Description:** `The phase of the pipeline (e.g., Build, Test, Deploy).`

      * **Choice Parameter:**

          * **Name:** `env`
          * **Choices:** (Enter each choice on a new line)
            ```
            Development
            Staging
            Production
            ```
          * **Description:** `The target environment for the job execution.`

### C. Configure Build Step

1.  Scroll down to the **Build** section.

2.  Click **Add build step** and select **Execute shell**.

3.  Enter the following shell command to echo both parameter values:

    ```bash
    echo "The execution stage is: $Stage"
    echo "The target environment is: $env"
    echo "--- Job Finished ---"
    ```

4.  Click **Save**.

-----

## ✅ 3. Build the Job with Specified Parameters

1.  On the `parameterized-job` page, click **Build with Parameters** in the left-hand menu.
2.  Set the parameter values:
      * **Stage:** (Leave as default: `Build`)
      * **env:** Select **`Production`** from the dropdown menu.
3.  Click the **Build** button.

### Verification

  * The job will start and should appear in the **Build History** as **\#1**.

  * Click on the build number ($\#1$) and select **Console Output**.

  * The output should confirm the successful execution and display the parameters:

    ```
    ...
    [parameterized-job] $ /bin/sh -xe /tmp/jenkins4567890.sh
    + echo '--- Starting Parameterized Job ---'
    --- Starting Parameterized Job ---
    + echo 'The execution stage is: Build'
    The execution stage is: Build
    + echo 'The target environment is: Production'
    The target environment is: Production
    + echo '--- Job Finished ---'
    --- Job Finished ---
    Finished: SUCCESS
    ```

The parameterized job is now created and successfully built once with the required `Production` environment setting.