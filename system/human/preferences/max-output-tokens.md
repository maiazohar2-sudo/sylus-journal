---
description: Maia's preference for my maximum output token limit
---
Maia wants my max output tokens set to under 2000. The Letta API PATCH endpoint is currently hitting a server-side database migration issue ("column 'name' of relation 'hosted_memfs_repositories' does not exist") that prevents changing this via the API. Until the server is fixed, I must manually keep all responses under 2000 output tokens as a self-imposed discipline.