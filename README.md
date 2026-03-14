# MLOps Assignment 4: CI/CD Pipeline Implementation

## CI/CD Pipeline Logical Explanation
The workflow is configured with a `push` trigger set to `branches-ignore: - main`. 

**Logical Justification:** Pushing code directly to the `main` branch is bad practice in a production setting. By ignoring `main`, we force the pipeline to execute on developer feature branches (e.g., `feature/model-update`). This validates the code *before* it affects the production environment. The `pull_request` trigger acts as the final gatekeeper, running the pipeline one last time before a feature branch is merged.

### Pipeline Steps and Bug Fixes Reflection
The original provided YAML file contained several intentional bugs. The pipeline now executes the following corrected steps:

1. **Checkout Repository:** *Fix:* Added `actions/checkout@v4`. The original script failed to pull the repository into the runner environment, meaning `requirements.txt` was missing.
2. **Setup Python & Install Dependencies:** *Fix:* Corrected global indentation errors in the YAML block to properly pass the python version and install the pip requirements.
3. **Linter Check:** *Fix:* The original file had an empty step. Added the `flake8` command to actively scan the codebase for syntax errors to ensure dirty code fails the build immediately.
4. **Model Dry Test:** *Fix:* Fixed the YAML block scalar `|` indentation for the python print command.
5. **Artifact Generation:** *Addition:* Implemented `actions/upload-artifact@v4` to package this `README.md` file as an artifact named `project-doc`, making it available for download directly from the Actions run summary.