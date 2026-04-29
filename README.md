# University Digital Notice Board

The **University Digital Notice Board** is a responsive, production-ready static website designed to keep students updated on academic notices, exam schedules, and admissions. It features a complete DevOps CI/CD pipeline ensuring code quality, automated builds, and containerized deployment.

## 📁 Project Structure

```
uni-notice-board/
├── .github/workflows/ci.yml
├── .gitignore
├── .dockerignore
├── Dockerfile
├── package.json
├── README.md
├── src/
│   ├── index.html
│   ├── notices.html
│   ├── exams.html
│   ├── admissions.html
│   └── contact.html
└── styles/
    └── style.css
```

## 🛠️ Tech Stack & Tools

*   **Frontend**: HTML5, Vanilla CSS3 (Custom Design System, Responsive)
*   **Bundler**: Parcel
*   **Linters**: HTMLHint, Stylelint
*   **CI/CD**: GitHub Actions
*   **Containerization**: Docker, Docker Hub

---

## 🚀 Setup Instructions & Local Development

Follow these steps to run the project on your local machine:

### 1. Install Dependencies
Make sure you have Node.js installed, then run:
```bash
npm install
```

### 2. Run Linters
To ensure the code meets the quality standards:

**Run HTMLHint:**
```bash
npm run lint:html
```

**Run Stylelint:**
```bash
npm run lint:css
```

### 3. Local Development & Build
To run the local development server with Hot Module Replacement:
```bash
npm start
```

To build the static files for production (output goes to `dist/`):
```bash
npm run build
```

---

## 🐳 Docker Usage

To containerize the application locally:

### 1. Build the Docker Image
Make sure you have built the application first (`npm run build`). Then run:
```bash
docker build -t uni-notice-board:latest .
```

### 2. Run the Docker Container
Start the container on port 8080:
```bash
docker run -d -p 8080:80 --name uniboard-app uni-notice-board:latest
```
Visit `http://localhost:8080` in your browser.

---

## ⚙️ CI/CD Pipeline Explanation

The project utilizes **GitHub Actions** (`.github/workflows/ci.yml`) to enforce code quality and automate deployments.

The pipeline consists of exactly **3 Jobs**, running sequentially:

1.  **Job 1: Linting**: Checks out the code and runs `htmlhint` and `stylelint`. If any linting errors are found, the pipeline fails.
2.  **Job 2: Build**: Depends on `lint`. Installs dependencies and runs the Parcel bundler. The resulting `dist/` directory is saved as an artifact.
3.  **Job 3: Docker**: Depends on `build`. Downloads the compiled artifacts, builds a lightweight NGINX Docker image, logs into Docker Hub (using GitHub Secrets), and pushes the image.
    *   *Rule enforced*: Docker job **MUST** only run if both linting and build succeed.

---

## 🔐 GitHub Best Practices & Team Workflow

### 1. Feature Branch Workflow
Never commit directly to `develop` or `main`.
*   Create a new branch for your feature: `git checkout -b feature/your-feature-name`
*   Commit your changes: `git commit -m "Add new exam schedule UI"`
*   Push your branch: `git push origin feature/your-feature-name`

### 2. Pull Requests (PRs)
*   Open a Pull Request against the `develop` branch.
*   Ensure the CI pipeline passes (Linting and Build).
*   Add descriptions to your PR and request code reviews from your peers.

### 3. Branch Protection Rules
*   The `develop` and `main` branches are protected.
*   **Required Status Checks**: The CI pipeline (`lint`, `build`) must pass before merging.
*   **Review Requirements**: Only the **Team Lead** has the authority to merge PRs into `develop` or `main`. Code must be reviewed and approved by at least one other developer.
