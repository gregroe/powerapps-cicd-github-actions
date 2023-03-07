# Power Apps CICD with Github Actions

This resuable solution uses GitHub Actions to create a CICD Process for your  Power Platform Solutions. The workflows automate exporting your solution from dev, checking souce into repo,importing to test. When you've tested, you create a pull request to push code changes from a development branch to the Main branch (golden version). Then when you want to push to production you creaate a "Release" and the latest souce in Main is pushed to Production


 ## Dev-Build-Test Process
- runs export-and-branch-solution-import-to-test Action
- calls import-to-test Action
- Automates deployment from Dev-Build-Test-Prod using a pull requests and releases
- Exports your Power Apps Solution  from your Development dataverse Environment, creats a dev branch based off of Main. Checks unpacked solution in to dev branch
- Imports a managed copy of your solution to Test
- uses a build server to build managed solutions
- uses the pull request process to check the tested dev branch solution back into the Main Source Branch.

## Release to Prod Process
- Create a release
- Calles release-solution-to-prod Action
- builds a managed solution based on latest source in MAIN
- Deploys to Production


## Instructions:   
    
### Create Required Dev,Test,Build,Prod environments    
- see https://learn.microsoft.com/en-us/power-platform/alm/tutorials/github-actions-start#create-required-environments
### Create Service Principal/AAD App Registraion.
-  see https://learn.microsoft.com/en-us/power-platform/alm/tutorials/github-actions-start#create-the-service-principal-account-and-give-it-rights-to-the-environments-created 
- copy the TenantID, AppID,Client-Secret
### Create an Application User in each of the Dev,Test,Build,Prod environments with the Service Principal
 - see https://learn.microsoft.com/en-us/power-platform/alm/tutorials/github-actions-start#application-user-creation



### Create the following Github Environment Variables and Secrets and copy their respective values from your dataverse environment:
- Github Secret for the client-secret  "PowerPlatformSPN"
   - copy client-secret
- see https://docs.github.com/en/codespaces/managing-codespaces-for-your-organization/managing-encrypted-secrets-for-your-repository-and-organization-for-github-codespaces
 
- Github variables
    - BUILD_ENVIRONMENT_URL
    - CLIENT_ID
    - DEFAULTSOLUTION
    - DEVENVURL
    - PRODUCTION_ENVIRONMENT__URL
    - TENANT_ID
    - TEST_ENVIRONMENT_URL
- see https://docs.github.com/en/actions/learn-github-actions/variables
 
### Run Workflow  export-and-branch-solution-import-to-test
- Select Actions  at top of menu
- Select export-and-branch-solution-import-to-test
- Run Workflow
- see https://learn.microsoft.com/en-us/power-platform/alm/tutorials/github-actions-deploy#test-the-export-and-unpack-workflow

### Test the app in the Test Environment
- test the app
- when satisfied, Create a pull request in the Dev Branch to check into MAIN

### To Release to Production:
- Create a new Release. 
- release-solution-to-prod action will run. which will export power apps solution source, send it to the Build environment, builds a managed solution
- then inports to your production environemt.
- see https://learn.microsoft.com/en-us/power-platform/alm/tutorials/github-actions-deploy#test-the-release-to-production-workflow
