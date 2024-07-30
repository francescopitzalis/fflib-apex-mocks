# Create and release a new version
Have you installed the Salesforce CLI?
- If you haven't installed the Salesforce CLI, you can do so by following the instructions [here](https://developer.salesforce.com/tools/salesforcecli).
## Authorize the Production org as a Dev Hub
- If your configured Dev Hub is already set to the production org, you can skip this step.
- If you already have a connection to the production org with alias YOUR_PROD_ORG_ALIAS, you can set it as default
  dev hub by running the following command:
```sh
sf config set target-dev-hub=YOUR_PROD_ORG_ALIAS
```
- If you don't have a connection to the production org (or you're not sure about the above), you can authorize it as a
  dev hub by running the following command:
```sh
sf org login web --set-default-dev-hub --alias YOUR_PROD_ORG_ALIAS
```
E.g. `sf org login web --set-default-dev-hub --alias MyDevHub

A browser window will open and you will be prompted to log in to the production org. After you log in, the production
org will be authorized as a dev hub and set as the default dev hub.

## Create the Unlocked Package (only the first time)
- Create the unlocked package by running the following command:
```sh
sf package create --name "Apex Mocks" --package-type Unlocked --path sfdx-source
```

## Create a new version of the package
- Open the sfdx-project.json file and update the `versionName` and optionally the `versionNumber` of the package.
  The `versionName` should be in the format: YYYY-MM-DD
- Notice the list of the `packageAliases`, as when you'll create a new version a new element will be added to this list.
- Create a new version of the package by running the following command:
```sh
sf package version create --package "Apex Mocks" --definition-file config/project-scratch-def.json --wait 10 --installation-key-bypass --code-coverage
```
- A new `packageAlias` is created. Use that in the next command.
- Promote the new package version to "released" by running the following command:
```sh
sf package version promote --package NEW_PACKAGE_ALIAS
```
E.g. `sf package version promote --package "Apex Mocks@1.0.0-1"`

## Install the new version of the package to the production org
- Install the new version of the package to the production org by running the following command:
```sh
sf package install --package NEW_PACKAGE_ALIAS --target-org YOUR_PROD_ORG_ALIAS --wait 10 --publish-wait 10
```
E.g. `sf package install --package "Apex Mocks@1.0.0-2" --target-org MyProdOrg --wait 10 --publish-wait 10`
