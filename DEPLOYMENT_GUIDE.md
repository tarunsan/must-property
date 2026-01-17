# Deployment Guide

## Option 1: Deploy with Vercel (Recommended)
Vercel is the creators of Next.js and provides the easiest deployment experience.

1.  **Open your terminal** (Command Prompt or PowerShell).
2.  **Navigate to the project folder**:
    ```bash
    cd "d:/Property App/real-estate-app"
    ```
3.  **Run the deploy command**:
    ```bash
    npx vercel
    ```
4.  **Follow the prompts**:
    *   **Log in**: It will ask you to log in via your browser.
    *   **Set up and deploy**:
        *   "Set up and deploy “~\Property App\real-estate-app”?" -> **Y**
        *   "Which scope do you want to deploy to?" -> Select your account.
        *   "Link to existing project?" -> **N**
        *   "What’s your project’s name?" -> Press Enter (default is fine).
        *   "In which directory is your code located?" -> Press Enter (default `./` is correct).
        *   "Want to modify these settings?" -> **N**
5.  **Wait for deployment**: Vercel will build and deploy your site. It will provide a **Production** URL (e.g., `https://real-estate-app-xyz.vercel.app`) that you can share.

## Option 2: Deploy via GitHub (Best for long term)
If you have a GitHub account, this is the standard way.

1.  **Initialize Git** (if not done):
    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    ```
2.  **Create a Repository** on GitHub.
3.  **Push your code**:
    ```bash
    git remote add origin <your-repo-url>
    git branch -M main
    git push -u origin main
    ```
4.  **Go to Vercel.com**:
    *   Click "Add New..." -> "Project".
    *   Import your GitHub repository.
    *   Click "Deploy".

## Option 3: Static Export (Netlify Drop)
If you prefer to just drag and drop a folder.

1.  **Update `next.config.ts`**:
    Change the file to:
    ```typescript
    import type { NextConfig } from "next";

    const nextConfig: NextConfig = {
      output: 'export',
      images: {
        unoptimized: true,
      }
    };

    export default nextConfig;
    ```
2.  **Build the project**:
    ```bash
    npm run build
    ```
3.  **Deploy**:
    *   Locate the `out` folder in your project directory.
    *   Drag and drop this `out` folder to [Netlify Drop](https://app.netlify.com/drop).
