
QA Assessment Summary Report: News API & Bikroy Website


1. Scope, Time Spent, Environment, and Testing Assumptions

• Scope: This assessment covers automated API validation for the live News API service using Postman, alongside manual exploratory UI testing on the public Bikroy marketplace website.
• Time Spent: 2.5 hours of active test design and execution.
• Environment:
	• API Layer: Postman application connected to live endpoints at https://newsapi.org.
	• UI Layer: Google Chrome browser running on a desktop Windows 11 environment visiting https://bikroy.com.
• Testing Assumptions:
	1. We assume the public News API server allows steady automation testing loops without hitting sudden rate limit blocks.
	2. We assume the public flows on the Bikroy marketplace do not require a logged-in user profile to test the core search and location filtering systems.

2. API Test Execution Summary (News API)

• Total Test Requests Evaluated: 7 Cases (6 Required Suite Cases + 1 Optional Bonus Request)
• Passed Status Matrix: 6 Requests executed with 100% assertion script clearance.
• Failed Status Matrix: 1 Request (TC-NEWS-06 - Invalid Parameter Value) failed because a live contract discrepancy was caught.
• Blocked / Not Run Status Matrix: 0

3. Bikroy UI Scenarios Explored and Findings

• Charter Target Mapping: Explored the platform as an anonymous buyer trying to discover a used phone in Dhaka.
• Total UI Scenarios Logged: 8 Scenarios.
• Passed Status Matrix: 7 Scenarios successfully validated that the home page loading, category filtering, subcategory layout mapping, location filtering to Dhaka, and detailed page views are highly stable.
• Failed Status Matrix: 1 Scenario (SC-BIK-UI-08 - Whitespace Search Vector) failed due to missing text input validation filters.

4. Prioritized Issues and Contract Discrepancies

Note: Below are the top two highest-priority observations recorded during live infrastructure execution, supported by real sanitized evidence.

Issue 01 (High Priority): Live API Contract Discrepancy on Invalid Sorting Parameter

• Title: The live microservice silently ignores invalid query parameters instead of returning a client error code.
• Steps to Reproduce: Run a GET call to https://newsapi.org (TC-NEWS-06).
• Expected Result: The server should return an HTTP 400 Bad Request code with a clear validation payload mapping parameterInvalid.
• Actual Result: The live backend completely ignored the broken parameter value, used default sorting instead, and successfully returned an HTTP 200 OK response with article items.
• Evidence: Recorded inside the Part A - News API Matrix spreadsheet layout matching your live execution parameters.

Issue 02 (Medium Priority): Missing Input Validation for Whitespace-Only Queries on Website Search Box

• Title: Submitting blank spaces inside the primary search entry bar bypasses text filtering constraints.
• Steps to Reproduce:
	1. Go to https://bikroy.com.
	2. Click inside the search input box, press the spacebar exactly three times (   ), and press Enter.
• Expected Result: The frontend layout should trim the blank string input and block the empty search request, displaying a clear validation popup warning like "Please enter a valid keyword to search".
• Actual Result: The site executes a silent page reload and fetches all 298,103 raw listing results across All Bangladesh without showing any user alert messages.
• Evidence: Verified screenshot proof saved inside the directory as scr-bikroy-whitespace-defect.png.

5. Remaining Risks and Gaps to Investigate Next

• Remaining Risk: There is a threat of database performance lag or extreme response delay if users search using mixed character styles or complex localized languages (like raw Bangla Unicode text streams) that bypass initial sanitation filters.
• Next 3 Tests to Run with More Time:
	1. API Concurrency Load Test: Run multiple automation loops at the max limit of 50 records to evaluate server performance behavior under traffic load.
	2. String Length Overflow Boundary Test: Input a long text value string exceeding 100 characters in parameter q to see if the system truncates it safely without internal errors.
	3. Conflicting Query Argument Test: Pass contradictory parameters simultaneously (like two different location parameters together) to review how the query parser decides priority.

6. 5-Minute Walkthrough of One Postman Request

• Selected Target Request: TC-NEWS-01 - Search articles - valid request
• Configuration Setup: The request is mapped via GET targeting the endpoint https://newsapi.org. The collection safely manages the secret authentication token via a dynamic global header parameter ({{newsApiKey}}) at the collection root layer.
• Assertion Breakdown & Business Logic:
	• Status code is 200 OK: Asserts that the cloud server transaction connection is fully active.
	• Response contains valid envelope structure and types: A critical script that double-checks data shapes to confirm status returns "ok", articles is an array layout, and counters map to numbers. Validating data types instead of hardcoded numbers ensures that the test suites stay stable and do not fail just because live news content updates.
• Live Finding Result Walkthrough: During runtime execution, all 6 automated test assertions successfully turned green (PASSED). This confirms that our Postman test framework handles dynamic, changing data structures correctly while successfully keeping secret parameters completely separate from the collection files.