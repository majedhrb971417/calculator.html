<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حاسبة المخالصات العمالية</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            margin: 0;
            padding: 20px;
            color: #333;
        }
        .container {
            max-width: 800px;
            background: #fff;
            margin: 0 auto;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }
        h2 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
            border-bottom: 2px solid #1abc9c;
            padding-bottom: 10px;
        }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #34495e;
        }
        input, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 16px;
        }
        button {
            width: 100%;
            padding: 12px;
            background-color: #1abc9c;
            border: none;
            color: white;
            font-size: 18px;
            font-weight: bold;
            border-radius: 4px;
            cursor: pointer;
            transition: background 0.3s;
        }
        button:hover {
            background-color: #16a085;
        }
        .result-box {
            margin-top: 30px;
            padding: 20px;
            background-color: #eef2f3;
            border-right: 5px solid #2980b9;
            border-radius: 4px;
            display: none;
        }
        .result-box h3 {
            margin-top: 0;
            color: #2980b9;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        table, th, td {
            border: 1px solid #bdc3c7;
        }
        th, td {
            padding: 12px;
            text-align: right;
        }
        th {
            background-color: #34495e;
            color: white;
        }
        .total-row {
            font-weight: bold;
            background-color: #d5dbdb;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>حاسبة المخالصات ونهاية الخدمة النظامية</h2>
    
    <div class="form-group">
        <label for="empName">اسم الموظف:</label>
        <input type="text" id="empName" placeholder="أدخل اسم الموظف">
    </div>
    
    <div class="form-group">
        <label for="salary">الراتب الفعلي (الأساسي + البدلات الثابتة مثل السكن والنقل):</label>
        <input type="number" id="salary" placeholder="مثال: 6000">
    </div>
    
    <div class="form-group">
        <label for="startDate">تاريخ المباشرة:</label>
        <input type="date" id="startDate">
    </div>
    
    <div class="form-group">
        <label for="endDate">تاريخ آخر يوم عمل (إنهاء العلاقة):</label>
        <input type="date" id="endDate">
    </div>
    
    <div class="form-group">
        <label for="vacationDays">رصيد الإجازات المستحقة (بالأيام):</label>
        <input type="number" id="vacationDays" placeholder="مثال: 15" value="0">
    </div>
    
    <div class="form-group">
        <label for="reason">سبب إنهاء العلاقة التعاقدية:</label>
        <select id="reason">
            <option value="standard">إنهاء خدمات من صاحب العمل / نهاية العقد / اتفاق الطرفين</option>
            <option value="resignation">استقالة الموظف</option>
            <option value="article77">فصل غير مشروع بموجب المادة 77 (من قِبَل الشركة)</option>
            <option value="article80">فصل بموجب المادة 80 (سقوط المكافأة وحفظ الإجازات)</option>
        </select>
    </div>
    
    <button onclick="calculateSettlement()">احسب المخالصة</button>
    
    <div class="result-box" id="resultBox">
        <h3>تقرير المخالصة المالية للموظف: <span id="resName"></span></h3>
        <p><strong>مدة الخدمة الكلية:</strong> <span id="resDuration"></span></p>
        
        <table>
            <thead>
                <tr>
                    <th>البند</th>
                    <th>طريقة الحسبة / التفاصيل</th>
                    <th>المبلغ المستحق (ريال)</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>مكافأة نهاية الخدمة</td>
                    <td id="eosDetail">-</td>
                    <td id="eosAmount">0</td>
                </tr>
                <tr>
                    <td>تعويض الإجازات الحالي</td>
                    <td id="vacDetail">-</td>
                    <td id="vacAmount">0</td>
                </tr>
                <tr>
                    <td>تعويضات إضافية (مادة 77)</td>
                    <td id="compensationDetail">لا يوجد</td>
                    <td id="compensationAmount">0</td>
                </tr>
                <tr class="total-row">
                    <td>إجمالي صافي المخالصة</td>
                    <td>المجموع المستحق للصرف</td>
                    <td id="totalAmount">0</td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<script>
function calculateSettlement() {
    // جلب المدخلات
    const name = document.getElementById('empName').value || 'الموظف المحترم';
    const salary = parseFloat(document.getElementById('salary').value);
    const startDate = new Date(document.getElementById('startDate').value);
    const endDate = new Date(document.getElementById('endDate').value);
    const vacationDays = parseFloat(document.getElementById('vacationDays').value) || 0;
    const reason = document.getElementById('reason').value;

    // التحقق من صحة البيانات الأساسية
    if (isNaN(salary) || isNaN(startDate.getTime()) || isNaN(endDate.getTime())) {
        alert('الرجاء إدخال الراتب وتواريخ المباشرة والنهاية بشكل صحيح.');
        return;
    }

    if (endDate < startDate) {
        alert('تاريخ نهاية العمل لا يمكن أن يكون قبل تاريخ المباشرة!');
        return;
    }

    // حساب مدة الخدمة بالأيام وتحويلها لسنوات بدقة
    const diffTime = Math.abs(endDate - startDate);
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
    const totalYears = diffDays / 365;

    // تحويل المدة لنص مقروء (سنوات، أشهر، أيام)
    const yearsDisplay = Math.floor(totalYears);
    const monthsDisplay = Math.floor((totalYears - yearsDisplay) * 12);
    const daysDisplay = Math.round(diffDays - (yearsDisplay * 365) - (monthsDisplay * 30.41));
    document.getElementById('resDuration').innerText = `${yearsDisplay} سنة و ${monthsDisplay} شهر و ${daysDisplay} يوم`;

    // 1. حساب مكافأة نهاية الخدمة الأساسية
    let baseEos = 0;
    if (totalYears <= 5) {
        baseEos = totalYears * (salary / 2);
    } else {
        baseEos = (5 * (salary / 2)) + ((totalYears - 5) * salary);
    }

    // تطبيق الشروط حسب سبب إنهاء الخدمة
    let finalEos = 0;
    let eosDetailText = "";

    if (reason === "standard" || reason === "article77") {
        finalEos = baseEos;
        eosDetailText = "مكافأة كاملة نظامية (نصف راتب لأول 5 سنوات + راتب كامل لكل سنة بعدها)";
    } else if (reason === "resignation") {
        if (totalYears < 2) {
            finalEos = 0;
            eosDetailText = "استقالة (خدمة أقل من سنتين: لا يستحق مكافأة)";
        } else if (totalYears >= 2 && totalYears < 5) {
            finalEos = baseEos * (1/3);
            eosDetailText = "استقالة (خدمة من 2 إلى 5 سنوات: يستحق ثلث المكافأة)";
        } else if (totalYears >= 5 && totalYears < 10) {
            finalEos = baseEos * (2/3);
            eosDetailText = "استقالة (خدمة من 5 إلى 10 سنوات: يستحق ثلثي المكافأة)";
        } else {
            finalEos = baseEos;
            eosDetailText = "استقالة (خدمة 10 سنوات فأكثر: يستحق المكافأة كاملة)";
        }
    } else if (reason === "article80") {
        finalEos = 0;
        eosDetailText = "فصل بموجب المادة 80 (سقوط الحق في المكافأة تماماً)";
    }

    // 2. حساب تعويض الإجازات (مضمون ومستحق دائماً للجميع)
    const vacationCompensation = (salary / 30) * vacationDays;
    const vacDetailText = `أجر يومي (${(salary/30).toFixed(2)} ريال) × ${vacationDays} يوم إجازة متبقي`;

    // 3. حساب تعويض المادة 77 (إذا تم اختيارها)
    let compensation77 = 0;
    let compDetailText = "لا يوجد";
    if (reason === "article77") {
        compensation77 = totalYears * (salary / 2); // الحسبة الافتراضية للعقود غير محددة المدة (أجر 15 يوم عن كل سنة)
        if (compensation77 < (salary * 2)) compensation77 = salary * 2; // نظاماً لا يقل التعويض عن أجر شهرين
        compDetailText = "تعويض مادة 77 (أجر 15 يوم عن كل سنة خدمة، بحد أدنى راتب شهرين)";
    }

    // إجمالي الصافي
    const totalSettlement = finalEos + vacationCompensation + compensation77;

    // عرض وتحديث جدول البيانات
    document.getElementById('resName').innerText = name;
    document.getElementById('eosDetail').innerText = eosDetailText;
    document.getElementById('eosAmount').innerText = finalEos.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('vacDetail').innerText = vacDetailText;
    document.getElementById('vacAmount').innerText = vacationCompensation.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('compensationDetail').innerText = compDetailText;
    document.getElementById('compensationAmount').innerText = compensation77.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('totalAmount').innerText = totalSettlement.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});

    // إظهار تقرير النتيجة
    document.getElementById('resultBox').style.display = 'block';
}
</script>

</body>
</html>
