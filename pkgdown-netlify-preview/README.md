# `pkgdown-netlify-preview`

Use the hubverse developer custom pkgdown GitHub Action

The functions creates a `pkgdown-netlify-preview.yaml` action in the `.github/workflows`
directory. The action follows the standard pkgdown publishing to `gh-pages` proceedure in the
event of a merge into `main` branch but can also be set up to publish internal
PR previews to Netlify.
@details
To activate Netlify Previews, you must:
- Create a new site on Netlify. See <https://docs.netlify.com/welcome/add-new-site/> for more details.
- "Add the netlify API site ID to the GitHub repository Action secrets as
`NETLIFY_SITE_ID`. See <https://docs.netlify.com/api/get-started/#get-site> for more details.
- Get developer token from Netlify developer account settings and add it to
GitHub repository Action secrets as `NETLIFY_AUTH_TOKEN`. See
<https://docs.netlify.com/cli/get-started/#obtain-a-token-in-the-netlify-ui>
for more details.
#'
See  [pkgdown]() docs section on
[PR previews](https://pkgdown.r-lib.org/articles/customise.html#pr-previews)
for more details.
