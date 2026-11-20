# 🚀 School Management System - Keep-Alive Service

এই service টি আপনার Render server এবং database কে sleep থেকে জাগিয়ে রাখে।

## 🎯 কেন প্রয়োজন?

Render এর free tier এ 15 মিনিট inactivity থাকলে server sleep mode এ চলে যায়। এই service নিয়মিত server এবং database কে ping করে sleep হওয়া থেকে বাঁচায়।

## 📦 Features

### Server Ping (প্রতি 5 মিনিট):
- ✅ Health endpoint check
- ✅ Public home verification
- ✅ **3 বার retry** করে প্রতি endpoint এ
- ✅ **30 second timeout** per request
- ✅ **Never fails** - continue-on-error enabled

### Database Ping (দিনে 3 বার):
- ✅ সকাল 9:00 AM (Bangladesh Time)
- ✅ দুপুর 2:00 PM (Bangladesh Time)
- ✅ রাত 9:00 PM (Bangladesh Time)
- ✅ **5 বার retry** with 15-second delays
- ✅ Query capability test
- ✅ সম্পূর্ণ Free (GitHub Actions)
- ✅ Automatic operation

---

## 🚀 Setup - GitHub Actions (Recommended)

এটি **সবচেয়ে ভালো এবং সম্পূর্ণ FREE** solution!

### Step 1: Workflow Files Setup

এই folder এ **2টি workflow file** আছে:

1. **`.github-workflows-keep-alive.yml`** - Server ping (প্রতি 5 মিনিট)
2. **`.github-workflows-database-ping.yml`** - Database ping (দিনে 3 বার)

এই দুটি file project এর `.github/workflows/` folder এ copy করুন:

**Option A: Command Line (সহজ)**
```bash
# ping folder থেকে
cp .github-workflows-keep-alive.yml ../.github/workflows/keep-alive.yml
cp .github-workflows-database-ping.yml ../.github/workflows/database-ping.yml
```

**Option B: Manually**
1. `.github/workflows/` folder এ যান (না থাকলে তৈরি করুন)
2. দুটি file তৈরি করুন:
   - `keep-alive.yml` - Server ping এর জন্য
   - `database-ping.yml` - Database ping এর জন্য
3. সংশ্লিষ্ট `.github-workflows-*.yml` files এর content copy করে paste করুন

### Step 2: GitHub এ Push করুন

```bash
git add .
git commit -m "Add keep-alive GitHub Action"
git push
```

### Step 3: Verify করুন

1. GitHub repository তে যান
2. **"Actions"** tab এ click করুন
3. **2টি workflow** দেখতে পাবেন:
   - **"Keep Server Alive"** - প্রতি 5 মিনিটে run হয়
   - **"Database Keep Alive"** - দিনে 3 বার run হয়

### Step 4: Manual Test (Optional)

Actions tab থেকে:
1. "Keep Server Alive" workflow select করুন
2. "Run workflow" button এ click করুন
3. কিছু সেকেন্ডের মধ্যে result দেখতে পাবেন

---

## ✨ Benefits of GitHub Actions

| Feature | GitHub Actions | Other Services |
|---------|---------------|----------------|
| **Cost** | 100% Free | Free tier limited |
| **Setup** | 1-time only | May need account |
| **Maintenance** | Zero | May expire |
| **Reliability** | GitHub infrastructure | Varies |
| **Customization** | Full control | Limited |

---

## 📊 What Gets Pinged?

### Server Workflow (Every 5 minutes):
1. **Health Endpoint:** `/api/health`
   - Server status check
   - 3 retry attempts
   
2. **Public Home:** `/api/public-home`
   - Public endpoints check
   - 3 retry attempts

### Database Workflow (3 times daily):
1. **Auth Check:** `/api/auth/check`
   - Database connection verify
   - 5 retry attempts with 15s delay
   
2. **Students Query:** `/api/students`
   - Database query capability test
   - Ensures database is responsive

---

## ⏱️ Schedule

### Server Ping:
- **Frequency:** Every 5 minutes
- **Total pings per day:** ~288 pings
- **What it pings:**
  - Health endpoint (`/api/health`)
  - Public home (`/api/public-home`)

### Database Ping:
- **Frequency:** 3 times daily
- **Times (Bangladesh):**
  - 9:00 AM
  - 2:00 PM
  - 9:00 PM
- **What it pings:**
  - Auth check (`/api/auth/check`)
  - Students query (`/api/students`)

### Common Features:
- **Retry Logic:** Multiple attempts with delays
- **Timeout:** 30 seconds per request
- **Error Handling:** `continue-on-error: true` - workflows কখনো fail হবে না

---

## 🔍 Monitoring

### View Workflow Runs:
1. Go to your GitHub repository
2. Click **Actions** tab
3. See all workflow runs with status
4. Click on any run to see detailed logs

### Sample Log Output:

**Server Ping (Every 5 min):**
```
🚀 Pinging server at Thu Nov 20 15:30:00 UTC 2026
✅ Server is alive (Status: 200)
🏠 Checking public home...
✅ Public home is accessible (Status: 200)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ Server ping completed at Thu Nov 20 15:30:05 UTC 2026
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Database Ping (3 times daily):**
```
🗄️  Checking database connection at Thu Nov 20 03:00:00 UTC 2026
✅ Database is responding (Status: 401)
📊 Testing database query capability...
✅ Database queries working (Status: 401)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ Database ping completed at Thu Nov 20 03:00:10 UTC 2026
   Next ping: 8 hours from now
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🛠️ Customization

### Server Ping Frequency পরিবর্তন:

`keep-alive.yml` file এ:
```yaml
schedule:
  - cron: '*/3 * * * *'  # 3 মিনিট
  - cron: '*/10 * * * *' # 10 মিনিট
```

### Database Ping Times পরিবর্তন:

`database-ping.yml` file এ:
```yaml
schedule:
  # UTC time use করতে হবে (Bangladesh = UTC + 6)
  - cron: '0 0,6,12,18 * * *'  # দিনে 4 বার
  - cron: '0 2,14 * * *'        # দিনে 2 বার
```

### আরো Endpoints যোগ করুন:
```yaml
- name: Ping Another Endpoint
  continue-on-error: true
  run: |
    curl -s https://your-server.com/api/endpoint
```

---

## ⚠️ Important Notes

- ✅ GitHub Actions **সম্পূর্ণ free** public repositories এর জন্য
- ✅ Private repositories এ 2000 minutes/month free (যথেষ্ট)
- ✅ Workflow automatically disabled হয় যদি 60 days repository inactive থাকে
- ✅ কোন maintenance লাগে না
- ✅ Server awake থাকলে response time fast থাকবে

---

## 🎯 Alternative Options (যদি GitHub Actions use করতে না চান)

### Option 1: UptimeRobot
- Free: 50 monitors
- Interval: 5 minutes minimum
- Setup: https://uptimerobot.com

### Option 2: Cron-job.org
- Free: Unlimited cron jobs
- Interval: 1 minute minimum
- Setup: https://cron-job.org

### Option 3: Node.js Script (keep-alive.js)
- Local বা hosting এ deploy করুন
- 24/7 running service
- নিজের control

---

## 📝 Troubleshooting

### Workflow দেখাচ্ছে না?
- `.github/workflows/` folder সঠিক path এ আছে কিনা check করুন
- File extension `.yml` বা `.yaml` হতে হবে
- GitHub এ push করেছেন কিনা verify করুন

### Workflow fail হচ্ছে?
- Actions tab এ logs check করুন
- Server URL সঠিক আছে কিনা verify করুন
- Server down আছে কিনা check করুন

### Manual trigger কাজ করছে না?
- `workflow_dispatch:` line টি workflow file এ আছে কিনা check করুন

---

## 📄 License

MIT

---

## 👨‍💻 Support

কোন সমস্যা হলে GitHub Issues এ জানান।
