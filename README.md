<html lang="en"><head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Native English Helper</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; }
  body {
    min-height: 100vh;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    display: flex; justify-content: center; align-items: center; padding: 20px;
  }
  .container {
    background: #fff; border-radius: 16px; box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    width: 100%; max-width: 760px; padding: 30px;
  }
  h1 { text-align: center; color: #333; margin-bottom: 8px; font-size: 26px; }
  .subtitle { text-align: center; color: #666; margin-bottom: 20px; font-size: 13px; }
  .api-key { margin-bottom: 12px; display: flex; gap: 8px; align-items: center; }
  .api-key input { flex: 1; padding: 10px; border: 1px solid #ddd; border-radius: 8px; font-size: 14px; }
  .api-key a { font-size: 12px; color: #667eea; text-decoration: none; white-space: nowrap; }
  .model-row { margin-bottom: 12px; display: flex; gap: 8px; align-items: center; }
  .model-row select { flex: 1; padding: 8px; border: 1px solid #ddd; border-radius: 6px; font-size: 13px; }
  .btn-check { padding: 8px 12px; background: #f59e0b; color: #fff; border: none; border-radius: 6px; font-size: 12px; cursor: pointer; white-space: nowrap; }
 
.mode-tabs { display: flex; gap: 8px; margin-bottom: 12px; }
.mode-tab {
flex: 1; padding: 10px; border: 2px solid #e0e0e0; border-radius: 8px;
background: #f9fafb; color: #555; font-size: 13px; cursor: pointer;
text-align: center; transition: all 0.2s;
}
.mode-tab.active { background: #667eea; color: #fff; border-color: #667eea; }
 
.voice-settings { display: flex; gap: 10px; margin-bottom: 15px; align-items: center; flex-wrap: wrap; }
.voice-settings label { font-size: 13px; color: #555; }
.voice-settings select, .voice-settings input[type=range] { padding: 6px; border: 1px solid #ddd; border-radius: 6px; font-size: 13px; }
 
textarea { width: 100%; min-height: 110px; padding: 14px; border: 2px solid #e0e0e0; border-radius: 12px; font-size: 16px; resize: vertical; }
textarea:focus { outline: none; border-color: #667eea; }
 
.lang-badge {
display: inline-block; padding: 3px 10px; border-radius: 12px;
font-size: 11px; font-weight: 600; margin-left: 8px;
}
.badge-en { background: #dbeafe; color: #1e40af; }
.badge-vi { background: #fef3c7; color: #92400e; }
 
.buttons { display: flex; gap: 10px; margin-top: 15px; flex-wrap: wrap; }
button { flex: 1; min-width: 100px; padding: 12px 16px; border: none; border-radius: 10px; font-size: 14px; font-weight: 600; cursor: pointer; color: #fff; }
.btn-mic { background: #ef4444; }
.btn-mic.recording { background: #b91c1c; animation: pulse 1s infinite; }
.btn-process { background: #667eea; }
.btn-speak-input { background: #10b981; }
.btn-clear { background: #6b7280; }
button:hover { opacity: 0.9; }
button:disabled { opacity: 0.5; cursor: not-allowed; }
@keyframes pulse {
0%,100% { box-shadow: 0 0 0 0 rgba(239,68,68,0.7); }
50% { box-shadow: 0 0 0 12px rgba(239,68,68,0); }
}
 
.result { margin-top: 20px; padding: 18px; background: #f9fafb; border-radius: 12px; border-left: 4px solid #667eea; display: none; }
.result h3 { color: #333; margin-bottom: 10px; font-size: 15px; display: flex; align-items: center; flex-wrap: wrap; }
.corrected-wrap { background: #ecfdf5; padding: 12px; border-radius: 8px; margin-bottom: 15px; display: flex; align-items: center; gap: 10px; }
.corrected { flex: 1; color: #065f46; font-size: 16px; line-height: 1.5; font-weight: 500; }
.speak-btn { background: #10b981; color: #fff; border: none; border-radius: 50%; width: 38px; height: 38px; cursor: pointer; font-size: 16px; flex-shrink: 0; padding: 0; min-width: 38px; }
.explanation { background: #fff; padding: 14px; border-radius: 8px; color: #444; font-size: 14px; line-height: 1.7; white-space: pre-wrap; }
 
.alternatives { background: #fff7ed; padding: 12px; border-radius: 8px; margin-top: 12px; }
.alternatives h4 { color: #9a3412; font-size: 13px; margin-bottom: 8px; }
.alt-item {
background: #fff; padding: 10px; border-radius: 6px; margin-bottom: 6px;
display: flex; align-items: center; gap: 8px; font-size: 14px;
}
.alt-item .alt-text { flex: 1; color: #1f2937; }
.alt-item .alt-tag { font-size: 10px; padding: 2px 6px; border-radius: 4px; background: #f3f4f6; color: #6b7280; text-transform: uppercase; font-weight: 600; }
.alt-speak {
background: #f59e0b; color: #fff; border: none; border-radius: 50%;
width: 28px; height: 28px; cursor: pointer; font-size: 12px; flex-shrink: 0;
padding: 0; min-width: 28px;
}
 
.loading { text-align: center; color: #667eea; margin-top: 15px; display: none; }
.status { font-size: 13px; color: #666; margin-top: 8px; text-align: center; min-height: 20px; }
.error-box { background: #fef2f2; border-left: 4px solid #ef4444; color: #991b1b; padding: 12px; border-radius: 8px; margin-top: 15px; font-size: 13px; display: none; white-space: pre-wrap; }
.success-box { background: #f0fdf4; border-left: 4px solid #10b981; color: #166534; padding: 12px; border-radius: 8px; margin-top: 15px; font-size: 13px; display: none; white-space: pre-wrap; }
</style>
 
</head>
<body>
  <div class="container">
    <h1>🎯 Native English Helper</h1>
    <p class="subtitle">Auto-detect: English → Native English • Vietnamese → Native English</p>
 
<div class="api-key">
  <input type="password" id="apiKey" placeholder="Paste your Gemini API Key (AIzaSy...)">
  <a href="https://aistudio.google.com/apikey" target="_blank">Get key →</a>
</div>
 
<div class="model-row">
  <label style="font-size:13px;color:#555;">Model:</label>
  <select id="modelSelect">
    <option value="gemini-2.5-flash" selected="">gemini-2.5-flash (recommended)</option>
    <option value="gemini-2.0-flash">gemini-2.0-flash</option>
    <option value="gemini-2.0-flash-lite">gemini-2.0-flash-lite (fastest)</option>
    <option value="gemini-1.5-flash">gemini-1.5-flash</option>
  </select>
  <button class="btn-check" id="checkBtn">🔍 Test Key</button>
</div>
 
<div class="mode-tabs">
  <div class="mode-tab active" data-mode="auto">🤖 Auto Detect</div>
  <div class="mode-tab" data-mode="en">🇬🇧 EN → Native</div>
  <div class="mode-tab" data-mode="vi">🇻🇳 VI → EN</div>
</div>
 
<div class="voice-settings">
  <label>Voice:</label>
  <select id="voiceSelect"><option value="0">Samantha (en-US)</option><option value="1">Albert (en-US)</option><option value="2">Bad News (en-US)</option><option value="3">Bahh (en-US)</option><option value="4">Bells (en-US)</option><option value="5">Boing (en-US)</option><option value="6">Bubbles (en-US)</option><option value="7">Cellos (en-US)</option><option value="8">Wobble (en-US)</option><option value="9">Fred (en-US)</option><option value="10">Good News (en-US)</option><option value="11">Jester (en-US)</option><option value="12">Junior (en-US)</option><option value="13">Kathy (en-US)</option><option value="14">Organ (en-US)</option><option value="15">Superstar (en-US)</option><option value="16">Ralph (en-US)</option><option value="17">Trinoids (en-US)</option><option value="18">Whisper (en-US)</option><option value="19">Zarvox (en-US)</option><option value="20">Daniel (en-GB)</option><option value="21">Karen (en-AU)</option><option value="22">Moira (en-IE)</option><option value="23">Rishi (en-IN)</option><option value="24">Tessa (en-ZA)</option></select>
  <label>Speed:</label>
  <input type="range" id="rate" min="0.5" max="1.5" step="0.1" value="1">
  <span id="rateVal">1.0x</span>
</div>
 
<textarea id="inputText" placeholder="Nhập tiếng Anh để sửa, hoặc tiếng Việt để dịch sang English native..."></textarea>
<div class="status" id="status"></div>
 
<div class="buttons">
  <button class="btn-mic" id="micBtn">🎤 Speak</button>
  <button class="btn-speak-input" id="speakInputBtn">🔊 Listen</button>
  <button class="btn-process" id="processBtn">✨ Process</button>
  <button class="btn-clear" id="clearBtn">🗑 Clear</button>
</div>
 
<div class="loading" id="loading">⏳ Tom Analyzing...</div>
<div class="error-box" id="errorBox"></div>
<div class="success-box" id="successBox"></div>
 
<div class="result" id="result">
  <h3 id="resultTitle">✅ Native Version: <span class="lang-badge" id="langBadge"></span></h3>
  <div class="corrected-wrap">
    <div class="corrected" id="corrected"></div>
    <button class="speak-btn" id="speakCorrectedBtn" title="Listen">🔊</button>
  </div>
 
  <div class="alternatives" id="altBox" style="display:none;">
    <h4>🌟 Other native ways to say it:</h4>
    <div id="altList"></div>
  </div>
 
  <h3 style="margin-top:15px;">💡 Explanation:</h3>
  <div class="explanation" id="explanation"></div>
</div>
```
 
  </div>
 
<script>
  const $ = id => document.getElementById(id);
  const inputText=$('inputText'), micBtn=$('micBtn'), speakInputBtn=$('speakInputBtn');
  const processBtn=$('processBtn'), clearBtn=$('clearBtn'), result=$('result');
  const correctedEl=$('corrected'), explanationEl=$('explanation');
  const loading=$('loading'), statusEl=$('status'), apiKeyInput=$('apiKey');
  const speakCorrectedBtn=$('speakCorrectedBtn'), voiceSelect=$('voiceSelect');
  const rateSlider=$('rate'), rateVal=$('rateVal');
  const errorBox=$('errorBox'), successBox=$('successBox');
  const modelSelect=$('modelSelect'), checkBtn=$('checkBtn');
  const langBadge=$('langBadge'), resultTitle=$('resultTitle');
  const altBox=$('altBox'), altList=$('altList');
 
  apiKeyInput.value = localStorage.getItem('gemini_key') || '';
  apiKeyInput.addEventListener('change', () => localStorage.setItem('gemini_key', apiKeyInput.value));
  if (localStorage.getItem('gemini_model')) modelSelect.value = localStorage.getItem('gemini_model');
  modelSelect.addEventListener('change', () => localStorage.setItem('gemini_model', modelSelect.value));
 
  // ---------- Mode tabs ----------
  let currentMode = localStorage.getItem('gemini_mode') || 'auto';
  document.querySelectorAll('.mode-tab').forEach(tab => {
    if (tab.dataset.mode === currentMode) tab.classList.add('active');
    else tab.classList.remove('active');
    tab.addEventListener('click', () => {
      document.querySelectorAll('.mode-tab').forEach(t => t.classList.remove('active'));
      tab.classList.add('active');
      currentMode = tab.dataset.mode;
      localStorage.setItem('gemini_mode', currentMode);
      // Update mic language hint
      if (recognition) {
        recognition.lang = currentMode === 'vi' ? 'vi-VN' : 'en-US';
      }
    });
  });
 
  // ---------- TTS ----------
  const synth = window.speechSynthesis;
  let voices = [];
  function loadVoices() {
    voices = synth.getVoices().filter(v => v.lang.startsWith('en'));
    voiceSelect.innerHTML = '';
    voices.forEach((v, i) => {
      const opt = document.createElement('option');
      opt.value = i;
      opt.textContent = `${v.name} (${v.lang})`;
      if (v.name.includes('Samantha') || v.name.includes('Google US')) opt.selected = true;
      voiceSelect.appendChild(opt);
    });
  }
  loadVoices();
  if (synth.onvoiceschanged !== undefined) synth.onvoiceschanged = loadVoices;
  rateSlider.addEventListener('input', () => rateVal.textContent = parseFloat(rateSlider.value).toFixed(1)+'x');
 
  function speak(text) {
    if (!text) return;
    synth.cancel();
    const utter = new SpeechSynthesisUtterance(text);
    const v = voices[voiceSelect.value];
    if (v) utter.voice = v;
    utter.rate = parseFloat(rateSlider.value);
    utter.lang = v ? v.lang : 'en-US';
    synth.speak(utter);
  }
  speakInputBtn.addEventListener('click', () => speak(inputText.value.trim()));
  speakCorrectedBtn.addEventListener('click', () => speak(correctedEl.textContent));
 
  // ---------- Speech Recognition ----------
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  let recognition = null, isRecording = false;
  if (SR) {
    recognition = new SR();
    recognition.lang = currentMode === 'vi' ? 'vi-VN' : 'en-US';
    recognition.interimResults = true;
    recognition.onstart = () => { isRecording=true; micBtn.classList.add('recording'); micBtn.textContent='⏹ Stop'; statusEl.textContent='🎙 Listening...'; };
    recognition.onresult = (e) => { let t=''; for(let i=0;i<e.results.length;i++) t+=e.results[i][0].transcript; inputText.value=t; };
    recognition.onerror = (e) => statusEl.textContent = '⚠ '+e.error;
    recognition.onend = () => { isRecording=false; micBtn.classList.remove('recording'); micBtn.textContent='🎤 Speak'; statusEl.textContent=''; };
  } else { micBtn.disabled=true; micBtn.textContent='🚫 No mic'; }
  micBtn.addEventListener('click', () => recognition && (isRecording ? recognition.stop() : recognition.start()));
 
  // ---------- UI helpers ----------
  function showError(msg) { errorBox.textContent='❌ '+msg; errorBox.style.display='block'; successBox.style.display='none'; }
  function showSuccess(msg) { successBox.textContent='✅ '+msg; successBox.style.display='block'; errorBox.style.display='none'; }
  function hideMessages() { errorBox.style.display='none'; successBox.style.display='none'; }
 
  // ---------- Detect language locally (quick heuristic) ----------
  function detectLanguage(text) {
    if (!text) return 'en';
    // Vietnamese has these unique diacritical characters
    const viChars = /[àáảãạăằắẳẵặâầấẩẫậèéẻẽẹêềếểễệìíỉĩịòóỏõọôồốổỗộơờớởỡợùúủũụưừứửữựỳýỷỹỵđÀÁẢÃẠĂẰẮẲẴẶÂẦẤẨẪẬÈÉẺẼẸÊỀẾỂỄỆÌÍỈĨỊÒÓỎÕỌÔỒỐỔỖỘƠỜỚỞỠỢÙÚỦŨỤƯỪỨỬỮỰỲÝỶỸỴĐ]/;
    if (viChars.test(text)) return 'vi';
 
    // Common Vietnamese words (including without diacritics, "telex" style)
    const viWordList = [
      'toi','tôi','ban','bạn','minh','mình','chung','chúng','la','là','cua','của',
      'va','và','hay','khong','không','duoc','được','muon','muốn','can','cần',
      'biet','biết','hieu','hiểu','nhu','như','nao','nào','gi','gì','sao',
      'nguoi','người','nha','nhà','cong','công','viec','việc','co','có',
      'di','đi','den','đến','lam','làm','noi','nói','nghe','xem','an','ăn',
      'uong','uống','ngu','ngủ','hoc','học','choi','chơi','rat','rất',
      'qua','quá','lam','lắm','nhieu','nhiều','it','ít','dep','đẹp','xau','xấu',
      'tot','tốt','hay','do','đó','nay','này','ay','ấy','the','thế'
    ];
    const lowerText = text.toLowerCase();
    const words = lowerText.match(/\b[a-zàáảãạăằắẳẵặâầấẩẫậèéẻẽẹêềếểễệìíỉĩịòóỏõọôồốổỗộơờớởỡợùúủũụưừứửữựỳýỷỹỵđ]+\b/gi) || [];
    if (words.length === 0) return 'en';
 
    let viMatches = 0;
    for (const w of words) {
      if (viWordList.includes(w)) viMatches++;
    }
    // If more than 25% of words look Vietnamese → it's Vietnamese
    if (viMatches / words.length > 0.25) return 'vi';
    return 'en';
  }
 
  // ---------- Test Key ----------
  checkBtn.addEventListener('click', async () => {
    const apiKey = apiKeyInput.value.trim();
    hideMessages();
    if (!apiKey) { showError('Please paste your API key first.'); return; }
    statusEl.textContent = '🔍 Testing key...';
    try {
      const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models?key=${apiKey}`);
      const data = await res.json();
      if (!res.ok || data.error) throw new Error((data.error && data.error.message) || 'HTTP '+res.status);
      const usable = data.models
        .filter(m => m.supportedGenerationMethods && m.supportedGenerationMethods.includes('generateContent'))
        .map(m => m.name.replace('models/', ''))
        .filter(n => n.startsWith('gemini'));
      modelSelect.innerHTML = '';
      usable.forEach(n => {
        const opt = document.createElement('option');
        opt.value = n; opt.textContent = n;
        modelSelect.appendChild(opt);
      });
      const preferred = usable.find(n => n === 'gemini-2.5-flash')
        || usable.find(n => n.includes('2.0-flash') && !n.includes('thinking') && !n.includes('exp'))
        || usable.find(n => n.includes('1.5-flash')) || usable[0];
      if (preferred) modelSelect.value = preferred;
      showSuccess(`Key works! Found ${usable.length} models.\nSelected: ${preferred}`);
      statusEl.textContent = '';
    } catch (err) {
      showError('Key test failed:\n' + err.message);
      statusEl.textContent = '';
    }
  });
 
  // ---------- JSON extractor ----------
  function extractJSON(text) {
    text = text.trim().replace(/^```(?:json)?\s*/i,'').replace(/```\s*$/i,'').trim();
    try { return JSON.parse(text); } catch(e) {}
    const m = text.match(/\{[\s\S]*\}/);
    if (m) { try { return JSON.parse(m[0]); } catch(e) {} }
    return null;
  }
 
  // ---------- Call Gemini ----------
  async function callGemini(prompt, apiKey, model) {
    const url = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;
    const res = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        contents: [{ parts: [{ text: prompt }] }],
        generationConfig: { temperature: 0.4, responseMimeType: 'application/json' }
      })
    });
    const data = await res.json();
    if (!res.ok || data.error) throw new Error((data.error && data.error.message) || 'HTTP '+res.status);
    if (!data.candidates || !data.candidates[0]) throw new Error('No response (maybe safety filter).');
    return data.candidates[0].content.parts[0].text;
  }
 
  // ---------- Build prompt by mode ----------
  function buildPrompt(text, mode) {
    const safeText = text.replace(/\\/g,'\\\\').replace(/"/g,'\\"').replace(/\n/g,'\\n');
 
    if (mode === 'vi') {
      return `Bạn là giáo viên tiếng Anh bản ngữ chuyên dịch tiếng Việt sang tiếng Anh tự nhiên (native style).
 
Người dùng nhập câu tiếng Việt. Nhiệm vụ của bạn:
1. Dịch sang tiếng Anh theo cách người bản xứ thực sự sẽ nói (KHÔNG dịch word-by-word).
2. Đưa ra 2-3 cách diễn đạt khác cũng tự nhiên (casual / formal / idiomatic).
3. Giải thích BẰNG TIẾNG VIỆT: từ vựng quan trọng, cấu trúc ngữ pháp, ngữ cảnh sử dụng, và lý do chọn cách dịch đó.
 
Chỉ trả lời JSON hợp lệ (không markdown, không text thừa):
{
  "detected_language": "vi",
  "corrected": "câu tiếng Anh native chính",
  "alternatives": [
    {"text": "cách 2", "tag": "casual"},
    {"text": "cách 3", "tag": "formal"}
  ],
  "explanation": "giải thích chi tiết bằng tiếng Việt: từ vựng, ngữ pháp, ngữ cảnh sử dụng..."
}
 
Câu của người dùng: "${safeText}"`;
    }
 
    if (mode === 'en') {
      return `Bạn là giáo viên tiếng Anh bản ngữ. Người dùng nhập câu tiếng Anh có thể có lỗi ngữ pháp, diễn đạt vụng về, hoặc nghe không tự nhiên.
 
Nhiệm vụ:
1. Viết lại câu cho thật tự nhiên như người bản xứ nói.
2. Đề xuất 2-3 cách diễn đạt thay thế cũng tự nhiên (casual / formal / idiomatic).
3. Giải thích BẰNG TIẾNG VIỆT: đã sửa gì, tại sao, và tips về từ vựng/ngữ pháp.
 
Chỉ trả lời JSON hợp lệ (không markdown, không text thừa):
{
  "detected_language": "en",
  "corrected": "câu tiếng Anh đã sửa cho native",
  "alternatives": [
    {"text": "cách 1", "tag": "casual"},
    {"text": "cách 2", "tag": "formal"}
  ],
  "explanation": "giải thích bằng tiếng Việt: đã sửa gì, tại sao, từ vựng và ngữ pháp..."
}
 
Câu của người dùng: "${safeText}"`;
    }
 
    // AUTO MODE
    return `Bạn là giáo viên tiếng Anh bản ngữ. Người dùng nhập một câu có thể bằng TIẾNG ANH hoặc TIẾNG VIỆT. Hãy TỰ ĐỘNG phát hiện ngôn ngữ và xử lý phù hợp.
 
QUY TẮC:
- Nếu là TIẾNG VIỆT → dịch sang tiếng Anh tự nhiên (native style, KHÔNG word-by-word).
- Nếu là TIẾNG ANH → viết lại cho thật tự nhiên như người bản xứ (sửa lỗi ngữ pháp, diễn đạt vụng về nếu có).
- Trong cả 2 trường hợp: đưa ra 2-3 cách diễn đạt thay thế (casual / formal / idiomatic).
- Giải thích BẰNG TIẾNG VIỆT: từ vựng quan trọng, ngữ pháp, ngữ cảnh sử dụng, lý do chọn cách diễn đạt đó. Nếu input là tiếng Anh, nói rõ đã sửa gì và tại sao.
 
Chỉ trả lời JSON hợp lệ (không markdown, không text thừa):
{
  "detected_language": "vi" hoặc "en",
  "corrected": "câu tiếng Anh native chính (kết quả)",
  "alternatives": [
    {"text": "cách thay thế 1", "tag": "casual"},
    {"text": "cách thay thế 2", "tag": "formal"}
  ],
  "explanation": "giải thích chi tiết bằng tiếng Việt..."
}
 
Câu của người dùng: "${safeText}"`;
  }
 
  // ---------- Render result ----------
  function renderResult(parsed) {
    const lang = parsed.detected_language === 'vi' ? 'vi' : 'en';
    if (lang === 'vi') {
      langBadge.textContent = '🇻🇳 Vietnamese → English';
      langBadge.className = 'lang-badge badge-vi';
      resultTitle.firstChild.textContent = '✅ Native English Translation: ';
    } else {
      langBadge.textContent = '🇬🇧 English (corrected)';
      langBadge.className = 'lang-badge badge-en';
      resultTitle.firstChild.textContent = '✅ Native Version: ';
    }
 
    correctedEl.textContent = parsed.corrected || '(no result)';
    explanationEl.textContent = parsed.explanation || '(no explanation)';
 
    // Alternatives
    altList.innerHTML = '';
    if (Array.isArray(parsed.alternatives) && parsed.alternatives.length > 0) {
      parsed.alternatives.forEach(alt => {
        if (!alt || !alt.text) return;
        const div = document.createElement('div');
        div.className = 'alt-item';
        const tag = (alt.tag || 'alt').toString();
        const text = alt.text.toString();
        div.innerHTML = `
          <span class="alt-tag"></span>
          <span class="alt-text"></span>
          <button class="alt-speak" title="Listen">🔊</button>
        `;
        div.querySelector('.alt-tag').textContent = tag;
        div.querySelector('.alt-text').textContent = text;
        div.querySelector('.alt-speak').addEventListener('click', () => speak(text));
        altList.appendChild(div);
      });
      altBox.style.display = 'block';
    } else {
      altBox.style.display = 'none';
    }
 
    result.style.display = 'block';
  }
 
  // ---------- Process ----------
  processBtn.addEventListener('click', async () => {
    hideMessages();
    const text = inputText.value.trim();
    const apiKey = apiKeyInput.value.trim();
    const model = modelSelect.value;
 
    if (!apiKey) { showError('Please paste your Gemini API key first.'); return; }
    if (!text) { showError('Please enter some text.'); return; }
 
    // Decide effective mode
    let effectiveMode = currentMode;
    if (currentMode === 'auto') {
      const detected = detectLanguage(text);
      statusEl.textContent = `🔍 Detected: ${detected === 'vi' ? 'Vietnamese' : 'English'}`;
    } else {
      statusEl.textContent = '';
    }
 
    const prompt = buildPrompt(text, effectiveMode);
 
    loading.style.display = 'block';
    result.style.display = 'none';
    processBtn.disabled = true;
 
    try {
      const raw = await callGemini(prompt, apiKey, model);
      const parsed = extractJSON(raw);
      if (!parsed || !parsed.corrected) {
        throw new Error('Could not parse response. Raw:\n' + raw.substring(0, 400));
      }
      renderResult(parsed);
    } catch (err) {
      showError(err.message);
    } finally {
      loading.style.display = 'none';
      processBtn.disabled = false;
    }
  });
 
  // ---------- Clear ----------
  clearBtn.addEventListener('click', () => {
    inputText.value = '';
    result.style.display = 'none';
    hideMessages();
    statusEl.textContent = '';
    inputText.focus();
  });
 
  // ---------- Keyboard shortcut: Ctrl/Cmd + Enter to process ----------
  inputText.addEventListener('keydown', (e) => {
    if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
      e.preventDefault();
      processBtn.click();
    }
  });
</script>
 
 
</body></html>