# 🚀 School Management System - Keep-Alive Service

এই service টি আপনার Render server কে sleep থেকে জাগিয়ে রাখে।

## 🎯 কেন প্রয়োজন?

Render এর free tier এ 15 মিনিট inactivity থাকলে server sleep mode এ চলে যায়। এই service নিয়মিত server এবং database কে ping করে sleep হওয়া থেকে বাঁচায়।

## 📦 Features

- ✅ প্রতি 10 মিনিটে server ping করে
- ✅ Database connection check করে
- ✅ Public endpoints verify করে
- ✅ সম্পূর্ণ Free (GitHub Actions)
- ✅ Automatic operation

---

## 🚀 Setup - GitHub Actions (Recommended)

এটি **সবচেয়ে ভালো এবং সম্পূর্ণ FREE** solution!

### Step 1: Workflow File Setup

এই file টি already `.github-workflows-keep-alive.yml` নামে এই folder এ আছে। এটিকে project এর `.github/workflows/` folder এ copy করুন:

**Option A: Command Line (সহজ)**
```bash
# ping folder থেকে
cp .github-workflows-keep-alive.yml ../.github/workflows/keep-alive.yml
```

**Option B: Manually**
1. `.github/workflows/` folder এ যান (না থাকলে তৈরি করুন)
2. `keep-alive.yml` নামে নতুন file তৈরি করুন
3. `.github-workflows-keep-alive.yml` এর সব content copy করে paste করুন

### Step 2: GitHub এ Push করুন

```bash
git add .
git commit -m "Add keep-alive GitHub Action"
git push
```

### Step 3: Verify করুন

1. GitHub repository তে যান
2. **"Actions"** tab এ click করুন
3. **"Keep Server Alive"** workflow দেখতে পাবেন
4. প্রতি 10 মিনিটে automatically run হবে

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

1. **Health Endpoint:** `/api/health`
   - Server status check
   
2. **Database:** `/api/auth/check`
   - Database connection verify
   
3. **Public Home:** `/api/public-home`
   - Public endpoints check

---

## ⏱️ Schedule

- **Frequency:** Every 10 minutes
- **Why 10 minutes?** Render free tier sleeps after 15 minutes inactivity
- **Total pings per day:** ~144 pings
- **GitHub Actions limit:** 2000 minutes/month (এর চেয়ে অনেক কম use হবে)

---

## 🔍 Monitoring

### View Workflow Runs:
1. Go to your GitHub repository
2. Click **Actions** tab
3. See all workflow runs with status
4. Click on any run to see detailed logs

### Sample Log Output:
```
🚀 Pinging server at Thu Nov 20 15:30:00 UTC 2026
✅ Server is alive (Status: 200)
🗄️  Checking database connection...
✅ Database is responding (Status: 401)
🏠 Checking public home...
✅ Public home is accessible (Status: 200)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ Keep-alive ping completed at Thu Nov 20 15:30:05 UTC 2026
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🛠️ Customization

Workflow file edit করে customize করতে পারেন:

### Ping Frequency পরিবর্তন:
```yaml
schedule:
  - cron: '*/5 * * * *'  # 5 মিনিট
  - cron: '*/15 * * * *' # 15 মিনিট
```

### আরো Endpoints যোগ করুন:
```yaml
- name: Ping Another Endpoint
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
