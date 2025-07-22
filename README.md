<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<title>محلل وزن الشعر العربي - نسخة محسنة</title>
<style>
  body {
    font-family: 'Cairo', sans-serif;
    max-width: 700px;
    margin: auto;
    background: #f8faff;
    color: #003366;
    padding: 20px;
    text-align: center;
  }
  textarea, button {
    width: 100%;
    font-size: 18px;
    padding: 12px;
    margin: 10px 0;
    border-radius: 8px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }
  button {
    background-color: #0056b3;
    color: white;
    border: none;
    cursor: pointer;
    font-weight: bold;
  }
  button:hover {
    background-color: #003366;
  }
  .result {
    background: #e0f0ff;
    padding: 20px;
    border-radius: 10px;
    margin-top: 20px;
    font-size: 20px;
    min-height: 120px;
    text-align: right;
    direction: rtl;
    white-space: pre-line;
  }
  h1 {
    margin-bottom: 15px;
  }
</style>
</head>
<body>

<h1>📏 محلل وزن الشعر العربي - نسخة محسنة</h1>
<textarea id="poem" rows="5" placeholder="أدخل بيتًا شعريًا عربيًا مع التشكيل (يفضل)"></textarea>
<button onclick="analyzeMeter()">حلل الوزن الآن</button>

<div class="result" id="result">النتيجة ستظهر هنا بعد التحليل</div>

<script>
// رمز الحركات (فتحة = 1، ضمة = 1، كسرة = 1، سكون = 0)
// لأن الوزن يعتمد على التفعيلات المكونة من تتابع حركات وسكوت

const tashkeelMap = {
  'َ': '1', // فتحة
  'ُ': '1', // ضمة
  'ِ': '1', // كسرة
  'ْ': '0', // سكون
  'ً': '1', // تنوين فتح
  'ٌ': '1', // تنوين ضم
  'ٍ': '1', // تنوين كسر
};

// تعريف التفعيلات بالرموز 1 و0 لكل بحر
const metersPatterns = {
  "الطويل":       ["101011", "101011", "101011", "1010"],
  "المديد":       ["101011", "1011011", "101011", "1011011"],
  "البسيط":       ["1011011", "1011011", "1011011", "101011"],
  "الكامل":       ["1011011", "1011011", "1011011", "1011011"],
  "الوافر":       ["1011011", "1011011", "1011010", "1011010"],
  "الرمل":        ["1101010", "1101010", "1101010", "1101010"],
  "الهزج":        ["1011011", "1011011", "1011011", "1011011"],
  "الرجز":        ["1011011", "1011011", "1011011"],
  "السريع":       ["1011011", "101011", "1011011"],
  "المنسرح":      ["101010", "101010", "101010", "1010"],
  "المتدارك":     ["101010", "101010", "101010", "1010"],
  "المتقارب":     ["101011", "101011", "101011", "101011"],
  "المقتضب":      ["101011", "101011", "101011"],
  "المجتث":       ["101011", "101011", "101011"],
  "المضارع":      ["11010", "11010", "11010", "11010"]
};

function extractRhythm(text) {
  let rhythm = '';
  for (let char of text) {
    if (tashkeelMap[char] !== undefined) {
      rhythm += tashkeelMap[char];
    }
  }
  return rhythm;
}

// دالة لحساب التشابه بين النمطين باستخدام التشابه الجزئي
function similarity(str1, str2) {
  let maxScore = 0;
  const len = Math.min(str1.length, str2.length);

  for(let start=0; start <= str1.length - len; start++) {
    let score = 0;
    for(let i=0; i<len; i++) {
      if(str1[start+i] === str2[i]) score++;
    }
    if(score > maxScore) maxScore = score;
  }
  return maxScore / len;
}

function analyzeMeter() {
  const poem = document.getElementById("poem").value.trim();
  if (!poem) {
    alert("يرجى إدخال بيت شعري للتحليل مع التشكيل إن أمكن");
    return;
  }

  const rhythm = extractRhythm(poem);

  if (rhythm.length < 6) {
    document.getElementById('result').innerText = "يرجى إدخال بيت به تشكيل كافٍ لتحليل الوزن بدقة.";
    return;
  }

  let bestMatch = {name: null, score: 0};

  for (const [name, patterns] of Object.entries(metersPatterns)) {
    for (const pattern of patterns) {
      const score = similarity(rhythm, pattern);
      if(score > bestMatch.score) {
        bestMatch = {name, score};
      }
    }
  }

  if(bestMatch.name) {
    document.getElementById('result').innerText = 
      `البيت:\n${poem}\n\n` +
      `النمط المستخرج (الحركات والسكون):\n${rhythm}\n\n` +
      `أقرب بحر شعري: ${bestMatch.name}\n` +
      `درجة التشابه: ${(bestMatch.score * 100).toFixed(2)}%`;
  } else {
    document.getElementById('result').innerText = "لم يتم التعرف على البحر الشعري.";
  }
}
</script>

</body>
</html>
