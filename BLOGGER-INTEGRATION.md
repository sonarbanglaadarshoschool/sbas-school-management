# SBAS: GitHub + Firebase + Blogger deployment

এই প্রকল্পে GitHub Pages হবে ওয়েব অ্যাপের ফাইল হোস্টিং, Firebase হবে Authentication ও Firestore backend, আর Blogger হবে মূল ওয়েবসাইট/পাবলিক পেজ যেখানে অ্যাপটি embed করা যাবে। Blogger নিজে Firebase database নয়।

## 1) GitHub Pages-এ প্রকাশ
1. GitHub-এ একটি repository তৈরি করুন, যেমন `sbas-school-management`।
2. ZIP থেকে `school-management-starter` ফোল্ডারের ভেতরের সব ফাইল repository-র root-এ upload/commit করুন।
3. Repository → Settings → Pages → Deploy from a branch নির্বাচন করুন।
4. Branch `main`, folder `/ (root)` দিয়ে Save করুন।
5. প্রকাশিত URL হবে সাধারণত `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`।

## 2) Firebase সেটিংস
1. Firebase Console → Authentication → Settings → Authorized domains-এ আপনার GitHub Pages host যোগ করুন (যেমন `YOUR-USERNAME.github.io`)।
2. Authentication-এ Email/Password provider enabled রাখুন।
3. Firestore-এ প্রথম admin-এর জন্য `users/{AUTH_UID}` document-এ `role: "admin"`, `status: "active"` রাখুন।
4. `firestore.rules` পরীক্ষা করে Publish করুন।
5. বাস্তব শিক্ষার্থীর তথ্য দেওয়ার আগে Rules Emulator/পরীক্ষামূলক account দিয়ে যাচাই করুন।

## 3) Blogger-এ অ্যাপ দেখানো
Blogger Dashboard → Pages → New Page (অথবা Layout → Add a Gadget → HTML/JavaScript) খুলুন। HTML view-তে নিচের iframe বসান। `src`-এ আপনার আসল GitHub Pages URL দিন:

```html
<div style="width:100%;max-width:100%;margin:0 auto;">
  <iframe
    src="https://YOUR-USERNAME.github.io/REPOSITORY-NAME/"
    title="SBAS School Management System"
    loading="lazy"
    style="display:block;width:100%;height:100vh;min-height:900px;border:0;border-radius:12px;background:#f4f7fc;"
    allow="clipboard-read; clipboard-write"
    referrerpolicy="strict-origin-when-cross-origin">
  </iframe>
</div>
```

Blogger theme-এর header/sidebar সরিয়ে dedicated full-width page ব্যবহার করলে অ্যাপটি ভালো দেখাবে। Mobile-এ iframe-এর height প্রয়োজনে 1100px বা তার বেশি করুন।

## গুরুত্বপূর্ণ সীমাবদ্ধতা
- বর্তমান সংস্করণে Admin login, Dashboard summary এবং Student Management (add/list/search/filter/edit/delete) কাজ করে। Teachers, attendance, exams, fees, notices, reports, settings মেনুগুলো পরবর্তী ধাপে তৈরি হবে; এখনো সম্পূর্ণ কার্যকর নয়।
- GitHub Pages-এ কোড public থাকে। Firebase web config public client config—কিন্তু service-account key বা private key কখনো GitHub-এ দেবেন না।
- Firestore Rules-ই ডেটা সুরক্ষার আসল সীমা। বর্তমান starter rules admin-only; শিক্ষক/শিক্ষার্থী/অভিভাবক account চালুর আগে আলাদা role-based rules লাগবে।
- Blogger iframe embed হলো presentation পদ্ধতি। Authentication বা browser privacy policy-তে সমস্যা হলে GitHub Pages URL-টি Blogger-এ নতুন tab-এ খোলার link ব্যবহার করুন।
