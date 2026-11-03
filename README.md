# StudioSpace · Start here

StudioSpace helps a creator rent a production room by the hour. A host can publish a space, review incoming bookings and manage the studio’s calendar. The first directory contains six fictional spaces in Bangladesh with locally bundled photographs, USD rates and Bangladesh-local booking hours.

## What to open

| Item | What it contains |
| --- | --- |
| client/ | Next.js pages, UI components, styles, images and the Netlify setup |
| server/ | Editable Express API, Better Auth, MongoDB queries, seed accounts and tests |
| design/StudioSpace-UI.svg | Six editable design plates covering the desktop homepage, mobile homepage, dark directory, listing, booking dialog and brand. Open the SVG in a browser and zoom in, or import it into a vector editor |
| DEPLOYMENT.md | Step-by-step instructions for pushing the repositories and deploying the complete application on Netlify |
| history/ | Git bundles that restore the separate client and server commit histories |
| client/README.md | The examiner-facing overview, login credentials and live URL field |
| server/README.md | API endpoints, booking rules and how to release a server change |

## What the three submission links mean

| Submission field | What the examiner opens | Example format only |
| --- | --- | --- |
| Client-side GitHub link | Pages, forms, styles, images and the client commit log | github.com/YOUR-NAME/studiospace-client |
| Server-side GitHub link | Authentication, database access, booking rules and the server commit log | github.com/YOUR-NAME/studiospace-server |
| Live website link | The deployed application a visitor uses | YOUR-SITE.netlify.app |

বাংলায় বললে, প্রথম দুইটি লিংক দিয়ে পরীক্ষক কোড ও কমিট দেখবেন। তৃতীয় লিংক দিয়ে তিনি ওয়েবসাইটে ঢুকে লগইন ও বুকিং পরীক্ষা করবেন। এখানে পুরো ওয়েবসাইটের জন্য একটি Netlify প্রজেক্টই যথেষ্ট।

## How the project fits together

| Step | Start reading here | What happens |
| --- | --- | --- |
| 1. Design the directory | design/StudioSpace-UI.svg, client/app/globals.css, client/app/catalog.css, client/app/desk.css | The Studio Index pairs a full-width campaign photograph with Manrope and italic Newsreader headings. Cobalt marks search actions and the active route. |
| 2. Find a space | client/components/Explore.jsx, server/src/studios.js | A category click fills the URL query. Express filters by category and text. The home page uses a separate sorted query capped with limit(6). |
| 3. Identify the member | client/components/AuthForm.jsx, client/components/Session.jsx, server/src/auth.js | Better Auth checks the email/password or Google login. On refresh, private pages wait for the session. Protected API calls also need a short-lived signed HttpOnly JWT. |
| 4. Publish a space | client/components/StudioForm.jsx, server/src/validation.js | A host submits the listing and gallery. Express validates it and attaches the owner ID from the active session. The browser cannot choose who owns a studio. |
| 5. Request the hours | client/components/BookingPanel.jsx, server/src/bookings.js | The modal shows local hours, rental and deposit. The server reads the current rate and checks overlapping active bookings inside a MongoDB transaction before writing Pending. |
| 6. Manage the booking | client/components/Reservations.jsx, server/src/bookings.js | The creator can cancel a pending request. The host can confirm it and mark a finished session complete. The desk refreshes every 20 seconds while visible and when you return to the tab. |
| 7. Deliver the website | client/netlify.toml, client/netlify/functions/studio.mjs | Netlify serves Next.js and routes /api/* to Express under the same domain. The API connects to Atlas. |

**Example.** A creator requests Northlight Loft from 10:00 to 12:00 at $42/hr. The rental is $84 and the refundable deposit is $50. The displayed total is $134. A second request from 11:00 to 13:00 receives a conflict. A request starting at 12:00 can proceed. No payment is collected.

## Run the project locally

Use Node 22.13 or newer, npm and two terminal windows. Use Docker Desktop with Compose for the included replica set, or connect to Atlas.

**Terminal A — server**

```sh
cd server
npm ci
npm run setup
docker compose up -d --wait
npm run seed
npm run dev
```

If you use Atlas, skip the Docker command. Edit server/.env with your Atlas URI, database name and allowed network access before seeding. The setup script generates separate session and JWT secrets and preserves an existing .env. Local signup, email login, bookings and URL-based listing photos work with just the database and these secrets. Add Google and ImgBB credentials for their respective buttons.

**Terminal B — client**

```sh
cd client
npm ci
npm run dev
```

Open [the local website](http://localhost:3000). On Windows, Git Bash or PowerShell can run these npm and Docker commands. The local API listens on port 4000. Use `npm run build` in client/ to check the production build.

## Publish and submit

Follow [DEPLOYMENT.md](DEPLOYMENT.md) for the Git commands, Netlify settings, Atlas connection, Google callback, ImgBB key and final live URL. The first deployment and the two public repository addresses belong to your accounts.

## Examiner walkthrough

| Check | How to verify |
| --- | --- |
| Guest access | Open /my-bookings in a fresh browser profile. Log in as the creator and confirm it returns to that route. Reload it while signed in. |
| Directory | Choose Podcast rooms on Home. Explore should show only that category. Use a nonsense search to see the clear-filters action. |
| Studio CRUD | Sign in as the host. Add a studio, upload an image, edit the rate and delete the test listing after confirming the modal. |
| Booking | Use the creator account to reserve a future slot. In another browser profile use the host dashboard to confirm. Check the status in My bookings. |
| Collision | Have two creators request overlapping hours. One holds the slot. Adjacent hours remain bookable. |
| Release and history | Cancel a separate Pending booking and check the slot opens again. An active booking prevents listing deletion. |
| Appearance and routes | Switch theme and reload. Check the menu on a phone, open a studio URL directly and visit an unknown path for the custom 404. |
| Provider setup | Complete one Google sign-in and one ImgBB upload on the live domain. Confirm the gallery URL opens after refresh. |

The seeded creator login is **creator@studiospace.example / StudioGuest42**. The seeded host login is **host@studiospace.example / StudioHost42**. These are intentionally shared examiner accounts for a demonstration database.

## Verification and references

The production Next.js build and server unit checks passed during packaging. A disposable MongoDB replica set exercised email signup/login, ownership checks, unchanged history after listing deletion, competing bookings, confirmed conflicts, adjacent slots, guest cancellation and revoked sessions. The packaged Netlify function was also executed locally against MongoDB. Live Google OAuth, ImgBB and the public Netlify deployment require credentials from your accounts.

The layout, copy, sample listings and SVG design board were made for this brief. The campaign portrait and studio interiors are generated images of fictional locations. See client/ASSETS.md for the image credits and both font licences. Peerspace’s activity-based directory informed the browsing structure. The code uses the framework and provider interfaces documented below.

- [Directory design reference](https://www.peerspace.com/)
- [Next.js on Netlify](https://docs.netlify.com/build/frameworks/framework-setup-guides/nextjs/overview/)
- [Express on Netlify](https://docs.netlify.com/build/frameworks/framework-setup-guides/express/)
- [Better Auth Express integration](https://better-auth.com/docs/integrations/express)
- [Better Auth MongoDB adapter](https://better-auth.com/docs/adapters/mongo)
- [Better Auth Google provider](https://better-auth.com/docs/authentication/google)
- [ImgBB upload API](https://api.imgbb.com/)
