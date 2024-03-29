# OpenSCA Scan Action<!-- omit in toc -->

This action using [OpenSCA-cli](https://github.com/XmirrorSecurity/OpenSCA-cli) to check your application for software supply chain risk.

- [Usage](#usage)
- [Inputs](#inputs)
- [Scenarios](#scenarios)
  - [Bind to OpenSCA SaaS project](#bind-to-opensca-saas-project)
  - [Save the scan log for troubleshooting](#save-the-scan-log-for-troubleshooting)
  - [Upload log and reports to repository](#upload-log-and-reports-to-repository)
- [Troubleshooting](#troubleshooting)
  - [Permission denied](#permission-denied)
  - [Where does the artifact go?](#where-does-the-artifact-go)


# Usage

sample workflow

```yaml
on:
  push:
    branches:
        - master
        - main
  pull_request:
    branches:
        - master
        - main

jobs:
  opensca-scan:
    runs-on: ubuntu-latest
    name: OpenSCA Scan
    steps:
      - name: Checkout your code
        uses: actions/checkout@v4
      - name: Run OpenSCA Scan
        uses: XmirrorSecurity/opensca-scan-action@v1
        with:
          token: ${{ secrets.OPENSCA_TOKEN }}
```

> You need create secrets `OPENSCA_TOKEN` first. [See here](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository)

After finished scan, you can see the report in `Security/Code scanning` tab in your repository. 

![sarif result](/resources/sarif-result.jpg)

You can also view the full result in [OpenSCA SaaS](https://opensca.xmirror.cn/console), the url can be found in the action log.

![action log](/resources/action-log.jpg)

# Inputs

| Name | Required | Description |
| :---: | :---: | --- |
| token | ✔ | OpenSCA auth token. [Get from here](https://opensca.xmirror.cn/console/auth-token) |
| proj | ✖ | The OpenSCA SaaS projectID to bind to |  |
| out | ✖ | Report to upload to repository. Use ',' to separate, only reports in the 'outputs' directory will be uploaded. |
| need-artifact | ✖ | Whether to upload the log and reports to your workflow runs. Default: "false" |

> How to get the token? [See here]()
> 
> How to get the projectID? [See here]()

# Scenarios

## Bind to OpenSCA SaaS project

```yaml
- name: Run OpenSCA Scan
  uses: XmirrorSecurity/opensca-scan-action@v1
  with:
    token: ${{ secrets.OPENSCA_TOKEN }}
    proj: ${{ secrets.OPENSCA_PROJECT_ID }}
```

## Save the scan log for troubleshooting

```yaml
- name: Run OpenSCA Scan
  uses: XmirrorSecurity/opensca-scan-action@v1
  with:
    token: ${{ secrets.OPENSCA_TOKEN }}
    need-artifact: "true"
```

## Upload log and reports to repository

```yaml
- name: Run OpenSCA Scan
  uses: XmirrorSecurity/opensca-scan-action@v1
  with:
    token: ${{ secrets.OPENSCA_TOKEN }}
    out: "outputs/result.json,outputs/result.html"
    need-artifact: "true"
```

> Note: Only reports in the 'outputs' directory will be uploaded.

# Troubleshooting

If you have any questions, please free to create an issue.

## Permission denied

If the action run failed with permission denied error, you may need to check the permission of the action.

Go to `Settings` -> `Actions` -> `General`, in the `Workflow permissions` section, check "Read and write permissions", then click "Save".

## Where does the artifact go?

At the bottom of the workflow summary page, there is a dedicated section for artifacts. Here's a screenshot of something you might see:

![artifacts](/resources/artifacts.jpg)
