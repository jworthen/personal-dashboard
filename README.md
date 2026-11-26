# My Day dashboard: setup guide

One file (`index.html`), hosted free on GitHub Pages. No Vercel, no server, nothing to install.

**Time needed:** about 25 minutes, once. Do it on your laptop.

You can try the dashboard before any setup: double-click `index.html` and it opens with sample data. Everything works except the Google connection, which only works once the page is on GitHub Pages.

---

## Part 1. Put it on GitHub Pages (5 minutes)

1. Go to github.com, click **+** (top right), then **New repository**.
2. Name it `dashboard`. Choose **Public** (free GitHub Pages needs a public repo; see "Is a public repo safe?" below). Click **Create repository**.
3. On the new repo page, click **uploading an existing file**. Drag in `index.html` and this `README.md`. Click **Commit changes**.
4. Go to **Settings**, then **Pages** (left sidebar). Under "Build and deployment", set Source to **Deploy from a branch**, Branch to **main** and folder to **/ (root)**. Click **Save**.
5. Wait a minute or two, then open `https://YOUR-USERNAME.github.io/dashboard/`. Your TaskScheduler lives at `jworthen.github.io`, so this should be `https://jworthen.github.io/dashboard/`. You should see the dashboard with sample data.

## Part 2. Create your Google sign-in ID (15 minutes)

This tells Google that your dashboard is allowed to ask you to sign in. Google reorganizes this console from time to time, so a menu name may differ slightly from what is written here.

1. Go to **console.cloud.google.com** and sign in with your **personal** Google account (the one that shows all three calendars).
2. At the top, click the project picker, then **New project**. Name it `My Day Dashboard`. Click **Create**, then make sure it is selected.
3. Turn on three services. Open **APIs & Services**, then **Library**. Search for each of these, open it, and click **Enable**:
   - **Google Calendar API**
   - **Google Sheets API**
   - **Google Drive API**
4. Set up the sign-in screen. Open **APIs & Services**, then **OAuth consent screen** (it may be called **Google Auth Platform**). Click **Get started**.
   - App name: `My Day`
   - Support email: your email
   - Audience: **External**
   - Contact email: your email
   - Finish and create.
5. Still in that area, open **Audience**. Under **Test users**, click **Add users** and add your own Gmail address. Leave the app in **Testing**. This means only you can ever sign in.
6. Open **Clients** (or **Credentials**), then **Create client** (or **Create credentials**, then **OAuth client ID**).
   - Application type: **Web application**
   - Name: `Dashboard`
   - Under **Authorized JavaScript origins**, click **Add URI** and enter `https://jworthen.github.io` (your GitHub Pages address with no slash and no `/dashboard` on the end).
   - Click **Create**.
7. Copy the **Client ID**. It looks like `1234567890-abc123.apps.googleusercontent.com`. You do not need the "client secret".

## Part 3. Add the Client ID to the dashboard (3 minutes)

1. In your GitHub repo, click `index.html`, then the **pencil** icon (Edit).
2. Near the top, find this line:
   ```
   const CLIENT_ID = '';
   ```
3. Paste your Client ID between the quotes:
   ```
   const CLIENT_ID = '1234567890-abc123.apps.googleusercontent.com';
   ```
4. Click **Commit changes**. Wait a minute, then reload your dashboard page.

## Part 4. Connect

1. On the dashboard, click **Connect Google**.
2. Pick your personal account. You will see a "Google hasn't verified this app" screen. That is expected for a private app that only you use. Click **Continue** (you may need **Advanced** first).
3. Google shows two permissions. **Tick both boxes**:
   - *View your calendars* (view-only; the dashboard cannot add, change, or delete events)
   - *See, edit, create, and delete only the specific Google Drive files you use with this app* (this is the one spreadsheet the dashboard creates)
4. The dashboard creates a spreadsheet called **Daily Dashboard Data** in your Google Drive and loads your calendars.
5. Open **Settings** in the dashboard and, under **Calendars to show**, tick your personal calendar, Claire's, and SBQ. Untick anything else (holidays, birthdays).

## Part 5. Put it on your Pixel

1. Open `https://jworthen.github.io/dashboard/` in Chrome on your phone.
2. Tap the three-dot menu, then **Add to Home screen**.
3. Open it from the home screen and tap **Connect Google**. It finds the same spreadsheet, so your tasks and trackers match your laptop.

---

## Good to know

**Staying connected.** Google only hands out one-hour passes to browser-only apps, so the dashboard renews its pass by itself a few minutes before it runs out, and whenever you open the page. For that to work, allow pop-ups for your dashboard address once on each device (in Chrome: the pop-up blocked icon in the address bar, then "Always allow"; on Android: Chrome menu, Settings, Site settings, Pop-ups and redirects). You may see a small Google window flash open and shut about once an hour. If pop-ups are blocked, a yellow bar appears and your next tap reconnects instead. Anything you change while disconnected is kept on the device and saved once it reconnects.

**Refreshing.** The dashboard reloads when you open it, every 5 minutes while it is open, and whenever you tap a Refresh button. Changes you make save within a second or so.

**Using two devices.** Changes from one device appear on the other at its next refresh. Avoid editing the same list on both within the same few minutes; the later save wins.

**Editing the spreadsheet directly.** You can open it from Settings, then "Open my data sheet". To bulk-add tasks, add rows to the **Tasks** tab: fill in `title`, `category`, and optionally `due` (as `2026-11-30`), and leave `id` blank. The dashboard fills in the rest at its next refresh. Do not rename the tabs or the header row.

**Is a public repo safe?** Yes. The file contains no events, tasks, or notes. Those stay in your Google account. The Client ID is not a password: it only works from your GitHub Pages address, and only for the test user you listed (you).

**Updating the dashboard later.** Replace `index.html` in the repo (Add file, then Upload files), and keep your `CLIENT_ID` line. On your phone, pull down to refresh once.

**Starting over.** Settings, then "Disconnect on this device" returns that device to sample data. Your spreadsheet is untouched.

## If something goes wrong

| What you see | What to do |
|---|---|
| "Error 400: origin_mismatch" or "redirect_uri_mismatch" | In Part 2 step 6, the origin must be exactly `https://jworthen.github.io` with no trailing slash or path. Changes can take a few minutes. |
| "Access blocked" or "Error 403: access_denied" | Add your Gmail address as a test user (Part 2 step 5). |
| "Sign-in did not finish: the popup was blocked" | Allow popups for your dashboard address, then tap Connect again. |
| A message that an API "has not been used" or "is disabled" | Enable the API it names (Part 2 step 3), wait a minute, then tap Refresh all. |
| "Both boxes need to be ticked" | Tap Connect again and tick both permissions on the Google screen. |
| Connect Google does nothing when opening the file from your computer | Google sign-in only works from the GitHub Pages address, not a file on disk. |
