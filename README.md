# strava-heatmap-proxy

This is a simple [Cloudflare Worker](https://workers.dev) allowing
unauthenticated access to personal and global Strava heatmaps. If you want to
use your personal Strava heatmap in Gaia or Locus, this will give you a URL
that you can use for that.

Note: you **will** need to be a Strava premium subscriber to use the personal
heatmap, while the global heatmaps are available to all Strava accounts. Personal
use only, please. Strava will ratelimit you.

# Setup

Follow either of the two paths described below to deploy your Cloudflare
Worker.

If you want to use these heatmaps as a tile layer in another app, here are the
template URLs to use:

- Personal: `https://strava-heatmap-proxy.YOUR_NAMESPACE.workers.dev/personal/orange/all/{zoom}/{x}/{y}@2x.png`
- Global: `https://strava-heatmap-proxy.YOUR_NAMESPACE.workers.dev/global/orange/all/{zoom}/{x}/{y}@2x.png`

Check `https://strava-heatmap-proxy.YOUR_NAMESPACE.workers.dev/` for full list
of supported tile colors, activities, and sizes.

## The manual but easy way

If automatic credential fetching is not working (e.g. due to changes in Strava’s login flow), you can manually provide the required authentication values.

Start by forking this repository and setting up the following GitHub secrets  
(`github.com/XXXXXXXX/strava-heatmap-proxy/settings/secrets/actions`):

- `STRAVA_ID`
- `STRAVA_COOKIES`
- [`CF_ACCOUNT_ID`](https://developers.cloudflare.com/fundamentals/get-started/basic-tasks/find-account-and-zone-ids/)
- [`CF_API_TOKEN`](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)

These secrets will be used by two GitHub Actions.. 

1. Manually obtain valid `STRAVA_ID` and `STRAVA_COOKIES` using JOSM Chrome Browser extension. 
2. Add them as GitHub repository secrets `STRAVA_ID` and `STRAVA_COOKIES`
3. Retrieve CF_ACCOUNT_ID and CF_API_TOKEN from setting up an useraccount API Token using these permissions: Workers KV Storage:Edit, Workers Scripts:Edit, Account Settings:Read.
4. Just edit any file in the repository and push the changes (e.g. README, just adding a few whitespaces) to make the Action deploy the Fork to a CF Worker.
5. Trigger the workflow "02 Manually Push Strava AUTH to CF"

Your site should now be live on  
`strava-heatmap-proxy.YOUR-NAMESPACE.workers.dev`, the exact link can be seen from the CF Dashboard under "Workers".
