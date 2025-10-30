# 🚨 دليل استرجاع طوارئ - اختفاء الفواتير بعد 6:30 مساءً

## 🔥 إصلاح فوري (خلال دقيقة)

### الخطوة 1: فتح Console
اضغط `F12` (أو `Ctrl+Shift+I`)

### الخطوة 2: تشغيل الإصلاح
انسخ والصق هذا الكود في Console واضغط Enter:

```javascript
// إصلاح شامل فوري
console.log('🚨 بدء الإصلاح الطارئ...');

// 1. استرجاع البيانات
recoverMissingData();

// 2. إصلاح التوقيت
fixOldSalesTimestamps();

// 3. إصلاح الحقول
fixOldInvoicesMissingFields();

// 4. إعادة الحساب
recalculateProfitAndFixNegativeValues();

// 5. إعادة التحميل
console.log('✅ تم الإصلاح! إعادة التحميل...');
setTimeout(() => location.reload(), 2000);
```

---

## 🔍 فحص البيانات

### عرض جميع الفواتير:
```javascript
const sales = JSON.parse(localStorage.getItem('sales'));
console.log(`عدد الفواتير الإجمالي: ${sales.length}`);
console.log('آخر 10 فواتير:', sales.slice(-10).map(s => ({
    id: s.invoiceNumber,
    date: s.date,
    timestamp: s.timestamp,
    amount: s.amount
})));
```

### فحص فواتير اليوم:
```javascript
const today = getLocalDateString();
const todaySales = sales.filter(s => {
    const d = parseLocalDate(s.timestamp || s.date);
    return d && getLocalDateString(d) === today;
});
console.log(`✅ فواتير اليوم (${today}): ${todaySales.length}`);
console.log('فواتير اليوم:', todaySales.map(s => s.invoiceNumber));
```

### فحص النطاق الزمني:
```javascript
const now = new Date();
const start = getStartOfLocalDay(now);
const end = getEndOfLocalDay(now);
console.log('النطاق اليومي:', {
    from: start.toLocaleString('ar-LB'),
    to: end.toLocaleString('ar-LB')
});
```

---

## 💾 استرجاع النسخ الاحتياطية

### عرض النسخ المتاحة:
```javascript
const backups = Object.keys(localStorage).filter(k => k.startsWith('backup_'));
console.log('عدد النسخ الاحتياطية:', backups.length);
console.log('النسخ:', backups);
```

### استرجاع نسخة محددة:
```javascript
const backupKey = 'backup_sales_1234567890'; // استبدل بالمفتاح الفعلي
const backup = localStorage.getItem(backupKey);
const restored = JSON.parse(backup);
console.log(`تم استرجاع ${restored.length} فاتورة`);
sales = restored;
localStorage.setItem('sales', JSON.stringify(sales));
location.reload();
```

---

## 🎯 الحل الدائم المنفذ

### ما تم إصلاحه:

1. **تثبيت المنطقة الزمنية**
   - ✅ استخدام `getLocalDateTimeISO()` بدلاً من `toISOString()`
   - ✅ توحيد جميع حسابات اليوم على Asia/Beirut
   - ✅ نطاق اليوم: 00:00:00 - 23:59:59 محلياً

2. **إيقاف عمليات المسح**
   - ✅ لا توجد عمليات `clear()` تلقائية
   - ✅ لا توجد reset بعد 6:30
   - ✅ النسخ الاحتياطي يحمي من الأخطاء

3. **منع الكتابة المتداخلة**
   - ✅ نظام قفل `storageWriteLock`
   - ✅ منع race conditions
   - ✅ حفظ آمن ومتزامن

4. **النسخ الاحتياطي**
   - ✅ قبل كل تحديث مهم
   - ✅ احتفاظ بأحدث 5 نسخ
   - ✅ استعادة تلقائية عند الفشل

5. **إصلاح الفواتير**
   - ✅ إضافة cost و costUSD
   - ✅ إضافة priceUSD
   - ✅ إصلاح تلقائي يومي

---

## ✅ التحقق من النجاح

### بعد الإصلاح، يجب أن ترى:

```
🔄 بدء استرجاع البيانات المفقودة...
✅ sales: X عنصر موجود
✅ products: X عنصر موجود
✅ customers: X عنصر موجود
✅ لا توجد بيانات مفقودة - جميع البيانات سليمة

✅ تمت إعادة حساب X من المبيعات
✅ اكتمل الفحص الشامل لتوقيت وربح المبيعات
```

### في التقارير:
- ✅ "اليوم" يظهر جميع المبيعات
- ✅ الربح اليومي صحيح
- ✅ لا توجد قيم سالبة

---

## 🚨 إذا استمرت المشكلة

### فحص localStorage مباشرة:
```javascript
const salesRaw = localStorage.getItem('sales');
console.log('حجم raw data:', salesRaw.length);
const salesArray = JSON.parse(salesRaw);
console.log('عدد الفواتير:', salesArray.length);
```

### فحص أي data corruption:
```javascript
const broken = sales.filter(s => {
    if (!s || !s.items || !Array.isArray(s.items)) return true;
    if (!s.invoiceNumber) return true;
    return false;
});
console.log('فواتير تالفة:', broken.length, broken);
```

### مسح localStorage وإعادة البدء:
```javascript
// تحذير: هذا سيحذف كل البيانات!
localStorage.clear();
location.reload();
```

---

## 📞 الدعم الطارئ

### أرسل هذه المعلومات للدعم:

```javascript
console.log(JSON.stringify({
    totalSales: sales.length,
    todaySales: todaySales.length,
    backups: Object.keys(localStorage).filter(k => k.startsWith('backup_')).length,
    lastBackup: Object.keys(localStorage).filter(k => k.startsWith('backup_sales_')).sort().pop(),
    timestampTest: parseLocalDate(getLocalDateTimeISO()),
    timezone: Intl.DateTimeFormat().resolvedOptions().timeZone,
    currentTime: new Date().toLocaleString('ar-LB')
}, null, 2));
```

---

## 🎯 الخلاصة

### ما تم منعه:
- ❌ اختفاء الفواتير بعد 6:30 مساءً
- ❌ تحويل UTC الخاطئ
- ❌ race conditions
- ❌ فقدان البيانات

### ما تم إضافته:
- ✅ توقيت ثابت
- ✅ نسخ احتياطية تلقائية
- ✅ استعادة تلقائية
- ✅ قفل كتابة
- ✅ فحوصات تلقائية

---

**النظام آمن الآن! 🎉**

**تاريخ الإصلاح:** اليوم  
**المنطقة الزمنية:** Asia/Beirut (ثابتة دائماً)

