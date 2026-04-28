# Cloud Sync with Warp Drive

Sync your Warp settings across all your devices.

---

## What Is Cloud Sync?

Warp Drive is cloud synchronization that keeps your settings, history, and profiles in sync across all your devices.

**Without Cloud Sync:** Settings are local to each machine.  
**With Cloud Sync:** Change a setting on your laptop, and it's immediately available on your desktop.

---

## What Syncs?

✅ **Synced:**
- Settings (themes, keyboard shortcuts, shell preferences)
- AI Model selection (which model you prefer)
- Execution profiles (Agent Mode permissions)
- Workspace preferences
- Cloud history (shared command history)

❌ **Not Synced:**
- Local command history
- Secrets and API keys (encrypted separately)
- Personal workspace data

---

## Setting Up Cloud Sync

### Step 1: Create or Sign In to Account

When you first launch Warp:

1. **Click "Sign In"** in the welcome screen
2. **Choose sign-in method:**
   - Email
   - GitHub
   - Google
3. **Verify your email** (if using email signup)

### Step 2: Enable Cloud Sync

After signing in:

1. **Open Settings** (⚙️)
2. **Go to "Cloud"** or **"Sync"**
3. **Toggle "Cloud Sync"** to enabled
4. **Save**

That's it! Your settings are now synced.

### Step 3: On Other Devices

Repeat steps 1-2 on your other machines and use the **same account**.

Your settings will automatically sync.

---

## Managing Cloud Sync

### View Sync Status

1. **Open Settings** (⚙️)
2. **Go to "Cloud"** or **"Sync"**
3. **See last sync time** and status

### Manually Trigger Sync

Usually automatic, but you can force sync:

1. **Open Settings** (⚙️)
2. **Go to "Cloud"**
3. **Click "Sync Now"** button

### Pause Sync

Temporarily stop syncing (keep local, but don't upload):

1. **Settings → Cloud**
2. **Toggle Cloud Sync** off
3. **Changes stay local** until you re-enable

### Choose What Syncs

Some settings can be synced or kept local:

1. **Settings → Cloud → Advanced**
2. **Toggle specific items:**
   - Sync settings
   - Sync history
   - Sync profiles
3. **Save**

---

## Privacy and Security

### Your Data Is Encrypted

- **In transit:** TLS/HTTPS
- **At rest:** AES-256 encryption
- **Keys:** Your account credentials secure the keys

### What Warp Can See

Warp team can:
- ✅ Know you have a Warp account
- ✅ See metadata (sync timestamps)
- ❌ Cannot read your settings or history (encrypted)
- ❌ Cannot access your API keys

### Deleting Your Data

To delete all synced data:

1. **Settings → Cloud**
2. **Click "Delete All Synced Data"**
3. **Confirm** — Data is permanently deleted

---

## Workspaces and Teams

### Personal Workspace

By default, your account has a "Personal" workspace:
- Only you can access
- Settings are synced to your devices

### Team Workspace (Enterprise)

If using Warp at work:

1. **Admin creates workspace**
2. **You're invited to join**
3. **Workspace settings** are shared with your team
4. **Personal settings** override workspace defaults

---

## Troubleshooting

### "Cloud Sync not syncing"

1. **Check internet connection** — You need internet for sync
2. **Check you're signed in** — Go to Settings → Cloud
3. **Force sync** — Click "Sync Now" button
4. **Restart Warp** — Sometimes solves sync issues

### "Sync keeps reverting my changes"

Multiple devices syncing at once can cause conflicts:

1. **Wait a few seconds** — Let sync complete on all devices
2. **Manually resolve** — Check which version you prefer
3. **Keep local** — Disable sync if you want device-specific settings

### "Can't sign in"

1. **Check internet** — Sign-in requires internet
2. **Verify email** — You may need to verify your email first
3. **Reset password** — If you forgot password, use "Forgot Password"
4. **Try different method** — Email, GitHub, or Google

### "Settings disappeared after sync"

This might be a sync conflict:

1. **Check other devices** — Do you have the settings elsewhere?
2. **Undo recent changes** — Settings may have reverted
3. **Check sync status** — Go to Settings → Cloud

---

## Best Practices

### 1. Keep Settings Consistent

Once you sync, your settings are everywhere. Be intentional about changes:

- Change once, takes effect everywhere
- Good: Update theme globally
- Bad: Change shell on one device only (will sync back)

### 2. Manage Multiple Accounts

If you have work and personal Warp accounts:

- **Option 1:** Stick to one account, use different workspaces
- **Option 2:** Keep separate installations (different users)
- **Don't:** Switch accounts frequently (confusing)

### 3. Backup Important Settings

Though Warp backs up to cloud, also consider:

- Exporting your settings as JSON (if supported)
- Taking notes of custom keyboard shortcuts
- Keeping a config file in version control

---

## Advanced: Workspace Admin

If you're a workspace admin:

### Create a Workspace

1. **Sign in at warp.dev**
2. **Go to Workspace Settings**
3. **Click "New Workspace"**
4. **Configure defaults** for your team

### Enforce Workspace Settings

Admin settings override user settings:

- **Default theme**
- **Required AI model**
- **Minimum permissions**
- **Shared history/profiles**

Users can override most settings locally.

### Invite Team Members

1. **Go to Workspace → Members**
2. **Click "Invite"**
3. **Enter email addresses**
4. **They receive invite link**
5. **They join your workspace**

---

## Syncing Different Machine Types

### Mac ↔ Linux ↔ Windows

Cloud Sync works across platforms:

- **Theme** — Syncs (UI adapts to platform)
- **Shortcuts** — Mostly sync (some are platform-specific)
- **Shell** — **Doesn't fully sync** (each OS has different shells)
- **History** — Syncs

### Developing on Multiple Machines

Example setup:

```
Laptop (macOS) ← Cloud Sync → Desktop (Linux)
     ↓
  Settings:
  - Theme: Dark
  - AI Model: Claude 3.5 Sonnet
  - Shell: zsh (on Mac), bash (on Linux)
  ↓
Both machines stay in sync!
```

---

## Migration: Moving to a New Device

1. **Install Warp** on your new device
2. **Sign in** with your account
3. **Wait for sync** — All settings download automatically
4. **Done!** — Your new device now matches your old one

No manual file copying needed.

---

## Next Steps

- **[Quick Start](../QUICK_START.md)** — Getting started
- **[Agent Mode](./AGENT_MODE.md)** — Autonomous tasks
- **[Terminal AI](./TERMINAL_AI.md)** — Command help
- **[Back to Features](./README.md)**
