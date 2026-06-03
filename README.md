
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منظومة المخالصات العمالية | Employee Settlement System</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        /* نظام الألوان المعتمد: رمادي، أزرق فاتح، أخضر فاتح */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f6f7; 
            margin: 0;
            padding: 30px;
            color: #333333;
        }
        .container {
            max-width: 950px;
            background: #ffffff;
            margin: 0 auto;
            padding: 35px;
            border-radius: 12px;
            box-shadow: 0 4px 25px rgba(0,0,0,0.06);
            border-top: 6px solid #a8dadc; /* أزرق فاتح */
            position: relative;
        }
        h2 {
            text-align: center;
            color: #1d3557;
            margin-bottom: 5px;
            font-weight: 700;
        }
        h4 {
            text-align: center;
            color: #457b9d;
            margin-top: 0;
            margin-bottom: 35px;
            font-weight: 500;
        }
        .form-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 25px;
            background-color: #f8f9fa; /* رمادي فاتح */
            padding: 25px;
            border-radius: 8px;
            border: 1px solid #e9ecef;
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
            color: #495057;
            font-size: 14px;
        }
        label span {
            color: #457b9d;
            font-size: 13px;
            font-weight: normal;
        }
        input, select {
            padding: 12px;
            border: 1px solid #ced4da;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 14px;
            background-color: #ffffff;
        }
        input:focus, select:focus {
            outline: none;
            border-color: #457b9d;
        }
        /* خانة المادة 77 الديناميكية */
        #article77Container {
            display: none;
            background-color: #e8f1f5;
            padding: 15px;
            border-right: 4px solid #457b9d;
            border-radius: 4px;
            margin-top: 10px;
        }
        /* أزرار مخصصة بالألوان الجديدة */
        .btn-calc {
            width: 100%;
            padding: 15px;
            background-color: #2a9d8f; /* أخضر فاتح */
            border: none;
            color: white;
            font-size: 18px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            transition: background 0.3s;
        }
        .btn-calc:hover {
            background-color: #218c74;
        }
        .btn-pdf {
            width: 100%;
            padding: 15px;
            background-color: #457b9d; /* أزرق فاتح */
            border: none;
            color: white;
            font-size: 18px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            transition: background 0.3s;
            margin-top: 25px;
            display: none;
        }
        .btn-pdf:hover {
            background-color: #1d3557;
        }
        
        /* تصميم ورقة كشف حساب المخالصة الرسمية المزدوجة اللغات */
        .result-box {
            margin-top: 40px;
            padding: 30px;
            background-color: #ffffff;
            border: 1px solid #dee2e6;
            border-radius: 8px;
            display: none;
        }
        .clearance-header {
            text-align: center;
            margin-bottom: 30px;
            border-bottom: 2px solid #457b9d;
            padding-bottom: 20px;
        }
        .clearance-header h3 {
            margin: 0 0 5px 0;
            font-size: 24px;
            color: #1d3557;
        }
        .clearance-header h4 {
            margin: 0 0 10px 0;
            font-size: 18px;
            color: #457b9d;
        }
        .clearance-header p {
            margin: 2px 0;
            color: #6c757d;
            font-size: 13px;
        }
        .info-summary {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 25px;
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 6px;
            border: 1px solid #e9ecef;
            font-size: 14px;
        }
        .info-summary div {
            display: flex;
            justify-content: space-between;
            border-bottom: 1px dashed #dee2e6;
            padding-bottom: 5px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 25px;
        }
        table, th, td {
            border: 1px solid #cfd4da;
        }
        th, td {
            padding: 12px 15px;
            font-size: 13px;
        }
        th {
            background-color: #e9ecef;
            color: #1d3557;
            font-weight: 600;
            text-align: center;
        }
        .text-right { text-align: right; }
        .text-left { text-align: left; }
        .text-center { text-align: center; }
        
        .total-row {
            font-weight: bold;
            background-color: #e8f1f5;
        }
        .deduction td {
            color: #b7094c;
        }
        /* نص الإقرار والتعهد ثنائي اللغة بقسمين متوازيين */
        .legal-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            border: 1px solid #ced4da;
            padding: 20px;
            background-color: #f8f9fa;
            border-radius: 6px;
            margin-top: 25px;
        }
        .legal-block {
            font-size: 13px;
            line-height: 1.7;
            text-align: justify;
            color: #212529;
        }
        .legal-block.ar { dir: rtl; }
        .legal-block.en { dir: ltr; }
        .legal-block strong {
            color: #1d3557;
            font-size: 14px;
        }
        .highlight-text {
            font-weight: bold;
            color: #1d3557;
            text-decoration: underline;
        }
        /* التواقيع والاعتمادات جنباً إلى جنب */
        .signatures {
            display: flex;
            justify-content: space-between;
            margin-top: 40px;
            padding-top: 10px;
        }
        .sig-block {
            width: 48%;
            font-size: 13px;
            line-height: 2;
            background: #ffffff;
            border: 1px dashed #ced4da;
            padding: 15px;
            border-radius: 6px;
        }

        /* 📞 تصميم زر النافذة العائمة المنبثقة للدعم الفني */
        .support-btn {
            position: fixed;
            bottom: 30px;
            left: 30px;
            width: 60px;
            height: 60px;
            background-color: #a8dadc; /* أزرق فاتح مائل للأخضر البارد */
            border-radius: 50%;
            box-shadow: 0 4px 15px rgba(0,0,0,0.15);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 9999;
            transition: transform 0.2s;
        }
        .support-btn:hover {
            transform: scale(1.1);
        }
        .support-btn svg {
            width: 28px;
            height: 28px;
            fill: #1d3557;
        }
        /* نافذة الدعم الفني المنبثقة (Popup) */
        .support-popup {
            position: fixed;
            bottom: 105px;
            left: 30px;
            width: 320px;
            background: #ffffff;
            border-radius: 14px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
            border: 1px solid #e9ecef;
            display: none;
            flex-direction: column;
            padding: 20px;
            z-index: 10000;
            animation: fadeIn 0.3s ease-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .support-popup-title {
            font-size: 16px;
            font-weight: bold;
            color: #1d3557;
            text-align: center;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .support-link {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            padding: 12px 15px;
            margin-bottom: 10px;
            border-radius: 10px;
            text-decoration: none;
            transition: background 0.2s;
            border-right: 5px solid;
        }
        .support-link.whatsapp {
            background-color: #f1fbf4;
            border-color: #2a9d8f; /* أخضر */
        }
        .support-link.whatsapp:hover { background-color: #e3f7eb; }
        .support-link.linkedin {
            background-color: #f0f7fa;
            border-color: #457b9d; /* أزرق */
        }
        .support-link.linkedin:hover { background-color: #e1f0f7; }
        
        .support-link-label {
            font-size: 14px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .support-link.whatsapp .support-link-label { color: #2a9d8f; }
        .support-link.linkedin .support-link-label { color: #457b9d; }
        
        .support-link-val {
            font-size: 13px;
            color: #333333;
            margin-top: 4px;
            word-break: break-all;
            font-family: monospace;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>منظومة إدارة وحساب المخالصات العمالية</h2>
    <h4>Labor Settlement & Clearance Management System</h4>
    
    <div class="form-grid">
        <div class="form-group">
            <label>اسم الموظف الثلاثي / <span>Employee Full Name:</span></label>
            <input type="text" id="empName" placeholder="أدخل اسم الموظف / Enter Name">
        </div>

        <div class="form-group">
            <label>الرقم الوظيفي / <span>Employee ID:</span></label>
            <input type="text" id="empId" placeholder="مثال: 100245">
        </div>
        
        <div class="form-group">
            <label>الراتب الفعلي الحسابي / <span>Actual Salary (Basic + Allowances):</span></label>
            <input type="number" id="salary" placeholder="مثال: 9000">
        </div>
        
        <div class="form-group">
            <label>تاريخ المباشرة / <span>Joining Date:</span></label>
            <input type="date" id="startDate">
        </div>
        
        <div class="form-group">
            <label>تاريخ آخر يوم عمل / <span>Last Working Date:</span></label>
            <input type="date" id="endDate">
        </div>
        
        <div class="form-group">
            <label>رصيد الإجازات المستحقة (بالأيام) / <span>Vacation Balance (Days):</span></label>
            <input type="number" id="vacationDays" value="0">
        </div>

        <div class="form-group">
            <label>أيام العمل المستحقة بالشهر الأخير / <span>Worked Days in Last Month:</span></label>
            <input type="number" id="workingDays" value="0">
        </div>

        <div class="form-group full-width">
            <label>إجمالي الاستقطاعات والخصومات (سُلف/عُهد) / <span>Total Deductions (Loans/Dues):</span></label>
            <input type="number" id="deductions" value="0" placeholder="تطرح من الصافي / Will be deducted">
        </div>
        
        <div class="form-group full-width">
            <label>سبب إنهاء العلاقة التعاقدية (نظام العمل) / <span>Reason of Termination (Saudi Labor Law):</span></label>
            <select id="reason" onchange="toggleArticle77Input()">
                <option value="standard">المادة 74 - انتهاء مدة العقد أو اتفاق الطرفين / Art. 74 - Contract Expiry or Mutual Agreement</option>
                <option value="resignation">المادة 74 - استقالة الموظف / Art. 74 - Employee Resignation</option>
                <option value="article75">المادة 75 - إنهاء عقد غير محدد المدة بناءً على سبب مشروع / Art. 75 - Termination of Unspecified Contract</option>
                <option value="article77">المادة 77 - إنهاء العقد لسبب غير مشروع من الشركة / Art. 77 - Termination for Unspecified Reason</option>
                <option value="article80">المادة 80 - فسخ العقد بدون مكافأة لحالات النظام / Art. 80 - Dismissal Without End of Service</option>
                <option value="article82">المادة 82 - إنهاء العقد بسبب العجز أو المرض المستمر / Art. 82 - Termination Due to Illness/Disability</option>
            </select>

            <div id="article77Container">
                <label style="color: #457b9d;">مبلغ تعويض المادة 77 المعتمد (ريال) / <span>Art. 77 Approved Compensation Amount (SAR):</span></label>
                <input type="number" id="customComp77" value="0" placeholder="أدخل مبلغ التعويض يدوياً">
            </div>
        </div>
    </div>
    
    <button class="btn-calc" onclick="calculateSettlement()">احسب وأنشئ وثيقة المخالصة | Calculate & Generate Clearance</button>
    
    <div class="result-box" id="resultBox">
        <div id="pdfContent">
            <div class="clearance-header">
                <h3>مخالصة مالية وإبراء ذمة نهائي</h3>
                <h4>Final Financial Settlement & Clearance Certificate</h4>
                <p>صادرة طبقاً لأنظمة وزارة الموارد البشرية والتنمية الاجتماعية بالمملكة العربية السعودية</p>
                <p dir="ltr" style="font-style: italic;">Issued in accordance with the regulations of the Ministry of Human Resources and Social Development - KSA</p>
            </div>

            <div class="info-summary">
                <div>
                    <strong>اسم الموظف / Employee Name:</strong>
                    <span id="printName"></span>
                </div>
                <div>
                    <strong>الرقم الوظيفي / Employee ID:</strong>
                    <span id="printId" style="font-weight: bold; color: #1d3557;"></span>
                </div>
                <div>
                    <strong>تاريخ الكشف / Date of Issue:</strong>
                    <span id="printDate"></span>
                </div>
                <div>
                    <strong>مدة الخدمة الإجمالية / Service Period:</strong>
                    <span id="printDuration"></span>
                </div>
            </div>
            
            <table>
                <thead>
                    <tr>
                        <th style="width: 25%;">البند المالي <br><span dir="ltr" style="font-size:11px; font-weight:normal; color:#666;">Financial Item</span></th>
                        <th style="width: 55%;">تفاصيل ومستند الحسبة النظامية <br><span dir="ltr" style="font-size:11px; font-weight:normal; color:#666;">Legal Calculation & Reference Details</span></th>
                        <th style="width: 20%;">المبلغ المستحق (ريال) <br><span dir="ltr" style="font-size:11px; font-weight:normal; color:#666;">Amount (SAR)</span></th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td class="text-right"><b>مكافأة نهاية الخدمة</b><br><span dir="ltr" style="font-size:11px; color:#666;">End of Service Award (EOS)</span></td>
                        <td id="eosDetail" class="text-right">-</td>
                        <td id="eosAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr>
                        <td class="text-right"><b>تعويض رصيد الإجازات</b><br><span dir="ltr" style="font-size:11px; color:#666;">Vacation Balance Compensation</span></td>
                        <td id="vacDetail" class="text-right">-</td>
                        <td id="vacAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr>
                        <td class="text-right"><b>أجر الأيام بالشهر الأخير</b><br><span dir="ltr" style="font-size:11px; color:#666;">Last Month Worked Days Salary</span></td>
                        <td id="workDetail" class="text-right">-</td>
                        <td id="workAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr>
                        <td class="text-right"><b>التعويضات الإضافية (مادة 77)</b><br><span dir="ltr" style="font-size:11px; color:#666;">Additional Compensation (Art. 77)</span></td>
                        <td id="compDetail" class="text-right">-</td>
                        <td id="compAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr class="deduction">
                        <td class="text-right"><b>الاستقطاعات والخصومات</b><br><span dir="ltr" style="font-size:11px; color:#b7094c;">Deductions & Deductibles</span></td>
                        <td class="text-right">سُلف / عُهد مادية / غيابات مستقطعة <br><span dir="ltr" style="font-size:11px; color:#666;">Loans / Company Assets / Legal Deductions</span></td>
                        <td id="deductAmount" class="text-center" style="font-weight: bold;">0.00</td>
                    </tr>
                    <tr class="total-row">
                        <td class="text-right"><b>إجمالي صافي المستحقات</b><br><span dir="ltr" style="font-size:11px; color:#1d3557;">Total Net Settlement</span></td>
                        <td class="text-right">صافي المبلغ النهائي المعتمد للتحويل البنكي <br><span dir="ltr" style="font-size:11px; color:#1d3557;">Net amount approved for bank transfer</span></td>
                        <td id="totalAmount" class="text-center" style="color:#1d3557; font-size:15px;">0.00</td>
                    </tr>
                </tbody>
            </table>

            <div class="legal-container">
                <div class="legal-block ar">
                    <strong>إقرار وتعهد الموظف بالصحة والمعاينة:</strong><br>
                    أقر أنا الموظف/ <span id="legalNameAr" class="highlight-text">......................</span>، بالرقم الوظيفي/ <span id="legalIdAr" class="highlight-text">......................</span> وبكامل أهليتي المعتبرة شرعاً ونظاماً، بأنني اطلعت ودققت وراجعت كافة البنود المالية وتفاصيل الحسبة الواردة في هذا الكشف بدقة، وأؤكد صحتها وسلامة احتسابها وعدم وجود أي مبالغ ناقصة. وبناءً عليه، فإنني أقر بأن استلامي لصافي المبلغ الموضح أعلاه وقدره (<span id="textTotalAr" style="font-weight:bold;"></span> ريال سعودي) يعتبر تسوية نهائية وشاملة لكافة حقوقي العمالية، وأبرئ ذمة الشركة إبراءً مطلقاً وشاملاً لا رجعة فيه من أي حق أو دعوى عمالية حالية أو مستقبلية، والتوقيع يعد تعهداً بالصحة ويتم التحويل فوراً للبنك.
                </div>
                <div class="legal-block en">
                    <strong>Employee Acknowledgment & Declaration:</strong><br>
                    I, the undersigned employee/ <span id="legalNameEn" class="highlight-text">......................</span>, holding Employee ID/ <span id="legalIdEn" class="highlight-text">......................</span>, hereby declare with full legal capacity that I have reviewed, verified, and audited all financial items and calculation details listed in this statement. I confirm their accuracy and completeness. Accordingly, I acknowledge that receiving the net amount of (<span id="textTotalEn" style="font-weight:bold;"></span> SAR) represents a final and comprehensive settlement of all my labor and contractual rights. I fully, absolutely, and irrevocably release the company from any current or future labor claims or lawsuits.
                </div>
            </div>

            <div class="signatures">
                <div class="sig-block" dir="rtl">
                    <strong>إعداد واعتماد (الموارد البشرية):</strong><br>
                    Prepared & Approved by (HR Dept):<br>
                    الاسم / Name: ............................................<br>
                    التوقيع / Signature: ....................................<br>
                    التاريخ / Date: .... / .... / 2026م
                </div>
                <div class="sig-block" dir="rtl">
                    <strong>الموظف المقر بما فيه (صاحب العلاقة):</strong><br>
                    Employee Acknowledgment & Signature:<br>
                    الاسم / Name: ............................................<br>
                    التوقيع / Signature: ....................................<br>
                    التاريخ / Date: .... / .... / 2026م
                </div>
            </div>
        </div>
        
        <button class="btn-pdf" id="pdfBtn" onclick="exportToPDF()">تصدير المخالصة كملف PDF فوراً | Export to PDF Now</button>
    </div>
</div>

<div class="support-btn" onclick="toggleSupportPopup()" title="للتواصل والدعم الفني">
    <svg viewBox="0 0 24 24">
        <path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z"/>
    </svg>
</div>

<div class="support-popup" id="supportPopup">
    <div class="support-popup-title">
        <span>📞 للتواصل والدعم الفني</span>
    </div>
    
    <a href="https://wa.me/966532219777" target="_blank" class="support-link whatsapp">
        <div class="support-link-label">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor" style="vertical-align:middle;"><path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946C.06 5.348 5.397.01 12.008.01c3.202.001 6.212 1.246 8.477 3.514 2.266 2.268 3.507 5.28 3.505 8.484-.004 6.657-5.34 11.997-11.953 11.997-2.005-.001-3.973-.502-5.713-1.455L0 24zm6.59-4.846c1.6.95 3.188 1.449 4.825 1.451 5.436 0 9.86-4.37 9.864-9.742.002-2.602-1.01-5.05-2.85-6.892-1.84-1.842-4.29-2.856-6.894-2.856-5.442 0-9.866 4.372-9.87 9.746-.002 1.741.465 3.441 1.352 4.954l-.452 1.65 1.692-.444z"/></svg>
            WhatsApp:
        </div>
        <div class="support-link-val">0532219777</div>
    </a>
    
    <a href="https://www.linkedin.com/in/majed-alharbi-shrm-cp-2651071b6" target="_blank" class="support-link linkedin">
        <div class="support-link-label">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor" style="vertical-align:middle;"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
            LinkedIn:
        </div>
        <div class="support-link-val">majed-alharbi-shrm-cp-2651071b6</div>
    </a>
</div>

<script>
// دالة إظهار وإخفاء نافذة الدعم الفني المنبثقة
function toggleSupportPopup() {
    const popup = document.getElementById('supportPopup');
    if (popup.style.display === 'flex') {
        popup.style.display = 'none';
    } else {
        popup.style.display = 'flex';
    }
}

// دالة إظهار الحقل المخصص للمادة 77 ديناميكياً
function toggleArticle77Input() {
    const reason = document.getElementById('reason').value;
    const container = document.getElementById('article77Container');
    if (reason === 'article77') {
        container.style.display = 'block';
    } else {
        container.style.display = 'none';
        document.getElementById('customComp77').value = 0;
    }
}

function calculateSettlement() {
    const name = document.getElementById('empName').value || 'الموظف المكرم / Respected Employee';
    const empId = document.getElementById('empId').value || 'N/A';
    const salary = parseFloat(document.getElementById('salary').value);
    const startDate = new Date(document.getElementById('startDate').value);
    const endDate = new Date(document.getElementById('endDate').value);
    const vacationDays = parseFloat(document.getElementById('vacationDays').value) || 0;
    const workingDays = parseFloat(document.getElementById('workingDays').value) || 0;
    const deductions = parseFloat(document.getElementById('deductions').value) || 0;
    const reason = document.getElementById('reason').value;
    const customComp77 = parseFloat(document.getElementById('customComp77').value) || 0;

    if (isNaN(salary) || isNaN(startDate.getTime()) || isNaN(endDate.getTime())) {
        alert('الرجاء إدخال البيانات والتواريخ بشكل صحيح \n Please enter valid salary and dates.');
        return;
    }
    if (endDate < startDate) {
        alert('خطأ: تاريخ النهاية يسبق تاريخ البداية \n Error: Last working date is before joining date.');
        return;
    }

    // حساب مدة الخدمة
    const diffTime = Math.abs(endDate - startDate);
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
    const totalYears = diffDays / 365;

    const yearsDisplay = Math.floor(totalYears);
    const monthsDisplay = Math.floor((totalYears - yearsDisplay) * 12);
    const daysDisplay = Math.round(diffDays - (yearsDisplay * 365) - (monthsDisplay * 30.41));
    
    document.getElementById('printDuration').innerText = 
        `${yearsDisplay} سنة/Years - ${monthsDisplay} شهر/Months - ${daysDisplay} يوم/Days`;

    // 1. حساب مكافأة نهاية الخدمة طبقاً للمواد القانونية
    let baseEos = totalYears <= 5 ? totalYears * (salary / 2) : (5 * (salary / 2)) + ((totalYears - 5) * salary);
    let finalEos = 0, eosDetailText = "";

    if (reason === "standard" || reason === "article77" || reason === "article75" || reason === "article82") {
        finalEos = baseEos;
        eosDetailText = "مكافأة كاملة نظامية (نصف راتب لأول 5 سنوات + راتب كامل للتالي) <br> <span dir='ltr' style='font-size:11px; color:#666;'>Full EOS Award (Half salary for first 5 years + Full salary for remaining years)</span>";
    } else if (reason === "resignation") {
        if (totalYears < 2) { 
            finalEos = 0; 
            eosDetailText = "المادة 74 (استقالة: خدمة أقل من سنتين - لا يستحق) <br> <span dir='ltr' style='font-size:11px; color:#666;'>Art. 74 (Resignation: Service < 2 Years - No Award)</span>"; 
        } else if (totalYears >= 2 && totalYears < 5) { 
            finalEos = baseEos * (1/3); 
            eosDetailText = "المادة 74 (استقالة: يستحق ثلث المكافأة) <br> <span dir='ltr' style='font-size:11px; color:#666;'>Art. 74 (Resignation: Entitled to 1/3 of Award)</span>"; 
        } else if (totalYears >= 5 && totalYears < 10) { 
            finalEos = baseEos * (2/3); 
            eosDetailText = "المادة 74 (استقالة: يستحق ثلثي المكافأة) <br> <span dir='ltr' style='font-size:11px; color:#666;'>Art. 74 (Resignation: Entitled to 2/3 of Award)</span>"; 
        } else { 
            finalEos = baseEos; 
            eosDetailText = "المادة 74 (استقالة: خدمة فوق 10 سنوات - كاملة) <br> <span dir='ltr' style='font-size:11px; color:#666;'>Art. 74 (Resignation: Service > 10 Years - Full Award)</span>"; 
        }
    } else if (reason === "article80") {
        finalEos = 0;
        eosDetailText = "فسخ بموجب المادة 80 (سقوط الحق في المكافأة نظاماً) <br> <span dir='ltr' style='font-size:11px; color:#666;'>Art. 80 (Dismissal: EOS Award forfeited legally)</span>";
    }

    // 2. تعويض رصيد الإجازات
    const vacationCompensation = (salary / 30) * vacationDays;
    const vacDetailText = `أجر يومي (${(salary/30).toFixed(2)} ريال) × ${vacationDays} يوم إجازة <br> <span dir='ltr' style='font-size:11px; color:#666;'>Daily Wage × ${vacationDays} Days of Vacation balance</span>`;

    // 3. أجر الأيام المستحقة بالشهر الأخير
    const workingDaysCompensation = (salary / 30) * workingDays;
    const workDetailText = `أجر الأيام المستحقة لكسر الشهر × ${workingDays} يوم عمل <br> <span dir='ltr' style='font-size:11px; color:#666;'>Worked days salary for partial month × ${workingDays} Days</span>`;

    // 4. تعويض المادة 77
    let compensation77 = 0, compDetailText = "لا يوجد تعويض مدرج / No compensation added";
    if (reason === "article77") {
        compensation77 = customComp77;
        compDetailText = `تعويض مادة 77 محدد يدوياً بناءً على القرار أو الاتفاق <br> <span dir='ltr' style='font-size:11px; color:#666;'>Art. 77 compensation specified manually based on agreement</span>`;
    }

    // الحسبة الصافية الإجمالية
    const totalSettlement = (finalEos + vacationCompensation + workingDaysCompensation + compensation77) - deductions;

    // تعبئة حقول المخالصة وتفاصيل جدول الطباعة والملخص العلوي
    document.getElementById('printName').innerText = name;
    document.getElementById('printId').innerText = empId;
    document.getElementById('printDate').innerText = new Date().toLocaleDateString('ar-SA') + "م / " + new Date().toLocaleDateString('en-US');
    
    // ربط وعكس الاسم والرقم الوظيفي داخل نص التعهد القانوني تلقائياً
    document.getElementById('legalNameAr').innerText = name;
    document.getElementById('legalIdAr').innerText = empId;
    document.getElementById('legalNameEn').innerText = name;
    document.getElementById('legalIdEn').innerText = empId;

    document.getElementById('eosDetail').innerHTML = eosDetailText;
    document.getElementById('eosAmount').innerText = finalEos.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('vacDetail').innerHTML = vacDetailText;
    document.getElementById('vacAmount').innerText = vacationCompensation.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('workDetail').innerHTML = workDetailText;
    document.getElementById('workAmount').innerText = workingDaysCompensation.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('compDetail').innerHTML = compDetailText;
    document.getElementById('compAmount').innerText = compensation77.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    
    document.getElementById('deductAmount').innerText = deductions > 0 ? `-${deductions.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}` : "0.00";
    
    const formattedTotal = totalSettlement.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    document.getElementById('totalAmount').innerText = formattedTotal;
    document.getElementById('textTotalAr').innerText = formattedTotal;
    document.getElementById('textTotalEn').innerText = formattedTotal;

    // إظهار النتائج وزر الـ PDF المباشر فوراً
    document.getElementById('resultBox').style.display = 'block';
    document.getElementById('pdfBtn').style.display = 'block';
}

// دالة التصدير المباشرة والمضمونة لملف PDF
function exportToPDF() {
    const resultBox = document.getElementById('resultBox');
    const element = document.getElementById('pdfContent');
    const empName = document.getElementById('empName').value || 'Employee';
    
    const originalStyle = resultBox.style.display;
    resultBox.style.display = 'block';

    const opt = {
        margin:       12,
        filename:     `Clearance_Statement_${empName}.pdf`,
        image:        { type: 'jpeg', quality: 0.98 },
        html2canvas:  { scale: 2, useCORS: true, logging: false },
        jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
    };

    html2pdf().set(opt).from(element).save().then(() => {
        resultBox.style.display = originalStyle;
    });
}
</script>

</body>
</html>
