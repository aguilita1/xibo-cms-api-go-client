# xibo-api-go-client
This is a package to integrate Xibo CMS API authenticating via OAuth2.

---
## Project Overview
Create a Go package called xibocmsapi that provides a client for developer to programmatically use the Xibo CMS API. Package should simplify authenticating to Xibo CMS API via OAuth2 provider integration.  Provides Go-native abstractions—such as client objects, structs, and methods—to simplify interaction with Xibo CMS services.

### Review Xibo API Go-Client respository
https://github.com/aguilita1/xibo-api-go-client

### Review Xibo CMS API
https://github.com/xibosignage/xibo-cms/blob/develop/web/swagger.json

### Create REST API Endpoint Wrappers. 
Define Go structs for request and response payloads for Xibo CMS API OpenAPI/Swagger (https://github.com/xibosignage/xibo-cms/blob/develop/web/swagger.json). Implement client methods for each REST API endpoints in Miscellaneous, Schedules, Notifications, Layouts, Playlists, Widgets, Campaigns, Templates, Resolutions, Libraries, Displays, Display Groups, Display Settings, DataSets, Folders, Statistics, Users, User Groups, Widgets, Modules, Commands, Dayparting, and Tags. Handle pagination, and error codes. Support context for cancellation and timeouts.

### Create an OAuth2 Authentication Module.
Leverage golang.org/x/oauth2 for implementation. Implement OAuth2 client credentials flow for grant types: access_code and client_credentials. Create a token manager to cache, refresh, and store access tokens. Implement secure handling of client secrets via environment variables. Support token expiration and automatic refresh logic. 

### Create HTTP client layer.
Create a reusable HTTP client with configurable timeouts. Set up request middleware to inject Bearer tokens from the OAuth2 module. Implement centralized error handling, and retries for transient failures. Use a constant that globally sets number of retry attempts for each client call. Log errors at error severity, retries at warn severity, max retry limit breached at error severity. Add request/response logging at debug severity.

### Add retry logic to HTTP client layer to handle API rate limiting.
Xibo Cloud imposes rate limits on connections to the XIBO CMS API to ensure performance for all users. Exceeding these limits may result in an HTTP 429 "Too Many Requests" error.  When a 429 error is received, the client should slow down its request rate. If a Retry-After header is present in the response, the client should wait for the specified duration before retrying the request. If Retry-After is absent, it is conventional to wait for the collection interval, a constant that can be set globally for client, before retrying the request. If Retry-After logic fails client should raise exception for failed request.
### Create tests and mocks for code base.
Unit test OAuth2 logic (mocking token server responses). Unit test API client methods with mocked HTTP responses. Add integration tests.

### Document Go package.
Write README with installation, build, test, usage, and examples (how a developer can use client to authenticate and call Xibo CMS API). Document public methods and exported types using GoDoc.  Include authentication setup guide (e.g., how to obtain credentials). Add API coverage and limitations.

### Create Github Action CI/CD pipeline.
Create a pipeline that builds, tests, performs linter, static code analysis, scan for security vulnerabilities, and scan for dependency vulnerabilities, enable semantic versioning, publish package.
