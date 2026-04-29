# Smart Tests Results Upload Action

This GitHub Action automatically finds and uploads test report files (XML, JSON etc.) as GitHub artifacts.

It is designed to work with most CI pipelines without requiring any repo-specific configuration.


## Usage

Add this step at the end of your test job. Always use `if: always()` so results are collected even if tests fail.

### Default usage (recommended)

```yaml
- name: Store Test Results for Smart Tests
  if: always()
  uses: launchableinc/smart-tests-results-upload-action@v1
````


### Optional configuration

```yaml
- name: Store Test Results for Smart Tests
  if: always()
  uses: launchableinc/smart-tests-results-upload-action@v1
  with:
    files: |
      **/*.xml
      **/*.json
    days: 7
```


## Inputs

| Input | Default                 | Description                                  |
| ----- | ----------------------- | -------------------------------------------- |
| files | `**/*.xml`, `**/*.json` | Glob patterns used to find test report files |
| days  | `21`                    | Artifact retention period in days            |



## How it works

* Recursively scans the workspace for matching test report files
* Uploads all matched files as a single GitHub artifact
* Runs even if tests fail (`if: always()`)


## Artifact naming

Each run generates a unique artifact name:

```
test-artifacts-<job>-<run_id>-<run_attempt>
```

This prevents collisions across parallel jobs and retries.


## Notes

* This action does not generate test reports
* It only collects existing files from the CI workspace
* Works best with standard test outputs like XML or JSON
* No additional setup is required in most projects


## Common formats supported

* JUnit XML
* Jest / Mocha JSON reports
* Playwright JSON output
* .NET TRX files
* TAP reports


## License

Apache-2.0
