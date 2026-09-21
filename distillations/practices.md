# Initial practices

These practices are an initial synthesis of public playlist metadata and conventions visible in the linked FreelanceDesk repositories. They are **not transcript-derived claims**; no transcript was retrieved.

| Practice | Source links | Evidence |
| --- | --- | --- |
| Keep the Laravel API and Vue frontend as separate application surfaces. | [FreelanceDesk API](https://github.com/micio86dev/freelancedesk-api), [FreelanceDesk frontend](https://github.com/micio86dev/freelancedesk-web) | Code-derived repository convention; playlist metadata supports the Laravel + Vue scope; transcript not retrieved. |
| Use Laravel FormRequest classes for request validation and API Resource classes for response shaping. | [FreelanceDesk API](https://github.com/micio86dev/freelancedesk-api), [CRUD/FormRequest/API Resource video](https://www.youtube.com/watch?v=QYGMMaBhA3M) | Code-derived convention plus playlist title/topic metadata; transcript not retrieved. |
| Enforce authorization with Laravel Policies instead of treating authentication as authorization. | [FreelanceDesk API](https://github.com/micio86dev/freelancedesk-api), [Policies video](https://www.youtube.com/watch?v=ciMmwi4gnnc) | Code-derived convention plus playlist title/topic metadata; transcript not retrieved. |
| Use Sanctum cookie and CSRF protection for the SPA authentication flow. | [FreelanceDesk API](https://github.com/micio86dev/freelancedesk-api), [Sanctum video](https://www.youtube.com/watch?v=-JDAc6m4IQI) | Code-derived convention plus playlist title/topic metadata; transcript not retrieved. |
| Isolate owned records so one authenticated user cannot read another user's customers or resources. | [FreelanceDesk API](https://github.com/micio86dev/freelancedesk-api), [Ownership video](https://www.youtube.com/watch?v=3Dpim5Vk9sY) | Code-derived convention plus playlist title/topic metadata; transcript not retrieved. |
| Keep frontend API state, HTTP calls, and navigation responsibilities explicit with Vue Axios, Pinia, and Router. | [FreelanceDesk frontend](https://github.com/micio86dev/freelancedesk-web), [Vue integration video](https://www.youtube.com/watch?v=sARWP3eIlgA), [Vue auth/route guard video](https://www.youtube.com/watch?v=StJRKILhPIE) | Code-derived convention plus playlist title/topic metadata; transcript not retrieved. |

## Verification rule

Before promoting any practice to a transcript-backed lesson, add a per-video note with the exact timestamp, source URL, confidence, and human verification status.
