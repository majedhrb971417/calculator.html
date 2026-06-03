
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منظومة إدارة وحساب المخالصات العمالية المحدثة</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

    <style>
        /* نظام الألوان والتنسيق البصري المريح والواضح جداً */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f6f7; 
            margin: 0;
            padding: 30px;
            color: #212529;
        }
        .container {
            max-width: 1000px;
            background: #ffffff;
            margin: 0 auto;
            padding: 35px;
            border-radius: 12px;
            box-shadow: 0 4px 25px rgba(0,0,0,0.08);
            border-top: 6px solid #457b9d;
        }
        h2 {
            text-align: center;
            color: #1d3557;
            margin-bottom: 5px;
            font-weight: 700;
            font-size: 24px;
        }
        h4 {
            text-align: center;
            color: #457b9d;
            margin-top: 0;
            margin-bottom: 35px;
            font-size: 16px;
            font-weight: 500;
        }
        .form-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 25px;
            background-color: #f8f9fa;
            padding: 25px;
            border-radius: 8px;
            border: 1px solid #dee2e6;
        }
        .form-group {
            display: flex;
            flex-direction: column;
        }
        .form-group.full-width {
            grid-column: span 2;
        }
        label {
            margin-bottom: 8px;
            font-weight: 600;
            color: #343a40;
            font-size: 14px;
        }
        input, select {
            padding: 12px;
            border: 1px solid #ced4da;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 15px;
            color: #212529;
            background-color: #ffffff;
        }
        input:focus, select:focus {
            outline: none;
            border-color: #457b9d;
            box-shadow: 0 0 0 3px rgba(69, 123, 157, 0.15);
        }
        
        /* إبراز حقل الخصم وتلوينه باللون الأحمر للتنبيه الواضح */
        .deduction-input-group label {
            color: #e63946 !important;
            font-weight: 700;
        }
        .deduction-input-group input {
            border-color: #f5c2c7;
            color: #e63946;
            font-weight: bold;
            background-color: #fff8f8;
        }

        #article77Container {
            display: none;
            background-color: #e8f1f5;
            padding: 15px;
            border-right: 4px solid #457b9d;
            border-radius: 4px;
            margin-top: 10px;
        }
        .btn-calc {
            width: 100%;
            padding: 16px;
            background-color: #2a9d8f;
            border: none;
            color: white;
            font-size: 18px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            transition: background 0.2s;
            box-shadow: 0 4px 6px rgba(42, 157, 143, 0.2);
        }
        .btn-calc:hover { background-color: #218c74; }
        
        .btn-pdf {
            width: 100%;
            padding: 16px;
            background-color: #457b9d;
            border: none;
            color: white;
            font-size: 18px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            margin-top: 25px;
            display: none;
            box-shadow: 0 4px 6px rgba(69, 123, 157, 0.2);
        }
        .btn-pdf:hover { background-color: #1d3557; }
        
        /* صندوق عرض وثيقة المخالصة للطباعة */
        .result-box {
            margin-top: 40px;
            padding: 35px;
            background-color: #ffffff;
            border: 2px solid #dee2e6;
            border-radius: 8px;
            display: none;
        }
        .clearance-header {
            text-align: center;
            margin-bottom: 30px;
            border-bottom: 3px solid #1d3557;
            padding-bottom: 20px;
        }
        .clearance-header h3 {
            margin: 0 0 8px 0;
            font-size: 26px;
            color: #1d3557;
            font-weight: 700;
        }
        .info-summary {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
            background-color: #f8f9fa;
            padding: 20px;
            border-radius: 6px;
            border: 1px solid #e9ecef;
            font-size: 15px;
        }
        .info-summary div {
            display: flex;
            justify-content: space-between;
            border-bottom: 1px dashed #dee2e6;
            padding-bottom: 8px;
        }
        
        /* تنسيق الجداول لتكون واضحة جداً والكلام مقروء بنسبة 100% */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 30px;
            background-color: #ffffff;
        }
        table, th, td { border: 1px solid #adb5bd; }
        th, td { padding: 14px 16px; font-size: 14px; color: #212529; }
        th { background-color: #e9ecef; color: #1d3557; font-weight: 700; text-align: center; }
        
        .text-right { text-align: right; }
        .text-center { text-align: center; }
        .text-left { text-align: left; }
        
        .total-row { font-weight: bold; background-color: #e8f1f5; }
        .total-row td { font-size: 16px; color: #1d3557; }
        .deduction td { color: #e63946; font-weight: bold; background-color: #fff5f5; }

        .signatures { display: flex; justify-content: space-between; margin-top: 50px; }
        .sig-block { width: 48%; border: 1px dashed #adb5bd; padding: 20px; border-radius: 6px; background-color: #fafafa; font-size: 14px; line-height: 2; }

        /* التحكم بالأرشيف السري */
        .archive-box {
            display: none;
            margin-top: 40px;
            background: #ffffff;
            border: 2px solid #1d3557;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 6px 20px rgba(0,0,0,0.05);
        }
        .archive-title {
            color: #1d3557;
            font-weight: bold;
            margin-bottom: 25px;
            font-size: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .archive-actions { display: flex; gap: 12px; }
        
        /* أزرار الأرشيف الجديدة والمحدثة */
        .btn-archive-pdf { background-color: #457b9d; color: white; border: none; padding: 9px 18px; border-radius: 4px; cursor: pointer; font-weight: 600; font-size: 13px; }
        .btn-archive-excel { background-color: #2a9d8f; color: white; border: none; padding: 9px 18px; border-radius: 4px; cursor: pointer; font-weight: 600; font-size: 13px; }
        .btn-clear-all { background-color: #b7094c; color: white; border: none; padding: 9px 18px; border-radius: 4px; cursor: pointer; font-size: 13px; }
        .btn-delete { background-color: #e63946; color: white; border: none; padding: 5px 12px; border-radius: 4px; cursor: pointer; font-weight: bold; }
        
        .archive-filter-area {
            background-color: #f8f9fa;
            border: 1px solid #dee2e6;
            padding: 18px;
            border-radius: 6px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        .archive-summary-row { background-color: #e8f1f5; font-weight: bold; border-top: 2px solid #1d3557; }
        .archive-summary-row td { font-size: 15px; color: #1d3557; padding: 15px; }

        .hidden-archive-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 52px;
            height: 52px;
            background-color: #e9ecef;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            border: 1px solid #ced4da;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        .hidden-archive-btn svg { width: 24px; height: 24px; fill: #6c757d; }
    </style>
</head>
<body>

<div class="container">
    <h2>منظومة إدارة وحساب المخالصات العمالية الذكية</h2>
    <h4>Labor Settlement & Clearance Management System</h4>
    
    <div class="form-grid">
        <div class="form-group">
            <label>اسم الموظف بالكامل:</label>
            <input type="text" id="empName" placeholder="أدخل اسم الموظف الثلاثي">
        </div>
        <div class="form-group">
            <label>الرقم الوظيفي (ID):</label>
            <input type="text" id="empId" placeholder="مثال: 10024">
        </div>
        <div class="form-group">
            <label>الراتب الفعلي الحسابي (الأساسي + البدلات):</label>
            <input type="number" id="salary" placeholder="مثال: 9000">
        </div>
        <div class="form-group">
            <label>تاريخ المباشرة والتعيين:</label>
            <input type="date" id="startDate">
        </div>
        <div class="form-group">
            <label>تاريخ آخر يوم عمل (الاعتمادي للفلترة):</label>
            <input type="date" id="endDate">
        </div>
        <div class="form-group">
            <label>رصيد الإجازات المستحقة المتبقية (أيام):</label>
            <input type="number" id="vacationDays" value="0">
        </div>
        <div class="form-group">
            <label>أيام العمل الفعلية بالشهر الأخير:</label>
            <input type="number" id="workingDays" value="0">
        </div>
        
        <div class="form-group full-width deduction-input-group">
            <label>إجمالي الاستقطاعات والخصومات (سُلف / عُهد مادية / جزاءات):</label>
            <input type="number" id="deductions" value="0" placeholder="يتم خصمها تلقائياً من الصافي وتظهر باللون الأحمر">
        </div>
        
        <div class="form-group full-width">
            <label>سبب إنهاء العلاقة التعاقدية (حسب نظام العمل السعودي):</label>
            <select id="reason" onchange="toggleArticle77Input()">
                <option value="standard">المادة 74 - انتهاء مدة العقد أو اتفاق الطرفين بالرضا</option>
                <option value="resignation">المادة 74 - استقالة رسمية مقدمة من الموظف</option>
                <option value="article77">المادة 77 - إنهاء العقد لسبب غير مشروع من جهة الشركة</option>
                <option value="article80">المادة 80 - فسخ العقد لارتكاب مخالفة نظامية (بدون مكافأة)</option>
            </select>
            <div id="article77Container">
                <label>مبلغ تعويض المادة 77 المعتمد يدويًا (ريال):</label>
                <input type="number" id="customComp77" value="0" placeholder="أدخل مبلغ التعويض الإضافي">
            </div>
        </div>
    </div>
    
    <button class="btn-calc" onclick="calculateSettlement()">احسب وأنشئ وثيقة المخالصة الرسمية</button>
    
    <div class="result-box" id="resultBox">
        <div id="pdfContent">
            <div class="clearance-header">
                <h3>مخالصة مالية وإبراء ذمة نهائي نهائي</h3>
                <p style="font-size: 14px; color: #555; margin: 5px 0 0 0;">وثيقة رسمية صادرة بموجب أنظمة وزارة الموارد البشرية بالمملكة العربية السعودية</p>
            </div>
            <div class="info-summary">
                <div><strong>اسم الموظف المعني:</strong><span id="printName" style="font-weight: bold; color: #1d3557;"></span></div>
                <div><strong>الرقم الوظيفي الخاص:</strong><span id="printId" style="font-weight: bold;"></span></div>
                <div><strong>مدة الخدمة الإجمالية في الشركة:</strong><span id="printDuration"></span></div>
            </div>
            <table>
                <thead>
                    <tr>
                        <th style="width: 25%;">البند المالي المستحق</th>
                        <th style="width: 55%;">تفاصيل الحسبة النظامية المستندة للنظام</th>
                        <th style="width: 20%;">المبلغ المستحق (ريال)</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td class="text-right"><b>مكافأة نهاية الخدمة</b></td>
                        <td id="eosDetail" class="text-right">-</td>
                        <td id="eosAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr>
                        <td class="text-right"><b>تعويض رصيد الإجازات</b></td>
                        <td id="vacDetail" class="text-right">-</td>
                        <td id="vacAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr>
                        <td class="text-right"><b>أجر الأيام بالشهر الأخير</b></td>
                        <td id="workDetail" class="text-right">-</td>
                        <td id="workAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr>
                        <td class="text-right"><b>تعويضات إضافية (مادة 77)</b></td>
                        <td id="compDetail" class="text-right">-</td>
                        <td id="compAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr class="deduction">
                        <td class="text-right"><b>الاستقطاعات والخصومات</b></td>
                        <td class="text-right">تطرح تلقائياً من المجموع (سُلف وعُهد وأقساط)</td>
                        <td id="deductAmount" class="text-center">0.00</td>
                    </tr>
                    <tr class="total-row">
                        <td class="text-right"><b>إجمالي صافي المستحقات النهائي</b></td>
                        <td class="text-right">صافي المبلغ المعتمد للتحويل لحساب الموظف البنكي</td>
                        <td id="totalAmount" class="text-center" style="font-weight: 700; font-size: 16px;">0.00</td>
                    </tr>
                </tbody>
            </table>
            <div class="signatures">
                <div class="sig-block"><b>جهة إعداد واعتماد التصفية (الموارد البشرية):</b><br><br>الاسم المسئول:<br>التوقيع والختم:</div>
                <div class="sig-block"><b>إقرار وتوقيع الموظف المخلى طرفه:</b><br><br>أقر أنا الموقع أعلاه باستلامي كامل مستحقاتي الموضحة.<br>التوقيع البصري:</div>
            </div>
        </div>
        <button class="btn-pdf" id="pdfBtn" onclick="exportToPDF()">تصدير المخالصة كملف PDF واضح ومكبر</button>
    </div>

    <div class="archive-box" id="archiveBox">
        <div id="archivePdfWrapper">
            <div class="archive-title">
                <span>📋 السجل العام لأرشيف ومستحقات الموظفين</span>
                <div class="archive-actions">
                    <button class="btn-archive-excel" onclick="exportArchiveToExcel()">📊 تصدير إلى ملف Excel</button>
                    <button class="btn-archive-pdf" onclick="exportArchiveToPDF()">🖨️ تصدير التقرير PDF</button>
                    <button class="btn-clear-all" onclick="clearAllArchive()">مسح الأرشيف</button>
                </div>
            </div>

            <div class="archive-filter-area">
                <div>
                    <label style="font-weight: bold; color: #1d3557;">📅 تصفية ذكية حسب سنة نهاية العمل:</label>
                    <select id="yearFilter" onchange="renderArchiveTable()">
                        <option value="all">كل السنوات المتوفرة / All Years</option>
                    </select>
                </div>
                <div id="archiveStatsCount" style="font-weight: bold; color: #457b9d; font-size: 15px;">عدد الموظفين: 0</div>
            </div>

            <table id="archiveTable">
                <thead>
                    <tr>
                        <th>الرقم الوظيفي</th>
                        <th>اسم الموظف بالكامل</th>
                        <th>تاريخ آخر يوم عمل</th>
                        <th>سبب الإنهاء المستند</th>
                        <th>صافي المستحقات الصافية (ريال)</th>
                        <th>إجراء الحذف</th>
                    </tr>
                </thead>
                <tbody id="archiveBody"></tbody>
                <tfoot>
                    <tr class="archive-summary-row">
                        <td colspan="4" style="text-align: left; padding-left: 20px;">إجمالي المبالغ والمستحقات التراكمية المعتمدة في التصفية الحالية:</td>
                        <td id="archiveSumAmount" style="text-align: center; color: #2a9d8f; font-size: 16px; font-weight: 700;">0.00</td>
                        <td></td>
                    </tr>
                </tfoot>
            </table>
        </div>
    </div>
</div>

<div class="hidden-archive-btn" onclick="accessArchive()">
    <svg viewBox="0 0 24 24"><path d="M14 2H6c-1.1 0-1.99.9-1.99 2L4 20c0 1.1.89 2 1.99 2H18c1.1 0 2-.9 2-2V8l-6-6zm2 16H8v-2h8v2zm0-4H8v-2h8v2zm-3-5V3.5L18.5 9H13z"/></svg>
</div>

<script>
const COMPANY_PASSWORD = "14171997";

function toggleArticle77Input() {
    const reason = document.getElementById('reason').value;
    document.getElementById('article77Container').style.display = reason === 'article77' ? 'block' : 'none';
}

function calculateSettlement() {
    const name = document.getElementById('empName').value || 'الموظف المكرم';
    const empId = document.getElementById('empId').value || 'N/A';
    const salary = parseFloat(document.getElementById('salary').value) || 0;
    const startDate = new Date(document.getElementById('startDate').value);
    const endDate = new Date(document.getElementById('endDate').value);
    const vacationDays = parseFloat(document.getElementById('vacationDays').value) || 0;
    const workingDays = parseFloat(document.getElementById('workingDays').value) || 0;
    const deductions = parseFloat(document.getElementById('deductions').value) || 0;
    const reason = document.getElementById('reason').value;
    const customComp77 = parseFloat(document.getElementById('customComp77').value) || 0;

    if (isNaN(startDate.getTime()) || isNaN(endDate.getTime())) {
        alert('الرجاء تعيين التواريخ بشكل كامل وصحيح أولاً.');
        return;
    }

    const diffTime = Math.abs(endDate - startDate);
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
    const totalYears = diffDays / 365;

    document.getElementById('printDuration').innerText = `${Math.floor(totalYears)} سنة و ${Math.floor((diffDays % 365) / 30)} شهر`;

    let baseEos = totalYears <= 5 ? totalYears * (salary / 2) : (5 * (salary / 2)) + ((totalYears - 5) * salary);
    let finalEos = reason === "resignation" ? (totalYears < 2 ? 0 : baseEos * (1/3)) : (reason === "article80" ? 0 : baseEos);

    const vacationCompensation = (salary / 30) * vacationDays;
    const workingDaysCompensation = (salary / 30) * workingDays;
    let compensation77 = reason === "article77" ? customComp77 : 0;
    
    // الحسبة الحسابية الرياضية النهائية الصافية
    const totalSettlement = (finalEos + vacationCompensation + workingDaysCompensation + compensation77) - deductions;

    document.getElementById('printName').innerText = name;
    document.getElementById('printId').innerText = empId;
    
    document.getElementById('eosAmount').innerText = finalEos.toFixed(2);
    document.getElementById('vacAmount').innerText = vacationCompensation.toFixed(2);
    document.getElementById('workAmount').innerText = workingDaysCompensation.toFixed(2);
    document.getElementById('compAmount').innerText = compensation77.toFixed(2);
    
    // إبراز علامة طرح الخصومات باللون الأحمر الصريح
    document.getElementById('deductAmount').innerText = deductions > 0 ? `- ${deductions.toFixed(2)}` : "0.00";
    document.getElementById('totalAmount').innerText = totalSettlement.toFixed(2);

    document.getElementById('resultBox').style.display = 'block';
    document.getElementById('pdfBtn').style.display = 'block';

    // حفظ البيانات كـ أرقام نقية بالأرشيف لمنع تداخل النصوص
    saveToArchive({
        id: empId,
        name: name,
        date: document.getElementById('endDate').value,
        reason: document.getElementById('reason').options[document.getElementById('reason').selectedIndex].text.split('-')[0].trim(),
        total: parseFloat(totalSettlement.toFixed(2))
    });
}

function saveToArchive(empData) {
    let archive = JSON.parse(localStorage.getItem('hr_archive')) || [];
    archive = archive.filter(item => item.id !== empData.id);
    archive.push(empData);
    localStorage.setItem('hr_archive', JSON.stringify(archive));
    buildYearFilterOptions();
    if (document.getElementById('archiveBox').style.display === 'block') {
        renderArchiveTable();
    }
}

function accessArchive() {
    const archiveBox = document.getElementById('archiveBox');
    if (archiveBox.style.display === 'block') {
        archiveBox.style.display = 'none';
        return;
    }
    const password = prompt("الرجاء كتابة رمز الدخول السري لعرض أرشيف التصفية والرواتب:");
    if (password === COMPANY_PASSWORD) {
        archiveBox.style.display = 'block';
        buildYearFilterOptions();
        renderArchiveTable();
    } else if (password !== null) {
        alert("رمز الدخول السري غير صحيح!");
    }
}

function buildYearFilterOptions() {
    const archive = JSON.parse(localStorage.getItem('hr_archive')) || [];
    const filterSelect = document.getElementById('yearFilter');
    const selectedValue = filterSelect.value;
    
    let years = [];
    archive.forEach(emp => {
        if (emp.date) {
            const year = new Date(emp.date).getFullYear();
            if (!isNaN(year) && !years.includes(year)) years.push(year);
        }
    });
    years.sort((a, b) => b - a);

    filterSelect.innerHTML = '<option value="all">كل السنوات المتوفرة / All Years</option>';
    years.forEach(year => {
        filterSelect.innerHTML += `<option value="${year}">${year}</option>`;
    });
    if (years.includes(parseInt(selectedValue))) filterSelect.value = selectedValue;
}

function renderArchiveTable() {
    const archive = JSON.parse(localStorage.getItem('hr_archive')) || [];
    const tbody = document.getElementById('archiveBody');
    const selectedYear = document.getElementById('yearFilter').value;
    tbody.innerHTML = '';

    const filteredData = archive.filter(emp => {
        if (selectedYear === 'all') return true;
        if (!emp.date) return false;
        return new Date(emp.date).getFullYear().toString() === selectedYear;
    });

    document.getElementById('archiveStatsCount').innerText = `عدد الموظفين في هذه السنة: ${filteredData.length}`;

    if (filteredData.length === 0) {
        tbody.innerHTML = `<tr><td colspan="6" style="text-align:center; color:#888; padding:20px;">لا توجد سجلات مؤرشفة تابعة للفلتر المختار.</td></tr>`;
        document.getElementById('archiveSumAmount').innerText = "0.00";
        return;
    }

    // إعداد الحسبة الرياضية الصحيحة 100% للتراكمي
    let runningTotalSum = 0.0;

    filteredData.forEach((emp) => {
        const empTotalValue = parseFloat(emp.total) || 0;
        runningTotalSum += empTotalValue;
        
        const totalStyle = empTotalValue < 0 ? 'color:#e63946; font-weight:bold; background-color:#fff5f5;' : 'color:#2a9d8f; font-weight:bold;';

        tbody.innerHTML += `
            <tr>
                <td style="text-align:center;"><b>${emp.id}</b></td>
                <td><b>${emp.name}</b></td>
                <td style="text-align:center;">${emp.date || 'N/A'}</td>
                <td>${emp.reason}</td>
                <td style="text-align:center; ${totalStyle}">${empTotalValue.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                <td style="text-align:center;"><button class="btn-delete" onclick="deleteEmp('${emp.id}')">حذف</button></td>
            </tr>
        `;
    });

    document.getElementById('archiveSumAmount').innerText = runningTotalSum.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
}

// 📊 دالة تصدير البيانات إلى ملف Excel الحقيقي والمنظم بضغطة زر واحدة
function exportArchiveToExcel() {
    const archive = JSON.parse(localStorage.getItem('hr_archive')) || [];
    const selectedYear = document.getElementById('yearFilter').value;
    
    const filteredData = archive.filter(emp => {
        if (selectedYear === 'all') return true;
        return emp.date && new Date(emp.date).getFullYear().toString() === selectedYear;
    });

    if (filteredData.length === 0) {
        alert("لا توجد بيانات لتصديرها إلى إكسل!");
        return;
    }

    // تجهيز لستة الداتا والصفوف للإكسل في جدول مرتب
    const excelData = filteredData.map(emp => ({
        "الرقم الوظيفي": emp.id,
        "اسم الموظف": emp.name,
        "تاريخ آخر يوم عمل": emp.date,
        "سبب إنهاء الخدمة": emp.reason,
        "صافي مستحقات الموظف (ريال)": emp.total
    }));

    // إنشاء ورقة العمل والمصنف للتحميل الفوري
    const worksheet = XLSX.utils.json_to_sheet(excelData);
    const workbook = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(workbook, worksheet, "المخالصات المؤرشفة");

    // تنزيل ملف الإكسل باسم مرن حسب السنة المحددة في الفلتر
    XLSX.writeFile(workbook, `سجل_مخالصات_الموظفين_لسنة_${selectedYear}.xlsx`);
}

function deleteEmp(id) {
    if (confirm("هل تريد تأكيد حذف سجل هذا الموظف نهائياً؟")) {
        let archive = JSON.parse(localStorage.getItem('hr_archive')) || [];
        archive = archive.filter(item => item.id !== id);
        localStorage.setItem('hr_archive', JSON.stringify(archive));
        buildYearFilterOptions();
        renderArchiveTable();
    }
}

function clearAllArchive() {
    if (confirm("⚠️ تحذير: هل أنت متأكد من مسح وإفراغ سجل الأرشيف بالكامل؟")) {
        localStorage.removeItem('hr_archive');
        buildYearFilterOptions();
        renderArchiveTable();
    }
}

// 🖨️ تحسين دقة تصدير الـ PDF وكبر الخطوط لتكون مقروءة وواضحة جداً كصورة أو مستند رسمي رائع
function exportToPDF() {
    const element = document.getElementById('pdfContent');
    const empName = document.getElementById('empName').value || 'Employee';
    const opt = {
        margin:       [15, 15, 15, 15],
        filename:     `مخالصة_الموظف_${empName}.pdf`,
        image:        { type: 'jpeg', quality: 1.0 },
        html2canvas:  { scale: 2.5, useCORS: true, letterRendering: true }, // رفع الاسكيل ليكون خطوط الكلام واضحة جداً ومكبرة
        jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
    };
    html2pdf().set(opt).from(element).save();
}

function exportArchiveToPDF() {
    const element = document.getElementById('archiveTable');
    const selectedYear = document.getElementById('yearFilter').value;
    const opt = {
        margin:       [15, 15, 15, 15],
        filename:     `تقرير_الأرشيف_سنة_${selectedYear}.pdf`,
        image:        { type: 'jpeg', quality: 1.0 },
        html2canvas:  { scale: 2.5, useCORS: true },
        jsPDF:        { unit: 'mm', format: 'a4', orientation: 'landscape' } // عرض لاندسكيب ليناسب صفوف الجدول بوضوح كلي
    };
    html2pdf().set(opt).from(element).save();
}
</script>
</body>
</html>
