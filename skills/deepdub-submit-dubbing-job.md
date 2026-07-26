---
name: Submit and track a Deepdub dubbing job
description: Create a managed dubbing job from a source video, then poll status and manage the job lifecycle.
api: openapi/deepdub-managed-dub-openapi-original.json
operations: [create_dubbing_job_dubbing_job_post, get_dubbing_job_dubbing_job__request_id__get, list_dubbing_jobs_dubbing_jobs_get, create_redubbing_job_handler_dubbing_job__request_id__patch]
method: generated
generated: '2026-07-18'
---

# Submit and track a Deepdub dubbing job

Use the Managed Dubbing API (`https://dubbing.deepdub.app`) to dub a video end-to-end.

## Auth
Same `x-api-key` header as the REST API.

## Steps
1. **Submit.** `create_dubbing_job_dubbing_job_post` (`POST /dubbing/job`) with the source video URL and
   target locales. The source is validated, locales resolved, and a `requestId` is returned.
2. **Track.** Poll `get_dubbing_job_dubbing_job__request_id__get` (`GET /dubbing/job/{request_id}`) for
   status, progress trace, and export path. List all jobs with `list_dubbing_jobs_dubbing_jobs_get`
   (`GET /dubbing/jobs`, cancelled jobs excluded).
3. **Refine (optional).** Submit redub feedback with
   `create_redubbing_job_handler_dubbing_job__request_id__patch` (`PATCH /dubbing/job/{request_id}`)
   to trigger a resynthesis workflow.

## Conventions & errors
- Jobs are identified by the server-issued `requestId` (no client idempotency key).
- Validation failures return HTTP `422` (`HTTPValidationError`); see errors/deepdub-error-codes.yml.
- Cancellation is permanent — submit a new job rather than reviving a cancelled one.
