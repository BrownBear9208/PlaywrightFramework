# Playwright Framework — SauceDemo E2E Tests
===========================================

Enterprise-grade end-to-end test automation framework for SauceDemo (https://www.saucedemo.com) built with Playwright and TypeScript.

Repository: https://github.com/BrownBear9208/PlaywrightFramework


========================================
 ARCHITECTURE (Mermaid Diagram)
========================================

graph TD
    subgraph CI["CI/CD Pipeline - GitHub Actions"]
        VAL["Validate\n(typecheck + lint + format)"]
        SMK["Smoke Tests\n(Chromium)"]
        REG_C["Regression\n(Chromium)"]
        REG_F["Regression\n(Firefox)"]
        REG_W["Regression\n(WebKit)"]

        VAL --> SMK
        SMK --> REG_C
        SMK --> REG_F
        SMK --> REG_W
    end

    subgraph Specs ["Test Specs (82+ tests)"]
        LOGIN["login.spec.ts\nTC-001 – TC-005"]
        INV["inventory.spec.ts\nTC-100 – TC-133"]
        DET["item-detail.spec.ts\nTC-200 – TC-206"]
        CART["cart.spec.ts\nTC-300 – TC-306"]
        CHK["checkout.spec.ts\nTC-400 – TC-422"]
        E2E["e2e-journey.spec.ts\nTC-500 – TC-503"]
        ALL["all-products.spec.ts\nTC-600 – TC-611"]
        PROB["problem-user.spec.ts\nTC-700 – TC-704"]
        PERS["cart-persistence.spec.ts\nTC-800 – TC-804"]
    end

    subgraph Support ["Fixtures & Data"]
        FIX["test-fixtures.ts\n(auto-injected page objects)"]
        DATA["test-data.ts\n(users, checkout info)"]
        ENV[".env\n(environment config)"]
    end

    subgraph Pages ["Pages (Methods)"]
        LP["login.page.ts"]
        IP["inventory.page.ts"]
        CP["cart.page.ts"]
        CHP["checkout.page.ts"]
        IDP["item-detail.page.ts"]
    end

    subgraph Maps ["PageMaps (Locators)"]
        LM["login.map.ts"]
        IM["inventory.map.ts"]
        CM["cart.map.ts"]
        CHM["checkout.map.ts"]
        IDM["item-detail.map.ts"]
    end

    Specs --> Support
    Specs --> Pages
    Support --> Pages
    Support --> DATA
    Support --> ENV
    Pages --> Maps


========================================
 PROJECT STRUCTURE
========================================

PlaywrightFramework/
├── .github/workflows/
│   └── playwright.yml              CI/CD: validate → smoke → regression matrix
├── PlaywrightTests/
│   ├── PageMaps/                    Locator definitions (what to find)
│   │   ├── login.map.ts
│   │   ├── inventory.map.ts
│   │   ├── cart.map.ts
│   │   ├── checkout.map.ts
│   │   └── item-detail.map.ts
│   ├── pages/                       Page methods (what to do)
│   │   ├── login.page.ts
│   │   ├── inventory.page.ts
│   │   ├── cart.page.ts
│   │   ├── checkout.page.ts
│   │   └── item-detail.page.ts
│   ├── fixtures/
│   │   └── test-fixtures.ts         Custom Playwright fixtures with auto-login
│   ├── data/
│   │   └── test-data.ts             Centralized test data constants
│   ├── login.spec.ts                TC-001–005: Authentication
│   ├── inventory.spec.ts            TC-100–133: Inventory, sorting, navigation, footer
│   ├── item-detail.spec.ts          TC-200–206: Product detail page
│   ├── cart.spec.ts                 TC-300–306: Shopping cart
│   ├── checkout.spec.ts             TC-400–422: Checkout form, overview, confirmation
│   ├── e2e-journey.spec.ts          TC-500–503: Full E2E purchase workflows
│   ├── all-products.spec.ts         TC-600–611: Full product catalog coverage
│   ├── problem-user.spec.ts         TC-700–704: Known defect documentation
│   └── cart-persistence.spec.ts     TC-800–804: Cart state persistence
├── playwright.config.ts             Playwright configuration (browsers, timeouts, reporters)
├── tsconfig.json                    TypeScript strict mode + path aliases
├── eslint.config.js                 ESLint + TypeScript rules
├── .prettierrc                      Prettier formatting rules
├── .env.example                     Environment variable template
├── .gitignore                       Git ignore rules
└── package.json                     Dependencies, scripts, lint-staged config


========================================
 SETUP
========================================

1. Clone the repository
   git clone https://github.com/BrownBear9208/PlaywrightFramework.git
   cd PlaywrightFramework

2. Install dependencies
   npm install

3. Install Playwright browsers
   npx playwright install

4. Configure environment
   cp .env.example .env
   (edit .env with your values if needed)


========================================
 RUN TESTS
========================================

Command                         Description
------------------------------  ------------------------------------------
npm test                        Run all tests on all browsers
npm run test:smoke              Run smoke tests only (@smoke tag)
npm run test:regression         Run regression tests only (@regression tag)
npm run test:e2e                Run E2E journey tests
npm run test:chromium           Run on Chromium only
npm run test:firefox            Run on Firefox only
npm run test:webkit             Run on WebKit only
npm run test:headed             Run with visible browser
npm run test:debug              Run in Playwright debug mode
npm run report                  Open the HTML test report


========================================
 CODE QUALITY
========================================

Command                         Description
------------------------------  ------------------------------------------
npm run validate                Run all checks (typecheck + lint + format)
npm run typecheck               TypeScript strict compilation check
npm run lint                    ESLint static analysis
npm run lint:fix                ESLint auto-fix
npm run format                  Prettier auto-format all files
npm run format:check            Prettier check without modifying files

Pre-commit hooks (Husky + lint-staged) automatically run ESLint and Prettier
on every staged .ts file before committing.


========================================
 CI/CD PIPELINE
========================================

GitHub Actions pipeline triggers on push/PR to main branch.

Pipeline stages:

  1. VALIDATE        Type check + ESLint + Prettier format check
         |
         v
  2. SMOKE           Fast smoke suite on Chromium (~10 min timeout)
         |
         v
  3. REGRESSION      Full test suite in parallel matrix:
                     - Chromium
                     - Firefox
                     - WebKit
                     (~30 min timeout per browser)

Artifacts uploaded after each stage:
  - playwright-report/    HTML test report (retained 30 days)
  - test-results/         Traces, screenshots, videos (retained 30 days)


========================================
 TEST COVERAGE MAP
========================================

Test ID Range    Spec File                    Area                    Count
--------------   -------------------------    --------------------    -----
TC-001 – 005     login.spec.ts                Authentication            5
TC-100 – 133     inventory.spec.ts            Product listing          17
TC-200 – 206     item-detail.spec.ts          Product detail page       7
TC-300 – 306     cart.spec.ts                 Shopping cart             7
TC-400 – 422     checkout.spec.ts             Checkout (3 phases)      14
TC-500 – 503     e2e-journey.spec.ts          E2E purchase workflows    4
TC-600 – 611     all-products.spec.ts         Full product catalog     15
TC-700 – 704     problem-user.spec.ts         Known defect docs         5
TC-800 – 804     cart-persistence.spec.ts     Cart state persistence    5
                                                                     ----
                                              TOTAL                   79+


========================================
 TECH STACK
========================================

- Playwright 1.58+         Browser automation and test runner
- TypeScript 5.8+          Strict type safety
- Node.js 20+              Runtime
- ESLint 10+               Static analysis
- Prettier 3.5+            Code formatting
- Husky 9+                 Git hooks
- lint-staged 16+          Pre-commit quality gates
- dotenv 17+               Environment configuration
- GitHub Actions           CI/CD pipeline


========================================
 ENVIRONMENT VARIABLES
========================================

Variable              Default                          Description
-------------------   ------------------------------   ---------------------------
BASE_URL              https://www.saucedemo.com        Application under test URL
STANDARD_USER         standard_user                    Default login username
STANDARD_PASSWORD     secret_sauce                     Default login password
CI                    (not set)                        Set by GitHub Actions


========================================
 DESIGN PATTERNS
========================================

- Page Object Model (POM)    Split into PageMaps (locators) and Pages (methods)
- Custom Fixtures             Auto-injected page objects via test.extend
- Test Tagging                @smoke and @regression for selective execution
- Data-Driven Tests           Parameterized product catalog tests (all 6 items)
- Environment Abstraction     dotenv + process.env with fallback defaults
