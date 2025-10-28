# Cloudflare Pages Deployment

## Setup Instructions

### 1. Deploy with Wrangler CLI

The site is deployed using Wrangler CLI (already configured):

```bash
# Install dependencies
npm install

# Deploy to Cloudflare Pages
npx wrangler pages deploy . --project-name=qfax-website

# Or use npm script (if added)
npm run deploy
```

**First-time setup:**
- Login: `npx wrangler login`
- Deploy creates the project automatically

### 2. Configure Custom Domain

After the first deployment completes:

1. In the Cloudflare Pages dashboard, go to your project
2. Click **Custom domains** tab
3. Add custom domain: `qfax.football`
4. Cloudflare will automatically configure DNS if the domain is already managed by Cloudflare
5. Add www subdomain: `www.qfax.football`
6. Enable **Always Use HTTPS**

### 3. DNS Configuration

If the domain is managed by Cloudflare (which it should be):

1. DNS records will be automatically created:
   - `qfax.football` → CNAME to Cloudflare Pages
   - `www.qfax.football` → CNAME to Cloudflare Pages

2. Verify SSL/TLS is set to **Full** or **Full (strict)**

### 4. Redirect Configuration

To redirect `qfax.soccer` to `qfax.football`:

1. In Cloudflare dashboard, go to **Rules** → **Redirect Rules**
2. Create a new redirect rule:
   - **Name**: Redirect qfax.soccer to qfax.football
   - **When incoming requests match**: Custom filter expression
     - `http.host eq "qfax.soccer" or http.host eq "www.qfax.soccer"`
   - **Then**: Dynamic redirect
     - **URL**: `https://www.qfax.football${http.request.uri.path}`
     - **Status code**: 301 (Permanent Redirect)

### 5. Verify Deployment

Once deployed, verify:

- [ ] https://www.qfax.football/ loads landing page
- [ ] https://www.qfax.football/privacy.html loads privacy policy
- [ ] https://www.qfax.football/terms.html loads terms of service
- [ ] MODE7 font renders correctly
- [ ] Mobile responsive design works
- [ ] SSL certificate is valid
- [ ] qfax.soccer redirects to qfax.football (if configured)

## Auto-Deploy on Push

Cloudflare Pages automatically deploys when you push to the `main` branch:

```bash
git add .
git commit -m "Update content"
git push origin main
```

Cloudflare will automatically build and deploy within 1-2 minutes.

## URLs for App Store Submission

Once deployed, use these URLs in App Store Connect and Google Play Console:

- **Privacy Policy**: `https://www.qfax.football/privacy.html`
- **Terms of Service**: `https://www.qfax.football/terms.html`

## Testing Locally

To test the site locally before deploying:

```bash
# Using Python's built-in server
cd /Users/rahulkeerthi/code/qfax-website
python3 -m http.server 8000

# Then open http://localhost:8000 in your browser
```

## Notes

- **No build step required**: Static HTML/CSS/JS
- **Zero cost**: Cloudflare Pages free tier
- **Automatic SSL**: Cloudflare provides free SSL certificates
- **Global CDN**: Content served from edge locations worldwide
- **Instant rollback**: Revert to previous deployment in dashboard if needed
