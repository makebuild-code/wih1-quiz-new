# MakeBuild Project Starter

A template for MakeBuild client projects, built using the Finsweet Developer Starter template.

This template provides a modern development environment for building custom Webflow integrations with TypeScript, automatic building, and live reloading.

Before starting to work with this project, please take some time to read through the documentation.

---

## Reference

- [Included Tools](#included-tools)
- [Requirements](#requirements)
- [Getting Started](#getting-started)
  - [Creating a New Project](#creating-a-new-project)
  - [Cloning the Repository](#cloning-the-repository)
  - [Installing Dependencies](#installing-dependencies)
  - [Building](#building)
- [Adding New Code](#adding-new-code)
- [Git Workflow](#git-workflow)
  - [Understanding Git Concepts](#understanding-git-concepts)
  - [Branch Structure](#branch-structure)
  - [Development Workflow](#development-workflow)
  - [Pull Requests & Merging](#pull-requests--merging)
  - [Resolving Merge Conflicts](#resolving-merge-conflicts)
- [Deployment](#deployment)
  - [How It Works](#how-it-works)
  - [Setting Up Deployment](#setting-up-deployment)
- [Pre-defined Scripts](#pre-defined-scripts)

---

## Included Tools

This template contains some preconfigured development tools:

- [TypeScript](https://www.typescriptlang.org/): A superset of JavaScript that adds an additional layer of typings, bringing more security and efficiency to the written code.
- [Prettier](https://prettier.io/): Code formatting that assures consistency across projects.
- [ESLint](https://eslint.org/): Code linting that enforces industry best practices.
- [esbuild](https://esbuild.github.io/): JavaScript bundler that compiles, bundles and minifies the original TypeScript files.
- [Finsweet's TypeScript Utils](https://github.com/finsweet/ts-utils): Utilities to help with Webflow development.

---

## Requirements

This template requires the following to be installed on your machine:

- [Node.js](https://nodejs.org/) (npm comes bundled with Node.js)
- [GitHub Desktop](https://desktop.github.com/)
- [VS Code](https://code.visualstudio.com/) or [Cursor](https://cursor.sh/)

It is also recommended that you install the following extensions in your editor:

- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

---

## Getting Started

### Creating a New Project

1. Navigate to the **Project Starter** template repository on GitHub
2. Click the green **Code** button
3. Click **Use this template**
4. Follow the prompts to create a new repository
5. Give your repository a name and configure settings as needed

Once created, update the [package.json](package.json) file with the correct project name and description.

### Cloning the Repository

1. Open GitHub Desktop
2. Go to **File** → **Clone Repository**
3. Select your newly created repository from the list (or paste the URL)
4. Choose a local path for the project files
5. Click **Clone**

### Creating the Development Branch

After cloning, set up the branch structure:

1. In GitHub Desktop, ensure your repository is selected
2. Click **Current Branch** (shows "main")
3. Click **New Branch**
4. Name it `development`
5. Ensure it says "Create branch based on **main**"
6. Click **Create Branch**
7. Click **Publish Branch** to push it to GitHub

### Installing Dependencies

Open your terminal in the project directory and run:

```bash
npm install
```

### Building

To build the files, you have two options:

- `npm run dev`: Builds and creates a local server that serves all files (see [Serving Files on Development Mode](#serving-files-on-development-mode)).
- `npm run build`: Builds to the production directory (`dist`).

#### Serving Files on Development Mode

When you run `npm run dev`, two things happen:

1. esbuild is set to `watch` mode. Every time you save your files, the project will be rebuilt.
2. A local server is created at `http://localhost:3000` that serves all your project files.

You can import them in your Webflow projects like:

```html
<script defer src="http://localhost:3000/{FILE_PATH}.js"></script>
```

Live Reloading is enabled by default, meaning that every time you save a change, the website you're working on will reload automatically. You can disable it in [bin/build.js](bin/build.js).

#### Building Multiple Files

If you need to build multiple files into different outputs, update the build settings in [bin/build.js](bin/build.js):

```javascript
const ENTRY_POINTS = ['src/home/index.ts', 'src/contact/index.ts', 'src/about/index.ts'];
```

This tells `esbuild` to build all those files and output them in the `dist` folder for production and at `http://localhost:3000` for development.

#### Building CSS Files

CSS files are also supported by the bundler. You can include CSS by either:

- Manually defining it in the [bin/build.js](bin/build.js) config
- Or importing the file inside any of your TypeScript files:

```typescript
// src/index.ts
import './index.css';
```

#### Setting Up a Path Alias

Path aliases help avoid deeply nested imports like:

```typescript
import example from '../../../../utils/example';
```

Instead, use cleaner imports:

```typescript
import example from '$utils/example';
```

Set up path aliases using the `paths` setting in [tsconfig.json](tsconfig.json). This template includes a predefined example:

```json
{
  "paths": {
    "$utils/*": ["src/utils/*"]
  }
}
```

---

## Adding New Code

The project follows a modular structure to keep code organized and maintainable.

### Project Structure

```
src/
├── index.ts          # Main entry point - imports and runs all modules
├── modules/          # Feature modules go here
│   ├── example.ts    # Example module
│   └── ...
└── utils/            # Shared utility functions
    └── ...
```

### Creating a New Module

1. **Create a new file** in the [src/modules/](src/modules/) directory:

```typescript
// src/modules/myFeature.ts
export const myFeature = () => {
  console.log('My feature is running!');
  // Your code here
};
```

2. **Export your function** from the module file using named exports.

3. **Import and run** the function in [src/index.ts](src/index.ts):

```typescript
// src/index.ts
import { myFeature } from './modules/myFeature';

myFeature();
```

### Best Practices

- Keep each module focused on a single feature or functionality
- Use descriptive names for your modules and functions
- Export only what needs to be used outside the module
- Add utility functions used across multiple modules to [src/utils/](src/utils/)
- Document complex logic with comments

---

## Git Workflow

This project uses a structured branching workflow to manage development and production code.

### Understanding Git Concepts

#### Local vs Remote

| Term       | Meaning                                                 |
| ---------- | ------------------------------------------------------- |
| **Local**  | The version of the code on your computer                |
| **Remote** | The version of the code stored on GitHub (in the cloud) |
| **Origin** | The default name for your remote repository             |

#### Key Operations

| Operation  | What It Does                                                     |
| ---------- | ---------------------------------------------------------------- |
| **Clone**  | Download a repository from GitHub to your computer               |
| **Fetch**  | Check if there are any changes on GitHub (doesn't download them) |
| **Pull**   | Download changes from GitHub to your local machine               |
| **Commit** | Save a snapshot of your changes locally (with a description)     |
| **Push**   | Upload your committed changes to GitHub                          |

#### Commits vs Pushes

Think of it like writing a document:

- **Commit** = Saving your document locally (Ctrl+S)
- **Push** = Uploading your document to the cloud

You can make multiple commits before pushing. This lets you break your work into logical chunks, undo specific changes if needed, and keep a clear history of what changed and why.

**Best practice:** Make small, focused commits with clear descriptions rather than one large commit at the end of the day.

### Branch Structure

```
main (production)
  └── development (staging/testing)
        ├── feature/new-animation
        ├── feature/form-validation
        └── bugfix/scroll-issue
```

| Branch        | Purpose                  | Deploys To                  |
| ------------- | ------------------------ | --------------------------- |
| `main`        | Production-ready code    | Production environment      |
| `development` | Testing and integration  | Preview/staging environment |
| `feature/*`   | New features in progress | Not deployed until merged   |
| `bugfix/*`    | Bug fixes in progress    | Not deployed until merged   |

### Development Workflow

#### 1. Update Your Local Development Branch

Before creating a new branch, make sure you have the latest code:

1. Open GitHub Desktop
2. Select the `development` branch from the **Current Branch** dropdown
3. Click **Fetch origin** to check for updates
4. If updates are available, click **Pull origin** to download them

#### 2. Create a New Feature Branch

1. With `development` selected, click **Current Branch**
2. Click **New Branch**
3. Name your branch descriptively (e.g., `feature/hero-animation` or `bugfix/mobile-nav`)
4. Ensure it says "Create branch based on **development**"
5. Click **Create Branch**
6. Click **Publish Branch** to push it to GitHub

#### 3. Make Your Changes

1. Open the project in VS Code/Cursor
2. Check the bottom-left corner to confirm you're on the correct branch
3. Make your changes and save your files

#### 4. Commit Your Changes

1. Open GitHub Desktop — you'll see all changed files listed with a diff view
2. Review what you've changed
3. Write a commit message in the bottom-left (keep it short but descriptive)
4. Click **Commit to [branch-name]**

#### 5. Push to GitHub

1. Click **Push origin** to upload your commits
2. Your changes are now on your branch in the cloud

**Remember:** Pushing to your feature branch doesn't affect `development` or `main`. Your branch is your safe space to experiment.

#### 6. Keep Your Branch Up to Date (Rebasing)

If others have made changes to `development` while you've been working:

1. In GitHub Desktop, go to **Branch** → **Rebase current branch**
2. Select `development`
3. This applies your changes on top of the latest `development` code

**Why rebase?** It prevents merge conflicts by keeping your branch in sync with the latest changes.

### Pull Requests & Merging

#### Creating a Pull Request

When your feature is ready for testing:

1. Push all your changes to GitHub
2. In GitHub Desktop, click **Create Pull Request** (or go to GitHub web)
3. Set the **base** branch to `development`
4. Set the **compare** branch to your feature branch
5. Add a title and description explaining your changes
6. Click **Create Pull Request**

#### Merging to Development

1. Review the changed files in GitHub
2. Once reviewed, click **Merge Pull Request**
3. Click **Confirm Merge**
4. Optionally delete the feature branch

#### Promoting to Production

Once changes are tested in the development environment:

1. Create a new Pull Request
2. Set **base** to `main`
3. Set **compare** to `development`
4. Review the combined changes
5. Merge to deploy to production

### Resolving Merge Conflicts

If GitHub shows conflicts:

1. In VS Code, you'll see conflict markers:

```javascript
// Current code in development
const speed = 1.0;
// Your incoming changes
const speed = 1.2;
```

2. Choose which code to keep:
   - **Accept Current Change** — keep what's in development
   - **Accept Incoming Change** — keep your changes
   - **Accept Both Changes** — keep both (they'll be stacked)
3. Save the file and commit the merge resolution

---

## Deployment

This project uses GitHub Actions to automatically deploy to Cloudflare Pages whenever changes are pushed.

### How It Works

The deployment workflow:

1. Triggers on pushes to `main` or `development`
2. Installs dependencies with npm
3. Builds the project to the `dist/` folder
4. Deploys the built files to Cloudflare Pages

| Action                 | Result                                 |
| ---------------------- | -------------------------------------- |
| Merge to `development` | Deploys to preview/staging environment |
| Merge to `main`        | Deploys to production environment      |

### Setting Up Deployment

To enable automatic deployments, you need to configure GitHub with Cloudflare credentials.

#### 1. Get Cloudflare Credentials

Get the Cloudflare credentials from **Bitwarden**. If you don't have a Bitwarden account, ask Dan.

You'll need three pieces of information:

1. **Account ID**
2. **API Token**
3. **Pages Project Name** — choose something descriptive for your project

#### 2. Configure GitHub Repository Settings

Grant the necessary permissions for GitHub Actions:

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Actions** → **General**
3. Scroll down to **Workflow permissions**
4. Select **Read and write permissions**
5. Click **Save**

#### 3. Add Secrets to GitHub

Add the Cloudflare credentials as secrets:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret** for each of the following:

| Secret Name                     | Value                      |
| ------------------------------- | -------------------------- |
| `CLOUDFLARE_ACCOUNT_ID`         | Your Cloudflare Account ID |
| `CLOUDFLARE_API_TOKEN`          | Your Cloudflare API Token  |
| `CLOUDFLARE_PAGES_PROJECT_NAME` | Your chosen project name   |

#### 4. Test the Deployment

Once configured:

1. Make a commit and push to the `main` branch
2. Go to the **Actions** tab in your GitHub repository
3. Watch the deployment workflow run
4. Green checkmark = successful deployment
5. Red X = something went wrong (check the logs)

---
#### 3. Add Secrets and Variables to GitHub

Now add the Cloudflare credentials to your GitHub repository:

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Secrets and variables** → **Actions**

**Add the following secrets** by clicking **New repository secret**:

## Pre-defined Scripts

This template contains a set of predefined scripts in [package.json](package.json):

| Script             | Description                                             |
| ------------------ | ------------------------------------------------------- |
| `npm run dev`      | Builds and creates a local server that serves all files |
| `npm run build`    | Builds to the production directory (`dist`)             |
| `npm run lint`     | Scans the codebase with ESLint and Prettier for errors  |
| `npm run lint:fix` | Fixes all auto-fixable ESLint issues                    |
| `npm run check`    | Checks for TypeScript errors in the codebase            |
| `npm run format`   | Formats all files using Prettier                        |
**Add the following variable** by clicking the **Variables** tab, then **New repository variable**:

   - **Name:** `PROJECT_NAME`
     **Value:** Your Cloudflare Pages project name from step 2

---

## Troubleshooting

### "You don't have write access"

**Cause:** You haven't been added to the repository with write permissions.

**Solution:** Ask Dan to add you as a collaborator with write access.

### Deployment Failed — Missing Secrets

**Cause:** The Cloudflare credentials aren't set up correctly.

**Solution:**

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Verify all three secrets are present
3. Re-run the failed workflow from the Actions tab

### Changes Not Appearing After Merge

**Possible causes:**

1. Browser caching — try a hard refresh (Cmd+Shift+R / Ctrl+Shift+R)
2. Deployment still in progress — check the Actions tab
3. Viewing the wrong environment URL

### Merge Conflicts

**Prevention:**

- Regularly rebase your branch against `development`
- Keep feature branches short-lived
- Communicate with team members about who's working on what
