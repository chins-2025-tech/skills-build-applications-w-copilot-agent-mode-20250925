# GitHub Copilot Agent Instructions for Octofit Tracker App

## Architecture Overview
- The app consists of a Django backend (`octofit-tracker/backend/octofit_tracker`) and a React frontend (`octofit-tracker/frontend`).
- MongoDB is used as the database, accessed via Djongo from Django. The database name is `octofit_db`.
- Key features: user authentication, activity logging, team management, leaderboard, and workout suggestions.

## Developer Workflows
- **Do not change directories** when running commands; always use absolute paths.
- Backend Python virtual environment is at `octofit-tracker/backend/venv`.
- Use `source octofit-tracker/backend/venv/bin/activate` to activate the backend environment.
- Install backend requirements from `octofit-tracker/backend/requirements.txt`.
- Use `python octofit-tracker/backend/manage.py makemigrations` and `python octofit-tracker/backend/manage.py migrate` for DB setup.
- Test REST API endpoints using `curl` (see endpoint format below).
- For React, always point commands to `octofit-tracker/frontend`.

## Project-Specific Conventions
- **ALLOWED_HOSTS** in Django must include `localhost`, `127.0.0.1`, and the Codespace URL using the `CODESPACE_NAME` environment variable.
- **REST API endpoints** should be referenced as `https://$CODESPACE_NAME-8000.app.github.dev/api/[component]/` (do not hardcode `$CODESPACE_NAME`).
- **urls.py**: Use the environment variable to construct API URLs for responses.
- **serializers.py**: Convert MongoDB ObjectId fields to strings for API output.
- **Forwarded ports**: 8000 (backend, public), 3000 (frontend, public), 27017 (MongoDB, private).
- **Images**: Use `docs/octofitapp-small.png` for branding in the frontend.

## Integration Points
- Django backend communicates with MongoDB via Djongo; do not use direct MongoDB scripts for schema/data creation—use Django ORM.
- React frontend communicates with backend via REST API endpoints.

## Key Files & Examples
- Backend: `octofit-tracker/backend/octofit_tracker/settings.py`, `models.py`, `serializers.py`, `urls.py`, `views.py`, `admin.py`, `tests.py`
- Frontend: `octofit-tracker/frontend/src/index.js` (Bootstrap import), `App.js`
- Setup: `.github/instructions/octofit_tracker_setup_project.instructions.md`, `.github/instructions/octofit_tracker_django_backend.instructions.md`, `.github/instructions/octofit_tracker_react_frontend.instructions.md`

## Example: Dynamic API Root in Django
```python
import os
codespace_name = os.environ.get('CODESPACE_NAME')
if codespace_name:
    base_url = f"https://{codespace_name}-8000.app.github.dev"
else:
    base_url = "http://localhost:8000"
```

## Example: ALLOWED_HOSTS in Django
```python
import os
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
if os.environ.get('CODESPACE_NAME'):
    ALLOWED_HOSTS.append(f"{os.environ.get('CODESPACE_NAME')}-8000.app.github.dev")
```

---

**For questions or unclear conventions, review the setup instructions in `.github/instructions/` or ask for clarification.**
