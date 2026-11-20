# 🚀 School Management System - Keep-Alive Service

এই service টি আপনার Render server এবং database কে sleep থেকে জাগিয়ে রাখে।

## ⚡ একটি Workflow - সব কিছু!

**একটি মাত্র workflow file** সব কাজ করবে:
- ✅ Server health check
- ✅ Public home verification  
- ✅ Database connection test
- ✅ Multiple schedule offsets (reliability)
- ✅ Aggressive retry logic

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

---

## 🚀 Setup

### ✅ Already Configured!

এই repository তে **একটি unified workflow** আছে:

📁 `.github/workflows/keep-alive.yml`

এই file এ আছে:
- Server health check (5 retries)
- Public home verification (3 retries)
- Database connection test (3 retries)
- Multiple schedules for reliability

### 🎯 এটি কিভাবে কাজ করে:

1. ✅ Repository GitHub এ আছে
2. ✅ Workflow automatically active
3. ✅ প্রতি **~5 মিনিট** এ 3টি আলাদা schedule থেকে ping
4. ✅ GitHub Actions tab এ logs দেখুন
5. ✅ Manual trigger: Actions tab → "Run workflow"

---

## ⏱️ Schedule Details

### 3টি Schedule Patterns:
```yaml
- cron: '*/5 * * * *'           # Every 5 minutes (0,5,10,15...)
- cron: '1,11,21,31,41,51 * * * *'  # Every 10 min starting at 1
- cron: '3,13,23,33,43,53 * * * *'  # Every 10 min starting at 3
```

এই multiple schedules ব্যবহার করার কারণ:
- GitHub Actions এর cron প্রায়ই delay করে
- Multiple offset দিলে কোন না কোন একটা ঠিক সময়ে চলে
- Result: প্রতি 5 মিনিটের মধ্যে অন্তত একটা ping হয়

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
