# github-actions-poc

Hands-on proof of concept for the **GitHub Actions Certification Course** activity (video: *GitHub Actions Certification – Full Course to PASS the Exam*).

- **Student:** Abdul Rafay (`aabdulrafay`)
- **Purpose:** This repository is dedicated to the course activity. All workflow files are in [`.github/workflows/`](.github/workflows/).
- **Related repository:** [hello-custom-action](https://github.com/aabdulrafay/hello-custom-action) holds the custom JavaScript action used in Module 7.

---

## Workflows by module

| Module | File | Trigger | What it does |
|---|---|---|---|
| 1 | `hello.yml` | `push` | First test workflow, prints a hello message |
| 2A | `push-pr.yml` | `push`, `pull_request` on `main` and `dev` | Shows the event and branch that started the run |
| 2B | `2b-schedule.yml`, `schedule.yml` | `schedule` (cron) | Scheduled workflow, runs from the default branch |
| 2C | `2c-manual.yml` | `workflow_dispatch` | Manual run with three inputs: `name`, `greeting`, `data` |
| 2D | `2d-webhook.yml` | `repository_dispatch` (`customwebhook`) | Starts from an external `curl` call with a personal access token |
| 3 | `3-contexts.yml` | `push`, `workflow_dispatch` | Prints `github`, `runner`, `vars` and `env` contexts; uses `if:` with `contains()`, `success()`, `failure()`, `always()` |
| 4 | `4-secrets.yml` | `workflow_dispatch` | Uses a variable, a masked secret, and `GITHUB_TOKEN` for a REST API call and for creating an issue |
| 5A | `5a-multi-os.yml` | `workflow_dispatch` | Three jobs on `ubuntu-latest`, `windows-latest` (PowerShell + Chocolatey) and `macos-latest` (Swift) |
| 5B | `5b-self-hosted.yml` | `workflow_dispatch` | Runs on my own PC with `runs-on: [self-hosted, my-laptop]` |
| 6A | `6a-matrix.yml` | `workflow_dispatch` | Matrix of 2 operating systems x 2 Node.js versions (4 parallel jobs) |
| 6B | `6b-cache.yml` | `workflow_dispatch` | Dependency caching with `actions/cache` (cache miss, then cache hit) |
| 6C | `6c-service.yml` | `workflow_dispatch` | PostgreSQL 16 service container and test queries with `psql` |
| 7 | `7-custom-action.yml` | `workflow_dispatch` | Calls my custom action `aabdulrafay/hello-custom-action@v1.0.0` |

Other files: `package.json` (used by the cache demo) and `test.txt` (used to create the `dev` branch and pull request in Module 2A).

---

## What is needed to run the workflows

| Item | Where to set it | Used by |
|---|---|---|
| Repository variable `MY_VAR` (value `hello-from-vars`) | Settings → Secrets and variables → Actions → **Variables** | Modules 3 and 4 |
| Repository secret `MY_SECRET` | Settings → Secrets and variables → Actions → **Secrets** | Module 4 |
| Issues enabled | Settings → General → Features | Module 4 (issue created with `GITHUB_TOKEN`) |
| Self-hosted runner with label `my-laptop` | Settings → Actions → Runners | Module 5B |
| Personal access token with `repo` scope (not stored in the repo) | GitHub → Settings → Developer settings | Module 2D (`curl` call) |

> No token or secret value is stored in this repository.

---

## How to run

- **Manual workflows** (2C, 3, 4, 5A, 5B, 6A, 6B, 6C, 7): open the **Actions** tab, choose the workflow, click **Run workflow**.
- **2C from the terminal:**
  ```bash
  gh workflow run 2c-manual.yml -f name="Abdul Rafay" -f greeting="Hi" -f data="test123"
  ```
- **2D webhook:** send a `POST` request to the `dispatches` endpoint of this repository with the body `{"event_type":"customwebhook"}` and a token that has the `repo` scope. A response of `204 No Content` means success.
- **Scheduled workflow (2B):** runs by itself from the default branch. GitHub can delay scheduled runs when it is busy.

---

## Notes

- Scheduled workflows run only from the default branch, so the schedule files are on `main`.
- The self-hosted runner files are not stored in this repository.
- Screenshots and written answers are in the submitted PoC document.
