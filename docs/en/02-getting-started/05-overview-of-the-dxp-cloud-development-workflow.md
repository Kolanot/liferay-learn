# Overview of the DXP Cloud Development Workflow

This article outlines the path developers will take to develop for and deploy to a DXP Cloud project. The development process with DXP Cloud follows three stages:

* [Develop and Configure](#develop-and-configure)
* [Build and Test](#build-and-test)
* [Deploy](#deploy)

## Develop and Configure

 Although there are multiple paths for deploying to an environment, all paths begin with adding changes to the provisioned Git repository. The Git repository is used as the basis for any custom additions to a DXP Cloud project, including the Liferay DXP service instance itself.

The repository provides the following:

* Workspace for building Liferay DXP modules, themes, and extensions
* Shared version control for configuration and customizations for DXP Cloud services
* Single source of truth for DXP Cloud project deployments

See [Configuring Your GitHub Repository](./03-configuring-your-github-repository.md) for more information on setting up the workspace on your local system.

With the exception of the `common/` directory, changes added to a given service's environment folder (`dev`, `uat`, `prod`) will _only_ be propagated when deploying to the corresponding environment. Changes added to the `common/` directory will _always_ be deployed, regardless of the target deployment environment.

### Code Additions

The source for new code additions must be added into folders at the root of the repository: the `modules` folder for new modules, the `themes` folder for custom themes, and the `wars` folder for exploded WARs. When the build is deployed, code changes added to any of these locations will be automatically compiled and then added to the Liferay service.

### Compiled Additions

Compiled additions, like pre-built JARs or LPKGs, may be added into a `deploy` directory for the relevant service. When the build is deployed to an environment, these files will be copied into the corresponding folder within `$LIFERAY_HOME` (depending on the type of file). For example, adding a JAR file to `lcp/liferay/deploy/common/` will result in the file being copied to `$LIFERAY_HOME/osgi/modules/` for any environment the build is deployed to.

### Configuration Files

Configuration files (portal properties or OSGi configuration files) may be added into a `config` directory for the relevant service. When the build is deployed to an environment, these files will be copied to the corresponding location within `$LIFERAY_HOME`. Portal properties files will be copied directly inside of `$LIFERAY_HOME`, whereas `.cfg` or `.config` files will be copied into `$LIFERAY_HOME/osgi/configs/`.

## Build and Test

The CI service will automatically execute builds for any of the following events: commits are merged into the DXP Cloud repository, pull requests with changes are sent to the repository, or `lcp deploy` is invoked using the Command Line Interface (CLI) to deploy to a DXP Cloud environment. The `CI` service in the `infra` environment can be modified to include additional pipeline steps, including testing. See the article on [Builds]() for more information.

Navigate to the `Builds` tab to see all builds that have been initiated. Pending, passed, or failed builds are all displayed. If the build passes CI, then the Cloud console will offer the option in the UI to deploy the passing build to any applicable environment.

![Reviewing Builds](./overview-of-the-dxp-cloud-development-workflow/02.png)

## Deploy

There are two main ways to deploy to services on DXP Cloud: deploying through the CLI, or deploying a successful build from the `Builds` tab in the DXP Cloud console.

### Option 1: Deploying Through the Command Line Interface

The quickest way to deploy services from a repository locally is by using the CLI. See [Using the Command Line Interface](../10-reference/03-command-line-tool.markdown) for more information on setting up the CLI.

After logging in through the CLI, use `lcp deploy` to deploy any additions present in the local repository. The CLI will prompt you to choose one of the environments to deploy to (`dev`, `uat`, or `prd`). You must have permissions to deploy to the chosen environment for the deployment to be successful.

### Option 2: Deploying From `Builds` in DXP Cloud

Another way to deploy changes is to use a completed build in CI from the DXP Cloud console.

Committed changes to the repository will automatically trigger a new build in CI any time a pull request is sent or merged. This allows changes to be deployed to a testing environment at any point of the review process. See [Deployments]() to learn more.

![Deploying to Prod](./overview-of-the-dxp-cloud-development-workflow/01.png)

## Additional Information

* [Configuring Your GitHub Repository]()
* [Environments](../05-build-and-deploy/02-environments.md)
* [Using the Command Line Interface]()
* [Walking Through the Development Life Cycle](./06-walking-through-the-development-life-cycle.md)