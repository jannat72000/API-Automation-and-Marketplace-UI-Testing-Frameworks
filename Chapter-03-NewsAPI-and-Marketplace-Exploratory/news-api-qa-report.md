# Multi-Platform QA Architecture: News API Automation Engine & Live Marketplace Exploratory Testing Framework

## 1. Project Overview & Testing Strategy
This engineering module establishes a dual-scope QA architecture designed to validate production-grade software layers. The module contains a fully automated API validation suite executed directly against live cloud microservices and a manual exploratory testing workflow mapped onto a high-traffic e-commerce marketplace platform.

*   **Technology Stack Deployed:** Postman Automation Framework, Asynchronous JavaScript Assertion Scripting, and Clean Variable Sandboxing.
*   **Total Test Execution Vectors:** 7 Automated Request Models + 8 Structured User-Journey Manual Scenarios.
*   **Production Gateways Tested Live:** `https://newsapi.org` and `https://bikroy.com`.

---

## 2. API Automation Architecture Blueprint (News API Framework)

The automated script engine isolates credentials safely via secure environment variables (`{{newsApiKey}}`) and executes multi-tier assertions verifying both network protocols and JSON-Schema data structures.

### 🔹 Public Stream Discoveries (Positive Path Configurations)
*   **Dynamic Article Aggregator Validation (`TC-NEWS-01`):** Sends requests to the search route targeting queries like `?q=technology`. The suite validates the server return profile (`HTTP 200 OK`) and parses dynamic article objects array keys (`title`, `url`, `publishedAt`) automatically to ensure structural updates do not break the test suite.
*   **Curated Headline Analytics (`TC-NEWS-02`):** Implements localized geographical parameters (`country=us`) to check system filtering logic, validating the response envelope shape successfully without hardcoding dynamic content states.
*   **Source Channel Identification (`TC-NEWS-07 - Bonus Track`):** Deployed a dedicated array inspector checking top headline distribution sources (`/v2/top-headlines/sources`) to map and verify communication metrics on dynamic media networks.

### 🔹 Security Access Safeguards & Validation Exceptions (Negative Paths)
*   **Authentication Exclusions Monitor (`TC-NEWS-03`):** Intentionally blocks API key inclusion flags to verify backend defensive routing. The script validates that the remote gateway securely blocks the transaction with an `HTTP 401 Unauthorized` state code and logs a structured `apiKeyMissing` error signature.
*   **Credential Spoof Interceptor (`TC-NEWS-04`):** Passes a fake testing credentials string layout (`invalid-test-key`). The test framework confirms the gateway catches the exploit and safely responds with a documented `apiKeyInvalid` client block.
*   **Mandatory Field Incomplete Handler (`TC-NEWS-05`):** Bypasses the mandatory query parameters (`q`) to verify endpoint validation filters. The suite checks that the server intercepts the broken payload and responds with an explicit `parametersMissing` error envelope code.

---

## 3. Marketplace UI Behavioral Matrix & Defect Discovery (Bikroy Platform)

A structured exploratory testing execution session was managed under the operational charter: *"Analyze anonymous user discovery flows, regional filtering tools, and presentation data models across mobile inventory structures."* 

### 🔹 Deployed UI Scenario Logs & Integrity Tracking
*   **Landing Layout & Visual Check (`SC-BIK-UI-01`):** Verified home screen presentation shapes, category navigator nodes, and language headers. The full UI resolves seamlessly under 2 seconds with zero broken frames.
*   **Search Engine Retrieval Accuracy (`SC-BIK-UI-02`):** Input valid phone criteria words (`iPhone`). The platform dynamically sorts relevant ad grids based on text matching.
*   **Deep Subcategory Routing Flow (`SC-BIK-UI-03` & `04`):** Tested multi-filter applications by navigating into **Mobiles -> Phones & Tablets** and locking the location sieve filter strictly to the **Dhaka** area. The catalog drops external listings instantly.
*   **Data Consistency & Browser History (`SC-BIK-UI-05` & `06`):** Validated that individual listing detail panels present accurate metadata blocks (Title, explicit price fields, area info). Verified browser **Back/Forward** states operate cleanly without session timeouts.
*   **Empty State Graceful Resolution (`SC-BIK-UI-07`):** Tested a mismatched random text sequence string (`xyznonexistentterm123`). The interface intercepts the query and shows a clean "No results found" panel display.

---

## 4. Production Bug Identification & Architectural Discrepancies

The testing suite identified the following prioritized system errors during live production environments tracking execution:

### 🪲 Bug 01 (Medium Priority): Missing Input Validation for Whitespace-Only Marketplace Queries
*   **Defect Signature:** Bypassing standard string trimming checks on search fields allows blank queries to trigger unneeded server calls.
*   **Replication Steps:** Navigate to the main homepage search input bar, inject exactly three blank spaces (`q=   `), and press **Enter** (`SC-BIK-UI-08`).
*   **Expected Secure Behavior:** The client application layer should catch the blank entry string block, stop the empty payload refresh, and present a clear input warning message like *"Please enter a valid keyword to search"*.
*   **Live Actual Behavior:** The interface fails to catch the whitespace query and executes a full page refresh, fetching 298,103 raw database ads across All Bangladesh without alerting the user.
*   **Evidence Asset Captured:** Visual proof logged and uploaded as `scr-bikroy-whitespace-defect.png`.
*   **Status Classification:** **FAILED (Defect Confirmed)**

### 🪲 Bug 02 (Low Priority): API Contract Drift on Invalid Sorting Filter Configuration
*   **Defect Signature:** Live microservice architecture silently ignores unsupported arguments instead of returning clean boundary client errors.
*   **Replication Steps:** Dispatch a `GET` request to `https://newsapi.orgeverything?q=apple&sortBy=invalidSortParam` (`TC-NEWS-06`).
*   **Expected Secure Behavior:** Server should return an `HTTP 400 Bad Request` mapping out an explicit `parameterInvalid` code payload.
*   **Live Actual Behavior:** The backend completely bypassed the invalid sorting string argument, fell back to default sorting parameters, and processed the call as a successful `HTTP 200 OK` transaction loop.
*   **Status Classification:** **FAILED (Contract Drift Caught)**

---

## 5. Next Steps & Advanced Testing Roadmap
1.  **Automated Security Key Leak Scanning:** Integrate automated token scanner actions into pre-commit validation chains to ensure configuration credentials are never hardcoded inside repository code text files.
2.  **API Schema Special Character Stress Tests:** Execute edge validation loops passing mixed punctuation formatting values and Unicode character arrays into parameters to analyze backend sanitization boundaries.
3.  **Frontend Offline Network Caching Check:** Simulate sudden network disconnect drops on phone detail view paths to evaluate application fallback resilience.
