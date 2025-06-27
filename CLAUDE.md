# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Burp Suite DAST CI-driven scanning demonstration using OWASP Juice Shop as the target application. The repository demonstrates DevSecOps integration by embedding security scanning into CI/CD pipelines across GitHub Actions, GitLab CI, and Azure DevOps.

## Project Structure

- **`app/`** - Complete OWASP Juice Shop vulnerable web application
  - **Backend**: Node.js/Express with TypeScript, SQLite database, Sequelize ORM
  - **Frontend**: Angular application in `app/frontend/`
  - **Models**: 20+ Sequelize models for e-commerce functionality (`app/models/`)
  - **Routes**: 60+ API endpoints with intentional vulnerabilities (`app/routes/`)
  - **Security Challenges**: 125+ tracked security challenges system
- **`.github/`, `azure-pipelines.yml`, `.gitlab-ci.yml`** - CI/CD pipeline configurations
- **`scripts/`** - Pipeline utility scripts (e.g., `scan-cleanup.sh` for JUnit report processing)

## Common Development Commands

### Application Development
```bash
# Navigate to app directory first
cd app

# Install dependencies (builds frontend automatically)
npm install

# Development server with hot reload
npm run serve:dev

# Production build
npm run build:server
npm run build:frontend

# Start production server
npm start
```

### Testing Commands
```bash
# Run all tests
npm test

# Individual test suites
npm run test:unit
npm run test:integration  
npm run test:ui
npm run test:api
npm run test:server

# E2E testing with Cypress
npm run cypress:run
npm run cypress:open
```

### Code Quality
```bash
# Lint all code (TypeScript + Angular)
npm run lint

# Fix linting issues
npm run lint:fix

# Validate configuration schema
npm run lint:config
```

## Application Architecture

### Database Layer
- **ORM**: Sequelize with SQLite (`data/juiceshop.sqlite`)
- **Models**: Located in `app/models/` with relationships defined in `relations.ts`
- **Key Models**: User, Product, Basket, Challenge, Feedback, SecurityQuestion
- **Security**: Intentional vulnerabilities for educational purposes

### API Structure
- **RESTful API**: Auto-generated endpoints via finale-rest (`/api/*`)
- **Custom REST**: Manual endpoints (`/rest/*`)  
- **Authentication**: JWT-based with various bypass vulnerabilities
- **File Operations**: Multiple file upload/download endpoints with security flaws

### Frontend Architecture
- **Framework**: Angular (latest version in `app/frontend/`)
- **Build**: Angular CLI with custom webpack configuration
- **Components**: 40+ Angular components for e-commerce functionality
- **Services**: HTTP services for API communication
- **Routing**: Client-side routing with guards

### Security Challenge System
The application tracks 125+ security challenges across categories:
- **Injection**: SQL injection, XSS, XXE, Command injection
- **Authentication**: Broken authentication, session management
- **Authorization**: Insecure direct object references, privilege escalation  
- **File Operations**: Path traversal, malicious file upload
- **Business Logic**: Price manipulation, workflow bypasses

## CI/CD Pipeline Integration

### Pipeline Stages
1. **Build**: Containerize the application with Docker
2. **Test**: Run unit, integration, and UI tests (mocked for demo)
3. **Deploy**: Start application locally in container
4. **Scan**: Execute Burp Suite DAST scan against running application
5. **Publish**: Convert Burp results to JUnit format for pipeline reporting

### Pipeline Files
- **GitHub Actions**: `.github/workflows/` (check if directory exists)
- **GitLab CI**: `.gitlab-ci.yml`
- **Azure DevOps**: `azure-pipelines.yml`

### Burp Integration
- Configuration stored in `.burp/burp-config.yml`
- Scan results processed by `scripts/scan-cleanup.sh`
- Output converted to JUnit XML for CI/CD integration
- **DefectDojo Integration**: Burp JUnit reports automatically uploaded to DefectDojo

### DefectDojo Integration
The GitHub Actions workflow includes automatic upload of Burp scan results to DefectDojo:
- Uses JUnit XML format directly (no conversion needed)
- Creates new engagement for each scan run
- Requires repository secrets: `DEFECTDOJO_URL`, `DEFECTDOJO_API_KEY`, `DEFECTDOJO_PRODUCT_NAME`

## Key Configuration Files

- **`app/package.json`** - Node.js dependencies and scripts
- **`app/tsconfig.json`** - TypeScript compilation settings
- **`app/config/`** - Application configuration files (YAML)
- **`app/frontend/angular.json`** - Angular CLI configuration
- **`app/cypress.config.ts`** - E2E testing configuration

## Security Considerations

This is an intentionally vulnerable application designed for security testing. Key vulnerability categories include:
- SQL injection in search and authentication
- XSS in user inputs and product data
- File upload bypasses and path traversal
- Authentication and session management flaws
- Business logic vulnerabilities

When working with this codebase, be aware that security flaws are intentional and part of the educational design.