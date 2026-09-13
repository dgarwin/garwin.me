# garwin.me

A personal site styled like it is 1997. No framework, no JavaScript, no images,
nothing to compile.

- `index.html` — the site. 1997 styling on a modern layout: the look is
  deliberate (black ground, Impact orange, cyan links, yellow header bands,
  outset borders, a starfield of asterisks) but the layout underneath is grid,
  flexbox, and a media query, so it stacks on a phone. Keep the styling; the
  layout is fair game.
- `plain/index.html` — the earlier minimal version of the same content, kept at
  `/plain/` and linked from the top bar.
- `infra/site.yml` — CloudFormation for the hosting (S3 + CloudFront + the
  deploy role).
- `.github/workflows/deploy.yml` — publishes on every push to `master`.

## Viewing it locally

Open `index.html` in a browser, or serve the directory:

```sh
python3 -m http.server
```

Then visit http://localhost:8000. Note that `/plain/` works locally because
Python's server resolves directory indexes; in production a CloudFront function
does that job.

## Hosting

S3 holds the files, CloudFront serves them over HTTPS. The bucket is private —
CloudFront reaches it through Origin Access Control, so the only way in is
through the distribution.

### What it costs

| | |
|---|---|
| S3 storage and requests | fractions of a cent at this size |
| CloudFront | **$0** — the always-free tier is 1 TB/month out and 10M requests, and it does not expire |
| S3 → CloudFront transfer | $0, free for any AWS origin |
| ACM certificate | $0 |
| CloudFront function | $0 — free to 2M invocations/month |
| Invalidations | $0 — first 1,000 paths/month free, and `/*` counts as one |
| Route 53 hosted zone | $0.50/month, *only if* DNS moves to Route 53. Alias queries to CloudFront are free. |

So: free if DNS stays elsewhere, about $0.50/month if it moves to Route 53.

### First-time setup

1. **Request a certificate** in ACM, **in us-east-1** (CloudFront only accepts
   certificates from that region), covering `garwin.me` and `www.garwin.me`.
   Validate it by adding the CNAME records ACM gives you to whichever DNS
   provider you are using. Free.

2. **Deploy the stack** in us-east-1:

   ```sh
   aws cloudformation deploy \
     --region us-east-1 \
     --template-file infra/site.yml \
     --stack-name garwin-me \
     --capabilities CAPABILITY_IAM \
     --parameter-overrides \
       AcmCertificateArn=arn:aws:acm:us-east-1:<account>:certificate/<id> \
       HostedZoneId=<zone id, or omit>
   ```

   Pass `CreateGitHubOidcProvider=no` if the account already has the
   `token.actions.githubusercontent.com` provider — an account can only have
   one.

3. **Wire up the deploy.** The stack outputs `BucketName`, `DistributionId`,
   and `DeployRoleArn`. Add them as repository *variables* (Settings → Secrets
   and variables → Actions → Variables) named `SITE_BUCKET`,
   `CLOUDFRONT_DISTRIBUTION_ID`, and `AWS_DEPLOY_ROLE_ARN`. No secrets needed:
   the workflow assumes the role via OIDC, so there are no access keys to
   store or rotate.

4. **Point DNS at it.** See below — this is the one step with a decision in it.

5. **Push, or run the workflow manually** from the Actions tab to publish.

### DNS, and why the apex is awkward

CloudFront has no fixed IP addresses, so `garwin.me` (an apex domain) cannot be
a `CNAME`. That leaves two options:

- **Route 53** ($0.50/month): alias records handle the apex natively. Set
  `HostedZoneId` and the stack creates the records. **This means moving the
  domain's nameservers, so every mail record has to be recreated in Route 53 —
  including the Mailgun MX records that Squarespace's email forwarding depends
  on. Expect to re-do email forwarding (ImprovMX and similar are free) before
  cutting over.**
- **Keep DNS where it is** and serve the site from `www.garwin.me` with a
  `CNAME` to the distribution domain, leaving the apex to the registrar's
  forwarding. Free, and it does not touch any mail record — but registrar
  forwarding often cannot present a valid certificate for `https://garwin.me`.

### Deleting the stack

The bucket is set to `Retain`, so tearing down the stack leaves the site's
files intact. Empty and remove the bucket by hand if that is what you want.
