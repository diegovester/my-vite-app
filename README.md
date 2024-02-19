# Introduction
This is a step-by-step Mobi Byte guide on how to create your own Vite app and host it with GitHub Pages.  
This guide uses the [official documentation](https://vitejs.dev/guide/static-deploy) and builds off of it. 
This guide is intended for beginners so we will go into more detail and offer corrections that are needed at the time of this writing.

# Installation  
Make sure you have a GitHub account set up on https://github.com/
Make sure you have GitHub Desktop installed from https://desktop.github.com/
Make sure you have current Node.js installed from https://nodejs.org/en  
Make sure you have Visual Studio Code installed from https://code.visualstudio.com/download

# Setup  
## GitHub
Open [GitHub](https://github.com/)  
Create a new repository by clicking on the button that says "New".  
Name it "my-vite-app" or any name you prefer.  

Open GitHub Desktop.  
Clone your new repository. You can do this by going to the top-left and clicking File > Clone Repository. Open the GitHub.com tab and search for the name of your repository. Click on it and decide your local path. I recommend using a path such as: /Developer/githubusername/my-vite-app. But your local path can be anything you'd like.  
Now your GitHub workflow is connected! You should see a "Open in Visual Studio Code" button. Go ahead and click that.  

## Vite  
Now it's time to create your vite app.  
With your Visual Studio Code open, click on Terminal > New Terminal and run the command:  
> npm create vite@latest

type y to install the necessary packages  
It will ask you to name the project. I will name mine ["homer"](https://media1.tenor.com/m/tvjuVcJxIk0AAAAC/skinny-homer-homer-back-fat.gif).  
Project name: homer   
Framework: React  
Variant: TypeScript  

In your terminal run these commands:  
> cd homer  
> npm install  
> npm run dev  

Now you can see vite app running on your machine! Mine is running on [http://localhost:5173/](http://localhost:5173/)

## GitHub Actions  

Create a new directory named .github/workflows.  
Inside this directory, create a YAML file named deploy.yml.  
This is where you'll define your GitHub Actions workflow.  
Copy and paste the following code to your deploy.yml file. Find and replace homer with whatever you named your project here.
```
# Simple workflow for deploying static content to GitHub Pages
name: Deploy to GitHub Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ['main']

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets the GITHUB_TOKEN permissions to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
          cache-dependency-path: './homer/package-lock.json'
      - name: Install dependencies
        working-directory: ./homer
        run: npm install
      - name: Build
        working-directory: ./homer
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload dist repository
          path: './homer/dist/'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## Vite Config  
Inside the homer directory, you should see a file name vite.config.ts.  
Copy and paste this code into vite.config.ts:  

```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  /**
   * If you are deploying to https://<USERNAME>.github.io/<REPO>/
   * (eg. your repository is at https://github.com/<USERNAME>/<REPO>),
   * then set base to '/<REPO>/'.
   * https://vitejs.dev/guide/static-deploy#github-pages
   */
  base: '/my-vite-app/',
})
```  

# Deploying the App  

## Important! Settings setup  
On GitHub, in your new repository, go to Settings.  

Click on Actions > General in the sidebar. 
Scroll down to Workflow permissions.  
Click on "Read and write permissions".  
Click Save.  

Click on Pages in the left sidebar.  
Scroll down to Build and deployment.  
Under Source, select "GitHub Actions".  

## Make Changes and Push  
Go to GitHub Desktop.  
You'll see all the changes you've made on the left hand side.  
Below that, a Summary is requested. Type in something like:  
> feat: add vite app  

Press the blue "Commit to main" button.
Press the blue "Push origin" button.  

