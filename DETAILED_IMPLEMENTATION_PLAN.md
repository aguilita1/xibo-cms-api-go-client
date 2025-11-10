# Detailed Implementation Plan for Xibo CMS API Go Client

Based on the README requirements, I've developed a comprehensive plan to implement the `xibocmsapi` Go package. This is a greenfield project that needs to be built from scratch.

## Project Structure

```
xibo-cms-api-go-client/
├── go.mod
├── go.sum
├── README.md (update existing)
├── LICENSE (existing)
├── .gitignore (existing)
├── .env.example
├── Makefile
├── .github/
│   └── workflows/
│       └── ci.yml
├── pkg/
│   └── xibocmsapi/
│       ├── client.go           # Main client implementation
│       ├── auth.go             # OAuth2 authentication module
│       ├── http.go             # HTTP client layer with retry logic
│       ├── errors.go           # Custom error types
│       ├── config.go           # Configuration structures
│       ├── types.go            # Common types and interfaces
│       └── endpoints/          # API endpoint implementations
│           ├── miscellaneous.go
│           ├── schedules.go
│           ├── notifications.go
│           ├── layouts.go
│           ├── playlists.go
│           ├── widgets.go
│           ├── campaigns.go
│           ├── templates.go
│           ├── resolutions.go
│           ├── libraries.go
│           ├── displays.go
│           ├── display_groups.go
│           ├── display_settings.go
│           ├── datasets.go
│           ├── folders.go
│           ├── statistics.go
│           ├── users.go
│           ├── user_groups.go
│           ├── modules.go
│           ├── commands.go
│           ├── dayparting.go
│           └── tags.go
├── internal/
│   ├── models/                 # Request/Response structs
│   │   └── [model files for each endpoint]
│   └── utils/
│       ├── logger.go
│       └── helpers.go
├── tests/
│   ├── unit/
│   │   ├── auth_test.go
│   │   ├── client_test.go
│   │   └── endpoints/
│   │       └── [test files for each endpoint]
│   ├── integration/
│   │   └── integration_test.go
│   └── mocks/
│       ├── http_mock.go
│       └── auth_mock.go
├── examples/
│   ├── basic_usage.go
│   ├── authentication.go
│   ├── display_management.go
│   └── content_management.go
└── docs/
    ├── AUTHENTICATION.md
    ├── API_COVERAGE.md
    └── CONTRIBUTING.md
```

## Implementation Phases

### Phase 1: Core Foundation (Week 1)
1. **Initialize Go Module**
   - Create `go.mod` with module name `github.com/aguilita1/xibo-cms-api-go-client`
   - Add core dependencies:
     - `golang.org/x/oauth2`
     - `github.com/go-resty/resty/v2` (for HTTP client)
     - `github.com/sirupsen/logrus` (for logging)
     - `github.com/stretchr/testify` (for testing)

2. **Configuration Management**
   - Create `config.go` with client configuration options
   - Support for environment variables
   - Configurable timeouts, retry attempts, and collection intervals

3. **Logging Infrastructure**
   - Implement structured logging with configurable levels
   - Request/response logging at debug level
   - Error and retry logging

### Phase 2: OAuth2 Authentication (Week 1-2)
1. **OAuth2 Module (`auth.go`)**
   - Implement client credentials flow
   - Implement authorization code flow
   - Token caching and management
   - Automatic token refresh
   - Secure credential handling via environment variables

2. **Token Manager**
   - In-memory token storage with optional persistence
   - Thread-safe token operations
   - Expiration tracking and preemptive refresh

### Phase 3: HTTP Client Layer (Week 2)
1. **HTTP Client (`http.go`)**
   - Configurable HTTP client with timeouts
   - Request middleware for Bearer token injection
   - Response interceptors for error handling
   - Context support for cancellation

2. **Retry Logic & Rate Limiting**
   - Exponential backoff for transient failures
   - HTTP 429 handling with Retry-After header support
   - Configurable max retry attempts
   - Collection interval fallback

3. **Error Handling**
   - Custom error types for different failure scenarios
   - Detailed error messages with context
   - Error wrapping for better debugging

### Phase 4: API Endpoint Implementation (Week 3-4)
1. **Parse Swagger/OpenAPI Specification**
   - Download and analyze the Xibo CMS swagger.json
   - Generate initial struct definitions for requests/responses
   - Identify common patterns and shared models

2. **Implement Endpoint Wrappers**
   - Create method for each API endpoint
   - Support for:
     - Query parameters
     - Path parameters
     - Request bodies
     - File uploads (for library endpoints)
   - Pagination handling
   - Response parsing and validation

3. **Priority Order for Implementation:**
   - Authentication & User Management (users, user_groups)
   - Display Management (displays, display_groups, display_settings)
   - Content Management (layouts, playlists, campaigns, libraries)
   - Scheduling (schedules, dayparting)
   - Data Management (datasets, folders)
   - System (commands, modules, tags)
   - Analytics (statistics, notifications)
   - Others (miscellaneous, templates, resolutions, widgets)

### Phase 5: Testing (Week 4-5)
1. **Unit Tests**
   - Mock HTTP responses for all endpoints
   - Test OAuth2 flows with mocked token servers
   - Test retry logic and error handling
   - Achieve >80% code coverage

2. **Integration Tests**
   - Optional tests against real Xibo CMS instance
   - Environment-based test configuration
   - Test data cleanup

3. **Mocks and Test Utilities**
   - HTTP client mocks
   - OAuth2 server mocks
   - Test fixtures for common scenarios

### Phase 6: Documentation (Week 5)
1. **README Enhancement**
   - Installation instructions
   - Quick start guide
   - Configuration options
   - Usage examples

2. **GoDoc Documentation**
   - Document all public APIs
   - Include examples in documentation
   - Parameter descriptions and return values

3. **Additional Documentation**
   - Authentication setup guide
   - API coverage matrix
   - Troubleshooting guide
   - Contributing guidelines

### Phase 7: CI/CD Pipeline (Week 5-6)
1. **GitHub Actions Workflow**
   - Build and test on multiple Go versions
   - Linting (golangci-lint)
   - Static analysis (go vet, staticcheck)
   - Security scanning (gosec, nancy)
   - Dependency vulnerability scanning
   - Code coverage reporting
   - Semantic versioning and release automation

2. **Quality Gates**
   - Minimum test coverage threshold
   - No critical security issues
   - All tests passing
   - Linting compliance

## Key Technical Decisions

1. **Package Structure**: Using `pkg/xibocmsapi` for the public API to follow Go conventions
2. **HTTP Client**: Using resty for its built-in retry and middleware support
3. **Logging**: Structured logging with logrus for better debugging
4. **Testing**: Table-driven tests with testify for assertions
5. **Error Handling**: Wrapped errors with context for better debugging
6. **Concurrency**: Thread-safe operations with proper mutex usage
7. **Configuration**: Environment variables with fallback to config struct

## Implementation Priorities

1. **Must Have (MVP)**
   - OAuth2 authentication (both flows)
   - Core HTTP client with retry logic
   - Display and content management endpoints
   - Basic error handling
   - Unit tests for critical paths
   - Basic documentation

2. **Should Have**
   - All API endpoints covered
   - Comprehensive test coverage
   - Rate limiting handling
   - Detailed logging
   - Examples for common use cases

3. **Nice to Have**
   - Request/response caching
   - Webhook support
   - CLI tool for testing
   - Performance benchmarks
   - Advanced retry strategies