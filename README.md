# API Security Risk Analysis Report

![API Security](https://img.shields.io/badge/Security-Audit-red)
![OWASP](https://img.shields.io/badge/OWASP-API%20Top%2010-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📋 Project Overview

This repository contains a comprehensive API Security Risk Analysis performed as part of the Future Interns Cyber Security Task 3 (2026). The project simulates a real-world security consulting engagement where I assessed a public test API for common security vulnerabilities.

**Objective**: Identify API security risks, assess their business impact, and provide remediation recommendations - just like professional AppSec consultants do for SaaS clients.

## 🎯 Scope

- **Target API**: [JSONPlaceholder](https://jsonplaceholder.typicode.com/) (Free fake API for testing)
- **Endpoints Tested**: `/posts`, `/users`, `/comments`, `/todos`
- **Methodology**: Read-only security analysis based on OWASP API Security Top 10
- **Tools Used**: Postman, Browser DevTools, OWASP guidelines

## 🔧 Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Postman** | Manual API request crafting and response inspection |
| **Collection Runner** | Rate limiting testing with multiple requests |
| **Browser DevTools** | Network traffic analysis |
| **OWASP API Top 10** | Vulnerability classification framework |

## 🚨 Key Findings Summary

| Risk Category | Severity | Endpoint | Business Impact |
|---------------|----------|----------|-----------------|
| Broken Authentication | **High** | `/users` | Unauthorized data access |
| Broken Object Level Authorization | **High** | `/posts/{id}` | Users can access others' private data |
| Excessive Data Exposure | **Medium** | `/users` | Location data exposed unnecessarily |
| Missing Rate Limiting | **Medium** | `/posts` | Vulnerable to DoS and brute-force attacks |
| Input Validation Issues | **High** | `POST /posts` | Potential XSS vulnerability |

## 📊 Detailed Methodology

### Phase 1: Reconnaissance
- Reviewed API documentation
- Mapped all available endpoints
- Understood normal API behavior

### Phase 2: Security Testing
For each endpoint, I performed the following checks:

#### 🔐 Authentication Testing
- **Test**: Attempted to access endpoints without any credentials
- **Result**: All endpoints accessible without authentication
- **Risk**: Anyone on the internet can access all data

#### 🔑 Authorization Testing (BOLA)
- **Test**: Accessed `/posts/1` then changed ID to `/posts/2`
- **Result**: Successfully viewed different user's posts
- **Risk**: Horizontal privilege escalation

#### 📦 Data Exposure Analysis
- **Test**: Inspected `/users/1` response body
- **Result**: Response includes unnecessary fields (geo-location, phone)
- **Risk**: Excessive data leakage in breach scenarios

#### ⏱️ Rate Limiting Check
- **Test**: Sent 30 rapid requests using Postman Collection Runner
- **Result**: All requests succeeded, no 429 errors
- **Risk**: System vulnerable to brute-force attacks

#### 🔍 Input Validation
- **Test**: Submitted POST request with script tag payload
- **Result**: API accepted malicious input
- **Risk**: Potential XSS if data displayed without sanitization

## 📈 Complete Findings Table

| # | Risk Category | Endpoint | Observation | Business Impact | Severity | Remediation |
|---|---------------|----------|-------------|-----------------|----------|-------------|
| 1 | **Broken Authentication** | `GET /users/1` | API accessible without any API key or token | Unauthorized parties can access all user data, leading to data breaches and loss of customer trust | **High** | Implement OAuth 2.0 or API Key authentication. Require valid tokens for every request |
| 2 | **Excessive Data Exposure** | `GET /users/1` | Response includes `lat` and `lng` (geo-coordinates) when only name/email is needed | Exposes user location data unnecessarily, increasing impact of data breach | **Medium** | Implement GraphQL or response masking. Only return fields specifically requested |
| 3 | **Broken Object Level Authorization** | `GET /posts/2` | Changing ID from `1` to `2` allows access to another user's post | Users can view/edit other users' private data. Could lead to privacy scandals | **High** | Implement server-side access control checks. Verify user token owns the resource |
| 4 | **Missing Rate Limiting** | `GET /posts` | Sent 30 rapid requests (0ms delay), all 200 OK responses | System vulnerable to DoS and brute-force attacks, causing downtime | **Medium** | Implement rate limiting (100 requests/minute/IP). Return 429 status code |
| 5 | **Input Validation** | `POST /posts` | API accepted script tag in request body | If stored and displayed, could lead to XSS attacks against other users | **Medium** | Implement strict input validation and output encoding. Sanitize all user inputs |

## 🖼️ Evidence Screenshots

### Authentication Test
![Authentication Test](screenshots/auth-test.png)
*API responds without any authentication token*

### Broken Object Level Authorization
![BOLA Test](screenshots/bola-test.png)
*Accessing different user's post by changing ID*

### Excessive Data Exposure
![Data Exposure](screenshots/data-exposure.png)
*Location data exposed in response*

### Rate Limiting Test
![Rate Limit Test](screenshots/rate-limit-test.png)
*30 rapid requests all succeed - no rate limiting*

### Input Validation Test
![Input Validation](screenshots/input-validation.png)
*API accepts script tag in POST request*

## 📝 Remediation Recommendations

### Immediate Actions (High Priority)
1. **Implement Authentication**: Add OAuth 2.0 or JWT-based authentication
2. **Fix Broken Authorization**: Add server-side checks for resource ownership
3. **Review Data Exposure**: Minimize response fields to only what's necessary

### Short-term Improvements (Medium Priority)
4. **Add Rate Limiting**: Implement 100 requests/minute with 429 responses
5. **Input Validation**: Add server-side sanitization for all user inputs
6. **Security Headers**: Add proper CORS and security headers

### Long-term Strategy
7. **Regular Security Audits**: Schedule quarterly API security reviews
8. **Automated Testing**: Integrate API security scanning into CI/CD pipeline
9. **Developer Training**: Train developers on OWASP API Top 10

## 🎓 Lessons Learned

This project demonstrated that even simple APIs can have critical vulnerabilities:
- **Authentication cannot be optional** for sensitive endpoints
- **Authorization must be checked** for every single request
- **Data minimization** reduces breach impact
- **Rate limiting** prevents automated abuse

## 📚 References

- [OWASP API Security Top 10](https://github.com/OWASP/API-Security)
- [API Security Checklist](https://github.com/shieldfy/API-Security-Checklist)
- [JSONPlaceholder Documentation](https://jsonplaceholder.typicode.com/guide/)

## 👨‍💻 Author

**API Security Assessment** - Future Interns Task 3 (2026)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Share%20Post-blue)](https://www.linkedin.com/in/olaniyan-sodiq)

---

*This assessment was performed ethically on a test API designed for learning purposes. No production systems were targeted.*
