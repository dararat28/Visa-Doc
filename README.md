<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Visa Document Index</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      background: #f5f7fb;
      color: #222;
    }
    .container {
      max-width: 1300px;
      margin: 24px auto;
      background: #fff;
      padding: 24px;
      border-radius: 14px;
      box-shadow: 0 4px 16px rgba(0,0,0,0.08);
    }
    h1 {
      margin-top: 0;
      font-size: 28px;
    }
    .topbar {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      margin-bottom: 20px;
    }
    .controls {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      align-items: center;
    }
    select, button, input {
      padding: 8px 12px;
      border-radius: 8px;
      border: 1px solid #cfd7e6;
      font-size: 14px;
    }
    .tabs {
      display: flex;
      gap: 8px;
      margin-bottom: 18px;
      flex-wrap: wrap;
    }
    .tab-btn {
      background: #e8eefc;
      border: none;
      cursor: pointer;
      padding: 10px 16px;
      border-radius: 10px;
      font-weight: 600;
    }
    .tab-btn.active {
      background: #2f6fed;
      color: #fff;
    }
    .tab-content {
      display: none;
    }
    .tab-content.active {
      display: block;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 14px;
      font-size: 14px;
    }
    th, td {
      border: 1px solid #d9e1ef;
      padding: 10px;
      text-align: left;
      vertical-align: top;
    }
    th {
      background: #eef3ff;
    }
    .category {
      margin: 24px 0 10px;
      padding: 10px 14px;
      background: #f0f5ff;
      border-left: 5px solid #2f6fed;
      border-radius: 8px;
      font-weight: bold;
      font-size: 18px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 8px;
    }
    .category-count {
      font-size: 13px;
      font-weight: 500;
      color: #44516b;
    }
    .subtle {
      color: #666;
      font-size: 13px;
    }
    .badge {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 999px;
      background: #edf2ff;
      color: #234;
      font-size: 12px;
      margin-right: 6px;
      margin-bottom: 4px;
    }
    .note {
      background: #fff8db;
      border: 1px solid #f0df91;
      padding: 12px 14px;
      border-radius: 10px;
      margin-bottom: 18px;
      font-size: 14px;
    }
    .hidden {
      display: none;
    }
    .summary-box {
      background: #f8fafc;
      border: 1px solid #dde6f3;
      padding: 16px;
      border-radius: 12px;
      margin-bottom: 18px;
    }
    .stats-row {
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
      margin-top: 12px;
    }
    .stat-card {
      flex: 1;
      min-width: 140px;
      background: #fff;
      border: 1px solid #dde6f3;
      border-radius: 10px;
      padding: 12px 14px;
      text-align: center;
    }
    .stat-number {
      font-size: 26px;
      font-weight: 700;
      line-height: 1.1;
    }
    .stat-label {
      font-size: 13px;
      color: #555;
      margin-top: 4px;
    }
    .stat-total .stat-number { color: #2f6fed; }
    .stat-ready .stat-number { color: #1f9d55; }
    .stat-merge .stat-number { color: #e08a1e; }
    .status-pill {
      display: inline-block;
      padding: 4px 10px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 600;
      white-space: nowrap;
    }
    .status-ready {
      background: #e6f7ec;
      color: #1f9d55;
      border: 1px solid #bfe8cd;
    }
    .status-merge {
      background: #fff1de;
      color: #c4711a;
      border: 1px solid #f5d6ab;
    }
    ul {
      margin-top: 6px;
    }
    @media (max-width: 768px) {
      table {
        font-size: 12px;
      }
      h1 {
        font-size: 22px;
      }
      .stats-row {
        flex-direction: column;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="topbar">
      <div>
        <h1 id="pageTitle">รายการเอกสารวีซ่า</h1>
        <div class="subtle" id="pageSubtitle">สรุปรายการเอกสารทั้งหมด พร้อมชื่อไฟล์แนะนำและสถานะ</div>
      </div>
      <div class="controls">
        <label for="langSelect" id="langLabel">ภาษา:</label>
        <select id="langSelect">
          <option value="th">Th</option>
          <option value="en">Eng</option>
        </select>
      </div>
    </div>

    <div class="note" id="noteBox">
      หมายเหตุ: มีการรวมไฟล์บางรายการที่เป็นเอกสารชุดเดียวกัน เช่น เวอร์ชันภาษาไทย/อังกฤษ หรือไฟล์หลายเดือน ไว้ในชื่อไฟล์แนะนำเดียว รายการที่มีสถานะ "ต้องรวมไฟล์" หมายถึงยังต้องนำไฟล์ต้นฉบับหลายไฟล์มารวมเป็นไฟล์เดียวก่อนเปลี่ยนชื่อ
    </div>

    <div class="tabs">
      <button class="tab-btn active" data-tab="all" id="tabAll">รายการเอกสารทั้งหมด</button>
      <button class="tab-btn" data-tab="search" id="tabSearch">ค้นหาตามหมวดหมู่หลัก</button>
    </div>

    <div id="all" class="tab-content active">
      <div class="summary-box">
        <div id="summaryTitle"><strong>หมวดหมู่หลักทั้งหมด</strong></div>
        <div style="margin-top:10px;">
          <span class="badge">1. Travel Document</span>
          <span class="badge">2. Evidence of Home Government Support</span>
          <span class="badge">3. Photograph - Passport</span>
          <span class="badge">4. Evidence of Funds for Stay in Australia</span>
          <span class="badge">5. Language Ability</span>
          <span class="badge">6. Qualifications</span>
          <span class="badge">7. Other Documents</span>
          <span class="badge">8. Change of Name</span>
        </div>
        <div class="stats-row" id="statsRow">
          <div class="stat-card stat-total">
            <div class="stat-number" id="statTotal">0</div>
            <div class="stat-label" id="statTotalLabel">เอกสารทั้งหมด</div>
          </div>
          <div class="stat-card stat-ready">
            <div class="stat-number" id="statReady">0</div>
            <div class="stat-label" id="statReadyLabel">พร้อมแล้ว</div>
          </div>
          <div class="stat-card stat-merge">
            <div class="stat-number" id="statMerge">0</div>
            <div class="stat-label" id="statMergeLabel">ต้องรวมไฟล์</div>
          </div>
        </div>
      </div>

      <div id="allDocumentsContainer"></div>
    </div>

    <div id="search" class="tab-content">
      <div class="controls">
        <label for="mainCategorySelect" id="filterLabel">เลือกหมวดหมู่หลัก:</label>
        <select id="mainCategorySelect"></select>
      </div>
      <div id="searchResults" style="margin-top: 18px;"></div>
    </div>
  </div>

  <script>
    const STATUS = {
      READY: "ready",
      MERGE: "merge"
    };

    const documentData = [
      {
        key: "travel",
        categoryTh: "1. Travel Document",
        categoryEn: "1. Travel Document",
        items: [
          {
            subcategoryTh: "เอกสารการเดินทาง / การระบุตัวตน",
            subcategoryEn: "Travel / Identity Documents",
            originalName: "Passport-Dararat Poungmalee.pdf",
            suggestedName: "01_Passport_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "เอกสารการเดินทาง / การระบุตัวตน",
            subcategoryEn: "Travel / Identity Documents",
            originalName: "ID Card-Dararat Poungmalee.pdf",
            suggestedName: "02_National_ID_Card_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "ทะเบียนบ้าน",
            subcategoryEn: "Household Registration",
            originalName: "Household Registration-Dararat Poungmalee-Th.pdf + Household Registration-Dararat Poungmalee-Eng.pdf",
            suggestedName: "03_Household_Registration_TH_EN_Dararat_Poungmalee.pdf",
            status: STATUS.MERGE
          },
          {
            subcategoryTh: "เอกสารการจองการเดินทาง",
            subcategoryEn: "Travel Booking Confirmation",
            originalName: "Confirmation_for_Booking_ID___2022574992-Eng.pdf",
            suggestedName: "04_Travel_Booking_Confirmation_2022574992.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "Statement of Purpose",
            subcategoryEn: "Statement of Purpose",
            originalName: "Statement of Purpose-Dararat Poungmalee.docx + Statement of Purpose-Dararat Poungmalee.pdf",
            suggestedName: "05_Statement_of_Purpose_Dararat_Poungmalee.docx / 05_Statement_of_Purpose_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          }
        ]
      },
      {
        key: "govsupport",
        categoryTh: "2. หลักฐานการสนับสนุนจากหน่วยงานภาครัฐต้นสังกัด",
        categoryEn: "2. Evidence of Home Government Support",
        items: [
          {
            subcategoryTh: "หนังสือสนับสนุน",
            subcategoryEn: "Support Letter",
            originalName: "Goverment Support Letter.pdf",
            suggestedName: "01_Government_Support_Letter.pdf",
            status: STATUS.READY
          }
        ]
      },
      {
        key: "photo",
        categoryTh: "3. รูปถ่ายหนังสือเดินทาง",
        categoryEn: "3. Photograph - Passport",
        items: [
          {
            subcategoryTh: "รูปถ่ายสำหรับวีซ่า",
            subcategoryEn: "Passport Photo",
            originalName: "Passport Photo-Dararat Poungmalee.jpg",
            suggestedName: "01_Passport_Photo_Dararat_Poungmalee.jpg",
            status: STATUS.READY
          }
        ]
      },
      {
        key: "funds",
        categoryTh: "4. หลักฐานทางการเงินสำหรับการพำนักในออสเตรเลีย",
        categoryEn: "4. Evidence of Funds for Stay in Australia",
        items: [
          {
            subcategoryTh: "แหล่งที่มาของเงินทุน",
            subcategoryEn: "Source of Funds",
            originalName: "4.1 Evidence of Source of Funds.docx + 4.1 Evidence of Source of Funds.pdf",
            suggestedName: "01_Evidence_of_Source_of_Funds.docx / 01_Evidence_of_Source_of_Funds.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "Bank Statement - เงินเดือน",
            subcategoryEn: "Bank Statement - Salary",
            originalName: "Salary-Jan and Feb 26-Statement of -Bangkok.PDF + Salary-Mar to June 26-Statement of - SCB.PDF",
            suggestedName: "02_Salary_Statements_Jan_to_Jun_2026_BBL_SCB.pdf",
            status: STATUS.MERGE
          },
          {
            subcategoryTh: "Bank Statement - เงินออม",
            subcategoryEn: "Bank Statement - Savings",
            originalName: "Savings account-Jan to June 26-Statement of SCB.PDF",
            suggestedName: "03_Savings_Statement_Jan_to_Jun_2026_SCB.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "สำเนาหน้าสมุดบัญชี",
            subcategoryEn: "Bank Book Pages",
            originalName: "Bank book page (Bangkok) Salary.pdf + Bank book page (SCB) Salary.pdf + Bank book page (SCB) Saving.pdf",
            suggestedName: "04_Bank_Book_Pages_BBL_Salary_SCB_Salary_Savings.pdf",
            status: STATUS.MERGE
          },
          {
            subcategoryTh: "หลักฐานการโอนเงิน",
            subcategoryEn: "Transfer Receipt",
            originalName: "Transfer receipt from Bank book page (SCB) Saving (40,000)",
            suggestedName: "05_Transfer_Receipt_SCB_Savings_40000.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "หนังสือรับรองธนาคาร",
            subcategoryEn: "Bank Certificate",
            originalName: "4.3 Bank Cer",
            suggestedName: "06_Bank_Certificate.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "สลิปเงินเดือน",
            subcategoryEn: "Payslips",
            originalName: "PDF 1-payslip_2025_12_01_210960_20260611_162039.pdf to PDF 6-payslip_2026_05_01_210960_20260611_162005.pdf",
            suggestedName: "07_Payslips_Dec_2025_to_May_2026.pdf",
            status: STATUS.MERGE
          },
          {
            subcategoryTh: "หนังสือรับรองการทำงาน",
            subcategoryEn: "Employment Certificate",
            originalName: "PDF Employment Certificate-Dararat P.pdf",
            suggestedName: "08_Employment_Certificate_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "หนังสือปรับเงินเดือนและโบนัส",
            subcategoryEn: "Salary Increase and Bonus Letter",
            originalName: "PDF Dararat_POUNGMALEE Salary Increase and Bonus Letter-2026.pdf",
            suggestedName: "09_Salary_Increase_and_Bonus_Letter_2026_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "อีเมลแจ้งเปลี่ยนบัญชีเงินเดือน",
            subcategoryEn: "Salary Account Change Notification",
            originalName: "HR Notification-Salary Account Change to SCB.msg + HR Notification-Salary Account Change to SCB.pdf",
            suggestedName: "10_HR_Notification_Salary_Account_Change_to_SCB.msg / 10_HR_Notification_Salary_Account_Change_to_SCB.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "เอกสารภาษี",
            subcategoryEn: "Tax Documents",
            originalName: "PIT90_160360-ภงด.91-Th.pdf + PIT90_160360-ภงด.91-eng.pdf",
            suggestedName: "11_PIT90_PND91_TH_EN.pdf",
            status: STATUS.MERGE
          }
        ]
      },
      {
        key: "language",
        categoryTh: "5. ความสามารถทางภาษา",
        categoryEn: "5. Language Ability",
        items: [
          {
            subcategoryTh: "ผลสอบภาษาอังกฤษ",
            subcategoryEn: "English Test Result",
            originalName: "IELTS GENERAL-Dararat Poungmalee.pdf",
            suggestedName: "01_IELTS_General_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          }
        ]
      },
      {
        key: "qualifications",
        categoryTh: "6. คุณวุฒิการศึกษา",
        categoryEn: "6. Qualifications",
        items: [
          {
            subcategoryTh: "ใบปริญญาบัตร",
            subcategoryEn: "Degree Certificate",
            originalName: "PDF Degree Certificate-Dararat Poungmalee-Th.pdf + PDF Degree Certificate-Dararat Poungmalee-Eng.pdf",
            suggestedName: "01_Degree_Certificate_TH_EN_Dararat_Poungmalee.pdf",
            status: STATUS.MERGE
          },
          {
            subcategoryTh: "ใบแสดงผลการเรียน",
            subcategoryEn: "Transcript",
            originalName: "PDF Transcript-Dararat Poungmalee-Eng.pdf",
            suggestedName: "02_Transcript_English_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          }
        ]
      },
      {
        key: "other",
        categoryTh: "7. เอกสารอื่น ๆ",
        categoryEn: "7. Other Documents",
        items: [
          {
            subcategoryTh: "หลักฐานประสบการณ์ทำงานเพิ่มเติม",
            subcategoryEn: "Supporting Work Experience Evidence",
            originalName: "Appendix A - Supporting Evidence of Work Experience.pdf",
            suggestedName: "01_Appendix_A_Supporting_Evidence_of_Work_Experience.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "ใบประกาศนียบัตรวิชาชีพ",
            subcategoryEn: "Professional Certificates",
            originalName: "Appendix B - Professional Certificates.pdf",
            suggestedName: "02_Appendix_B_Professional_Certificates.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "สูติบัตร",
            subcategoryEn: "Birth Certificate",
            originalName: "Birth Certificate-Th.pdf + Birth Certificate-Eng.pdf",
            suggestedName: "03_Birth_Certificate_TH_EN.pdf",
            status: STATUS.MERGE
          },
          {
            subcategoryTh: "สารบัญเอกสาร",
            subcategoryEn: "Document Index",
            originalName: "Document Index.pdf",
            suggestedName: "04_Document_Index.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "ใบขับขี่",
            subcategoryEn: "Driver License",
            originalName: "Driver License-Dararat Poungmalee.pdf",
            suggestedName: "05_Driver_License_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          },
          {
            subcategoryTh: "ประวัติส่วนตัว",
            subcategoryEn: "Resume / CV",
            originalName: "Resume for Visa-Dararat Poungmalee.pdf",
            suggestedName: "06_Resume_for_Visa_Dararat_Poungmalee.pdf",
            status: STATUS.READY
          }
        ]
      },
      {
        key: "changename",
        categoryTh: "8. เอกสารเปลี่ยนชื่อ",
        categoryEn: "8. Change of Name",
        items: [
          {
            subcategoryTh: "หลักฐานการเปลี่ยนนามสกุล",
            subcategoryEn: "Proof of Change of Surname",
            originalName: "Proof of change of surname_Th.pdf",
            suggestedName: "01_Proof_of_Change_of_Surname_Thai.pdf",
            status: STATUS.READY
          }
        ]
      }
    ];

    const uiText = {
      th: {
        pageTitle: "รายการเอกสารวีซ่า",
        pageSubtitle: "สรุปรายการเอกสารทั้งหมด พร้อมชื่อไฟล์แนะนำและสถานะ",
        noteBox: "หมายเหตุ: มีการรวมไฟล์บางรายการที่เป็นเอกสารชุดเดียวกัน เช่น เวอร์ชันภาษาไทย/อังกฤษ หรือไฟล์หลายเดือน ไว้ในชื่อไฟล์แนะนำเดียว รายการที่มีสถานะ \"ต้องรวมไฟล์\" หมายถึงยังต้องนำไฟล์ต้นฉบับหลายไฟล์มารวมเป็นไฟล์เดียวก่อนเปลี่ยนชื่อ",
        tabAll: "รายการเอกสารทั้งหมด",
        tabSearch: "ค้นหาตามหมวดหมู่หลัก",
        langLabel: "ภาษา:",
        filterLabel: "เลือกหมวดหมู่หลัก:",
        summaryTitle: "<strong>หมวดหมู่หลักทั้งหมด</strong>",
        colSubcategory: "หมวดหมู่ย่อย",
        colOriginal: "ชื่อไฟล์เดิม",
        colSuggested: "ชื่อไฟล์แนะนำ",
        colStatus: "สถานะ",
        statusReady: "พร้อมแล้ว",
        statusMerge: "ต้องรวมไฟล์",
        statTotalLabel: "เอกสารทั้งหมด",
        statReadyLabel: "พร้อมแล้ว",
        statMergeLabel: "ต้องรวมไฟล์",
        categoryCountTemplate: (total, ready, merge) => `${total} รายการ • พร้อม ${ready} • ต้องรวมไฟล์ ${merge}`
      },
      en: {
        pageTitle: "Visa Document List",
        pageSubtitle: "Summary of all documents with recommended file names and status",
        noteBox: "Note: Some files that belong to the same document set, such as Thai/English versions or multi-month files, have been combined into one recommended naming entry. Items marked \"Needs Merging\" still require the original files to be combined into a single file before renaming.",
        tabAll: "All Documents",
        tabSearch: "Search by Main Category",
        langLabel: "Language:",
        filterLabel: "Select Main Category:",
        summaryTitle: "<strong>All Main Categories</strong>",
        colSubcategory: "Subcategory",
        colOriginal: "Original File Name",
        colSuggested: "Recommended File Name",
        colStatus: "Status",
        statusReady: "Ready",
        statusMerge: "Needs Merging",
        statTotalLabel: "Total Documents",
        statReadyLabel: "Ready",
        statMergeLabel: "Needs Merging",
        categoryCountTemplate: (total, ready, merge) => `${total} items • Ready ${ready} • Needs Merging ${merge}`
      }
    };

    const langSelect = document.getElementById("langSelect");
    const mainCategorySelect = document.getElementById("mainCategorySelect");
    const allDocumentsContainer = document.getElementById("allDocumentsContainer");
    const searchResults = document.getElementById("searchResults");

    function countItems(items) {
      const total = items.length;
      const ready = items.filter(i => i.status === STATUS.READY).length;
      const merge = items.filter(i => i.status === STATUS.MERGE).length;
      return { total, ready, merge };
    }

    function getAllItems() {
      return documentData.flatMap(category => category.items);
    }

    function renderOverallStats(lang) {
      const text = uiText[lang];
      const { total, ready, merge } = countItems(getAllItems());
      document.getElementById("statTotal").textContent = total;
      document.getElementById("statReady").textContent = ready;
      document.getElementById("statMerge").textContent = merge;
      document.getElementById("statTotalLabel").textContent = text.statTotalLabel;
      document.getElementById("statReadyLabel").textContent = text.statReadyLabel;
      document.getElementById("statMergeLabel").textContent = text.statMergeLabel;
    }

    function statusPillHtml(status, text) {
      if (status === STATUS.MERGE) {
        return `<span class="status-pill status-merge">${text.statusMerge}</span>`;
      }
      return `<span class="status-pill status-ready">${text.statusReady}</span>`;
    }

    function buildTableHtml(category, lang, text) {
      return `
        <table>
          <thead>
            <tr>
              <th>${text.colSubcategory}</th>
              <th>${text.colOriginal}</th>
              <th>${text.colSuggested}</th>
              <th>${text.colStatus}</th>
            </tr>
          </thead>
          <tbody>
            ${category.items.map(item => `
              <tr>
                <td>${lang === "th" ? item.subcategoryTh : item.subcategoryEn}</td>
                <td>${item.originalName}</td>
                <td>${item.suggestedName}</td>
                <td>${statusPillHtml(item.status, text)}</td>
              </tr>
            `).join("")}
          </tbody>
        </table>
      `;
    }

    function renderCategoryOptions(lang) {
      mainCategorySelect.innerHTML = "";
      documentData.forEach(category => {
        const option = document.createElement("option");
        option.value = category.key;
        option.textContent = lang === "th" ? category.categoryTh : category.categoryEn;
        mainCategorySelect.appendChild(option);
      });
    }

    function renderAllDocuments(lang) {
      const text = uiText[lang];
      allDocumentsContainer.innerHTML = "";

      documentData.forEach(category => {
        const { total, ready, merge } = countItems(category.items);

        const categoryTitle = document.createElement("div");
        categoryTitle.className = "category";
        categoryTitle.innerHTML = `
          <span>${lang === "th" ? category.categoryTh : category.categoryEn}</span>
          <span class="category-count">${text.categoryCountTemplate(total, ready, merge)}</span>
        `;
        allDocumentsContainer.appendChild(categoryTitle);

        const tableWrapper = document.createElement("div");
        tableWrapper.innerHTML = buildTableHtml(category, lang, text);
        allDocumentsContainer.appendChild(tableWrapper);
      });

      renderOverallStats(lang);
    }

    function renderSearchResults(lang, selectedKey) {
      const text = uiText[lang];
      const category = documentData.find(c => c.key === selectedKey);
      if (!category) return;

      const { total, ready, merge } = countItems(category.items);

      searchResults.innerHTML = `
        <div class="category">
          <span>${lang === "th" ? category.categoryTh : category.categoryEn}</span>
          <span class="category-count">${text.categoryCountTemplate(total, ready, merge)}</span>
        </div>
        ${buildTableHtml(category, lang, text)}
      `;
    }

    function updateLanguage(lang) {
      const text = uiText[lang];
      document.documentElement.lang = lang;
      document.getElementById("pageTitle").textContent = text.pageTitle;
      document.getElementById("pageSubtitle").textContent = text.pageSubtitle;
      document.getElementById("noteBox").textContent = text.noteBox;
      document.getElementById("tabAll").textContent = text.tabAll;
      document.getElementById("tabSearch").textContent = text.tabSearch;
      document.getElementById("langLabel").textContent = text.langLabel;
      document.getElementById("filterLabel").textContent = text.filterLabel;
      document.getElementById("summaryTitle").innerHTML = text.summaryTitle;

      renderCategoryOptions(lang);
      renderAllDocuments(lang);
      renderSearchResults(lang, mainCategorySelect.value || documentData[0].key);
    }

    document.querySelectorAll(".tab-btn").forEach(btn => {
      btn.addEventListener("click", () => {
        document.querySelectorAll(".tab-btn").forEach(b => b.classList.remove("active"));
        document.querySelectorAll(".tab-content").forEach(tab => tab.classList.remove("active"));
        btn.classList.add("active");
        document.getElementById(btn.dataset.tab).classList.add("active");
      });
    });

    langSelect.addEventListener("change", () => {
      updateLanguage(langSelect.value);
    });

    mainCategorySelect.addEventListener("change", () => {
      renderSearchResults(langSelect.value, mainCategorySelect.value);
    });

    updateLanguage("th");
  </script>
</body>
</html>
