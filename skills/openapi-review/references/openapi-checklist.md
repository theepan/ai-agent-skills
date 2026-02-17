# OpenAPI 3.1 Review Checklist

Quick reference for reviewing OpenAPI specifications. Organized by object type
with good and bad examples.

## Table of Contents
1. [Info Object](#info-object)
2. [Server Object](#server-object)
3. [Paths and Operations](#paths-and-operations)
4. [Parameters](#parameters)
5. [Schema Object](#schema-object)
6. [Security Schemes](#security-schemes)
7. [Component Reuse](#component-reuse)
8. [Common Anti-Patterns](#common-anti-patterns)

## Info Object

**Required**: `title`, `version`

```yaml
# BAD - Missing contact, license, description
info:
  title: API
  version: 1

# GOOD - Complete metadata
info:
  title: User Management API
  description: >-
    Manages user accounts, authentication, and profile data.
  version: 2.1.0
  contact:
    name: Platform Team
    email: platform@example.com
  license:
    name: Apache 2.0
    identifier: Apache-2.0
```

**Check for:**
- `version` follows semver (string, not number)
- `description` explains what the API does
- `contact` provides a way for consumers to report issues
- `license` present if the spec is shared externally

## Server Object

```yaml
# BAD - HTTP, no description, hardcoded environment
servers:
  - url: http://api.example.com/v1

# GOOD - HTTPS, variables for flexibility
servers:
  - url: https://{environment}.example.com/v1
    description: Production API
    variables:
      environment:
        default: api
        enum:
          - api
          - api-staging
```

**Check for:**
- All URLs use `https://` (except localhost for development)
- `description` distinguishes production from sandbox
- No trailing slashes on server URLs
- Server variables have `default` and `enum` when applicable

## Paths and Operations

```yaml
# BAD - Verbs in paths, singular nouns, no operationId
paths:
  /getUser/{id}:
    get:
      summary: Get user
      responses:
        '200':
          description: OK

# GOOD - RESTful nouns, plural, complete operations
paths:
  /users/{userId}:
    get:
      operationId: getUserById
      summary: Retrieve a user by ID
      description: Returns the user profile for the given identifier.
      tags:
        - Users
      parameters:
        - $ref: '#/components/parameters/UserId'
      responses:
        '200':
          description: User found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          $ref: '#/components/responses/NotFound'
```

**Check for:**
- Paths use plural nouns, no verbs (`/users`, not `/getUsers`)
- Path segments are lowercase with hyphens (`/user-profiles`)
- Every operation has `operationId`, `summary`, and `description`
- Operations are grouped with `tags`
- Responses include at least success and common error codes
- `operationId` values are unique and descriptive

### HTTP Method Correctness

| Method | Purpose | Request Body | Typical Success Code |
|--------|---------|-------------|---------------------|
| GET | Read | No | 200 |
| POST | Create | Yes | 201 |
| PUT | Full replace | Yes | 200 |
| PATCH | Partial update | Yes | 200 |
| DELETE | Remove | No (usually) | 204 |

## Parameters

```yaml
# BAD - Inline, missing schema details, secret in query
parameters:
  - name: api_key
    in: query
    schema:
      type: string
  - name: id
    in: path
    schema:
      type: string

# GOOD - Described, required set, correct placement
parameters:
  - name: userId
    in: path
    required: true
    description: Unique identifier of the user (UUID)
    schema:
      type: string
      format: uuid
  - name: limit
    in: query
    required: false
    description: Maximum number of results to return
    schema:
      type: integer
      minimum: 1
      maximum: 100
      default: 20
```

**Check for:**
- Path parameters always have `required: true`
- Parameter `name` matches the path template exactly
- Parameters are unique by `name` + `in` combination per operation
- Sensitive values (tokens, keys) use `header` or `cookie`, not `query`
- `description` explains the purpose and constraints
- `schema` includes `format`, `minimum`/`maximum`, `default` where applicable
- Use `$ref` for parameters shared across operations

### Serialization Rules

```yaml
# Simple value - use schema
- name: userId
  in: query
  schema:
    type: string

# Complex value - use content (not both schema and content)
- name: filter
  in: query
  content:
    application/json:
      schema:
        type: object
        properties:
          status:
            type: string
```

## Schema Object

```yaml
# BAD - Minimal schema, no constraints
schemas:
  User:
    type: object
    properties:
      name:
        type: string
      age:
        type: integer

# GOOD - Complete with constraints, examples, and documentation
schemas:
  User:
    type: object
    description: A registered user account
    required:
      - id
      - email
      - displayName
    properties:
      id:
        type: string
        format: uuid
        description: Unique user identifier
        readOnly: true
        example: 550e8400-e29b-41d4-a716-446655440000
      email:
        type: string
        format: email
        description: Primary email address
        example: alice@example.com
      displayName:
        type: string
        minLength: 1
        maxLength: 100
        description: User-chosen display name
        example: Alice Smith
      role:
        type: string
        enum:
          - admin
          - member
          - viewer
        default: viewer
        description: Access role within the organization
      createdAt:
        type: string
        format: date-time
        readOnly: true
    additionalProperties: false
```

**Check for:**
- `type` is specified on every schema
- `required` array lists mandatory properties
- `format` used where applicable (`uuid`, `email`, `date-time`, `uri`)
- `example` values are realistic and match the schema
- `readOnly` / `writeOnly` set for appropriate properties
- `additionalProperties` explicitly set on request body schemas
- `minLength`/`maxLength` on strings, `minimum`/`maximum` on numbers
- `enum` for fields with a fixed set of values
- `description` on every property explains its purpose

### Discriminator Usage

```yaml
# GOOD - Polymorphic schema with discriminator
Pet:
  oneOf:
    - $ref: '#/components/schemas/Cat'
    - $ref: '#/components/schemas/Dog'
  discriminator:
    propertyName: petType
    mapping:
      cat: '#/components/schemas/Cat'
      dog: '#/components/schemas/Dog'
```

### Nullable Fields (3.1.x)

```yaml
# OpenAPI 3.0.x style (deprecated in 3.1)
middleName:
  type: string
  nullable: true

# OpenAPI 3.1.x style (JSON Schema aligned)
middleName:
  type:
    - string
    - 'null'
```

## Security Schemes

```yaml
# BAD - API key in query, no global security
components:
  securitySchemes:
    apiKey:
      type: apiKey
      in: query
      name: api_key

# GOOD - Bearer token with global security applied
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: >-
        JWT token obtained from the /auth/token endpoint.

security:
  - bearerAuth: []

# Per-operation override (public endpoint)
paths:
  /health:
    get:
      security: []  # Explicitly no auth
      responses:
        '200':
          description: Service is healthy
```

**Check for:**
- At least one security scheme defined in `components/securitySchemes`
- `security` applied globally or on every operation
- Public endpoints explicitly opt out with `security: []`
- OAuth2 flows have valid `authorizationUrl` and `tokenUrl`
- OAuth2 scopes are defined and referenced in operation security
- API keys placed in `header` or `cookie`, not `query`

## Component Reuse

```yaml
# BAD - Duplicated inline schema
paths:
  /users:
    get:
      responses:
        '404':
          description: Not found
          content:
            application/json:
              schema:
                type: object
                properties:
                  title:
                    type: string
                  status:
                    type: integer
  /orders:
    get:
      responses:
        '404':
          description: Not found
          content:
            application/json:
              schema:
                type: object
                properties:
                  title:
                    type: string
                  status:
                    type: integer

# GOOD - Reusable components
components:
  schemas:
    ProblemDetail:
      type: object
      description: RFC 9457 Problem Details response
      required:
        - title
        - status
      properties:
        title:
          type: string
          description: Short human-readable summary
        status:
          type: integer
          description: HTTP status code
        detail:
          type: string
          description: Human-readable explanation
  responses:
    NotFound:
      description: The requested resource was not found
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ProblemDetail'
          example:
            title: Not Found
            status: 404
            detail: User with ID 123 does not exist

paths:
  /users:
    get:
      responses:
        '404':
          $ref: '#/components/responses/NotFound'
  /orders:
    get:
      responses:
        '404':
          $ref: '#/components/responses/NotFound'
```

**Check for:**
- Schemas appearing in multiple operations are defined in `components/schemas`
- Common responses (401, 403, 404, 500) use `components/responses`
- Shared parameters use `components/parameters`
- Shared request bodies use `components/requestBodies`
- `$ref` used instead of copy-pasting identical definitions

## Common Anti-Patterns

### 1. Missing Descriptions Everywhere

```yaml
# BAD
paths:
  /users:
    get:
      responses:
        '200':
          description: OK

# Every operation, parameter, and schema should have a meaningful description
```

### 2. Wrong Status Codes

```yaml
# BAD - POST returning 200 for creation
paths:
  /users:
    post:
      responses:
        '200':
          description: User created

# GOOD - 201 with Location header
paths:
  /users:
    post:
      responses:
        '201':
          description: User created successfully
          headers:
            Location:
              schema:
                type: string
                format: uri
              description: URL of the created user
```

### 3. No Error Responses Defined

```yaml
# BAD - Only success response
responses:
  '200':
    description: Success

# GOOD - Document expected errors
responses:
  '200':
    description: Success
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/User'
  '400':
    $ref: '#/components/responses/BadRequest'
  '401':
    $ref: '#/components/responses/Unauthorized'
  '404':
    $ref: '#/components/responses/NotFound'
  '500':
    $ref: '#/components/responses/InternalError'
```

### 4. Inconsistent Naming

```yaml
# BAD - Mixed conventions
properties:
  userId:        # camelCase
  display_name:  # snake_case
  EmailAddress:  # PascalCase

# Pick one convention and use it everywhere
```

### 5. No Pagination on List Endpoints

```yaml
# BAD - Unbounded list
/users:
  get:
    responses:
      '200':
        content:
          application/json:
            schema:
              type: array
              items:
                $ref: '#/components/schemas/User'

# GOOD - Paginated response
/users:
  get:
    parameters:
      - name: limit
        in: query
        schema:
          type: integer
          default: 20
          maximum: 100
      - name: offset
        in: query
        schema:
          type: integer
          default: 0
    responses:
      '200':
        content:
          application/json:
            schema:
              type: object
              properties:
                data:
                  type: array
                  items:
                    $ref: '#/components/schemas/User'
                total:
                  type: integer
                limit:
                  type: integer
                offset:
                  type: integer
```

### 6. Sensitive Data in Query/Path

```yaml
# BAD - Token in query string (visible in logs, browser history)
/users:
  get:
    parameters:
      - name: access_token
        in: query
        schema:
          type: string

# GOOD - Use header for sensitive values
/users:
  get:
    security:
      - bearerAuth: []
```
