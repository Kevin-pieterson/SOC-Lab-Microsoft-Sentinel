# 📘 GitHub Repository Setup Tutorial
### How to Create and Upload Your SOC Lab to GitHub

---

## What You Will Create

```
SOC-Lab-Microsoft-Sentinel/          ← Repository name
│
├── README.md                         ← Main project page
├── screenshots/                      ← All your lab screenshots
│   ├── 01-Azure-Portal.png
│   ├── 02-Resource-Group.png
│   ├── 03-Workspace.png
│   ├── 04-Sentinel.png
│   ├── 05-VM-Created___Running.png
│   ├── 06-RDP-Connected.png
│   ├── 07-Agent-Connected.png
│   ├── 08-User-Creation.png
│   ├── 09-Failed-Login-Attempts.png
│   ├── 10-KQL-Failed-Logins_png.PNG
│   ├── 11-Sentinel-Logs-4625.png
│   └── 12-Sentinel-Dashboard.png
├── KQL-queries/                      ← Your KQL detection queries
│   ├── failed-login-detection.kql
│   ├── targeted-user-investigation.kql
│   └── brute-force-summary.kql
└── investigation-notes/              ← Incident report
    └── brute-force-investigation.md
```

---

## PART 1 — Create Your GitHub Account (Skip if you have one)

1. Go to: **https://github.com**
2. Click **Sign up**
3. Enter your email, create a password, choose a username
4. Verify your email
5. Log in

---

## PART 2 — Create the Repository

1. After logging in, look at the **top right corner** — click the **`+`** icon
2. Click **"New repository"**

Fill in the form:

| Field | Value |
|-------|-------|
| Repository name | `SOC-Lab-Microsoft-Sentinel` |
| Description | `Microsoft Sentinel SOC Lab - Brute Force Detection and KQL Investigation` |
| Visibility | ✅ **Public** (so employers can see it) |
| Initialize | ✅ **Add a README file** |

3. Click **"Create repository"** (green button at the bottom)

You now have an empty repository with a basic README.

---

## PART 3 — Upload the README.md File

1. Open your repository page on GitHub
2. Click the **pencil icon** ✏️ on the README.md file (to edit it)
3. **Select all** the text inside (Ctrl+A)
4. **Delete** it all
5. **Paste** the contents of the `README.md` file provided in this repo package
6. Scroll down → Click **"Commit changes"**
7. In the popup, click **"Commit changes"** again

---

## PART 4 — Upload Your Screenshots

This creates the `screenshots/` folder automatically.

1. On your repository homepage, click **"Add file"** → **"Upload files"**

2. Drag and drop ALL your screenshot files:
   - `01-Azure-Portal.png`
   - `02-Resource-Group.png`
   - `03-Workspace.png`
   - `04-Sentinel.png`
   - `05-VM-Created___Running.png`
   - `06-RDP-Connected.png`
   - `07-Agent-Connected.png`
   - `08-User-Creation.png`
   - `09-Failed-Login-Attempts.png`
   - `10-KQL-Failed-Logins_png.PNG`
   - `11-Sentinel-Logs-4625.png`
   - `12-Sentinel-Dashboard.png`

3. **IMPORTANT:** Before committing, change the destination folder.
   - In the file name box at the top, type: `screenshots/01-Azure-Portal.png`
   - GitHub will automatically create the `screenshots/` folder

   **Easier method:** Just upload all files, then in the commit message box type:
   ```
   Add screenshots folder
   ```

4. Click **"Commit changes"**

---

## PART 5 — Create KQL Queries Folder

### File 1: failed-login-detection.kql

1. Click **"Add file"** → **"Create new file"**
2. In the filename box, type exactly:
   ```
   KQL-queries/failed-login-detection.kql
   ```
   (GitHub creates the folder when you type the `/`)

3. Paste the contents of `failed-login-detection.kql`
4. Click **"Commit changes"**

---

### File 2: targeted-user-investigation.kql

1. Click **"Add file"** → **"Create new file"**
2. Filename:
   ```
   KQL-queries/targeted-user-investigation.kql
   ```
3. Paste the contents of `targeted-user-investigation.kql`
4. Click **"Commit changes"**

---

### File 3: brute-force-summary.kql

1. Click **"Add file"** → **"Create new file"**
2. Filename:
   ```
   KQL-queries/brute-force-summary.kql
   ```
3. Paste the contents of `brute-force-summary.kql`
4. Click **"Commit changes"**

---

## PART 6 — Create Investigation Notes Folder

1. Click **"Add file"** → **"Create new file"**
2. Filename:
   ```
   investigation-notes/brute-force-investigation.md
   ```
3. Paste the contents of `brute-force-investigation.md`
4. Click **"Commit changes"**

---

## PART 7 — Verify Your Repository

Your finished repository should look like this on GitHub:

```
📁 SOC-Lab-Microsoft-Sentinel
├── 📄 README.md          ← Shows automatically on homepage
├── 📁 KQL-queries/
│   ├── 📄 brute-force-summary.kql
│   ├── 📄 failed-login-detection.kql
│   └── 📄 targeted-user-investigation.kql
├── 📁 investigation-notes/
│   └── 📄 brute-force-investigation.md
└── 📁 screenshots/
    ├── 🖼️ 01-Azure-Portal.png
    ├── 🖼️ 02-Resource-Group.png
    └── ... (all 12 screenshots)
```

---

## PART 8 — Get Your Repository Link

Your repository URL will be:
```
https://github.com/YOUR-USERNAME/SOC-Lab-Microsoft-Sentinel
```

Replace `YOUR-USERNAME` with your actual GitHub username.

**This is what you put on your CV!**

---

## PART 9 — Pin the Repository to Your Profile

Make your SOC lab the first thing employers see:

1. Go to your GitHub profile: `https://github.com/YOUR-USERNAME`
2. Click **"Customize your profile"** or the **pin icon**
3. Select **"Customize your pins"**
4. Check `SOC-Lab-Microsoft-Sentinel`
5. Click **"Save pins"**

Now it shows prominently on your GitHub profile page.

---

## PART 10 — Add to Your CV

Copy this text into your CV under Projects:

```
Microsoft Sentinel SOC Lab                               Jun 2026
GitHub: github.com/YOUR-USERNAME/SOC-Lab-Microsoft-Sentinel

• Built a cloud-based SOC monitoring environment using Microsoft Azure and Microsoft Sentinel
• Deployed Windows 10 VM (SOC-WIN10) with Azure Monitor Agent for automated log forwarding
• Simulated brute-force login attacks generating 35,000+ Event ID 4625 security events
• Investigated failed login activity using KQL queries in Microsoft Sentinel Log Analytics
• Documented complete detection and incident investigation workflow on GitHub
```

---

## Tips for Success

✅ Make sure the repository is **Public** — private repos can't be seen by employers

✅ Include all 12 screenshots — visual proof is very important

✅ Your README will display automatically on the repo homepage — make it look good

✅ The KQL files show you can actually write detection queries — very valuable

✅ The investigation report shows you understand SOC analyst workflow

---

*Tutorial created for the Microsoft Sentinel SOC Lab project — June 2026*
