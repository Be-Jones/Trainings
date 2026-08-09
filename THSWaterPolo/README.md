# THS Water Polo Hub

A public, mobile-first reference hub for Tomball High School water polo families. The site publishes reviewed announcements and durable logistics while keeping the coach-maintained Google Doc as the official schedule.

## Configure before launch

1. Edit `src/data/site.ts` and add the public Google Doc, Google Form, and Instagram URLs.
2. Replace `YOUR-GITHUB-USERNAME` in `astro.config.mjs` with the GitHub account or organization that owns the repository.
3. Confirm the schedule and form are accessible without signing into a private school account.
4. Confirm approval to use the THS/Tomball name and any future school branding.

## Content workflow

1. A coach or team representative submits a change with its deadline, urgency, and original source.
2. The site curator verifies it against the official message or source.
3. Copy `src/content/announcements/announcement-template.md`, rename it, fill in the frontmatter, and set `draft: false`.
4. Commit to `main`. GitHub Actions builds and deploys the update.
5. Announcements with an `expiresAt` date automatically move from the current feed to the archive.

Public content must not include student phone numbers, roster data, private contacts, private documents, or live locations.

## Resource links

Resource entries live in `src/content/resources/`. Add a verified `url` to the frontmatter when a public link is available. Missing links appear as `Link pending` instead of sending families to an unverified destination.

## Commands

| Command | Action |
| --- | --- |
| `npm install` | Install dependencies |
| `npm run dev` | Start the local site at `http://localhost:4321/THSWaterPolo/` |
| `npm run build` | Build the production site in `dist/` |
| `npm run preview` | Preview the production build |

## Deploy

Create a public GitHub repository named `THSWaterPolo`, push this project to its `main` branch, then choose **GitHub Actions** as the Pages source under **Settings > Pages**. The deployment workflow publishes to:

`https://<GitHub-username>.github.io/THSWaterPolo/`
