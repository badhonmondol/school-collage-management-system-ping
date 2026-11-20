# 🚀 School Management System - Keep-Alive Service

এই **public repository** টি আপনার Render server এবং database কে sleep থেকে জাগিয়ে রাখে।

## ⚡ কেন আলাদা Public Repository?

- ✅ Main repository **private** রাখতে পারবেন
- ✅ GitHub Actions **free** পাবেন (public repos এ unlimited)
- ✅ কোন sensitive code বা data expose হবে না
- ✅ শুধুমাত্র server ping এর জন্য dedicated repository

## 🎯 Features

**একটি মাত্র workflow file** সব কাজ করবে:
- ✅ Server health check (5 retry attempts)
- ✅ Public home verification (3 retries)
- ✅ Database connection test (3 retries)
- ✅ প্রতি 6 মিনিটে automatic ping
- ✅ Aggressive retry logic
- ✅ Detailed logging

---

## 🎯 কেন প্রয়োজন?

Render এর free tier এ 15 মিনিট inactivity থাকলে server sleep mode এ চলে যায়। এই service নিয়মিত server এবং database কে ping করে sleep হওয়া থেকে বাঁচায়।

---

## 📦 Features

- ✅ **একটি মাত্র workflow** - সব কিছু organized
- ✅ **3টি schedule offset** - প্রতি 5 মিনিট (0min, 1min, 3min)
- ✅ **Server ping** - Health endpoint + Public home
- ✅ **Database ping** - Connection test প্রতি run এ
- ✅ **5 retry attempts** - Server জাগাতে aggressive
- ✅ **3 retry attempts** - Public home ও database এর জন্য
- ✅ **Never fails** - continue-on-error enabled
- ✅ **Detailed logging** - সব step এর clear output
- ✅ সম্পূর্ণ **Free** (GitHub Actions)

## 🚀 Setup Instructions

### Step 1: GitHub এ Public Repository তৈরি করুন

1. GitHub এ যান: https://github.com/new
2. Repository name: `school-management-keep-alive` (অথবা যেকোনো নাম)
3. **Public** select করুন (এটি important!)
4. README, .gitignore, license - কিছু add করবেন না
5. "Create repository" click করুন

### Step 2: এই Code Push করুন

এই `ping` folder থেকে terminal এ run করুন:

```bash
# ping folder এ যান
cd "D:\management system school\ping"

# Check current status
git status

# Remote add করুন (নিচের URL আপনার repository URL দিয়ে replace করুন)
git remote add origin https://github.com/YOUR-USERNAME/school-management-keep-alive.git

# Push করুন
git branch -M main
git push -u origin main
```

### Step 3: Verify করুন

1. GitHub repository তে যান
2. **Actions** tab এ click করুন
3. "Keep Render Server Alive" workflow দেখতে পাবেন
4. Manual test: "Run workflow" → "Run workflow" button click করুন
5. Workflow logs check করে দেখুন ping successful হচ্ছে কিনা

### ✅ Done! 

এখন **প্রতি 6 মিনিটে** automatically আপনার server ping হবে এবং sleep হবে না।

---

## ⏱️ Schedule Details

```yaml
- cron: '*/6 * * * *'  # Every 6 minutes
```

**কেন 6 মিনিট?**
- Render free tier: 15 মিনিট inactivity এ sleep
- 6 মিনিট interval = নিরাপদ margin
- GitHub Actions এ delay হলেও সমস্যা হবে না

**Timeline Example:**
```
00:00 → Ping ✅
00:06 → Ping ✅
00:12 → Ping ✅
00:18 → Ping ✅
...
```

---

## 📊 What Gets Pinged?

### প্রতি Workflow Run এ:

1. **Server Health** (`/api/health`)
   - 5 retry attempts
   - 15 second delay between retries
   - Server sleep থেকে জাগায়

2. **Public Home** (`/api/public-home`)
   - 3 retry attempts
   - 10 second delay
   - Public endpoints verify

3. **Database Connection** (`/api/auth/check`)
   - 3 retry attempts
   - 10 second delay
   - Database active রাখে

---

## 🔍 Monitoring

### GitHub Actions এ দেখুন:

1. Repository: https://github.com/badhonmondol/school-collage-management-system-ping
2. Click **"Actions"** tab
3. Select **"Keep Render Server Alive"** workflow
4. সব workflow runs দেখুন timestamps সহ

### Sample Log Output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 Starting server ping at Thu Nov 20 15:30:00 UTC 2026
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔄 Attempt 1/5: Pinging /api/health...
✅ SUCCESS! Server is ALIVE (Status: 200)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🏠 Checking public home endpoint...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔄 Attempt 1/3: Pinging /api/public-home...
✅ SUCCESS! Public home is accessible (Status: 200)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🗄️  Checking database connection...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔄 Attempt 1/3: Pinging /api/auth/check...
✅ SUCCESS! Database is responding (Status: 401)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ Keep-alive ping completed!
📅 Time: Thu Nov 20 15:30:25 UTC 2026
🔄 Next ping: ~5 minutes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🛠️ Customization

### Ping Frequency পরিবর্তন করুন:

`keep-alive.yml` file edit করুন:

```yaml
schedule:
  # More frequent (every 3 minutes)
  - cron: '*/3 * * * *'
  
  # Less frequent (every 10 minutes)
  - cron: '*/10 * * * *'
```

### Retry Attempts পরিবর্তন:

```bash
# Server health: 5 retries থেকে change করুন
for i in {1..10}; do  # 10 retries

# Delay পরিবর্তন:
sleep 20  # 20 seconds
```

### আরো Endpoints যোগ করুন:

```yaml
- name: Ping Another Endpoint
  continue-on-error: true
  run: |
    curl -s https://your-url.com/api/endpoint
```

---

## 📝 Troubleshooting

### Workflow দেখাচ্ছে না?
- `.github/workflows/` folder check করুন
- File name: `keep-alive.yml` (exact)
- GitHub এ push করা আছে কিনা verify করুন

### Workflow fail হচ্ছে?
- Actions tab এ logs দেখুন
- Server URL সঠিক আছে কিনা check করুন
- Render dashboard এ server status দেখুন

### Server এখনো sleep করছে?
1. Actions tab এ যান
2. সর্বশেষ workflow run দেখুন
3. Manual trigger করুন: "Run workflow" button
4. Logs check করে দেখুন ping successful হচ্ছে কিনা
5. GitHub Actions এর cron delay হতে পারে - এটি normal

### GitHub Actions Delay সমস্যা?
- GitHub scheduled workflows often delayed (10-30 min)
- আমরা 3টি offset schedule use করছি এই সমস্যা কমাতে
- যদি critical হয় তাহলে UptimeRobot (free) use করতে পারেন

---

## ⚙️ Technical Details

- **Platform:** GitHub Actions
- **Runner:** ubuntu-latest
- **Timeout:** 5 minutes per run
- **Retry Logic:** Yes (with delays)
- **Error Handling:** continue-on-error: true
- **Cost:** $0 (Free for public repos)

---

## 📄 License

MIT

---

## 👨‍💻 Support

কোন সমস্যা হলে GitHub Issues এ জানান।

**Repository:** https://github.com/badhonmondol/school-collage-management-system-ping
