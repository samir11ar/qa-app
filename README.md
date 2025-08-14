<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>والأجوبة</title>
<style>
  body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #f0f4f8; margin:0; padding:20px; text-align:center; }
  h1 { color:#2c3e50; margin-bottom:20px; }

  .section { background:#fff; border-radius:12px; box-shadow:0 4px 10px rgba(0,0,0,0.1); padding:20px; margin:20px auto; max-width:500px; text-align:left; }
  .section h2 { color:#34495e; margin-bottom:15px; }

  input, textarea, select, button { width:100%; padding:10px; margin:5px 0; border-radius:8px; border:1px solid #ccc; font-size:16px; box-sizing:border-box; }
  button { background-color:#3498db; color:#fff; border:none; cursor:pointer; transition:0.3s; }
  button:hover { background-color:#2980b9; }

  #answer { margin-top:15px; font-size:16px; color:#e74c3c; white-space:pre-line; }

  .qa-card { background:#ecf0f1; border-radius:8px; padding:10px; margin:10px 0; }
  .qa-card strong { color:#2c3e50; }
</style>
</head>
<body>

<h1>تطبيق الأسئلة والأجوبة</h1>

<!-- قسم البحث -->
<div class="section">
  <h2>سؤال</h2>
  <input type="text" id="question" placeholder="سول شي سؤال...">
  <select id="category">
    <option value="">كل المواضيع</option>
    <option value="برمجة">برمجة</option>
    <option value="علوم">علوم</option>
    <option value="عام">عام</option>
  </select>
  <button onclick="getAnswer()">سول</button>
  <div id="answer"></div>
</div>

<!-- قسم إضافة الأسئلة (مخفي للمستخدم العادي) -->
<div class="section" id="adminSection" style="display:none;">
  <h2>زيد سؤال جديد (خاص بالمكلف)</h2>
  <input type="text" id="newQuestion" placeholder="السؤال الجديد">
  <textarea id="newAnswer" placeholder="الجواب الجديد" rows="3"></textarea>
  <select id="newCategory">
    <option value="عام">عام</option>
    <option value="برمجة">برمجة</option>
    <option value="علوم">علوم</option>
  </select>
  <button onclick="addQuestion()">زيد</button>
  <button onclick="adminLogout()" style="background-color:#e74c3c; margin-top:10px;">خروج</button>
  <div id="addMsg"></div>
</div>

<!-- قسم دخول المسؤول -->
<div class="section">
  <h2>دخول المسؤول</h2>
  <input type="password" id="adminPass" placeholder="كلمة السر">
  <button onclick="adminLogin()">دخول</button>
  <div id="adminMsg"></div>
</div>

<script>
  // قاعدة البيانات
  let qa = JSON.parse(localStorage.getItem("qa")) || [
    { question: "شنو هو الذكاء الاصطناعي؟", answer: "الذكاء الاصطناعي هو تقنية تمكّن الحواسيب من التعلم واتخاذ القرارات بطريقة تشبه البشر.", category: "علوم" },
    { question: "شنو هو HTML؟", answer: "HTML هي لغة الترميز لإنشاء صفحات الويب.", category: "برمجة" },
    { question: "كيفاش نصاوب تطبيق؟", answer: "باش تصايب تطبيق خاصك تختار لغة برمجة وتصميم واجهة المستخدم وربطها بالمنطق.", category: "برمجة" }
  ];

  const adminPassword = "1234"; // ممكن تغيّر كلمة السر

  // البحث المتقدم
  function getAnswer() {
    const questionInput = document.getElementById("question").value.trim().toLowerCase();
    const selectedCategory = document.getElementById("category").value;
    const answerDiv = document.getElementById("answer");

    let results = qa.filter(q => 
      q.question.toLowerCase().includes(questionInput) &&
      (selectedCategory === "" || q.category === selectedCategory)
    );

    answerDiv.innerHTML = "";
    if(results.length > 0){
      results.forEach(r => {
        const card = document.createElement("div");
        card.className = "qa-card";
        card.innerHTML = <strong>${r.question}</strong><br>${r.answer};
        answerDiv.appendChild(card);
      });
    } else {
      answerDiv.innerText = "ما لقيتش جواب على هاد السؤال 😅";
    }
  }

  // إضافة سؤال جديد
  function addQuestion() {
    const q = document.getElementById("newQuestion").value.trim();
    const a = document.getElementById("newAnswer").value.trim();
    const c = document.getElementById("newCategory").value;
    const msgDiv = document.getElementById("addMsg");

    if(q && a){
      qa.push({ question: q, answer: a, category: c });
      localStorage.setItem("qa", JSON.stringify(qa));
      msgDiv.innerText = "تمت إضافة السؤال بنجاح ✅";
      document.getElementById("newQuestion").value = "";
      document.getElementById("newAnswer").value = "";
    } else {
      msgDiv.innerText = "عافاك عَمّر السؤال والجواب ✋";
    }
  }

  // تسجيل دخول المسؤول
  function adminLogin() {
    const pass = document.getElementById("adminPass").value;
    const msgDiv = document.getElementById("adminMsg");
    if(pass === adminPassword){
      document.getElementById("adminSection").style.display = "block";
      msgDiv.innerText = "تم تسجيل الدخول كمسؤول ✅";
      document.getElementById("adminPass").value = "";
    } else {
      msgDiv.innerText = "كلمة السر خاطئة ❌";
    }
  }

  // تسجيل خروج المسؤول
  function adminLogout() {
    document.getElementById("adminSection").style.display = "none";
    document.getElementById("adminMsg").innerText = "تم تسجيل الخروج ✅";
  }
</script>

</body>
</html>
