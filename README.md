<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>سامانه ارزیابی خوابگاه پسرانه درگز</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Arial, sans-serif; }
        body { background: #f0f4f8; margin: 0; padding: 10px; direction: rtl; font-size: 15px; color: #1e2b3c; font-weight: 500; }
        .container { max-width: 1050px; margin: 0 auto; background: #fff; border-radius: 16px; box-shadow: 0 4px 16px rgba(0,0,0,0.1); padding: 14px 10px; }
        h1 { font-size: 20px; text-align: center; color: #0b3b5c; margin: 0 0 8px 0; font-weight: 800; }
        .subtitle { text-align: center; font-size: 14px; color: #4a5b6e; margin-bottom: 16px; font-weight: 600; }

        #loginScreen { max-width: 500px; margin: 30px auto; }
        .login-card { background: #fff; border-radius: 16px; box-shadow: 0 6px 20px rgba(0,0,0,0.12); padding: 26px 20px; border: 2px solid #1e3a5f; }
        .login-card h1 { font-size: 22px; color: #1e3a5f; margin-bottom: 6px; }
        .login-card .login-sub { text-align: center; font-size: 14px; color: #4a5b6e; margin-bottom: 22px; font-weight: 600; }
        .login-card label { display: block; font-weight: 800; color: #1e3a5f; margin-bottom: 8px; font-size: 16px; }
        .login-card select { width: 100%; padding: 14px 12px; border-radius: 10px; border: 2px solid #1e3a5f; font-size: 16px; font-weight: 700; color: #1e3a5f; background: #fff; cursor: pointer; margin-bottom: 18px; }
        .login-card .btn-login { background: #1e3a5f; color: #fff; border: none; padding: 16px; border-radius: 12px; font-size: 17px; font-weight: 800; width: 100%; cursor: pointer; transition: 0.2s; }
        .login-card .btn-login:active { background: #0b2a44; }
        .login-card .btn-login:disabled { background: #94a3b8; cursor: not-allowed; }
        .login-note { font-size: 13px; color: #4a5b6e; text-align: center; margin-top: 16px; font-weight: 600; }

        .user-bar { background: linear-gradient(135deg, #1e3a5f 0%, #0b6e99 100%); color: #fff; padding: 14px 16px; border-radius: 12px; margin-bottom: 16px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px; }
        .user-bar .info { display: flex; flex-direction: column; gap: 4px; }
        .user-bar .info .lbl { font-size: 12px; opacity: 0.85; font-weight: 600; }
        .user-bar .info .nm { font-size: 16px; font-weight: 800; }
        .user-bar .info .un { font-size: 13px; font-weight: 600; opacity: 0.95; }
        .user-bar .btn-logout { background: #ffb347; color: #1e2b3c; border: none; padding: 10px 16px; border-radius: 10px; font-size: 13px; font-weight: 800; cursor: pointer; }
        .user-bar .btn-logout:active { background: #f59e0b; }

        .selector-box { background: #eef4fa; padding: 14px; border-radius: 12px; margin-bottom: 16px; border: 2px solid #1e3a5f; }
        .selector-box label { font-weight: 800; color: #1e3a5f; display: block; margin-bottom: 8px; font-size: 16px; }
        .selector-box select { width: 100%; padding: 14px 12px; border-radius: 10px; border: 2px solid #1e3a5f; font-size: 17px; font-weight: 700; color: #1e3a5f; background: #fff; cursor: pointer; }

        .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; background: #f8fafc; padding: 12px; border-radius: 12px; margin-bottom: 18px; font-size: 14px; border: 1px solid #e2e8f0; }
        .info-item { display: flex; justify-content: space-between; background: #fff; padding: 8px 10px; border-radius: 8px; border: 1px solid #e2e8f0; }
        .info-item .lbl { color: #4a5b6e; font-weight: 600; }
        .info-item .val { font-weight: 800; color: #0b3b5c; }

        .accordion { border: 2px solid #1e3a5f; border-radius: 12px; margin-bottom: 14px; overflow: hidden; background: #fff; }
        .accordion-header { background: #1e3a5f; color: #fff; padding: 16px 14px; font-weight: 800; font-size: 17px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; user-select: none; }
        .accordion-header .weight { background: #ffb347; color: #1e2b3c; padding: 4px 12px; border-radius: 20px; font-size: 13px; font-weight: 800; }
        .accordion-header .arrow { font-size: 20px; transition: transform 0.25s; }
        .accordion-header.open .arrow { transform: rotate(180deg); }
        .accordion-body { display: none; }
        .accordion-body.open { display: block; }

        .card-list { padding: 10px; display: flex; flex-direction: column; gap: 10px; }
        .item-card { border: 2px solid #cbd5e1; border-radius: 10px; padding: 10px; background: #fff; }
        .item-card.discrepancy { border-color: #e2b04a; background: #fff9e6; }
        .item-title { font-weight: 800; font-size: 14px; color: #1e3a5f; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; }
        .item-title .w-badge { background: #fff7e6; color: #b45309; padding: 3px 10px; border-radius: 20px; font-size: 12px; font-weight: 800; border: 1px solid #ffb347; }
        .item-row { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; font-size: 13px; }
        .item-row .lbl { font-weight: 700; color: #4a5b6e; min-width: 90px; }
        .self-badge { display: inline-block; background: #f1f5f9; padding: 6px 12px; border-radius: 8px; font-weight: 800; color: #1e3a5f; font-size: 14px; }
        .score-select { width: 100%; padding: 12px 10px; border: 2px solid #1e3a5f; border-radius: 8px; font-size: 15px; font-weight: 800; color: #1e3a5f; background: #fff; cursor: pointer; }
        .score-select.empty { border-color: #e2b04a; background: #fffcf0; color: #b45309; }
        .reason-input { width: 100%; padding: 10px; border: 2px solid #e2b04a; border-radius: 8px; font-size: 14px; font-weight: 600; background: #fffcf0; display: none; margin-top: 6px; }
        .reason-input.active { display: block; }

        .total-box { background: #d9e8f5; padding: 12px; border-radius: 8px; text-align: center; font-weight: 800; font-size: 15px; color: #1e3a5f; margin-top: 8px; }

        .grand-total-box { background: #e8f0fe; padding: 18px; border-radius: 12px; display: flex; justify-content: space-between; align-items: center; font-weight: 800; font-size: 17px; margin-top: 20px; border: 2px solid #1e3a5f; }
        .grand-total-box span:last-child { font-size: 26px; color: #1e3a5f; }

        .btn-submit { background: #1e3a5f; color: #fff; border: none; padding: 18px; border-radius: 12px; font-size: 18px; font-weight: 800; width: 100%; margin-top: 18px; cursor: pointer; }
        .btn-submit:active { background: #0b2a44; }
        .btn-pdf { background: #2e7d32; color: #fff; border: none; padding: 18px; border-radius: 12px; font-size: 18px; font-weight: 800; width: 100%; margin-top: 10px; cursor: pointer; display: none; }
        .btn-pdf:active { background: #1b5e20; }
        .btn-bale { background: #007bff; color: #fff; border: none; padding: 18px; border-radius: 12px; font-size: 18px; font-weight: 800; width: 100%; margin-top: 10px; cursor: pointer; display: none; text-decoration: none; text-align: center; }
        .btn-bale:active { background: #0056b3; }

        .hint { font-size: 14px; font-weight: 700; color: #b45309; background: #fff7e6; padding: 12px 14px; border-radius: 10px; margin-top: 14px; border-right: 5px solid #b45309; }
        .footer { font-size: 12px; color: #64748b; text-align: center; margin-top: 16px; font-weight: 600; }

        @media (max-width: 600px) {
            .info-grid { grid-template-columns: 1fr; }
            .item-title { font-size: 13px; }
            .score-select { font-size: 14px; padding: 10px 8px; }
            .accordion-header { font-size: 15px; padding: 14px 10px; }
            .user-bar { flex-direction: column; align-items: stretch; }
            .user-bar .btn-logout { width: 100%; }
        }
    </style>
</head>
<body>

<div id="loginScreen" class="container">
    <div class="login-card">
        <h1>🔐 ورود به سامانه ارزیابی</h1>
        <div class="login-sub">ارزیابی خوابگاه پسرانه فوریت‌های پزشکی درگز</div>
        <label for="evaluatorSelect">نام خود را انتخاب کنید:</label>
        <select id="evaluatorSelect" onchange="checkLoginReady()">
            <option value="">— لطفاً نام خود را انتخاب کنید —</option>
        </select>
        <button class="btn-login" id="loginBtn" onclick="login()" disabled>ورود به سامانه</button>
        <div class="login-note">پس از ورود، می‌توانید فرم ارزیابی خوابگاه را تکمیل کنید.</div>
    </div>
</div>

<div id="evalScreen" class="container" style="display:none;">
    <h1>فرم اعتباربخشی خوابگاه پسرانه درگز</h1>
    <div class="subtitle">مرکز آموزش عالی فوریت‌های پزشکی</div>

    <div class="user-bar">
        <div class="info">
            <span class="lbl">ارزیاب فعلی</span>
            <span class="nm" id="currentEvaluatorName">—</span>
            <span class="un" id="currentEvaluatorUni">—</span>
        </div>
        <button class="btn-logout" onclick="logout()">🔄 تغییر کاربر</button>
    </div>

    <div class="selector-box">
        <label for="dormSelect">🏠 خوابگاه مورد ارزیابی</label>
        <select id="dormSelect" onchange="loadDormitory(this.value)">
            <option value="">— لطفاً انتخاب کنید —</option>
        </select>
    </div>

    <div id="dormInfo" class="info-grid" style="display:none;">
        <div class="info-item"><span class="lbl">نام دانشگاه</span><span class="val" id="infoUni">—</span></div>
        <div class="info-item"><span class="lbl">نام خوابگاه</span><span class="val" id="infoDorm">—</span></div>
        <div class="info-item"><span class="lbl">نوع</span><span class="val" id="infoType">—</span></div>
        <div class="info-item"><span class="lbl">مالکیت</span><span class="val" id="infoOwn">—</span></div>
        <div class="info-item"><span class="lbl">مدیریت</span><span class="val" id="infoMgmt">—</span></div>
        <div class="info-item"><span class="lbl">ظرفیت / ساکن</span><span class="val" id="infoFamilies">—</span></div>
    </div>

    <div id="formContainer"></div>

    <div id="grandTotalBox" class="grand-total-box" style="display:none;">
        <span>جمع کل امتیاز کسب‌شده (از ۱۰۰۰)</span>
        <span id="grand-total">۰</span>
    </div>

    <div id="hintBox" class="hint" style="display:none;">⚠️ در صورت مغایرت بین خوداظهاری و نمره ارزیاب، وارد کردن «علت مغایرت» الزامی است.</div>

    <button id="submitBtn" class="btn-submit" style="display:none;" onclick="submitForm()">ثبت و ارسال فرم</button>
    <button id="pdfBtn" class="btn-pdf" onclick="generatePDF()">📄 دانلود گزارش PDF</button>
    <a id="baleBtn" class="btn-bale" href="https://ble.ir/mums405" target="_blank">📤 ارسال به بله (خانم ابراهیمی)</a>

    <div class="footer">سامانه اعتباربخشی خوابگاه دانشجویی — نسخه ارزیاب درگز</div>
</div>

<div id="pdfContainer" style="display:none; padding: 20px; background: white; direction: rtl;"></div>

<script>
// ============ لیست ارزیاب‌ها (فقط دو نفر برای درگز) ============
const EVALUATORS = [
    { name: 'بهزاد سرداری', uni: 'ارزیاب خوابگاه پسرانه درگز' },
    { name: 'میثم نقوی مقدم', uni: 'ارزیاب خوابگاه پسرانه درگز' }
];

// ============ داده‌های خوابگاه (فقط درگز) ============
const DORMS = [
    {
        id: 'dargaz',
        name: 'خوابگاه پسرانه فوریت‌های پزشکی درگز',
        type: 'مجردی',
        ownership: 'ملکی',
        mgmt: 'دولتی',
        units: '—',
        families: '۸۰ نفر ظرفیت / ۱۰ نفر ساکن'
    }
];

// ============ شاخص‌های مجردی — خوابگاه پسرانه درگز ============
const SECTIONS_MOJARAD = [
    { title: '۱- استحکام بنا', weight: '۱۰۰ امتیاز', items: [
        { sub: '۱-۱ تأیید فنی ساختمان', w: 5, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۲-۱ پایداری لرزه‌ای', w: 5, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۳-۱ عمر ساختمان', w: 10, self: 1, options: [{label:'بالای ۳۲ سال', value:1}, {label:'۳۱-۲۴ سال', value:2}, {label:'۲۳-۱۶ سال', value:3}, {label:'۱۵-۸ سال', value:4}, {label:'۷-۱ سال', value:6}] }
    ]},
    { title: '۲- تأسیسات مکانیکی ساختمان', weight: '۱۳۶ امتیاز', items: [
        { sub: '۱-۲ موتورخانه', w: 6, self: 4, options: [{label:'۱', value:1}, {label:'۲', value:2}, {label:'۳', value:3}, {label:'۴', value:4}, {label:'۵', value:5}] },
        { sub: '۲-۲ سیستم گرمایش', w: 5, self: 1, options: [{label:'ندارد', value:0}, {label:'بخاری گازی', value:1}, {label:'ایرواشر', value:2}, {label:'شوفاژ و فن‌کوئل', value:4}, {label:'داکت اسپیلت/کف', value:5}] },
        { sub: '۳-۲ سیستم سرمایش', w: 5, self: 1, options: [{label:'ندارد', value:0}, {label:'کولر آبی', value:1}, {label:'ایرواشر', value:2}, {label:'اسپیلت/داکت', value:3}, {label:'چیلر/فن‌کوئل/کولر گازی', value:5}] },
        { sub: '۴-۲ سیستم آبرسانی', w: 6, self: 4, options: [{label:'مخازن ذخیره', value:2}, {label:'چاه و پمپ', value:3}, {label:'شبکه شهری', value:4}] },
        { sub: '۵-۲ مدیریت فاضلاب', w: 3, self: 3, options: [{label:'چاه جذبی', value:3}, {label:'اگوی شهری', value:4}] },
        { sub: '۶-۲ سیستم گازرسانی', w: 5, self: 4, options: [{label:'کپسول گاز', value:2}, {label:'شبکه گاز', value:3}, {label:'تأییدیه مرجع', value:4}] }
    ]},
    { title: '۳- تأسیسات برق ساختمان', weight: '۸۰ امتیاز', items: [
        { sub: '۱-۳ تابلو برق', w: 3, self: 5, options: [{label:'ندارد', value:0}, {label:'استاندارد دارد', value:5}] },
        { sub: '۲-۳ سیستم ارت', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'فعال دارد', value:5}] },
        { sub: '۳-۳ پریزهای ضد آب', w: 3, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۴-۳ برق اضطراری', w: 3, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:5}] },
        { sub: '۵-۳ یو پی اس', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۶-۳ ایمنی سیم‌کشی', w: 4, self: 3, options: [{label:'ندارد', value:0}, {label:'دارد', value:3}] },
        { sub: '۷-۳ کلید محافظ جان', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] }
    ]},
    { title: '۴- آسانسورها و خروج اضطراری', weight: '۶۴ امتیاز', items: [
        { sub: '۱-۴ راه‌پله اضطراری', w: 6, self: 4, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۲-۴ درب اضطراری', w: 3, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۳-۴ ایمنی آسانسور', w: 4, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:3}] },
        { sub: '۴-۴ خدمات تعمیر و نگهداری', w: 2, self: 2, options: [{label:'انجام نمی‌شود', value:0}, {label:'دوره‌ای انجام می‌شود', value:4}, {label:'فاقد آسانسور', value:2}] },
        { sub: '۵-۴ بیمه آسانسور', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] }
    ]},
    { title: '۵- تأسیسات و تجهیزات ایمنی و بهداشتی', weight: '۱۷۴ امتیاز', items: [
        { sub: '۱-۵ سیستم نشت گاز', w: 3, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:2}] },
        { sub: '۲-۵ سیستم اعلان حریق', w: 4, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:5}] },
        { sub: '۳-۵ سیستم اطفای حریق', w: 4, self: 3, options: [{label:'ندارد', value:0}, {label:'سایر', value:1}, {label:'آبفشان سقفی', value:2}, {label:'کپسول آتش‌نشانی', value:3}] },
        { sub: '۴-۵ حصارکشی محوطه', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۵-۵ حفاظ پنجره‌ها', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۶-۵ حفاظ راه‌پله‌ها', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۷-۵ حفاظ بالکن‌ها و تراس‌ها', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۸-۵ نردبان و حفاظ تخت‌خواب‌ها', w: 4, self: 4, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۹-۵ تهویه مناسب', w: 4, self: 4, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۱۰-۵ قطع اضطراری منابع انرژی', w: 3, self: 2, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۱۱-۵ دوربین مدار بسته', w: 4, self: 0, options: [{label:'ندارد', value:0}, {label:'محیط پیرامونی', value:1}, {label:'ورودی ساختمان', value:1}, {label:'ورودی خوابگاه', value:2}] },
        { sub: '۱۲-۵ گیت کنترل تردد', w: 4, self: 0, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۱۳-۵ روشنایی خوابگاه', w: 4, self: 3, options: [{label:'نامناسب', value:0}, {label:'تقریباً مناسب', value:1}, {label:'مناسب', value:3}] },
        { sub: '۱۴-۵ وضعیت سمپاشی', w: 3, self: 2, options: [{label:'انجام نمی‌شود', value:0}, {label:'دوره‌ای انجام می‌شود', value:2}] },
        { sub: '۱۵-۵ سر و صدا', w: 3, self: 2, options: [{label:'وجود دارد', value:0}, {label:'وجود ندارد', value:2}] },
        { sub: '۱۶-۵ علائم و تابلوهای ایمنی', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:3}] },
        { sub: '۱۷-۵ جعبه کمک‌های اولیه', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:3}] }
    ]},
    { title: '۶- طراحی خوابگاه (مجردی)', weight: '۱۸۴ امتیاز', items: [
        { sub: '۱-۶ سرانه سرویس بهداشتی', w: 4, self: 3, options: [{label:'بیشتر از ۱۵ نفر', value:1}, {label:'۱۲ تا ۱۴ نفر', value:2}, {label:'۹ تا ۱۱ نفر', value:3}, {label:'۶ تا ۸ نفر', value:4}] },
        { sub: '۲-۶ سرانه دوش حمام', w: 4, self: 3, options: [{label:'بیشتر از ۱۵ نفر', value:1}, {label:'۱۲ تا ۱۴ نفر', value:2}, {label:'۹ تا ۱۱ نفر', value:3}, {label:'۶ تا ۸ نفر', value:4}] },
        { sub: '۳-۶ سرویس بهداشتی در هر طبقه', w: 2, self: 3, options: [{label:'خیر', value:0}, {label:'بله', value:3}] },
        { sub: '۴-۶ حمام در هر طبقه', w: 2, self: 3, options: [{label:'خیر', value:0}, {label:'بله', value:3}] },
        { sub: '۵-۶ آشپزخانه در هر طبقه', w: 2, self: 3, options: [{label:'خیر', value:0}, {label:'بله', value:3}] },
        { sub: '۶-۶ لابی (اتاق ملاقات)', w: 2, self: 0, options: [{label:'خیر', value:0}, {label:'بله', value:3}] },
        { sub: '۷-۶ سرانه فیزیکی هر نفر', w: 5, self: 4, options: [{label:'کمتر از ۳ متر', value:0}, {label:'۳ تا ۳.۵ متر', value:1}, {label:'۳.۵ تا ۴ متر', value:2}, {label:'۴ تا ۵ متر', value:3}, {label:'بیشتر از ۵ متر', value:4}] },
        { sub: '۸-۶ سالن مطالعه', w: 3, self: 4, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۹-۶ اتاق رایانه', w: 2, self: 2, options: [{label:'ندارد', value:0}, {label:'دارد', value:2}] },
        { sub: '۱۰-۶ اتاق ویژه مهمان', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:2}] },
        { sub: '۱۱-۶ اتاق فعالیت‌های دانشجویی', w: 2, self: 2, options: [{label:'ندارد', value:0}, {label:'اتاق شورای صنفی', value:2}, {label:'اتاق فعالیت‌های فرهنگی', value:2}] },
        { sub: '۱۲-۶ اتاق مشاوره', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۱۳-۶ نمازخانه', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۱۴-۶ سالن ورزشی', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۱۵-۶ وضعیت پوشش اینترنت', w: 3, self: 4, options: [{label:'ندارد', value:0}, {label:'فقط مکان عمومی', value:1}, {label:'داخل اتاق‌ها', value:2}, {label:'کل ساختمان', value:4}] },
        { sub: '۱۶-۶ نصب تابلو قوانین', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۱۷-۶ علائم و تابلوهای راهنمای ساختمان', w: 2, self: 0, options: [{label:'وجود ندارد', value:0}, {label:'مناسب ولی ناکافی', value:2}, {label:'برای هر بخش وجود دارد', value:4}] },
        { sub: '۱۸-۶ رختشورخانه', w: 3, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۱۹-۶ اتاق تلویزیون', w: 2, self: 4, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۲۰-۶ رنگ‌آمیزی', w: 3, self: 3, options: [{label:'نامناسب', value:0}, {label:'تقریباً مناسب', value:3}, {label:'مناسب', value:4}] }
    ]},
    { title: '۷- امکانات رفاهی فردی و عمومی', weight: '۲۰۴ امتیاز', items: [
        { sub: '۱-۷ تعداد تخت‌خواب', w: 5, self: 2, options: [{label:'کمتر از ۷۵٪', value:0}, {label:'۷۵-۹۰٪', value:1}, {label:'۹۰-۱۰۰٪', value:2}, {label:'۱۰۰٪ ظرفیت', value:4}] },
        { sub: '۲-۷ کیفیت تخت‌خواب', w: 3, self: 4, options: [{label:'ناسالم و مستهلک', value:0}, {label:'سالم و غیرمستهلک', value:4}] },
        { sub: '۳-۷ تشک', w: 4, self: 1, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:4}] },
        { sub: '۴-۷ میز تحریر و صندلی', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:4}] },
        { sub: '۵-۷ کمد البسه', w: 3, self: 1, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:4}] },
        { sub: '۶-۷ فرش اتاق', w: 2, self: 2, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:2}, {label:'مناسب و ناکافی', value:3}, {label:'مناسب و کافی', value:4}] },
        { sub: '۷-۷ موکت', w: 2, self: 3, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:2}, {label:'مناسب و ناکافی', value:3}, {label:'مناسب و کافی', value:4}] },
        { sub: '۸-۷ جاکفشی', w: 2, self: 2, options: [{label:'بسیار ناکافی', value:1}, {label:'ناکافی', value:2}, {label:'مناسب و کافی', value:4}] },
        { sub: '۹-۷ رخت‌آویز', w: 2, self: 2, options: [{label:'بسیار ناکافی', value:1}, {label:'ناکافی', value:2}, {label:'مناسب و کافی', value:4}] },
        { sub: '۱۰-۷ پرده', w: 3, self: 3, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:2}, {label:'مناسب و ناکافی', value:3}, {label:'مناسب و کافی', value:4}] },
        { sub: '۱۱-۷ سرانه یخچال', w: 5, self: 2, options: [{label:'۰.۵-۱ فوت', value:1}, {label:'۱-۱.۵ فوت', value:2}, {label:'۱.۵-۲ فوت', value:3}, {label:'۲ فوت یا بیشتر', value:4}] },
        { sub: '۱۲-۷ آب سردکن', w: 3, self: 2, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:2}, {label:'مناسب و ناکافی', value:3}, {label:'مناسب و کافی', value:4}] },
        { sub: '۱۳-۷ سرانه جاروبرقی', w: 3, self: 2, options: [{label:'ندارد', value:0}, {label:'۴۱ نفر به بالا', value:1}, {label:'۲۱-۴۱ نفر', value:2}, {label:'۲۰ نفر و کمتر', value:4}] },
        { sub: '۱۴-۷ سرانه ماشین لباسشویی', w: 3, self: 2, options: [{label:'ندارد', value:0}, {label:'۴۱ نفر به بالا', value:1}, {label:'۲۱-۴۱ نفر', value:2}, {label:'۲۰ نفر و کمتر', value:4}] },
        { sub: '۱۵-۷ سرانه اتو برقی', w: 2, self: 2, options: [{label:'ندارد', value:0}, {label:'۳۳ نفر به بالا', value:1}, {label:'۲۴-۳۲ نفر', value:2}, {label:'۱۶-۲۴ نفر', value:3}, {label:'۱۶ نفر و کمتر', value:4}] },
        { sub: '۱۶-۷ اجاق گاز', w: 3, self: 1, options: [{label:'۱۰ نفر و بیشتر', value:0}, {label:'۹-۱۰ نفر', value:1}, {label:'۸-۹ نفر', value:2}, {label:'۶-۸ نفر', value:3}, {label:'کمتر از ۶ نفر', value:4}] },
        { sub: '۱۷-۷ واکس برقی', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'نامناسب', value:1}, {label:'مناسب', value:4}] },
        { sub: '۱۸-۷ مایکروفر', w: 2, self: 0, options: [{label:'۴۱ نفر به بالا', value:1}, {label:'۳۱-۴۱ نفر', value:2}, {label:'۲۱-۳۱ نفر', value:3}, {label:'۲۰ نفر و کمتر', value:4}] }
    ]},
    { title: '۸- امنیت و سیستم‌های مدیریتی', weight: '۵۸ امتیاز', items: [
        { sub: '۱-۸ بافت شهری', w: 2, self: 3, options: [{label:'در محدوده آسیب', value:0}, {label:'محدوده مناسب', value:3}] },
        { sub: '۲-۸ اشراف ساختمان مجاور', w: 2, self: 3, options: [{label:'مشرف هستند', value:0}, {label:'مشرف نیستند', value:3}] },
        { sub: '۳-۸ وضعیت امنیتی', w: 3, self: 4, options: [{label:'ندارد', value:0}, {label:'دارد', value:4}] },
        { sub: '۴-۸ دسترسی خوابگاه', w: 3, self: 4, options: [{label:'نامناسب', value:0}, {label:'مناسب و ناکافی', value:3}, {label:'نزدیک حمل‌ونقل عمومی', value:4}] },
        { sub: '۵-۸ روشنایی اطراف خوابگاه', w: 2, self: 4, options: [{label:'نامناسب', value:0}, {label:'تقریباً مناسب', value:2}, {label:'مناسب', value:4}] },
        { sub: '۶-۸ مدیریت پسماند', w: 3, self: 2, options: [{label:'ندارد', value:0}, {label:'دارد', value:2}] },
        { sub: '۷-۸ مدیریت HSE', w: 2, self: 0, options: [{label:'ندارد', value:0}, {label:'دارد', value:2}] },
        { sub: '۸-۸ مدیریت انرژی', w: 2, self: 0, options: [{label:'خیر', value:0}, {label:'دارد', value:2}] }
    ]}
];

let currentEvaluator = null;
let lastSubmittedData = null;

document.addEventListener('DOMContentLoaded', function() {
    initEvaluators();
    initDormSelect();
});

function initEvaluators() {
    const select = document.getElementById('evaluatorSelect');
    EVALUATORS.forEach((ev, idx) => {
        const opt = document.createElement('option');
        opt.value = idx;
        opt.textContent = `${ev.name} — ${ev.uni}`;
        select.appendChild(opt);
    });
}

function checkLoginReady() {
    const val = document.getElementById('evaluatorSelect').value;
    document.getElementById('loginBtn').disabled = (val === '');
}

function login() {
    const idx = document.getElementById('evaluatorSelect').value;
    if (idx === '') return;
    const evaluator = EVALUATORS[parseInt(idx)];
    if (!evaluator) return;
    currentEvaluator = evaluator;
    document.getElementById('currentEvaluatorName').textContent = evaluator.name;
    document.getElementById('currentEvaluatorUni').textContent = evaluator.uni;
    document.getElementById('loginScreen').style.display = 'none';
    document.getElementById('evalScreen').style.display = 'block';
    window.scrollTo({ top: 0, behavior: 'smooth' });
}

function logout() {
    if (!confirm('آیا می‌خواهید از حساب کاربری خارج شوید؟ (اطلاعات پر شده پاک خواهد شد)')) return;
    currentEvaluator = null;
    lastSubmittedData = null;
    document.getElementById('evalScreen').style.display = 'none';
    document.getElementById('loginScreen').style.display = 'block';
    document.getElementById('dormSelect').value = '';
    document.getElementById('formContainer').innerHTML = '';
    document.getElementById('dormInfo').style.display = 'none';
    document.getElementById('grandTotalBox').style.display = 'none';
    document.getElementById('hintBox').style.display = 'none';
    document.getElementById('submitBtn').style.display = 'none';
    document.getElementById('pdfBtn').style.display = 'none';
    document.getElementById('baleBtn').style.display = 'none';
    document.getElementById('evaluatorSelect').value = '';
    document.getElementById('loginBtn').disabled = true;
    window.scrollTo({ top: 0, behavior: 'smooth' });
}

function initDormSelect() {
    const select = document.getElementById('dormSelect');
    DORMS.forEach(d => {
        const opt = document.createElement('option');
        opt.value = d.id;
        opt.textContent = `${d.name} — ${d.type}`;
        select.appendChild(opt);
    });
}

function loadDormitory(id) {
    const dorm = DORMS.find(d => d.id === id);
    const infoBox = document.getElementById('dormInfo');
    const formContainer = document.getElementById('formContainer');
    const grandBox = document.getElementById('grandTotalBox');
    const hintBox = document.getElementById('hintBox');
    const submitBtn = document.getElementById('submitBtn');
    const pdfBtn = document.getElementById('pdfBtn');
    const baleBtn = document.getElementById('baleBtn');

    if (!dorm) {
        infoBox.style.display = 'none';
        formContainer.innerHTML = '';
        grandBox.style.display = 'none';
        hintBox.style.display = 'none';
        submitBtn.style.display = 'none';
        pdfBtn.style.display = 'none';
        baleBtn.style.display = 'none';
        return;
    }

    document.getElementById('infoUni').textContent = 'مرکز آموزش عالی فوریت‌های پزشکی';
    document.getElementById('infoDorm').textContent = dorm.name;
    document.getElementById('infoType').textContent = dorm.type;
    document.getElementById('infoOwn').textContent = dorm.ownership;
    document.getElementById('infoMgmt').textContent = dorm.mgmt;
    document.getElementById('infoFamilies').textContent = dorm.families;
    infoBox.style.display = 'grid';

    const sections = SECTIONS_MOJARAD;
    renderForm(dorm, sections);

    grandBox.style.display = 'flex';
    hintBox.style.display = 'block';
    submitBtn.style.display = 'block';
    pdfBtn.style.display = 'none';
    baleBtn.style.display = 'none';
}

function renderForm(dorm, sections) {
    const formContainer = document.getElementById('formContainer');
    formContainer.innerHTML = '';

    sections.forEach((sec, secIdx) => {
        const acc = document.createElement('div');
        acc.className = 'accordion';

        const header = document.createElement('div');
        header.className = 'accordion-header';
        header.innerHTML = `<span>${sec.title}</span><span><span class="weight">${sec.weight}</span> <span class="arrow">▼</span></span>`;
        header.onclick = () => toggleAccordion(header);
        acc.appendChild(header);

        const body = document.createElement('div');
        body.className = 'accordion-body';

        const cardList = document.createElement('div');
        cardList.className = 'card-list';

        sec.items.forEach((item) => {
            const card = document.createElement('div');
            card.className = 'item-card';

            let optionsHtml = `<option value="">— انتخاب کنید —</option>`;
            item.options.forEach(opt => {
                optionsHtml += `<option value="${opt.value}">${opt.label} (${opt.value} امتیاز)</option>`;
            });

            card.innerHTML = `
                <div class="item-title">
                    <span>${item.sub}</span>
                    <span class="w-badge">وزن: ${item.w}</span>
                </div>
                <div class="item-row">
                    <span class="lbl">خوداظهاری:</span>
                    <span class="self-badge">${item.self}</span>
                </div>
                <div class="item-row" style="flex-direction:column; align-items:stretch;">
                    <span class="lbl" style="margin-bottom:6px;">نمره ارزیاب:</span>
                    <select class="score-select empty" data-weight="${item.w}" data-self="${item.self}" onchange="onScoreChange(this)">
                        ${optionsHtml}
                    </select>
                </div>
                <input type="text" class="reason-input" placeholder="علت مغایرت را وارد کنید..." oninput="checkReason(this)">
            `;
            cardList.appendChild(card);
        });

        body.appendChild(cardList);

        const totalBox = document.createElement('div');
        totalBox.className = 'total-box';
        totalBox.innerHTML = `جمع بخش ${secIdx + 1}: <span id="total-section${secIdx + 1}">۰</span>`;
        body.appendChild(totalBox);

        acc.appendChild(body);
        formContainer.appendChild(acc);
    });

    setTimeout(() => {
        const firstHeader = document.querySelector('.accordion-header');
        if (firstHeader) toggleAccordion(firstHeader);
        calculateAllTotals();
    }, 30);
}

function toggleAccordion(header) {
    const body = header.nextElementSibling;
    header.classList.toggle('open');
    body.classList.toggle('open');
}

function onScoreChange(select) {
    const card = select.closest('.item-card');
    const selfScore = parseFloat(select.dataset.self) || 0;
    const reasonInput = card.querySelector('.reason-input');

    if (select.value === '') {
        select.classList.add('empty');
        card.classList.remove('discrepancy');
        reasonInput.classList.remove('active');
        reasonInput.required = false;
        reasonInput.value = '';
        calculateAllTotals();
        return;
    }

    select.classList.remove('empty');
    const assessorScore = parseFloat(select.value);
    const diff = Math.abs(selfScore - assessorScore) > 0.001;

    if (diff) {
        card.classList.add('discrepancy');
        reasonInput.classList.add('active');
        reasonInput.required = true;
    } else {
        card.classList.remove('discrepancy');
        reasonInput.classList.remove('active');
        reasonInput.required = false;
        reasonInput.value = '';
    }
    calculateAllTotals();
}

function checkReason(input) {
    if (input.value.trim() !== '') {
        input.style.borderColor = '#2e7d32';
        input.style.backgroundColor = '#f1f8e9';
    } else {
        input.style.borderColor = '#e2b04a';
        input.style.backgroundColor = '#fffcf0';
    }
}

function calculateAllTotals() {
    const accordions = document.querySelectorAll('.accordion');
    let grand = 0;

    accordions.forEach((acc, idx) => {
        const selects = acc.querySelectorAll('.score-select');
        let total = 0;
        selects.forEach(sel => {
            const w = parseFloat(sel.dataset.weight) || 0;
            const s = sel.value === '' ? 0 : parseFloat(sel.value);
            total += s * w;
        });
        grand += total;
        const cell = document.getElementById('total-section' + (idx + 1));
        if (cell) cell.textContent = formatNumber(total);
    });

    const grandEl = document.getElementById('grand-total');
    if (grandEl) grandEl.textContent = formatNumber(grand);
}

function formatNumber(num) {
    if (isNaN(num)) return '۰';
    const rounded = Math.round(num * 100) / 100;
    let str = rounded.toString();
    if (str.includes('.')) str = str.replace(/\.?0+$/, '');
    const parts = str.split('.');
    parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    return parts.join('.');
}

function submitForm() {
    if (!currentEvaluator) {
        alert('لطفاً ابتدا وارد سامانه شوید.');
        return;
    }

    const dormId = document.getElementById('dormSelect').value;
    const dorm = DORMS.find(d => d.id === dormId);
    if (!dorm) { alert('لطفاً خوابگاه را انتخاب کنید.'); return; }

    let unfilled = 0;
    document.querySelectorAll('.score-select').forEach(sel => {
        if (sel.value === '') unfilled++;
    });
    if (unfilled > 0) {
        if (!confirm(`⚠️ ${unfilled} مورد از نمرات ارزیاب انتخاب نشده است. ادامه می‌دهید؟`)) return;
    }

    let isValid = true;
    const missingReasons = [];
    document.querySelectorAll('.item-card.discrepancy').forEach(card => {
        const reasonInput = card.querySelector('.reason-input');
        if (!reasonInput.value.trim()) {
            isValid = false;
            reasonInput.style.borderColor = '#d32f2f';
            reasonInput.style.backgroundColor = '#ffebee';
            const title = card.querySelector('.item-title span')?.textContent || 'نامشخص';
            missingReasons.push(title);
        }
    });
    if (!isValid) {
        alert('لطفاً برای موارد مغایرت، علت را وارد کنید:\n' + missingReasons.join('\n'));
        return;
    }

    const confirmMsg = "آیا مایلید فرم را برای ارسال به خانم ابراهیمی (کارشناس اداره خوابگاه‌ها) تایید کنید؟\n\nبا تایید، فایل PDF ساخته شده و منوی ارسال باز می‌شود.";
    if (!confirm(confirmMsg)) return;

    lastSubmittedData = {
        evaluator: { name: currentEvaluator.name, university: currentEvaluator.uni },
        dormitory: { id: dorm.id, name: dorm.name, type: dorm.type },
        total: document.getElementById('grand-total').textContent,
        submittedAt: new Date().toISOString(),
        scores: []
    };

    document.querySelectorAll('.score-select').forEach(sel => {
        const card = sel.closest('.item-card');
        const subItem = card.querySelector('.item-title span')?.textContent?.trim() || '';
        const selfScore = parseFloat(sel.dataset.self) || 0;
        const assessorScore = sel.value === '' ? null : parseFloat(sel.value);
        const reason = card.querySelector('.reason-input')?.value || '';
        const weight = parseFloat(sel.dataset.weight) || 0;
        lastSubmittedData.scores.push({ subItem, weight, selfScore, assessorScore, reason, weightedScore: (assessorScore || 0) * weight });
    });

    const data = lastSubmittedData;
    const pdfContainer = document.getElementById('pdfContainer');

    let tableRows = '';
    data.scores.forEach((s, index) => {
        const assessorScore = s.assessorScore === null ? '—' : s.assessorScore;
        const reason = s.reason ? s.reason : '—';
        const rowColor = s.reason ? '#fffcf0' : '#fff';
        tableRows += `
            <tr style="background-color: ${rowColor}; border-bottom: 1px solid #ddd;">
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc;">${index + 1}</td>
                <td style="padding: 8px; text-align: right; border: 1px solid #ccc; font-size: 12px;">${s.subItem}</td>
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc;">${s.weight}</td>
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc;">${s.selfScore}</td>
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc; font-weight: bold; color: #1e3a5f;">${assessorScore}</td>
                <td style="padding: 8px; text-align: right; border: 1px solid #ccc; font-size: 11px;">${reason}</td>
            </tr>
        `;
    });

    const pdfHTML = `
        <div style="font-family: Tahoma, Arial, sans-serif; direction: rtl; max-width: 800px; margin: 0 auto;">
            <div style="text-align: center; margin-bottom: 20px; border-bottom: 2px solid #1e3a5f; padding-bottom: 10px;">
                <h2 style="color: #1e3a5f; margin: 0; font-size: 20px;">گزارش ارزیابی خوابگاه پسرانه درگز</h2>
                <p style="color: #4a5b6e; margin: 5px 0 0 0; font-size: 14px;">مرکز آموزش عالی فوریت‌های پزشکی</p>
            </div>
            <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 14px;">
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc; width: 25%;"><strong>نام ارزیاب:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd; width: 25%;">${data.evaluator.name}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc; width: 25%;"><strong>سمت:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd; width: 25%;">${data.evaluator.university}</td>
                </tr>
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc;"><strong>نام خوابگاه:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd;">${data.dormitory.name}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc;"><strong>نوع خوابگاه:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd;">${data.dormitory.type}</td>
                </tr>
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc;"><strong>تاریخ ثبت:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd;" colspan="3">${new Date(data.submittedAt).toLocaleString('fa-IR')}</td>
                </tr>
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #e8f0fe; font-size: 16px; font-weight: bold;">جمع کل امتیاز:</td>
                    <td style="padding: 8px; border: 1px solid #ddd; font-size: 16px; font-weight: bold; color: #1e3a5f;" colspan="3">${data.total} از ۱۰۰۰</td>
                </tr>
            </table>
            <h3 style="color: #1e3a5f; font-size: 16px; margin-bottom: 10px;">جزئیات نمرات:</h3>
            <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
                <thead>
                    <tr style="background-color: #1e3a5f; color: white;">
                        <th style="padding: 10px; border: 1px solid #ccc; width: 5%;">#</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 40%;">عنوان شاخص</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 10%;">وزن</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 10%;">خوداظهاری</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 10%;">نمره ارزیاب</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 25%;">علت مغایرت</th>
                    </tr>
                </thead>
                <tbody>${tableRows}</tbody>
            </table>
            <div style="margin-top: 30px; text-align: center; font-size: 12px; color: #64748b;">
                این گزارش به صورت خودکار توسط سامانه اعتباربخشی خوابگاه‌های دانشجویی تولید شده است.
            </div>
        </div>
    `;

    pdfContainer.innerHTML = pdfHTML;

    const opt = {
        margin: 10,
        filename: `گزارش_ارزیابی_درگز_${new Date().toLocaleDateString('fa-IR').replace(/\//g, '-')}.pdf`,
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2, useCORS: true },
        jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
    };

    const btn = document.getElementById('submitBtn');
    const originalText = btn.textContent;
    btn.textContent = '⏳ در حال ساخت گزارش...';
    btn.disabled = true;

    html2pdf().set(opt).from(pdfContainer).outputPdf('blob').then(blob => {
        const file = new File([blob], opt.filename, { type: 'application/pdf' });
        if (navigator.canShare && navigator.canShare({ files: [file] })) {
            navigator.share({
                files: [file],
                title: 'ارسال گزارش به بله',
                text: 'لطفاً این فایل PDF را برای خانم ابراهیمی در بله ارسال نمایید.'
            }).then(() => {
                document.getElementById('dormSelect').value = '';
                loadDormitory('');
            }).catch(err => {
                console.log('خطا در اشتراک‌گذاری:', err);
                downloadFallback(blob, opt.filename);
            });
        } else {
            downloadFallback(blob, opt.filename);
        }
    }).finally(() => {
        btn.textContent = originalText;
        btn.disabled = false;
    });

    function downloadFallback(blob, filename) {
        const link = document.createElement("a");
        const url = URL.createObjectURL(blob);
        link.setAttribute("href", url);
        link.setAttribute("download", filename);
        link.style.visibility = 'hidden';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        alert(`✅ فرم با موفقیت ثبت شد و فایل گزارش دانلود گردید.\n\nلطفاً فایل را برای خانم ابراهیمی در بله ارسال نمایید.`);
        document.getElementById('pdfBtn').style.display = 'block';
        document.getElementById('baleBtn').style.display = 'block';
        document.getElementById('dormSelect').value = '';
        loadDormitory('');
    }
}

function generatePDF() {
    if (!lastSubmittedData) {
        alert("داده‌ای برای ساخت PDF یافت نشد. لطفاً ابتدا فرم را ثبت کنید.");
        return;
    }
    const data = lastSubmittedData;
    const pdfContainer = document.getElementById('pdfContainer');
    let tableRows = '';
    data.scores.forEach((s, index) => {
        const assessorScore = s.assessorScore === null ? '—' : s.assessorScore;
        const reason = s.reason ? s.reason : '—';
        const rowColor = s.reason ? '#fffcf0' : '#fff';
        tableRows += `
            <tr style="background-color: ${rowColor}; border-bottom: 1px solid #ddd;">
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc;">${index + 1}</td>
                <td style="padding: 8px; text-align: right; border: 1px solid #ccc; font-size: 12px;">${s.subItem}</td>
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc;">${s.weight}</td>
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc;">${s.selfScore}</td>
                <td style="padding: 8px; text-align: center; border: 1px solid #ccc; font-weight: bold; color: #1e3a5f;">${assessorScore}</td>
                <td style="padding: 8px; text-align: right; border: 1px solid #ccc; font-size: 11px;">${reason}</td>
            </tr>
        `;
    });
    const pdfHTML = `
        <div style="font-family: Tahoma, Arial, sans-serif; direction: rtl; max-width: 800px; margin: 0 auto;">
            <div style="text-align: center; margin-bottom: 20px; border-bottom: 2px solid #1e3a5f; padding-bottom: 10px;">
                <h2 style="color: #1e3a5f; margin: 0; font-size: 20px;">گزارش ارزیابی خوابگاه پسرانه درگز</h2>
                <p style="color: #4a5b6e; margin: 5px 0 0 0; font-size: 14px;">مرکز آموزش عالی فوریت‌های پزشکی</p>
            </div>
            <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 14px;">
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc; width: 25%;"><strong>نام ارزیاب:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd; width: 25%;">${data.evaluator.name}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc; width: 25%;"><strong>سمت:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd; width: 25%;">${data.evaluator.university}</td>
                </tr>
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc;"><strong>نام خوابگاه:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd;">${data.dormitory.name}</td>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc;"><strong>نوع خوابگاه:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd;">${data.dormitory.type}</td>
                </tr>
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #f8fafc;"><strong>تاریخ ثبت:</strong></td>
                    <td style="padding: 8px; border: 1px solid #ddd;" colspan="3">${new Date(data.submittedAt).toLocaleString('fa-IR')}</td>
                </tr>
                <tr>
                    <td style="padding: 8px; border: 1px solid #ddd; background: #e8f0fe; font-size: 16px; font-weight: bold;">جمع کل امتیاز:</td>
                    <td style="padding: 8px; border: 1px solid #ddd; font-size: 16px; font-weight: bold; color: #1e3a5f;" colspan="3">${data.total} از ۱۰۰۰</td>
                </tr>
            </table>
            <h3 style="color: #1e3a5f; font-size: 16px; margin-bottom: 10px;">جزئیات نمرات:</h3>
            <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
                <thead>
                    <tr style="background-color: #1e3a5f; color: white;">
                        <th style="padding: 10px; border: 1px solid #ccc; width: 5%;">#</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 40%;">عنوان شاخص</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 10%;">وزن</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 10%;">خوداظهاری</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 10%;">نمره ارزیاب</th>
                        <th style="padding: 10px; border: 1px solid #ccc; width: 25%;">علت مغایرت</th>
                    </tr>
                </thead>
                <tbody>${tableRows}</tbody>
            </table>
            <div style="margin-top: 30px; text-align: center; font-size: 12px; color: #64748b;">
                این گزارش به صورت خودکار توسط سامانه اعتباربخشی خوابگاه‌های دانشجویی تولید شده است.
            </div>
        </div>
    `;
    pdfContainer.innerHTML = pdfHTML;
    const opt = {
        margin: 10,
        filename: `گزارش_ارزیابی_درگز_${new Date().toLocaleDateString('fa-IR').replace(/\//g, '-')}.pdf`,
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2, useCORS: true },
        jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
    };
    const btn = document.getElementById('pdfBtn');
    const originalText = btn.textContent;
    btn.textContent = '⏳ در حال ساخت PDF...';
    btn.disabled = true;
    html2pdf().set(opt).from(pdfContainer).save().then(() => {
        btn.textContent = originalText;
        btn.disabled = false;
    }).catch(err => {
        console.error(err);
        alert('خطا در ساخت PDF. لطفاً دوباره تلاش کنید.');
        btn.textContent = originalText;
        btn.disabled = false;
    });
}
</script>
</body>
</html>
