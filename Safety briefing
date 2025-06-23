<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>תדריך בטיחות - אוברסיז קומרס</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, sans-serif;
      background: #f0f4f8;
      color: #333;
      padding: 20px;
      direction: rtl;
    }
    h2, h3 {
      color: #1d70b8;
      margin: 10px 0;
    }
    .logo {
      height: 60px;
      margin-bottom: 15px;
    }
    #safetyInstructions, ol {
      max-width: 95%;
      margin: 0 0 20px 0;
      white-space: normal;
      word-break: break-word;
      padding-right: 10px;
      direction: rtl;
    }
    ol {
      background: #e8f0fe;
      border: 1px solid #b3d4fc;
      padding: 10px 20px;
      border-radius: 10px;
      line-height: 1.5;
    }
    .btn {
      background: #28a745;
      color: white;
      border: none;
      padding: 4px 8px;
      margin: 3px;
      border-radius: 4px;
      cursor: pointer;
      font-size: 14px;
    }
    .btn:hover { background: #218838; }
    .btn.blue {
      background: #007bff;
    }
    .btn.blue:hover {
      background: #0056b3;
    }
    .employee {
      background: white;
      padding: 6px 8px;
      margin: 4px 0;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 15px;
    }
    .employee div {
      display: flex;
      gap: 5px;
    }
    #newEmployeeInput {
      margin-top: 5px;
      padding: 6px;
      width: 220px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    .signature-pad {
      display: none;
      margin-top: 10px;
    }
    canvas {
      border: 2px dashed #555;
      background: #fff;
      border-radius: 8px;
      touch-action: none;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
      background: white;
    }
    th {
      background-color: #1d70b8;
      color: white;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: center;
    }
    .actions {
      margin-top: 25px;
    }
    #editInstructionsBox {
      display: none;
      margin-top: 10px;
    }
    textarea {
      width: 100%;
      height: 180px;
      padding: 8px;
      border-radius: 6px;
      border: 1px solid #ccc;
      font-family: Arial;
    }
    img.signature-img {
      height: 50px;
    }
  </style>
</head>
<body>
<div id="pdfContent">
  <img src="logo.png" alt="לוגו אוברסיז קומרס" class="logo" />
  <h2>תדריך בטיחות יומי לעובדי אוברסיז קומרס בע"מ</h2>
  <div id="safetyInstructions"></div>
  <button class="btn blue" onclick="toggleEditInstructions()">ערוך הוראות בטיחות</button>
  <div id="editInstructionsBox">
    <textarea id="instructionsTextarea"></textarea><br />
    <button class="btn" onclick="saveInstructions()">שמור</button>
    <button class="btn" onclick="toggleEditInstructions()">ביטול</button>
  </div>

  <h3>רשימת עובדים</h3>
  <div id="employeeList"></div>
  <input type="text" id="newEmployeeInput" placeholder="שם עובד חדש" />
  <button class="btn" onclick="addEmployee()">הוסף עובד</button>

  <div id="signaturePad" class="signature-pad">
    <p>חתום כאן עבור <span id="signingName"></span>:</p>
    <canvas id="canvas" width="300" height="120"></canvas><br />
    <button class="btn" onclick="saveSignature()">שמור חתימה</button>
    <button class="btn blue" onclick="clearCanvas()">נקה</button>
    <button class="btn" onclick="closePad()">סגור</button>
  </div>

  <h3>רשימת חתימות</h3>
  <table id="signaturesTable">
    <thead>
      <tr>
        <th>שם עובד</th>
        <th>תאריך</th>
        <th>חתימה</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>
</div>

<div class="actions">
  <button class="btn blue" onclick="downloadPDF()">הורד את כל הדף כ־PDF</button>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
<script>
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");
  let drawing = false;
  let currentEmployee = "";

  window.onload = function () {
    const employees = JSON.parse(localStorage.getItem("employees") || "[]");
    if (employees.length === 0) {
      localStorage.setItem("employees", JSON.stringify(["ימית אבוזגי", "טול יאמנקו", "טל ויטל"]));
    }
    renderEmployees();
    renderSignatures();
    loadInstructions();
  };

  function loadInstructions() {
    const saved = localStorage.getItem("safetyInstructions");
    const instructions = saved ? JSON.parse(saved) : defaultInstructions();
    document.getElementById("safetyInstructions").innerHTML = `<ol>${instructions.map(i => `<li>${i}</li>`).join('')}</ol>`;
    document.getElementById("instructionsTextarea").value = instructions.join("\n");
  }

  function defaultInstructions() {
    return [
      "יש להשתמש בציוד מיגון והבטיחות הקיימים במחסן.",
      "אין להפעיל את המלגזה מבלי שעברת הדרכת בטיחות.",
      "אין להסיע נוסעים במלגזה – מלגזה היא כלי לעובד בלבד.",
      "חובה לחגור חגורת בטיחות בכל נסיעה.",
      "אסור להשתמש בטלפון נייד בזמן נהיגה.",
      "יש לדווח מיידית על תקלות בכלי או במשטח.",
      "מותר להרים משטחים רק אם הם יציבים וסגורים.",
      "יש להניח את המשטח בצורה מאוזנת.",
      "יש להיזהר מהולכי רגל ולתת להם זכות קדימה.",
      "חובה לשמור מרחק מכל כלי רכב אחר.",
      "חובה על נעילת בלמים בכל חניית מלגזה.",
      "אין לעשן או לאכול באזורי העבודה.",
      "חובה לעטות אפוד זוהר בזמן העבודה.",
      "כל פעילות מסוכנת תיעשה אך ורק באישור מנהל.",
      "יש לשמור על סדר וניקיון באזור העבודה."
    ];
  }

  function toggleEditInstructions() {
    const box = document.getElementById("editInstructionsBox");
    box.style.display = box.style.display === "none" ? "block" : "none";
  }

  function saveInstructions() {
    const lines = document.getElementById("instructionsTextarea").value
      .split("\n")
      .map(line => line.trim())
      .filter(line => line !== "");
    localStorage.setItem("safetyInstructions", JSON.stringify(lines));
    loadInstructions();
    toggleEditInstructions();
  }

  function renderEmployees() {
    const list = document.getElementById("employeeList");
    list.innerHTML = "";
    const employees = JSON.parse(localStorage.getItem("employees") || "[]");
    employees.forEach((name, index) => {
      const div = document.createElement("div");
      div.className = "employee";
      div.innerHTML = `
        <span>${name}</span>
        <div>
          <button class="btn" onclick="sign('${name}')">חתום</button>
          <button class="btn blue" onclick="deleteEmployee(${index})">מחק</button>
        </div>`;
      list.appendChild(div);
    });
  }

  function addEmployee() {
    const input = document.getElementById("newEmployeeInput");
    const name = input.value.trim();
    if (!name) return alert("יש להקליד שם עובד");
    let employees = JSON.parse(localStorage.getItem("employees") || "[]");
    employees.push(name);
    localStorage.setItem("employees", JSON.stringify(employees));
    input.value = "";
    renderEmployees();
  }

  function deleteEmployee(index) {
    if (!confirm("האם אתה בטוח שברצונך למחוק את העובד?")) return;
    let employees = JSON.parse(localStorage.getItem("employees") || "[]");
    employees.splice(index, 1);
    localStorage.setItem("employees", JSON.stringify(employees));
    renderEmployees();
  }

  function sign(name) {
    currentEmployee = name;
    document.getElementById("signingName").innerText = name;
    document.getElementById("signaturePad").style.display = "block";
    clearCanvas();
  }

  function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  }

  function isCanvasEmpty(canvas) {
    const blank = document.createElement('canvas');
    blank.width = canvas.width;
    blank.height = canvas.height;
    return canvas.toDataURL() === blank.toDataURL();
  }

  function saveSignature() {
    if (isCanvasEmpty(canvas)) {
      alert("החתימה ריקה – אנא חתום לפני שמירה");
      return;
    }
    const dataURL = canvas.toDataURL();
    const date = new Date().toLocaleDateString('he-IL');
    let records = JSON.parse(localStorage.getItem("signedRecords") || "[]");
    records.push({ name: currentEmployee, date: date, signature: dataURL });
    localStorage.setItem("signedRecords", JSON.stringify(records));
    document.getElementById("signaturePad").style.display = "none";
    alert("החתימה נשמרה בהצלחה!");
    renderSignatures();
  }

  function closePad() {
    document.getElementById("signaturePad").style.display = "none";
  }

  function renderSignatures() {
    const tbody = document.querySelector("#signaturesTable tbody");
    tbody.innerHTML = "";
    const records = JSON.parse(localStorage.getItem("signedRecords") || "[]");
    records.forEach((rec, index) => {
      const tr = document.createElement("tr");
      tr.innerHTML = `
        <td>${rec.name}</td>
        <td>${rec.date}</td>
        <td>
          <img class="signature-img" src="${rec.signature}" alt="חתימה" />
          <br><button class="btn blue" onclick="deleteSignature(${index})">מחק</button>
        </td>`;
      tbody.appendChild(tr);
    });
  }

  function deleteSignature(index) {
    if (!confirm("האם למחוק את החתימה?")) return;
    let records = JSON.parse(localStorage.getItem("signedRecords") || "[]");
    records.splice(index, 1);
    localStorage.setItem("signedRecords", JSON.stringify(records));
    renderSignatures();
  }

  function downloadPDF() {
    const element = document.getElementById("pdfContent");
    const opt = {
      margin: 0.5,
      filename: 'תדריך_חתימות.pdf',
      image: { type: 'jpeg', quality: 0.98 },
      html2canvas: { scale: 2, useCORS: true },
      jsPDF: { unit: 'cm', format: 'a4', orientation: 'portrait' }
    };
    html2pdf().set(opt).from(element).save().catch(err => {
      console.error("שגיאה בהורדת PDF:", err);
      alert("אירעה שגיאה בהורדה. ודא שהדפדפן מאפשר הורדות.");
    });
  }

  function getPos(e) {
    const rect = canvas.getBoundingClientRect();
    let x = e.clientX || e.touches?.[0].clientX;
    let y = e.clientY || e.touches?.[0].clientY;
    return { x: x - rect.left, y: y - rect.top };
  }

  canvas.addEventListener("mousedown", e => { drawing = true; ctx.beginPath(); ctx.moveTo(...Object.values(getPos(e))); });
  canvas.addEventListener("mouseup", () => drawing = false);
  canvas.addEventListener("mousemove", e => { if (drawing) ctx.lineTo(...Object.values(getPos(e))), ctx.stroke(); });
  canvas.addEventListener("touchstart", e => { drawing = true; ctx.beginPath(); ctx.moveTo(...Object.values(getPos(e))); });
  canvas.addEventListener("touchend", () => drawing = false);
  canvas.addEventListener("touchmove", e => { if (drawing) ctx.lineTo(...Object.values(getPos(e))), ctx.stroke(); });
</script>
</body>
</html>

