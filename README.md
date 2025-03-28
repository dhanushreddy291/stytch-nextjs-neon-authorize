<img width="250px" src="https://neon.tech/brand/neon-logo-dark-color.svg" />

# Neon RLS + Stytch Example (SQL from the Backend)

A quick start Next.js template demonstrating secure user authentication and authorization using Neon RLS with Stytch integration. This guide primarily uses SQL from the backend to enforce row-level security policies.

## Features

- Next.js application with TypeScript
- Passwordless authentication powered by Stytch
- Row-level security using Neon RLS
- Database migrations with Drizzle ORM
- Ready-to-deploy configuration for Vercel, Netlify, and Render

## Prerequisites

- [Neon](https://neon.tech) account with a new project
- [Stytch](https://stytch.com) account with a new project (Consumer type)
- Node.js 18+ installed locally

## One-Click Deploy

Deploy directly to your preferred hosting platform:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/neondatabase-labs/stytch-nextjs-neon-rls&env=DATABASE_URL,DATABASE_AUTHENTICATED_URL,STYTCH_PROJECT_ENV,STYTCH_PROJECT_ID,NEXT_PUBLIC_STYTCH_PUBLIC_TOKEN,STYTCH_SECRET&project-name=stytch-nextjs-neon-rls&repository-name=stytch-nextjs-neon-rls)
[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/neondatabase-labs/stytch-nextjs-neon-rls)
[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/neondatabase-labs/stytch-nextjs-neon-rls)

> Ensure your domain is added to the Stytch project's authorized applications and allowed redirect URLs. You can add your domain in the "Frontend SDKs" tab of your Stytch project. You can add redirect URLs in the "Configuration" tab of your Stytch project.

   ![Add Domain in Stytch Authorized applications](/images/stytch-add-domain-authorized-applications.png)
   ![Add Redirect URL in Stytch Configuration](/images/stytch-add-redirect-url.png)

## Local Development Setup

### Configure Stytch

1. Go to your [Stytch Dashboard](https://stytch.com/dashboard/api-keys) and copy your **Project ID**, **Public token**, and **Secret**.
   ![Stytch Dashboard](/images/stytch-dashboard.png)
2. In the "Frontend SDKs" page of your Stytch project, ensure the SDK is enabled.

### Set Up Neon RLS

1. Open your Neon Console and navigate to "RLS" in your project's settings.
2. Click "Add Authentication Provider".
3. Set the JWKS URL to: `https://{{STYTCH_PROJECT_ENV}}.stytch.com/v1/sessions/jwks/{{STYTCH_PROJECT_ID}}`

   > Replace `{{STYTCH_PROJECT_ENV}}` with your Stytch project environment (`test` or `live`) and `{{STYTCH_PROJECT_ID}}` with your Stytch Project ID.

   ![Neon RLS](/images/neon-rls-jwks.png)

4. Follow the steps in the UI to setup the roles for Neon RLS. You should ignore the schema related steps if you're following this guide.
5. Note down the connection strings for both the **`neondb_owner` role** and the **`authenticated, passwordless` role**. You'll need both. The `neondb_owner` role has full privileges and is used for migrations, while the `authenticated` role will be used by the application and will have its access restricted by RLS.
   
   ![Neon RLS Connection Strings](/images/neon-rls-env-values.png)

### Local Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/neondatabase-labs/stytch-nextjs-neon-rls
   cd stytch-nextjs-neon-rls
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create `.env` file with the following variables:

   ```env
   # Database connections
   DATABASE_URL=              # neondb_owner role connection
   DATABASE_AUTHENTICATED_URL= # authenticated role connection

   # Stytch configuration
   STYTCH_PROJECT_ENV=test # or live
   STYTCH_PROJECT_ID="YOUR_STYTCH_PROJECT_ID"
   NEXT_PUBLIC_STYTCH_PUBLIC_TOKEN="YOUR_STYTCH_PUBLIC_TOKEN"
   STYTCH_SECRET="YOUR_STYTCH_SECRET"
   ```

   > **Note:** Get your Stytch Project ID, Public Token, and Secret from your Stytch Dashboard.

4. Set up the database:

   ```bash
   npm run drizzle:generate  # Generate migrations
   npm run drizzle:migrate  # Apply migrations
   ```

5. Start the development server:

   ```bash
   npm run dev
   ```

6. Visit `http://localhost:3000` to see the application running.

   ![Demo todo app](/images/todo-app.png)

## Important: Production Setup

   > **Note**: Before deploying to production, ensure your Stytch project is in the "live" environment and update your `.env` file and the JWKS URL in Neon RLS.

   ![Stytch Live Environment](/images/stytch-live-environment.png)

## Learn More

- [Neon RLS Tutorial](https://neon.tech/docs/guides/neon-rls-tutorial)
- [Simplify RLS with Drizzle](https://neon.tech/docs/guides/neon-rls-drizzle)
- [Stytch Documentation](https://stytch.com/docs)
- [Neon RLS + Stytch Integration](https://neon.tech/docs/guides/neon-rls-stytch)

## Authors

- [David Gomes](https://github.com/davidgomes)
- [Pedro Figueiredo](https://github.com/pffigueiredo)
- [Julianna Lamb](https://github.com/julianna-stytch)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
