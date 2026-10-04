# Becoming an Engineer - blog

A tiny static blog for becominganengineer.in. No frameworks, no build tools to
install, no server. Just Python 3 turning Markdown posts into plain HTML pages
you can host for free.

## Folder structure

```
becominganengineer/
  build.py          The build script. Reads Markdown, writes the site.
  style.css         The blog stylesheet (copied into docs/ on build).
  landing.html      Your custom homepage. Copied as-is to become index.html.
  about.md          Source for the About page. Edit this to write your bio.
  posts/            Your blog posts, one Markdown file per post.
    hello-world.md  Placeholder first post. Replace or delete it.
  docs/             The finished site. This is what GitHub Pages serves.
    index.html      Homepage (your landing page).
    blog.html       Post listing, newest posts first.
    about.html      About page.
    feed.xml        RSS feed.
    404.html        Shown for missing pages.
    CNAME           Tells GitHub Pages your custom domain.
    style.css
    posts/          One HTML page per post.
  README.md         This file.
```

You only ever edit the Markdown files, `landing.html`, `style.css`, and
`build.py` (rarely). The `docs/` folder is generated. If you ever want a
clean rebuild, just run `python3 build.py docs` again, it wipes `docs/`
and starts fresh.

## Writing and publishing a new post

1. Create a new file in `posts/`, for example
   `posts/my-first-week-with-playwright.md`. Use lowercase letters, numbers,
   and hyphens in the filename. The filename becomes the URL:
   `becominganengineer.in/posts/my-first-week-with-playwright.html`.

2. Start the file with this block (called front matter), then a blank line:

   ```markdown
   ---
   title: My first week with Playwright
   date: 2026-10-05
   description: One or two sentences shown on the home page and in the RSS feed.
   ---

   Your post starts here...
   ```

   - `title` is the post heading.
   - `date` must look like `2026-10-05`. Posts are sorted newest first.
   - `description` is optional but nice. If you skip it, the first paragraph
     is used instead.

3. Write the post in Markdown. Supported formatting:
   - `##` and `###` headings
   - **bold**, *italic*, `inline code`
   - Code blocks: indent with four spaces, or wrap in triple backticks
     (you can name the language, like ` ```python `, for styling hooks)
   - Bulleted lists (`-`) and numbered lists (`1.`)
   - Quotes (`> ...`), links (`[text](url)`), images (`![alt](url)`)
   - A line with `---` becomes a horizontal rule

4. Build the site from this folder:

   ```
   python3 build.py
   ```

5. Preview: open `public/index.html` in your browser. Or, from inside
   `public/`, run `python3 -m http.server` and visit `http://localhost:8000`.

6. Deploy (see below). The usual loop is: write, build, deploy.

To delete the placeholder post, just delete `posts/hello-world.md` and rebuild.

## Deploying for free

### Option A: Cloudflare Pages (recommended)

Free, fast, and you do not need a GitHub account for the simplest path.

1. Create a free account at cloudflare.com.
2. Go to Workers and Pages, then Create, then Upload assets.
3. Drag in the contents of the `public/` folder and deploy. You will get a
   `your-project.pages.dev` address.
4. If you prefer git: push this folder to GitHub, then in Pages choose
   "Connect to Git". Set the build command to `python3 build.py` and the
   build output directory to `public`.

### Option B: GitHub Pages

1. Create a free GitHub account and push this folder to a repository.
2. Build into a folder GitHub Pages understands:
   `python3 build.py docs` (GitHub Pages can only serve from the repo root
   or a `/docs` folder).
3. Commit and push the `docs/` folder.
4. In the repo, go to Settings, then Pages. Under "Build and deployment",
   choose "Deploy from a branch", pick your branch, and pick the `/docs`
   folder. Save.
5. Your site appears at `your-username.github.io/your-repo`.

## Connecting the becominganengineer.in domain

Your domain is registered at Hostinger, so its DNS records live in your
Hostinger control panel (Domains, then DNS / Nameservers). DNS changes can
take a few minutes to a few hours to take effect.

### With Cloudflare Pages (recommended path)

1. In Cloudflare, add becominganengineer.in as a site (free plan is fine).
   Cloudflare will give you two nameservers.
2. In Hostinger, change the domain's nameservers to Cloudflare's two.
   Wait until Cloudflare shows the domain as active.
3. In your Pages project, go to Custom domains and add
   `becominganengineer.in` (and `www.becominganengineer.in` if you want it).
   Cloudflare wires up the DNS records and the SSL certificate automatically.
   Nothing else to do.

If you would rather keep Hostinger's DNS: in Hostinger's DNS zone editor,
add a CNAME record for `www` pointing to `your-project.pages.dev`. The bare
domain works most smoothly with Cloudflare's DNS, so moving the nameservers
as above is the simpler path.

### With GitHub Pages

1. In the repo's Settings, then Pages, enter `becominganengineer.in` as the
   custom domain and save. (The `CNAME` file in the built site already
   contains the domain, which does the same thing.)
2. In Hostinger's DNS zone editor, add these records:
   - Four A records for `@` pointing to GitHub Pages:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
     `185.199.111.153`
   - One CNAME record for `www` pointing to `your-username.github.io`
3. Back in the Pages settings, tick "Enforce HTTPS" once the certificate is
   ready.

## Notes

- The RSS feed lives at `/feed.xml` and updates itself on every build.
  Readers can subscribe with any RSS reader app.
- Keep post filenames URL friendly: lowercase, hyphens instead of spaces.
- If the build complains about a missing date, check the front matter block
  at the top of that post. It must start and end with a line of exactly
  three dashes.
