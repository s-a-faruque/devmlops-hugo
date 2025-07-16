+++
title = "Stop Wasting Time Checking PDFs: Automate Link Testing with GitLab CI + Playwright"
date = 2025-07-04T12:00:00Z
author = "Safique Ahmed Faruque"
tags = ["python", "gitlab-ci", "playwright", "automation", "testing"]
categories = ["devops", "testing"]
description = "Learn how I automated PDF link testing across multiple sites using a GitLab CI setup, Playwright and Pytest."
draft = false
image = "images/blog/automated-test.png"
+++

As a developer or QA engineer, you know the frustration of discovering broken PDF links after a site update — especially when those links span multiple websites. I remember spending hours manually checking PDFs across five different sites, copying and pasting URLs into a browser, hoping I wouldn’t miss any. Each time a site updated, the tedious routine repeated itself, eating into time better spent building new features.
<!--more-->

Frustrated by this inefficiency, I decided there had to be a better way. What if I could automate the whole process, running tests that checked every PDF link across all sites — reliably, consistently, and with zero manual effort?

In this article, I’ll walk you through how I built an automated PDF link testing solution using Python’s `pytest`, Playwright for browser automation, and GitLab CI/CD for continuous integration. To make it scalable and easy to maintain, I placed all the testing logic in a separate GitLab project and ran the tests on self-hosted runners, avoiding costly CI minute limits. This setup not only saved me hours of repetitive work but also gave my team quick feedback on link health — no more surprises in production.

If you’re juggling multiple sites or just want a practical example of integrating automated tests in your CI pipelines, this guide is for you.

---

## Tools and Technology Stack

* **Pytest:** Python’s robust testing framework to write flexible tests.
* **Playwright:** Browser automation library for simulating user actions and checking PDFs.
* **GitLab CI/CD:** For running tests automatically in pipelines.
* **Self-hosted GitLab Runners:** Dedicated machines under my control to execute CI jobs without usage limits.
* **Text files with URLs:** Easy-to-edit files storing site-specific PDF URLs.

---

## The Challenge

I manage 5+ sites, each with a large list of PDF URLs. I needed a way to:

* Run link tests against any site dynamically without manual file copying.
* Keep the test code and config separate from each site’s repository for easier maintenance.
* Avoid limitations or costs associated with shared GitLab CI minutes.
* Provide a simple way for team members to run tests without complex setup.

---

## Solution Overview

### 1. Centralized Test Repository

I created a **dedicated GitLab project** containing all test code, URL lists, and CI configurations. This acts as a **single source of truth** for testing across sites.

### 2. Dynamic URL Loading

Using `pytest`'s CLI options and hooks, the tests load URLs from a file passed as a parameter, so one test script can run against any site by simply changing the URL list.

### 3. GitLab CI Jobs per Site

The `.gitlab-ci.yml` defines jobs for each site, passing the appropriate URL file as a test argument.

### 4. Self-Hosted Runners

To avoid consuming limited GitLab shared runner minutes, I installed self-hosted GitLab runners that execute these jobs on my own servers, giving me full control and no time limits.

---

## Implementation Details

### Parametrizing URLs in Pytest

In `conftest.py`, I added the following:

```python
import pytest
import os

def pytest_addoption(parser):
    parser.addoption(
        "--urlfile",
        action="store",
        default="test_urls.txt",
        help="Path to the file containing URLs to test"
    )

def pytest_generate_tests(metafunc):
    if "url" in metafunc.fixturenames:
        urlfile = metafunc.config.getoption("urlfile")
        if not os.path.exists(urlfile):
            pytest.fail(f"URL file '{urlfile}' does not exist")

        with open(urlfile) as f:
            urls = [line.strip() for line in f if line.strip()]

        if not urls:
            pytest.skip(f"No URLs found in {urlfile}")

        metafunc.parametrize("url", urls)
```

### Example Test Function

```python
def test_pdf_links(url):
    # Playwright logic to verify PDF links would go here
    assert url.startswith("https://"), f"Invalid URL: {url}"
```

### GitLab CI Configuration

```yaml
stages:
  - test

test_siteA:
  tags:
    - self-hosted
  stage: test
  script:
    - pip install -r requirements.txt
    - python -m playwright install chromium
    - pytest -s tests/test_pdf_links.py --urlfile=test_siteA_urls.txt --junitxml=report_siteA.xml
  artifacts:
    reports:
      junit: report_siteA.xml
    paths:
      - report_siteA.xml

test_siteB:
  tags:
    - self-hosted
  stage: test
  script:
    - pip install -r requirements.txt
    - python -m playwright install chromium
    - pytest -s tests/test_pdf_links.py --urlfile=test_siteB_urls.txt --junitxml=report_siteB.xml
  artifacts:
    reports:
      junit: report_siteB.xml
    paths:
      - report_siteB.xml
```

### Using Self-Hosted Runners

I registered a runner on my own infrastructure with the tag `self-hosted`. This allows GitLab to assign these test jobs to my machines, bypassing shared runner limits and enabling a stable, configurable environment.

---

## Benefits of This Approach

* **Centralized management:** One repo holds all test logic and URLs, simplifying updates and adding new sites.
* **Flexible and scalable:** New sites require only a URL file and a CI job.
* **Cost control:** Self-hosted runners eliminate CI minute costs and give full control.
* **Accessible to team:** Anyone with repo access can trigger tests without setup hassle.
* **Reliable and repeatable:** Tests run consistently in a clean, isolated environment.
