# Web Portal&#x20;

### Architecture Overview

Sunbird Portal is a modern, scalable monorepo consisting of a React-based frontend and an Express-based backend.

* **Frontend SPA**: React 19 + Vite + TanStack Query.
* **Backend API**: Node.js Express 5.
* **Upstream Core Services**: Kong API Gateway, Keycloak (Auth), Knowledge MW, Learn Service, Knowledge Service.

#### Production Flow

In production, the backend serves the compiled frontend assets from the `dist/public/` directory.

#### Development Flow

During development, the Vite dev server (port 5173) handles the UI and proxies API requests to the Backend (port 3000).

***

### Getting Started (Local Setup)

Follow these steps to set up the portal for local development.

#### Prerequisites

* **Node.js**: 24.12.0 (Recommended to use `nvm`)
* **npm**: Latest version
* **Git**: For version control

#### 1. Installation

Clone the repository and install dependencies for both services:

```bash
# Clone the repository
git clone https://github.com/Sunbird-Spark/sunbird-spark-portal
cd sunbird-spark-portal

# Install Frontend dependencies
cd frontend
npm install

# Install Backend dependencies
cd ../backend
npm install
```

#### 2. Configuration

Before running the services, ensure your environment is configured.

1. **Backend**: Create a `.env` file from [`backend/.envExample`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/.envExample).
2. **Local Dev**: Set `ENVIRONMENT=local` in your `.env` file and provide the required service URLs (Kong, Keycloak, etc.).

#### 3. Running the Portal

You need to run both the backend and the frontend simultaneously.

**Terminal 1 (Backend)**:

```bash
cd backend
npm run dev
```

**Terminal 2 (Frontend)**:

```bash
cd frontend
npm run dev
```

Visit `http://localhost:5173` to access the portal.

***

### Configuration Guide

Configuration is managed via environment variables and central configuration modules.

#### Key Configuration Modules

* **Backend Config**: [`backend/src/config/env.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/src/config/env.ts) - Provides typed access to all variables with defaults.
* **Frontend Config**: [`frontend/src/configs/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/frontend/src/configs) - Contains i18n, languages, and app-level constants.
* **Vite Proxy**: [`frontend/vite.config.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/vite.config.ts) - Defines API proxy routes for development.

***

### I18N (Resource Bundles)

Sunbird Portal supports multiple languages via resource bundles (JSON files).

#### Managing Languages

* **Configuration**: Supported languages and directions (LTR/RTL) are defined in [`frontend/src/configs/languages.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/src/configs/languages.ts).
* **Resource Bundles**: Translation keys are stored in JSON files under [`frontend/src/locales/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/frontend/src/locales).

#### To Add or Modify a Resource Bundle:

1. Go to the [`locales/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/frontend/src/locales) directory.
2. Edit an existing file (e.g., `en.json`) or create a new `{language_code}.json`.
3. Add or update keys following the existing JSON structure.

#### Internationalization (I18N) Sync with Keycloak theme

Keycloak login pages support 4 locales: English (en), French (fr), Arabic (ar), and Portuguese Brazil (pt\_BR). The locale is synced from the portal/mobile app so users see Keycloak pages in their selected language.

* **Mechanism**: The portal writes the selected language to `localStorage('app-language')`.
* **Keycloak Integration**: The `sunbird` Keycloak theme (specifically `template.ftl`) reads this value and sets the `KEYCLOAK_LOCALE` cookie, ensuring the login pages appear in the user's preferred language.
* **Configuration**: Refer [`keycloak theme`](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/develop/scripts/keycloak-21.1.2/README.md#internationalization-i18n) to configure new language in keycloak

***

### Branding (Name, Logo, and Theming)

Adopters can customize the portal's identity and visual appearance.

#### 1. Portal Name and Instance ID

* **Portal Instance**: Configure the `SUNBIRD_PORTAL_INSTANCE` variable in the backend `.env`.
* **App ID**: Generated dynamically in [`backend/src/config/env.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/src/config/env.ts) based on the instance name.

#### 2. Logos and Assets

* **Primary Logo**: Replace [`frontend/src/assets/sunbird-logo.svg`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/src/assets/sunbird-logo.svg) with your organization's logo.
* **Static Assets**: General images and icons are located in [`frontend/public/assets/images/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/frontend/public/assets/images).

#### 3. Theming implementation

The portal uses **Tailwind CSS** with a CSS-variable-based theming engine.

* **Global Styles**: Defined in [`frontend/src/index.css`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/src/index.css).
* **Customizing Colors**: Modify the `:root` variables in `index.css` (e.g., `--sunbird-primary`, `--sunbird-yellow`) to change the global color palette.
* **Design Tokens**: Sunbird-specific tokens are mapped in [`frontend/tailwind.config.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/tailwind.config.ts).

#### 4. Player Dependencies

Content players and editors loaded as framework-independent web components at runtimeare derived from **Sunbird Knowledge building block**.

* **Asset Consolidation**: The [`frontend/copy-assets.js`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/copy-assets.js) script automates the process of pulling player assets from `node_modules` into the `public/` directory during installation.

***

### External Service Dependencies

For a basic functional setup, the portal requires connectivity to these core services:

1. **Keycloak (OIDC)**: Essential for user authentication and session management.
2. **Kong Gateway**: The primary entry point for all proxied API calls to other Sunbird microservices.
3. **YugabyteDB**: Required if persistent session storage or form/review features are used.
4. **Knowledge Middleware**: Required for content exploration and management features.
5. **Lern Service**: Handles user, organization, and course batch management ([GitHub](https://github.com/Sunbird-Lern/lern-service)).
6. **Knowledge Platform**: The underlying platform for content management and orchestration ([GitHub](https://github.com/Sunbird-Knowlg/knowledge-platform/tree/develop)).

***

### Frontend Development Patterns

* **State Management**: Uses TanStack Query v5 for server state. Configuration: [`frontend/src/queryClient.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/src/queryClient.ts).
* **Routing**: React Router 7 (routes defined in `AppRoutes.tsx`).
* **Hooks**: Custom logic is extracted into hooks ([`frontend/src/hooks/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/frontend/src/hooks)).
* **RTL Support**: Arabic and other RTL languages are supported via [`I18nDirectionProvider`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/src/providers/I18nDirectionProvider.tsx) and [`rtl.css`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/frontend/src/styles/rtl.css).

***

### Backend Development Patterns

The backend serves as a secure proxy and session manager.

#### API Controllers

Core logic is organized into controllers in [`backend/src/controllers/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/backend/src/controllers):

* `appInfoController.ts`: Provides application metadata.
* `tenantController.ts`: Manages multi-tenancy redirection.
* `userAuthInfoController.ts`: Handles user authentication status.
* `formsController.ts`: Manages dynamic form configurations.
* `reviewCommentController.ts`: Handles content review feedback.
* `healthController.ts`: Service health checks.

#### Key Patterns

* **Authentication**: OIDC middleware in [`backend/src/auth/oidcMiddleware.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/src/auth/oidcMiddleware.ts).
* **Proxies**: Centralized proxying logic in [`backend/src/proxies/`](https://github.com/Sunbird-Spark/sunbird-spark-portal/tree/main/backend/src/proxies).
* **Multi-Tenancy**: Dynamic asset serving handled by [`backend/src/services/tenantService.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/src/services/tenantService.ts).
* **Main App**: Core routing logic is in [`backend/src/app.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/src/app.ts).

***

### Image Building & Deployment

#### CI/CD

Automated builds are triggered on tags via GitHub Actions ([`.github/workflows/image-push.yml`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/.github/workflows/image-push.yml)).

***

### Testing Strategy

* **Backend**: Vitest unit tests (e.g., [`backend/src/app.test.ts`](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/backend/src/app.test.ts)).
* **Frontend**: React Testing Library + Vitest.
* **Coverage**: Project enforces a 70% coverage threshold.
* **Commands**: `npm run test:run` in respective directories.

***

### Troubleshooting

1. **Node Version**: Mismatch can lead to build failures. Use 24.12.0.
2. **Missing Assets**: If players don't load, run `node frontend/copy-assets.js` manually.
3. **Authentication**: Ensure `OIDC_ISSUER_URL` is correct and Keycloak is reachable from the portal backend.
4. **CORS**: Local development relies on the Vite proxy; ensure it matches your backend port.

***

For more details, refer to the [README.md](https://github.com/Sunbird-Spark/sunbird-spark-portal/blob/main/README.md) file.
