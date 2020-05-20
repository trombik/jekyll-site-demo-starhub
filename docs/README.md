# Demo site for static site on AWS S3

## Requirements

- AWS account
- GitHub account
- Optionally, a domain name

## Architecture overview

GitHub Actions builds jekyll site in its build environment when a condition
meets pre-defined criteria. The site content, then, transferred to S3.

An S3 bucket is configured so that it serves the contents to public.

The deployment is completely automated.

### Pros

- You can use ruby jekyll plugins in the build process, which `gh-pages` does
  not allow
- You do not need EC2 instance to serve your content (no administration cost)
- S3 is far cheaper than EC2 instance (see [the pricing](https://aws.amazon.com/s3/pricing/))
- S3 scales automatically (more than you probably need)

### Cons

- No dynamic content (JavaScript is only your friend)
- No database (all you have is static files)
- No programming language is available (no ruby, no nothing)

## Implementation

The GitHub repository keeps all the contents and Jekyll installation. Nothing
new here.

A GitHub Workflow, `deploy-s3.yml`, is defined under
[.github/workflows](https://github.com/trombik/jekyll-site-demo-starhub/blob/master/.github/workflows/deploy-s3.yml).
This action is triggered when something is pushed (or merged) to `devel`
branch.  When triggered, the workflow does:

- checkout the branch
- set necessary AWS credential in the build environment
- install ruby in the build environment
- install gems using `bundler`
- build the jekyll site
- copy the site contents under `_site` to specified AWS S3 bucket

The S3 bucket is configured (manually, not included in the repository) so that
the bucket does:

- allows anyone to access to files in the bucket
- serve files as if it is a web server

By default, S3 creates a unique domain name for buckets, which means you do
not need a dedicated domain name for the site.

If you want to use your own domain name for the site, you can do so by using
[Route 53](https://aws.amazon.com/route53/) (additional configrations are
required).

If you want to restrcit access to a cetain group of people, you can do so by
using
[Lambda@Edge](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-at-the-edge.html)
(requires additional cost).
[serverless_static_website_with_basic_auth](https://github.com/dumrauf/serverless_static_website_with_basic_auth)
is an implementation. Depending on your requirements, you also need TLS
protection.
