
# API Security Checklist

These are some critical security measures to consider when designing, testing, and releasing API.

---

## Authentication

- [ ] Avoid using `Basic Auth`. Opt for standard authentication methods like [JWT](https://jwt.io/).
- [ ] Do not create custom solutions for `Authentication`, `token generation`, or `password storage`. Follow established standards.
- [ ] Implement `Max Retry` and lockout mechanisms for login attempts.
- [ ] Encrypt all sensitive data.
- [ ] Enable Multi-Factor Authentication (MFA) where possible.

### JWT (JSON Web Token)

- [ ] Use a complex, random key (`JWT Secret`) to prevent brute force attacks.
- [ ] Do not rely on the algorithm specified in the header; enforce the algorithm on the server-side (`HS256` or `RS256`).
- [ ] Set a short expiration time for tokens (`TTL`, `RTTL`).
- [ ] Avoid storing sensitive information in the JWT payload as it can be easily decoded [here](https://jwt.io/#debugger-io).
- [ ] Keep JWT payloads minimal as they are transmitted in headers, which have size limits.
- [ ] Periodically rotate secrets and invalidate old tokens.
- [ ] Implement token revocation to handle logout and compromised tokens.

## Access

- [ ] Implement rate limiting to mitigate DDoS and brute-force attacks.
- [ ] Use HTTPS with TLS 1.2 or higher and secure ciphers to prevent MITM (Man in the Middle) attacks.
- [ ] Employ `HSTS` headers with SSL to prevent SSL stripping attacks.
- [ ] Disable directory listings.
- [ ] For private APIs, restrict access to safelisted IPs/hosts.
- [ ] Implement IP whitelisting/blacklisting as applicable.
- [ ] Adhere to the principle of least privilege, granting only necessary permissions to users and services.

## Authorization

### OAuth

- [ ] Validate `redirect_uri` on the server-side to allow only safelisted URLs.
- [ ] Prefer exchanging codes for tokens; avoid `response_type=token`.
- [ ] Use the `state` parameter with a random hash to prevent CSRF during the OAuth authorization process.
- [ ] Define and validate the default scope and scope parameters for each application.
- [ ] Implement Proof Key for Code Exchange (PKCE) to enhance OAuth security for public clients.
- [ ] Regularly review and update scopes and permissions.

## Input

- [ ] Use appropriate HTTP methods for operations: `GET` for read, `POST` for create, `PUT/PATCH` for replace/update, and `DELETE` for deleting records. Return `405 Method Not Allowed` for incorrect methods.
- [ ] Validate `content-type` in the Accept header to allow only supported formats (e.g., `application/xml`, `application/json`). Return `406 Not Acceptable` if unmatched.
- [ ] Validate the `content-type` of submitted data to match accepted formats (e.g., `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`).
- [ ] Sanitize and validate user input to prevent vulnerabilities like `XSS`, `SQL Injection`, and `Remote Code Execution`.
- [ ] Avoid using sensitive data (`credentials`, `passwords`, `security tokens`, or `API keys`) in URLs. Use standard Authorization headers.
- [ ] Encrypt sensitive data on the server-side only.
- [ ] Utilize an API Gateway for caching, rate limit policies (e.g., `Quota`, `Spike Arrest`, `Concurrent Rate Limit`), and dynamic API resource deployment.
- [ ] Enforce strict schema validation for all input data.

## Processing

- [ ] Ensure all endpoints are protected by authentication to prevent unauthorized access.
- [ ] Avoid exposing user resource IDs. Use `/me/orders` instead of `/user/654321/orders`.
- [ ] Use `UUIDs` instead of auto-incrementing IDs.
- [ ] Disable entity parsing when processing XML to prevent `XXE` (XML External Entity) attacks.
- [ ] Disable entity expansion in languages like XML and YAML to prevent exponential entity expansion attacks (e.g., `Billion Laughs/XML bomb`).
- [ ] Use a CDN for file uploads.
- [ ] Use Workers and Queues for processing large amounts of data in the background to avoid HTTP blocking.
- [ ] Always disable DEBUG mode in production environments.
- [ ] Implement non-executable stacks where possible.
- [ ] Apply rate limiting and quotas at API endpoints.
- [ ] Validate and sanitize all file uploads.
- [ ] Encrypt sensitive data both in transit and at rest.
- [ ] Implement proper logging and monitoring of all processing activities.

## Output

- [ ] Set `X-Content-Type-Options: nosniff` header.
- [ ] Set `X-Frame-Options: deny` header.
- [ ] Set `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove fingerprinting headers such as `X-Powered-By`, `Server`, `X-AspNet-Version`.
- [ ] Enforce the `content-type` for responses. For example, return `application/json` if the response type is JSON.
- [ ] Do not return sensitive information such as `credentials`, `passwords`, or `security tokens`.
- [ ] Return appropriate HTTP status codes for operations (e.g., `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`).
- [ ] Implement proper error handling and avoid exposing stack traces.
- [ ] Ensure responses do not disclose internal implementation details.

## CI & CD

- [ ] Conduct design and implementation audits with unit and integration test coverage.
- [ ] Use a code review process with disallowance of self-approval.
- [ ] Ensure all service components are statically scanned by antivirus software before production deployment, including vendor libraries and dependencies.
- [ ] Continuously run security tests (static and dynamic analysis) on your code.
- [ ] Regularly check dependencies (both software and OS) for known vulnerabilities.
- [ ] Design a rollback plan for deployments.
- [ ] Automate security testing within the CI/CD pipeline.
- [ ] Implement Infrastructure as Code (IaC) for secure management and provisioning of infrastructure.

## Monitoring

- [ ] Centralize logging for all services and components.
- [ ] Use agents to monitor traffic, errors, requests, and responses.
- [ ] Set up alerts for SMS, Slack, Email, Telegram, Kibana, Cloudwatch, etc.
- [ ] Avoid logging sensitive information such as credit card numbers, passwords, and PINs.
- [ ] Use an IDS and/or IPS to monitor API requests and instances.
- [ ] Regularly review logs and monitor for suspicious activity.
- [ ] Implement Security Information and Event Management (SIEM) systems.
- [ ] Ensure compliance with relevant regulations (e.g., GDPR, HIPAA).

## Additional Considerations

- [ ] Provide regular security training for development and operations teams.
- [ ] Develop and maintain an incident response plan.
- [ ] Conduct regular security audits and penetration tests.
- [ ] Keep software and dependencies up to date with security patches.
- [ ] Ensure proper session management, including secure cookie settings and session expiration.
- [ ] Utilize Content Delivery Networks (CDNs) and Web Application Firewalls (WAFs) for enhanced security.
- [ ] Ensure proper backup and recovery procedures are in place.
- [ ] Regularly review and update security policies and procedures.

