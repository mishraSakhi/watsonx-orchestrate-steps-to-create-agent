```markdown
GitHub Documentation Reader API

Overview
The GitHub Documentation Reader API file defines 4 tools specifically(4 end points) for reading, analyzing, and searching documentation from public GitHub repositories. It's designed to help AI agents and developers quickly access project documentation without cloning repositories.

---

Features
- Read README Files — Fetch README files from any public repository
- Discover Documentation — Find all documentation-related files automatically
- Read Any File — Access content of any file in a repository
- Search Documentation — Search for specific terms across repository files
- Comprehensive Summaries — Get complete documentation overviews

---

Base URL: [https://api.github.com]

API Version
v3.0.0 (OpenAPI 3.0.0)

---

Endpoints

1. Get Repository README
Endpoint:
`GET /repos/{owner}/{repo}/readme`

Description:
Fetch the README file from a repository.

Parameters:

| Parameter | Type | Required | Example |
|----------|------|----------|--------- |
| owner    | path | Yes      | microsoft |
| repo     | path | Yes      | vscode   |

Response Example:
```json
{
  "name": "README.md",
  "content": "VGlzIGlzIGEgUkVBRE1F...",
  "encoding": "base64",
  "size": 5240
}
````

**Example Request:**

```sh
curl https://api.github.com/repos/microsoft/vscode/readme
```

---

### 2. Read Specific File

**Endpoint:**
`GET /repos/{owner}/{repo}/contents/{path}`

**Description:**
Read content of any file in the repository.

**Parameters:**

| Parameter | Type | Required | Example         |
| --------- | ---- | -------- | --------------- |
| owner     | path | Yes      | django          |
| repo      | path | Yes      | django          |
| path      | path | Yes      | CONTRIBUTING.md |

**Response Example:**

```json
{
  "name": "CONTRIBUTING.md",
  "path": "CONTRIBUTING.md",
  "content": "VGlzIGlzIGEgQ09OVFJJQlVUSU5H...",
  "encoding": "base64",
  "size": 3100
}
```

**Example Request:**

```sh
curl https://api.github.com/repos/django/django/contents/CONTRIBUTING.md
```

---

### 3. Find Documentation Files

**Endpoint:**
`GET /repos/{owner}/{repo}/contents`

**Description:**
List all documentation-related files in the repository.

**Parameters:**

| Parameter | Required | Example |
| --------- | -------- | ------- |
| owner     | Yes      | python  |
| repo      | Yes      | cpython |

**Response Example:**

```json
[
  {
    "name": "README.md",
    "path": "README.md",
    "type": "file",
    "size": 5240
  },
  {
    "name": "CONTRIBUTING.md",
    "path": "CONTRIBUTING.md",
    "type": "file",
    "size": 3100
  },
  {
    "name": "docs",
    "path": "docs",
    "type": "dir"
  }
]
```

**Example Request:**

```sh
curl https://api.github.com/repos/python/cpython/contents
```

---

## Use Cases

* AI Agents
  Provide AI assistants with up-to-date documentation for answering project-related queries.
* Documentation Search
  Quickly search across repository documentation without cloning.
* Documentation Analysis
  Extract and analyze documentation structure from multiple repositories.
* Developer Tools
  Build tools that require access to project README and contribution guidelines.

---

*Response Format*

All responses return data in **base64 encoding**.

***Python Example:***

```python
import base64
decoded = base64.b64decode(response['content']).decode('utf-8')
```

***JavaScript Example:***

```js
const decoded = Buffer.from(response.content, 'base64').toString('utf-8');
```

***cURL Example:***

```sh
curl https://api.github.com/repos/microsoft/vscode/readme | jq -r '.content' | base64 -d
```

---

## Authentication

For higher rate limits, include a GitHub token:

```sh
curl -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/microsoft/vscode/readme
```

### Rate Limits

| Without Token    | With Token          |
| ---------------- | ------------------- |
| 60 requests/hour | 5,000 requests/hour |

---

## Tags

* Documentation — README and documentation endpoints
* File Reading — File content retrieval
* Documentation Discovery — Finding documentation files
* Search — Search functionality

---

## HTTP Status Codes

| Code | Meaning                              |
| ---- | ------------------------------------ |
| 200  | Success                              |
| 404  | Repository or file not found         |
| 403  | Rate limit exceeded or access denied |
| 422  | Invalid parameters                   |

