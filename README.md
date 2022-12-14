# ccc-actions - GitHub Actions for [Cloud Cost Checker](https://github.com/kunitsuinc/ccc)

[ccc - Cloud Cost Checker](https://github.com/kunitsuinc/ccc) collects, calculates, graphs and notifies IaaS costs.  

## Example

`vim .github/workflows/ccc.yml`

```yml
name: 'ccc - Cloud Cost Checker'

on:
  schedule:
    - cron:  '0 1 * * *'
  workflow_dispatch:
    branches:
      - main
    inputs: {}

jobs:
  should-gitignore:
    name: 'Run ccc - Cloud Cost Checker'
    runs-on: ubuntu-latest
    timeout-minutes: 10
    permissions:
      id-token: write
    steps:
      - name: 'Authenticate to Google Cloud'
        # NOTE: cf. https://github.com/google-github-actions/auth
        uses: 'google-github-actions/auth@v0.8.1'
        with:
          workload_identity_provider: 'projects/999999999999/locations/global/workloadIdentityPools/your-pool/providers/your-provider'
          service_account: 'ccc@your-gcp-project.iam.gserviceaccount.com'
          access_token_lifetime: '600s'
      - uses: kunitsuinc/ccc-actions@v0.0.5
        with:
          TZ: 'Asia/Tokyo'
          GOOGLE_CLOUD_PROJECT: 'your-gcp-project'
          GCP_BILLING_TABLE: '${{ secrets.CCC_GCP_BILLING_TABLE }}'
          GCP_BILLING_PROJECT: 'your-gcp-project'
          DAYS: '30'
          IMAGE_FORMAT: 'png'
          MESSAGE: "'```your-gcp-project Cost```'"
          SLACK_TOKEN: '${{ secrets.CCC_SLACK_TOKEN }}'
          SLACK_CHANNEL: '#your-bot-invited-channel'
          DEBUG: 'true'
```
