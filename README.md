# 🚀 Personal Send Money Automated Payment Gateway (bKash & Nagad)

বিকাশ ও নগদ পার্সোনাল সেন্ড মানি অটোমেটেড পেমেন্ট গেটওয়ে এবং লাইভ ভেরিফিকেশন সিস্টেম। মার্চেন্ট একাউন্টের ঝামেলা ও অতিরিক্ত ক্যাশ-আউট/গেটওয়ে ফি ছাড়াই ISP বিলিং, ওয়াইফাই হটস্পট (MikroTik / Radius), VPN রিসেলার এবং ই-কমার্স/ওয়েবসাইট পেমেন্ট অটোমেশনের জন্য একটি নির্ভরযোগ্য ও স্বয়ংসম্পূর্ণ প্ল্যাটফর্ম।

---

## 📸 স্ক্রিনশট ও প্রধান কম্পোনেন্ট

1. **Android App (SMS Listener & Syncer):**
   - **Bearer Token Security:** নিরাপদ অথেনটিকেশন সহ সরাসরি সার্ভারে কানেক্ট হয়।
   - **Real-time Heartbeat & Status:** প্রতি ২৫ সেকেন্ড পর পর স্বয়ংক্রিয় হার্টবিট পাঠিয়ে সার্ভার ও অ্যাপের কানেকশন লাইভ রাখে।
   - **Device & Battery Info:** ফোনের মডেল, অ্যান্ড্রয়েড আইডি এবং লাইভ ব্যাটারি পারসেন্টেজ (চার্জিং স্টেট সহ) সার্ভার ড্যাশবোর্ডে পাঠায়।
   - **Connect / Disconnect কন্ট্রোল:** অ্যাপ থেকে সরাসরি "CONNECT TO SERVER" বা "DISCONNECT FROM SERVER" বাটন চাপলে সার্ভার ড্যাশবোর্ডে তাৎক্ষণিক `ONLINE` বা `OFFLINE` স্ট্যাটাস আপডেট হয়।
   - **SMS Read/Receive & Auto Sync:** লাইভ এসএমএস অটো-ডিটেক্ট করে এবং `SYNC RECENT SMS` বাটন দিয়ে ব্যাকলগ এসএমএস সিঙ্ক করা যায়।
   - **ডুয়াল প্রোভাইডার:** একই সাথে বিকাশ (`16247`) এবং নগদ (`16167`) রিসিভড এসএমএস পার্স করে।
   - **পে-লোড সিমুলেটর:** মোবাইল থেকেই টেস্ট সেন্ডার নম্বর এবং টেস্ট এসএমএস পাঠিয়ে সাথে সাথে টেস্ট করার সুবিধা।

2. **Incoming Payment Viewer (ওয়েব ড্যাশবোর্ড):**
   - **🖥️ Server Status Card:** সার্ভার ডাটাবেজ কানেক্টেড কি না তা রিয়েল-টাইমে `CONNECTED (ONLINE)` বা `DISCONNECTED (OFFLINE)` দেখায়।
   - **📱 Android App Listener Card:** লিসেনার ফোন কানেক্টেড কি না, ফোনের মডেল (যেমনঃ Samsung Galaxy / Xiaomi), ব্যাটারি পারসেন্টেজ (⚡), আইপি অ্যাড্রেস এবং লাস্ট পিং টাইম (যেমনঃ `4s ago`) লাইভ দেখায়। ফোন ডিসকানেক্ট হলে সাথে সাথে `DISCONNECTED (OFFLINE)` হয়ে যায়।
   - লাইভ রিয়েল-টাইম ট্রানজেকশন মনিটরিং (3 সেকেন্ড পর পর অটো-পোলিং)।
   - মেট্রিক্স কার্ড: `TOTAL`, `PROCESSED`, `PENDING`, `UNMATCHED`, `FAILED + IGNORED`।
   - `JSON Endpoint` ও লাইভ ইন্ডিকেটর।

3. **স্মার্ট অটো-ম্যাচিং ইঞ্জিন (Auto Billing & Invoice Match):**
   - এসএমএস-এর রেফারেন্স কোড (`Ref`) যদি ইনভয়েস নম্বরের সাথে মিলে যায় (যেমনঃ `INV-1025`), ইনভয়েসটি সাথে সাথে `PAID` হয়ে যাবে।
   - রেফারেন্স কোড বা সেন্ডার ফোন নম্বর যদি কোনো ক্লায়েন্টের সাথে মিলে (যেমনঃ `myhome2`), তবে তার কোনো বকেয়া বিল থাকলে তা পেইড হবে।
   - কোনো বকেয়া বিল না থাকলে স্বয়ংক্রিয়ভাবে ক্লায়েন্টের অ্যাডভান্স ব্যালেন্সে (`Advance Balance`) ক্রেডিট হয়ে যাবে (যেমনঃ `client:913` -> *"No due invoice found. Stored as advance payment. Payment #20"*).
   - কোনো ক্লায়েন্ট বা ইনভয়েস না মিললে স্ট্যাটাস হবে `UNMATCHED`।

4. **Billing & Payment History:**
   - ইনভয়েস এবং পেমেন্টের হিস্ট্রি টেবিল, অ্যাডভান্স ব্যালেন্স কাউন্টার, প্রিন্ট ও রিসিট ভিউ।

---

## 📁 প্রজেক্ট স্ট্রাকচার

```text
optimistic-bardeen/
├── server/
│   ├── config.php               # ডাটাবেজ কানেকশন ও বেয়ারার টোকেন হেল্পার
│   ├── database.sql             # সম্পূর্ণ MySQL ডাটাবেজ স্কিমা ও সিড ডাটা
│   ├── api_handler.php          # কোর এসএমএস পার্সিং ও ইনভয়েস অটো-ম্যাচিং ইঞ্জিন
│   ├── index.php                # রুট এন্ডপয়েন্ট (API ও ওয়েব রিডাইরেক্ট)
│   ├── bkash/
│   │   └── index.php            # Android অ্যাপের জন্য টার্গেট এন্ডপয়েন্ট (/bkash/index.php)
│   ├── viewer.php               # Incoming Payment Viewer লাইভ ড্যাশবোর্ড (Screenshot 3)
│   ├── download.php             # ওয়েব থেকেই APK ডাউনলোডের ডেডিকেটেড পেজ ও এন্ডপয়েন্ট
│   ├── billing_history.php      # Billing & Payment History ভিউ (Screenshot 1)
│   ├── test_simulator.php       # ব্রাউজার থেকেই বিকাশ ও নগদ টেস্ট করার সিমুলেটর
│   ├── transactions_json.php    # লাইভ স্ট্যাটাস ও ট্রানজেকশন JSON API
│   ├── api.php                  # ৩য় পক্ষ (ISP/Hotspot/VPN/Web) ইন্টিগ্রেশন API
│   └── api_client_example.php   # পিএইচপি ইন্টিগ্রেশন কোড এক্সাম্পল
└── android/
    ├── build.gradle
    ├── settings.gradle
    └── app/
        ├── build.gradle
        └── src/main/
            ├── AndroidManifest.xml
            ├── java/com/paygateway/smslistener/
            │   ├── MainActivity.java
            │   ├── SmsBroadcastReceiver.java
            │   ├── ApiManager.java
            │   └── BootCompletedReceiver.java
            └── res/
                ├── layout/activity_main.xml
                ├── values/{colors,strings,styles}.xml
                └── drawable/{btn_teal,bg_status_box}.xml
```

---

## ⚙️ সার্ভার সেটআপ ও ইনস্টলেশন

### ১. ডাটাবেজ ইমপোর্ট করুন:
আপনার MySQL / phpMyAdmin-এ গিয়ে `payment_gateway` নামে ডাটাবেজ তৈরি করে [database.sql](file:///c:/Users/sohag/Documents/antigravity/optimistic-bardeen/server/database.sql) ফাইলটি ইমপোর্ট করুন:
```sql
mysql -u root -p payment_gateway < server/database.sql
```

### ২. ডাটাবেজ ও সিক্রেট টোকেন কনফিগারেশন:
[config.php](file:///c:/Users/sohag/Documents/antigravity/optimistic-bardeen/server/config.php) ফাইলটি ওপেন করে আপনার ডাটাবেজ ইউজারনেম, পাসওয়ার্ড এবং সিক্রেট বেয়ারার টোকেন বসান:
```php
define('DB_HOST', 'localhost');
define('DB_USER', 'your_db_user');
define('DB_PASS', 'your_db_password');
define('DB_NAME', 'payment_gateway');
define('DEFAULT_BEARER_TOKEN', 'BKASH_PERSONAL_2026_SECRET_KEY');
```

### ৩. ব্রাউজারে ড্যাশবোর্ড ও অ্যাপ ডাউনলোড:
- **Incoming Payment Viewer:** `http://your-server-ip/viewer.php`
- **Direct APK Download Page:** `http://your-server-ip/download.php` (ওয়েব থেকেই সরাসরি APK ডাউনলোড ও QR স্ক্যান)
- **Billing & Payment History:** `http://your-server-ip/billing_history.php`
- **Web SMS Simulator:** `http://your-server-ip/test_simulator.php`

---

## 📱 অ্যান্ড্রয়েড অ্যাপ সেটআপ

১. Android Studio-তে `android/` ফোল্ডারটি ওপেন করুন।  
২. Gradle Sync সম্পন্ন হওয়ার পর `Build > Build Bundle(s) / APK(s) > Build APK(s)` নির্বাচন করে APK তৈরি করুন এবং আপনার পার্সোনাল বিকাশ/নগদ সিমে থাকা ফোনে ইনস্টল করুন।  
৩. অ্যাপ ওপেন করে:
   - **GRANT SMS PERMISSIONS** বাটনে ক্লিক করে এসএমএস রিড পারমিশন দিন।
   - **Server API URL** বক্সে আপনার সার্ভারের ঠিকানা দিন (যেমনঃ `http://your-domain.com/bkash/index.php`)।
   - **Bearer Token** বক্সে আপনার সিক্রেট টোকেন দিন (যেমনঃ `BKASH_PERSONAL_2026_SECRET_KEY`)।
   - **CONNECT TO SERVER** বাটনে ক্লিক করলে সবুজ বর্ডারে দেখাবে: `Connection status: Connected to server`।
   - **SEND TEST PAYLOAD** বাটনে ক্লিক করে টেস্ট করে দেখুন সার্ভারে সাথে সাথে ট্রানজেকশন প্রসেস হচ্ছে কিনা!

---

## 🔌 ISP বিলিং, হটস্পট বা ওয়েবসাইটে ইন্টিগ্রেশন

যেকোনো ক্লায়েন্ট বা কাস্টমার যখন সেন্ড মানি করে TrxID সাবমিট করবে, তখন আপনার বিলিং স্ক্রিপ্ট থেকে খুব সহজে ভেরিফাই করতে পারবেন:

```php
require_once 'api_client_example.php';

$gateway = new PaymentGatewayClient('https://your-server.com', 'BKASH_PERSONAL_2026_SECRET_KEY');

// গ্রাহকের দেওয়া TrxID চেক করুন
$result = $gateway->verifyTrx($_POST['trx_id']);

if (!empty($result['found']) && $result['status'] === 'PROCESSED') {
    $amount = $result['amount'];
    $provider = $result['provider']; // 'BKASH' অথবা 'NAGAD'
    $reference = $result['reference']; // যেমনঃ 'myhome2'

    // MikroTik Hotspot Voucher অ্যাক্টিভেশন অথবা ISP অ্যাকাউন্ট আনব্লক করার কোড
    // unblockUser($reference);

    echo "Payment verified! Tk $amount received via $provider";
} else {
    echo "পেমেন্ট পাওয়া যায়নি অথবা TrxID সঠিক নয়!";
}
```
