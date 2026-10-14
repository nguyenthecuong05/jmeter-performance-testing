# Apache JMeter – Basic Website Performance Testing

> **Student:** [Enter your full name]  
> **Student ID:** [Enter your student ID]  
> **Course / Class:** [Enter course or class]  
> **Date:** [Enter date]

## 1. Introduction

This project demonstrates a basic website performance test using Apache JMeter. It includes a reusable `.jmx` test plan, HTTP status assertions, and listeners for reviewing response-time and request statistics.

The test targets `https://example.com`, a public example website. This is a small learning exercise, not a benchmark of production capacity.

## 2. Objectives

- Understand the Test Plan and Thread Group concepts.
- Send HTTP GET requests to a website.
- Check HTTP response codes with Response Assertion.
- Review sample count, average/minimum/maximum response time, throughput, and error percentage.
- Save the test plan and evidence in a GitHub repository.

## 3. Tools

- Apache JMeter 5.6.3 (the included plan uses standard JMeter components)
- Java runtime compatible with the chosen JMeter release
- GitHub for source files, documentation, and evidence

## 4. Test Scenario

| Setting | Value |
|---|---|
| Target host | `example.com` |
| Protocol | HTTPS |
| Virtual users | 5 |
| Ramp-up period | 5 seconds |
| Loop count | 2 |
| Request 1 | `GET /` |
| Request 2 | `GET /changes.html` |
| Assertions | HTTP response code equals 200 |
| Expected sample count | 5 users × 2 requests × 2 loops = 20 samples |

The expected sample count assumes all threads complete both requests on both iterations. Actual results can differ if a request fails or execution stops early.

## 5. Repository Contents

- `test-plan/example-website-load-test.jmx` — JMeter test plan.
- `.github/workflows/jmeter-test.yml` — optional workflow to run the test in GitHub Actions without installing JMeter on the school computer.
- `results/README.md` — where to record actual test results.
- `screenshots/README.md` — screenshot checklist.

## 6. How to Run

### Option A — Run with JMeter already available

1. Open the `.jmx` file in Apache JMeter.
2. Review the Thread Group, HTTP Request samplers, Response Assertions, and listeners.
3. Press Start.
4. Inspect View Results Tree, Summary Report, and Aggregate Report.
5. Capture screenshots of the actual configuration and results.

### Option B — Run through GitHub Actions (no local installation)

1. Upload all project files to a GitHub repository.
2. Open the repository's **Actions** tab.
3. Select **JMeter Performance Test**.
4. Click **Run workflow** and confirm.
5. Open the completed workflow run and inspect the logs.
6. Download the `jmeter-results` artifact. It contains the raw `.jtl` result file and generated HTML report if the workflow completes successfully.
7. Capture screenshots of the successful workflow run and the generated report. Do not describe the test as passed unless the run and assertions actually pass.

The workflow downloads JMeter in the GitHub-hosted runner. It does not install software on the school computer. GitHub Actions availability depends on repository settings and usage limits.

## 7. Results

**Fill in this section only after running the test. Do not invent performance numbers.**

| Metric | Actual result |
|---|---|
| Total samples | [Fill from actual run] |
| Average response time (ms) | [Fill from actual run] |
| Minimum response time (ms) | [Fill from actual run] |
| Maximum response time (ms) | [Fill from actual run] |
| Throughput | [Fill from actual run] |
| Error percentage | [Fill from actual run] |
| Assertion status | [Passed / Failed, based on actual run] |

### Analysis

[Explain the observed results. Mention whether all requests returned HTTP 200, whether assertions passed, and what the measured response times and error percentage show. A small test against a public example site does not prove how many users a production website can support.]

## 8. Screenshots

Add your own screenshots of actual execution to the `screenshots/` folder. See `screenshots/README.md` for the list. Do not use mockups as evidence of a test run.

## 9. Conclusion

This exercise demonstrates how to build a basic JMeter test plan, simulate concurrent users, validate HTTP responses, and inspect performance metrics. The final conclusion should be updated after reviewing the actual run.

## 10. References

- Apache JMeter: https://jmeter.apache.org/
- Getting Started: https://jmeter.apache.org/usermanual/get-started.html
- Building a Web Test Plan: https://jmeter.apache.org/usermanual/build-web-test-plan
- Building a Test Plan: https://jmeter.apache.org/usermanual/build-test-plan
