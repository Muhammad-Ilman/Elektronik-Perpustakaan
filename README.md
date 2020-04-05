GitHub Help

Explore by product
GitHub Packages
English

Search topics, products...
GitHub Packages Using GitHub Packages with your project's ecosystem Configuring npm for use with GitHub Packages
Configuring npm for use with GitHub Packages
You can configure npm to publish packages to GitHub Packages and to use packages stored on GitHub Packages as dependencies in an npm project.

GitHub Packages is available with GitHub Free, GitHub Pro, GitHub Team, GitHub Enterprise Cloud, and GitHub One. GitHub Packages is not available for private repositories owned by accounts using legacy per-repository plans. For more information, see "GitHub's products."

In this article
Authenticating to GitHub Packages
Publishing a package
Publishing multiple packages to the same repository
Installing a package
Further reading
Authenticating to GitHub Packages
You need an access token to publish, install, and delete packages in GitHub Packages. You can use a personal access token to authenticate with your username directly to GitHub Packages or the GitHub API. You can use a GITHUB_TOKEN to authenticate using a GitHub Actions workflow.

Authenticating with a personal access token
You must use a personal access token with the appropriate scopes to publish and install packages in GitHub Packages. For more information, see "About GitHub Packages."

You can authenticate to GitHub Packages with npm by either editing your per-user ~/.npmrc file to include your personal access token or by logging in to npm on the command line using your username and personal access token.

To authenticate by adding your personal access token to your ~/.npmrc file, edit the ~/.npmrc file for your project to include the following line, replacing TOKEN with your personal access token. Create a new ~/.npmrc file if one doesn't exist.

//npm.pkg.github.com/:_authToken=TOKEN
To authenticate by logging in to npm, use the npm login command, replacing USERNAME with your GitHub username, TOKEN with your personal access token, and PUBLIC-EMAIL-ADDRESS with your email address.

$ npm login --registry=https://npm.pkg.github.com
> Username: USERNAME
> Password: TOKEN
> Email: PUBLIC-EMAIL-ADDRESS
Authenticating with the GITHUB_TOKEN
If you are using a GitHub Actions workflow, you can use a GITHUB_TOKEN to publish and consume packages in GitHub Packages without needing to store and manage a personal access token. For more information, see "Authenticating with the GITHUB_TOKEN."

Publishing a package
By default, GitHub Packages publishes a package in the GitHub repository you specify in the name field of the package.json file. For example, you would publish a package named @my-org/test to the my-org/test GitHub repository. You can add a summary for the package listing page by including a README.md file in your package directory. For more information, see "Working with package.json" and "How to create Node.js Modules" in the npm documentation.

You can publish multiple packages to the same GitHub repository by including a URL field in the package.json file. For more information, see "Publishing multiple packages to the same repository."

You can set up the scope mapping for your project using either a local .npmrc file in the project or using the publishConfig option in the package.json. GitHub Packages only supports scoped npm packages. Scoped packages have names with the format of @owner/name. Scoped packages always begin with an @ symbol. You may need to update the name in your package.json to use the scoped name. For example, "name": "@codertocat/hello-world-npm".

After you publish a package, you can view the package on GitHub. For more information, see "Viewing packages."

Publishing a package using a local .npmrc file
You can use an .npmrc file to configure the scope mapping for your project. In the .npmrc file, use the GitHub Packages URL and account owner so GitHub Packages knows where to route package requests. Using an .npmrc file prevents other developers from accidentally publishing the package to npmjs.org instead of GitHub Packages. Because upper case letters aren't supported, you must use lowercase letters for the repository owner even if the GitHub user or organization name contains uppercase letters.

Authenticate to GitHub Packages. For more information, see "Authenticating to GitHub Packages."

In the same directory as your package.json file, create or edit an .npmrc file to include a line specifying GitHub Packages URL and the account owner. Replace OWNER with the name of the user or organization account that owns the repository containing your project.

registry=https://npm.pkg.github.com/OWNER
Add the .npmrc file to the repository where GitHub Packages can find your project. For more information, see "Adding a file to a repository using the command line."

Verify the name of your package in your project's package.json. The name field must contain the scope and the name of the package. For example, if your package is called "test", and you are publishing to the "My-org" GitHub organization, the name field in your package.json should be @my-org/test.

Verify the repository field in your project's package.json. The repository field must match the URL for your GitHub repository. For example, if your repository URL is github.com/my-org/test then the repository field should be git://github.com/my-org/test.git.

Publish the package:

$ npm publish
Publishing a package using publishConfig in the package.json file
You can use publishConfig element in the package.json file to specify the registry where you want the package published. For more information, see "publishConfig" in the npm documentation.

Edit the package.json file for your package and include a publishConfig entry.

  "publishConfig": {
    "registry":"https://npm.pkg.github.com/"
  },
Verify the repository field in your project's package.json. The repository field must match the URL for your GitHub repository. For example, if your repository URL is github.com/my-org/test then the repository field should be git://github.com/my-org/test.git.

Publish the package:

$ npm publish
Publishing multiple packages to the same repository
To publish multiple packages to the same repository, you can include the URL of the GitHub repository in the repository field of the package.json file for each package.

To ensure the repository's URL is correct, replace REPOSITORY with the name of the repository containing the package you want to publish, and OWNER with the name of the user or organization account on GitHub that owns the repository.

GitHub Packages will match the repository based on the URL, instead of based on the package name. If you store the package.json file outside the root directory of your repository, you can use the directory field to specify the location where GitHub Packages can find the package.json files.

"repository" : {
    "type" : "git",
    "url": "ssh://git@github.com/OWNER/REPOSITORY.git",
    "directory": "packages/name"
  },
Installing a package
You can install packages from GitHub Packages by adding the packages as dependencies in the package.json file for your project. For more information on using a package.json in your project, see "Working with package.json" in the npm documentation.

By default, you can add packages from one organization. For more information, see Installing packages from other organizations

You also need to add the .npmrc file to your project so all requests to install packages will go through GitHub Packages. When you route all package requests through GitHub Packages, you can use both scoped and unscoped packages from npmjs.com. For more information, see "npm-scope" in the npm documentation.

Authenticate to GitHub Packages. For more information, see "Authenticating to GitHub Packages."

In the same directory as your package.json file, create or edit an .npmrc file to include a line specifying GitHub Packages URL and the account owner. Replace OWNER with the name of the user or organization account that owns the repository containing your project.

registry=https://npm.pkg.github.com/OWNER
Add the .npmrc file to the repository where GitHub Packages can find your project. For more information, see "Adding a file to a repository using the command line."

Configure package.json in your project to use the package you are installing. To add your package dependencies to the package.json file for GitHub Packages, specify the full-scoped package name, such as @my-org/server. For packages from npmjs.com, specify the full name, such as @babel/core or @lodash. For example, this following package.json uses the @octo-org/octo-app package as a dependency.

{
  "name": "@my-org/server",
  "version": "1.0.0",
  "description": "Server app that uses the @octo-org/octo-app package",
  "main": "index.js",
  "author": "",
  "license": "MIT",
  "dependencies": {
    "@octo-org/octo-app": "1.0.0"
  }
}
Install the package.

$ npm install
Installing packages from other organizations
By default, you can only use GitHub Packages packages from one organization. If you'd like to route package requests to multiple organizations and users, you can add additional lines to your .npmrc file, replacing OWNER with the name of the user or organization account that owns the repository containing your project. Because upper case letters aren't supported, you must use lowercase letters for the repository owner even if the GitHub user or organization name contains uppercase letters.

registry=https://npm.pkg.github.com/OWNER
@OWNER:registry=https://npm.pkg.github.com
@OWNER:registry=https://npm.pkg.github.com
Further reading
"Deleting a package"
Ask a human
Can't find what you're looking for?
Product
Features
Security
Enterprise
Case Studies
Pricing
Resources
Platform
Developer API
Partners
Atom
Electron
GitHub Desktop
Support
Help
Community Forum
Training
Status
Contact GitHub
Company
About
Blog
Careers
Press
Shop
© 2020 GitHub, Inc.
Terms
Privacy
# Pratama-ilman-
