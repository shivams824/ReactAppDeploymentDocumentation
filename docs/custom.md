# <b>React App Deployment On GitHub Pages Using GitHub Actions<b>

### Getting Started

To deploy a React application with routing on GitHub Pages using GitHub Actions, you'll need to create a workflow that builds your app and publishes it to a GitHub Pages branch. Here's a step-by-step guide:

## Step 1: Create a React Application with Routing

If you haven't already, create a React application with routing using a tool like Create React App or set up your own.

```bash
npx create-react-app my-app --template typescript
```

After this add routing to your project.

## Step 2: Set Up GitHub Repository

Ensure that your React app is in a GitHub repository. If it's not, create a new repository on GitHub.

##### Navigate to the directory containing your project.

Initialize a Git repository with the following command:

```
 $ git init
```

Add all the relevant files with command:

```
$ git add .
```

Create a .gitignore file right away to specify the files you don't want to track. Add it to Git with

```
$ git add .gitignore.
```

Commit your changes with a descriptive message:

```
$ git commit -m "Initial commit".
```

Link your local repository to the remote GitHub repository with:

```
$ git remote add origin git@github.com:username/new_repo.
```

Push your code to the master branch of the remote repository with:

```
$ git push -u origin master.
```

#

## Step 3: Add GitHub Actions Workflow

To create a GitHub Actions workflow file in your repository, you can use the GitHub UI or create it manually.
<br>For manually, Place workflow file in a folder named `.github/workflows` in your project's root directory.
<br>Name the file, for example, deploy.yml. The contents of this file should look like the following:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main # Change this to the branch you want to deploy from

jobs:
  build:
    runs-on: ubuntu-latest # Change this to configuration of the deployment enviornment. (If you are not sure use matrix for defining env)

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: 16 # Change this to node version of your project(If you are not sure use matrix for defining version)

    - name: Install dependencies
      run: npm install

    - name: Build
      run: npm run build # Change this to your build script

    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GH_TOKEN }}
          publish_dir: ./build

```

<br>
This workflow does the following:

- It runs when there's a push to the main branch. You can change the branch name if necessary.

- It checks out the repository's code.

- It sets up Node.js (adjust the version if needed).

- It installs your project's dependencies.

- It builds your project (modify the build command if necessary).

- It deploys the built files to the `gh-pages` branch, which is where GitHub Pages looks for content.

## Step 4: Add Changes in package.json file

Before your dependencies in package.json, add the following:

```
"homepage": "https://github_username.github.io/repo_name",
```

In the scripts section, add:

```
"predeploy": "npm run build",
"deploy": "gh-pages -d build",
```

Before the "start" script.

Now when you run your workflow you might get some errors, because Github Treats warnings as errors because `process.env.CI = true.`
<br>To avoid this happening:
<br>Add this in your script build:

```
"build": "CI=false && react-scripts build",
```

## Step 5: Create GitHub Secrets

You'll need to create a GitHub secret to store your GitHub token.
<br>Go to your repository on GitHub, navigate to "Settings" > "Secrets" and create a new secret named `GITHUB_TOKEN`.
<br><br>The workflow will use this token to authenticate and push changes to the `gh-pages` branch.
For `GITHUB_TOKEN` you will need to generate a Personal Access Token (PAT) by navigating to "Settings" > "Developer Setting" > "Personal access tokens" > "Generate new token".
<br><br>After authorization, GitHub will provide you with a token. Use this token for creating `GITHUB_TOKEN`. If GitHub doesn't allow using the name `GITHUB_TOKEN`, use `GH_TOKEN` or another suitable name.

## Step 6: Commit and Push Workflow File

Commit the `deploy.yml` file and push it to your repository on the `main` branch.

## Step 7: GitHub Actions Deployment

GitHub Actions will automatically run the workflow when you push to the `main` branch. You can monitor the workflow's progress on your repository's "Actions" tab.

Once the workflow is successful, your React application will be deployed to GitHub Pages.

## Step 8: Configure GitHub Pages

Go to your repository's settings on GitHub, scroll down to the GitHub Pages section, and set the branch to `gh-pages`. Your React app should now be accessible at the GitHub Pages URL provided.

That's it! Your React application with routing is now deployed on GitHub Pages using GitHub Actions. Make sure to adjust the workflow and settings according to your specific project structure and requirements.

## Step 9: After Deploying to GitHub pages

After deployment, you may encounter some issues, such as:

- When you reload the app, it displays a "404 Not Found" error. To handle this error, add a 404.html file with the following content:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your App Title</title>
    <script>
      location = '/'; // Redirect to the root URL
    </script>
  </head>
</html>

```

- You cannot route through direct change in URL; routing only works through relative paths.

## References:

<https://www.youtube.com/watch?v=5I37iVCDUTU>

##### GitHub Repo:

<https://docs.github.com/en/actions/using-workflows> <br>
<https://github.com/milosrancic/reactjs-website>

### References:

- [Video Tutorial](https://www.youtube.com/watch?v=5I37iVCDUTU)

##### GitHub Repo:

- [GitHub Actions Documentation](https://docs.github.com/en/actions/using-workflows)
- [GitHub Repository](https://github.com/milosrancic/reactjs-website)


You can replace `"https://github_username.github.io/repo_name"` with your actual GitHub Pages URL, and make any other necessary customizations.
