
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>FindMySchool | ระบบตามหาของหาย</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: "Segoe UI", Tahoma, sans-serif;
      background: #f5f6ff;
      color: #263052;
    }
    header {
      background: linear-gradient(120deg, #312e81, #7c3aed);
      color: white;
      padding: 36px 20px;
      text-align: center;
    }
    header h1 { margin: 0; font-size: 34px; }
    header p { opacity: .9; }
    nav {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 10px;
      padding: 15px;
      background: white;
      box-shadow: 0 2px 10px #0001;
    }
    button {
      border: 0;
      border-radius: 10px;
      padding: 11px 18px;
      background: #5943d8;
      color: white;
      cursor: pointer;
      font-weight: 600;
    }
    button:hover { background: #4230ae; }
    button.secondary { background: #e9e7ff; color: #4338ca; }
    button.green { background: #16845c; }
    main { max-width: 1100px; margin: auto; padding: 22px; }
    section { display: none; }
    section.active { display: block; }
    .panel {
      background: white;
      border-radius: 18px;
      padding: 24px;
      margin-bottom: 20px;
      box-shadow: 0 4px 18px #16144b0c;
    }
    .stats, .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 16px;
      margin: 20px 0;
    }
    .stat {
      padding: 25px;
      background: white;
      border-radius: 16px;
      text-align: center;
    }
    .stat strong {
      font-size: 35px;
      display: block;
      color: #5943d8;
    }
    .item {
      background: white;
      border-radius: 16px;
      padding: 14px;
      overflow: hidden;
      box-shadow: 0 3px 12px #0001;
    }
    .item img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      border-radius: 10px;
      background: #eee;
    }
    .placeholder {
      height: 180px;
      background: #eeeeff;
      display: grid;
      place-items: center;
      border-radius: 10px;
      font-size: 45px;
    }
    .tag {
      display: inline-block;
      padding: 5px 10px;
      border-radius: 20px;
      background: #ede9fe;
      color: #5b21b6;
      font-size: 12px;
    }
    .found { background: #dcfce7; color: #166534; }
    .done { background: #e5e7eb; color: #374151; }
    input, select, textarea {
      width: 100%;
      padding: 12px;
      border: 1px solid #d8d9e5;
      border-radius: 9px;
      margin: 6px 0 16px;
      font: inherit;
    }
    label { font-weight: 600; }
    textarea { min-height: 90px; }
    .two {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
    }
    .muted { color: #6b7280; font-size: 14px; }
    .match {
      padding: 15px;
      background: #f1f0ff;
      border-radius: 12px;
      margin-bottom: 12px;
    }
    .score { color: #15803d; font-weight: bold; }
    footer {
      text-align: center;
      padding: 30px;
      color: #777;
    }
    @media (max-width: 600px) {
      .two { grid-template-columns: 1fr; }
      header h1 { font-size: 27px; }
      main { padding: 12px; }
      .panel { padding: 16px; }
    }
  </style>
</head>

<body>
<header>
  <h1>🔎 FindMySchool</h1>
  <p>A Smarter Way to Find What's Lost</p>
  <p>ของหายไม่ต้องตกใจ ให้เราช่วยตามหา!</p>
</header>

<nav>
  <button onclick="showPage('home')">🏠 หน้าแรก</button>
  <button onclick="openReport('lost')">🔍 แจ้งของหาย</button>
  <button onclick="openReport('found')">📦 แจ้งเก็บได้</button>
  <button onclick="showPage('search')">📋 ค้นหาสิ่งของ</button>
  <button onclick="showPage('matches')">✨ Smart Matching</button>
</nav>

<main>

  <!-- HOME -->
  <section id="home" class="active">
    <div class="panel">
      <h2>ยินดีต้อนรับสู่ FindMySchool 👋</h2>
      <p>
        ระบบช่วยตามหาของหายภายในโรงเรียน
        สามารถแจ้งสิ่งของ ค้นหา และตรวจสอบรายการที่อาจตรงกัน
      </p>
      <button onclick="openReport('lost')">แจ้งของหาย</button>
      <button class="green" onclick="openReport('found')">แจ้งเก็บได้</button>
    </div>

    <div class="stats">
      <div class="stat">รายการของหาย<strong id="lostCount">0</strong></div>
      <div class="stat">รายการเก็บได้<strong id="foundCount">0</strong></div>
      <div class="stat">คืนสำเร็จ<strong id="returnCount">0</strong></div>
    </div>

    <h2>รายการล่าสุด</h2>
    <div id="recentItems" class="cards"></div>
  </section>

  <!-- REPORT -->
  <section id="report">
    <div class="panel">
      <h2 id="formTitle">แจ้งข้อมูลสิ่งของ</h2>

      <form id="itemForm">
        <label>ประเภทการแจ้ง</label>
        <select id="type" required>
          <option value="lost">ทำของหาย</option>
          <option value="found">เก็บของได้</option>
        </select>

        <label>ชื่อสิ่งของ</label>
        <input id="name" placeholder="เช่น กระเป๋าสตางค์สีดำ" required maxlength="100">

        <div class="two">
          <div>
            <label>หมวดหมู่</label>
            <select id="category" required>
              <option value="กระเป๋า">กระเป๋า</option>
              <option value="โทรศัพท์">โทรศัพท์</option>
              <option value="อุปกรณ์การเรียน">อุปกรณ์การเรียน</option>
              <option value="ขวดน้ำ">ขวดน้ำ</option>
              <option value="เครื่องแต่งกาย">เครื่องแต่งกาย</option>
              <option value="อื่น ๆ">อื่น ๆ</option>
            </select>
          </div>
          <div>
            <label>สีของสิ่งของ</label>
            <select id="color" required>
              <option>ดำ</option><option>ขาว</option>
              <option>แดง</option><option>น้ำเงิน</option>
              <option>ชมพู</option><option>เขียว</option>
              <option>เหลือง</option><option>เทา</option>
              <option>อื่น ๆ</option>
            </select>
          </div>
        </div>

        <div class="two">
          <div>
            <label>วันที่ทำหาย / พบ</label>
            <input id="date" type="date" required>
          </div>
          <div>
            <label>สถานที่ทำหาย / พบ</label>
            <input id="location" placeholder="เช่น โรงอาหาร" required maxlength="150">
          </div>
        </div>

        <label>รายละเอียดเพิ่มเติม</label>
        <textarea id="description" placeholder="ระบุลักษณะสิ่งของ" maxlength="1000"></textarea>

        <label>รูปภาพสิ่งของ (ไม่เกิน 5 MB)</label>
        <input id="photo" type="file" accept="image/*">

        <label>ชื่อผู้แจ้ง</label>
        <input id="reporter" placeholder="ชื่อ-นามสกุล" required maxlength="100">

        <label>ช่องทางติดต่อ</label>
        <input id="contact" placeholder="เช่น ห้องเรียน หรือช่องทางติดต่อ" required maxlength="150">

        <p class="muted">
          ข้อมูลผู้แจ้งไม่แสดงบนการ์ดสาธารณะ
          เวอร์ชันนี้เป็นระบบทดลองเท่านั้น
        </p>

        <button type="submit">💾 บันทึกข้อมูล</button>
      </form>
    </div>
  </section>

  <!-- SEARCH -->
  <section id="search">
    <div class="panel">
      <h2>📋 ค้นหาสิ่งของ</h2>
      <input id="keyword" placeholder="ค้นหาชื่อสิ่งของหรือสถานที่" oninput="renderSearch()">
      <select id="filterType" onchange="renderSearch()">
        <option value="all">ทั้งหมด</option>
        <option value="lost">ของหาย</option>
        <option value="found">เก็บได้</option>
      </select>
      <div id="searchResults" class="cards"></div>
    </div>
  </section>

  <!-- MATCHES -->
  <section id="matches">
    <div class="panel">
      <h2>✨ Smart Matching</h2>
      <p>
        ระบบเปรียบเทียบหมวดหมู่ ชื่อ สี และสถานที่
        เพื่อค้นหารายการที่อาจเป็นสิ่งของชิ้นเดียวกัน
      </p>
      <p class="muted">
        คะแนนนี้เป็นคะแนนตามกฎที่กำหนด
        ไม่ใช่ความแม่นยำจาก AI และไม่ใช่หลักฐานความเป็นเจ้าของ
      </p>
      <div id="matchResults"></div>
    </div>
  </section>

</main>

<footer>FindMySchool © 2026 | School Lost & Found Prototype</footer>

<script>
const STORE = "findmyschool_demo_v1";
let items = [];

try {
  items = JSON.parse(localStorage.getItem(STORE) || "[]");
  if (!Array.isArray(items)) items = [];
} catch {
  items = [];
}

function save() {
  try {
    localStorage.setItem(STORE, JSON.stringify(items));
    return true;
  } catch {
    alert("พื้นที่เก็บข้อมูลเต็ม กรุณาลดขนาดรูปภาพ");
    return false;
  }
}

function escapeHTML(value) {
  return String(value ?? "").replace(/[&<>"']/g, c => ({
    "&": "&amp;",
    "<": "&lt;",
    ">": "&gt;",
    '"': "&quot;",
    "'": "&#39;"
  }[c]));
}

function showPage(id) {
  document.querySelectorAll("main section").forEach(s => {
    s.classList.remove("active");
  });
  document.getElementById(id).classList.add("active");
  renderAll();
  window.scrollTo(0, 0);
}

function openReport(type) {
  document.getElementById("itemForm").reset();
  document.getElementById("type").value = type;
  document.getElementById("date").value =
    new Date().toLocaleDateString("en-CA");
  document.getElementById("formTitle").textContent =
    type === "lost" ? "🔍 แจ้งของหาย" : "📦 แจ้งเก็บได้";
  showPage("report");
}

function get(id) {
  return document.getElementById(id).value.trim();
}

// ลดขนาดรูปก่อนจัดเก็บในเบราว์เซอร์
function resizeImage(file) {
  return new Promise((resolve, reject) => {
    if (!file) return resolve("");
    if (!file.type.startsWith("image/")) {
      return reject(new Error("กรุณาเลือกไฟล์ภาพ"));
    }
    if (file.size > 5 * 1024 * 1024) {
      return reject(new Error("รูปภาพต้องไม่เกิน 5 MB"));
    }

    const reader = new FileReader();
    reader.onerror = () => reject(new Error("อ่านรูปภาพไม่สำเร็จ"));
    reader.onload = () => {
      const img = new Image();
      img.onerror = () => reject(new Error("เปิดรูปภาพไม่สำเร็จ"));
      img.onload = () => {
        const scale = Math.min(1, 700 / Math.max(img.width, img.height));
        const canvas = document.createElement("canvas");
        canvas.width = Math.round(img.width * scale);
        canvas.height = Math.round(img.height * scale);
        canvas.getContext("2d").drawImage(
          img, 0, 0, canvas.width, canvas.height
        );
        resolve(canvas.toDataURL("image/jpeg", 0.65));
      };
      img.src = reader.result;
    };
    reader.readAsDataURL(file);
  });
}

document.getElementById("itemForm").addEventListener("submit", async e => {
  e.preventDefault();
  const button = e.submitter;
  button.disabled = true;

  try {
    const image = await resizeImage(
      document.getElementById("photo").files[0]
    );

    const item = {
      id: crypto.randomUUID(),
      type: get("type"),
      name: get("name"),
      category: get("category"),
      color: get("color"),
      date: get("date"),
      location: get("location"),
      description: get("description"),
      reporter: get("reporter"),
      contact: get("contact"),
      image,
      status: "active",
      createdAt: new Date().toISOString()
    };

    items.unshift(item);

    if (!save()) {
      items.shift();
      return;
    }

    alert("บันทึกข้อมูลเรียบร้อยแล้ว");
    showPage("home");
  } catch (error) {
    alert(error.message);
  } finally {
    button.disabled = false;
  }
});

function itemCard(item) {
  const status = item.status === "returned"
    ? '<span class="tag done">คืนแล้ว</span>'
    : item.type === "lost"
    ? '<span class="tag">ของหาย</span>'
    : '<span class="tag found">เก็บได้</span>';

  const image = item.image
    ? `<img src="${item.image}" alt="รูปสิ่งของ">`
    : '<div class="placeholder">📦</div>';

  return `
    <div class="item">
      ${image}
      <h3>${escapeHTML(item.name)}</h3>
      ${status}
      <p>หมวดหมู่: ${escapeHTML(item.category)}</p>
      <p>สี: ${escapeHTML(item.color)}</p>
      <p>📍 ${escapeHTML(item.location)}</p>
      <p>📅 ${escapeHTML(item.date)}</p>
      <p class="muted">${escapeHTML(item.description)}</p>
    </div>`;
}

function renderHome() {
  document.getElementById("lostCount").textContent =
    items.filter(x => x.type === "lost" && x.status === "active").length;
  document.getElementById("foundCount").textContent =
    items.filter(x => x.type === "found" && x.status === "active").length;
  document.getElementById("returnCount").textContent =
    items.filter(x => x.status === "returned").length;

  document.getElementById("recentItems").innerHTML =
    items.slice(0, 6).map(itemCard).join("") ||
    '<p class="muted">ยังไม่มีรายการสิ่งของ</p>';
}

function renderSearch() {
  const keyword = get("keyword").toLowerCase();
  const filter = get("filterType");

  const results = items.filter(item => {
    const searchable = [
      item.name, item.category, item.location, item.color
    ].join(" ").toLowerCase();

    return searchable.includes(keyword) &&
      (filter === "all" || item.type === filter);
  });

  document.getElementById("searchResults").innerHTML =
    results.map(itemCard).join("") ||
    "<p>ไม่พบสิ่งของที่ค้นหา</p>";
}

// อัลกอริทึมจับคู่แบบใช้กฎ ไม่ใช่ AI
function similarity(a, b) {
  let score = 0;

  if (a.category === b.category) score += 40;
  if (a.color === b.color) score += 25;

  const wordsA = a.name.toLowerCase().split(/\s+/).filter(Boolean);
  const nameB = b.name.toLowerCase();

  if (wordsA.some(word => nameB.includes(word))) score += 25;

  if (a.location.toLowerCase() === b.location.toLowerCase()) {
    score += 10;
  }

  return Math.min(score, 100);
}

function renderMatches() {
  const lost = items.filter(x =>
    x.type === "lost" && x.status === "active"
  );
  const found = items.filter(x =>
    x.type === "found" && x.status === "active"
  );

  const matches = [];

  lost.forEach(a => {
    found.forEach(b => {
      const score = similarity(a, b);
      if (score >= 50) matches.push({ a, b, score });
    });
  });

  matches.sort((x, y) => y.score - x.score);

  document.getElementById("matchResults").innerHTML =
    matches.map(m => `
      <div class="match">
        <h3>${escapeHTML(m.a.name)} ↔ ${escapeHTML(m.b.name)}</h3>
        <p>ของหาย: ${escapeHTML(m.a.location)}</p>
        <p>เก็บได้: ${escapeHTML(m.b.location)}</p>
        <p class="score">คะแนนความคล้าย ${m.score}/100</p>
        <button class="green"
          onclick="requestReturn('${m.a.id}','${m.b.id}')">
          ขอรับคืน (โหมดทดลอง)
        </button>
      </div>
    `).join("") ||
    '<p class="muted">ยังไม่พบรายการที่อาจตรงกัน</p>';
}

// จำลองการรับคืน: ยังไม่มีการพิสูจน์ตัวตนจริง
function requestReturn(lostId, foundId) {
  const a = items.find(x => x.id === lostId);
  const b = items.find(x => x.id === foundId);
  if (!a || !b || a.status !== "active" || b.status !== "active") return;

  const evidence = prompt(
    "กรุณาระบุหลักฐานแสดงความเป็นเจ้าของ (ใช้ข้อมูลสมมติเท่านั้น)"
  );

  if (!evidence || !evidence.trim()) return;

  if (!confirm(
    "โหมดทดลอง: ต้องการจำลองว่าเจ้าหน้าที่ตรวจสอบและอนุมัติแล้วหรือไม่?"
  )) return;

  a.status = "returned";
  b.status = "returned";

  if (!save()) {
    a.status = "active";
    b.status = "active";
    return;
  }

  alert("จำลองการรับคืนเรียบร้อยแล้ว");
  renderAll();
}

function renderAll() {
  renderHome();
  renderSearch();
  renderMatches();
}

renderAll();
</script>
</body>
</html>
