# cliche-action

GitHub Action for generating a static site with [cliche](https://github.com/gdtroszak/cliche) and creating an artifact that can be uploaded to a hosting provider.

## Example workflow

Here's an example workflow that deploys to [GitHub Pages](https://pages.github.com/).
The artifact created by this action could be uploaded to any hosting provider though.

```yaml
name: deploy to pages
on:
  push:
    branches: ["main"]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: "pages"
  cancel-in-progress: false
jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v4
      - name: build site and upload artifact
        uses: gdtroszak/cliche-action@v1
      - name: setup pages
        uses: actions/configure-pages@v5
      - name: deploy to gh pages
        id: deployment
        uses: actions/deploy-pages@v4
        with:
          artifact_name: cliche-site
  
```

## Inputs

All inputs are optional.

<table>
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>name</td>
    <td>Artifact name. Default is "cliche-site".</td>
  </tr>
  <tr>
    <td>content-path</td>
    <td>Path to the site content. Default is "content".</td>
  </tr>
  <tr>
    <td>header-path</td>
    <td>Path to the site header. Default is "header.md".</td>
  </tr>
  <tr>
    <td>footer-path</td>
    <td>Path to the site footer. Default is "footer.md".</td>
  </tr>
  <tr>
    <td>style-path</td>
    <td>Path to the site style. Default is "style.css".</td>
  </tr>
  <tr>
    <td>domain</td>
    <td>Domain where the site will be hosted. If not specified, a sitemap will not be generated.</td>
  </tr>
</table>

## Outputs

<table>
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>artifact-id</td>
    <td>The ID of the artifact that was uploaded.</td>
  </tr>
</table>
