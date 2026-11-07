# JARV-S
...
<!DOCTYPE html>
<html lang="az">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>JARVIS | AI Assistant</title>
  <style>
    :root {
      --bg-1: #050b12;
      --bg-2: #0b1d2a;
      --panel: rgba(11, 19, 28, 0.92);
      --panel-2: rgba(14, 30, 42, 0.9);
      --border: rgba(88, 196, 255, 0.4);
      --primary: #69d9ff;
      --primary-2: #23bde9;
      --text: #eafcff;
      --muted: #9cc7d5;
      --success: #7efeb5;
      --warning: #ffd875;
      --danger: #ff7d7d;
      --shadow: rgba(0, 197, 255, 0.22);
    }

    * {
      box-sizing: border-box;
    }

    html, body {
      margin: 0;
      height: 100%;
      font-family: Arial, Helvetica, sans-serif;
      background:
        radial-gradient(circle at top left, rgba(32, 86, 108, 0.7), transparent 28%),
        radial-gradient(circle at top right, rgba(27, 151, 194, 0.35), transparent 24%),
        linear-gradient(135deg, var(--bg-1), var(--bg-2));
      color: var(--text);
    }

    body {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 24px;
    }

    .shell {
      width: min(1200px, 96vw);
      min-height: 780px;
      background: rgba(5, 12, 18, 0.82);
      border: 1px solid var(--border);
      border-radius: 22px;
      box-shadow: 0 0 45px var(--shadow);
      overflow: hidden;
      display: grid;
      grid-template-columns: 280px 1fr;
      position: relative;
    }

    .shell::before {
      content: "";
      position: absolute;
      inset: 0;
      background: linear-gradient(180deg, rgba(255,255,255,0.03), transparent 25%);
      pointer-events: none;
    }

    .sidebar {
      background: rgba(9, 18, 27, 0.95);
      border-right: 1px solid var(--border);
      padding: 22px 18px;
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 10px;
      border: 1px solid var(--border);
      border-radius: 14px;
      background: rgba(13, 28, 39, 0.9);
    }

    .brand-badge {
      width: 38px;
      height: 38px;
      border-radius: 50%;
      display: grid;
      place-items: center;
      background: radial-gradient(circle at 35% 35%, #93f0ff, #1dd6ff 28%, #0a2d39 72%);
      color: #03222b;
      font-weight: 700;
      box-shadow: 0 0 20px rgba(105, 217, 255, 0.7);
    }

    .brand h2 {
      margin: 0;
      font-size: 1.15rem;
      letter-spacing: 5px;
      color: var(--primary);
      text-shadow: 0 0 12px rgba(105, 217, 255, 0.8);
    }

    .system {
      background: rgba(8, 27, 37, 0.82);
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 14px 12px;
    }

    .status-title {
      font-size: 0.72rem;
      letter-spacing: 2px;
      color: var(--muted);
      margin-bottom: 12px;
      text-transform: uppercase;
    }

    .status-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 10px;
      font-size: 0.9rem;
    }

    .dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: var(--success);
      box-shadow: 0 0 12px var(--success);
      display: inline-block;
    }

    .status-value {
      color: var(--muted);
      font-weight: 600;
    }

    .quick-actions {
      display: grid;
      gap: 10px;
    }

    .quick-btn {
      border: 1px solid var(--border);
      background: linear-gradient(180deg, rgba(17, 32, 43, 0.9), rgba(9, 18, 27, 0.9));
      color: var(--text);
      border-radius: 12px;
      padding: 12px 14px;
      font-size: 0.92rem;
      text-align: left;
      cursor: pointer;
      transition: 0.2s ease;
    }

    .quick-btn:hover {
      transform: translateY(-1px);
      border-color: rgba(105, 217, 255, 0.9);
      box-shadow: 0 0 18px rgba(105, 217, 255, 0.18);
    }

    .main {
      display: flex;
      flex-direction: column;
      min-width: 0;
    }

    .topbar {
      height: 78px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 28px;
      border-bottom: 1px solid var(--border);
      background: rgba(10, 20, 29, 0.7);
    }

    .topbar .title {
      font-size: 1.05rem;
      letter-spacing: 2px;
      color: var(--primary);
      text-transform: uppercase;
    }

    .topbar .meta {
      color: var(--muted);
      font-size: 0.8rem;
      letter-spacing: 1px;
    }

    .chat {
      flex: 1;
      overflow-y: auto;
      padding: 22px 22px 10px;
      display: flex;
      flex-direction: column;
      gap: 18px;
      background:
        linear-gradient(180deg, rgba(14, 28, 40, 0.2), rgba(5, 12, 18, 0.12)),
        rgba(6, 14, 22, 0.4);
    }

    .message {
      max-width: 78%;
      padding: 14px 16px;
      border-radius: 16px;
      line-height: 1.65;
      font-size: 0.97rem;
      word-break: break-word;
      box-shadow: 0 0 16px rgba(0,0,0,0.12);
    }

    .message.user {
      background: linear-gradient(135deg, rgba(16, 79, 95, 0.94), rgba(21, 103, 125, 0.92));
      border: 1px solid rgba(95, 214, 255, 0.4);
      align-self: flex-end;
      border-bottom-right-radius: 6px;
    }

    .message.jarvis {
      background: linear-gradient(135deg, rgba(11, 28, 36, 0.98), rgba(15, 39, 47, 0.92));
      border: 1px solid rgba(101, 174, 201, 0.4);
      align-self: flex-start;
      border-bottom-left-radius: 6px;
    }

    .message .label {
      display: inline-block;
      margin-bottom: 8px;
      font-size: 0.72rem;
      letter-spacing: 2px;
      text-transform: uppercase;
      color: var(--muted);
    }

    .composer {
      border-top: 1px solid var(--border);
      background: rgba(9, 18, 27, 0.9);
      padding: 18px 20px 22px;
    }

    .quick-pills {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 12px;
    }

    .pill {
      background: rgba(15, 37, 49, 0.8);
      border: 1px solid rgba(120, 201, 228, 0.35);
      color: var(--muted);
      padding: 8px 12px;
      border-radius: 999px;
      cursor: pointer;
      font-size: 0.82rem;
    }

    .pill:hover {
      border-color: rgba(105, 217, 255, 0.8);
      color: var(--text);
    }

    form {
      display: flex;
      gap: 12px;
      align-items: center;
    }

    input[type="text"] {
      flex: 1;
      min-width: 0;
      background: rgba(6, 14, 22, 0.9);
      color: var(--text);
      border: 1px solid rgba(98, 185, 220, 0.42);
      border-radius: 14px;
      padding: 16px 18px;
      font-size: 1rem;
      outline: none;
    }

    input[type="text"]:focus {
      border-color: rgba(105, 217, 255, 0.9);
      box-shadow: 0 0 18px rgba(105, 217, 255, 0.15);
    }

    button.send {
      border: none;
      background: linear-gradient(135deg, var(--primary), var(--primary-2));
      color: #05212b;
      font-weight: 700;
      border-radius: 14px;
      padding: 16px 22px;
      cursor: pointer;
      min-width: 110px;
      transition: 0.2s ease;
    }

    button.send:hover {
      filter: brightness(1.08);
      transform: translateY(-1px);
    }

    .typing {
      display: none;
      align-self: flex-start;
      max-width: 180px;
      background: rgba(11, 31, 41, 0.95);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 12px 14px;
      border-bottom-left-radius: 6px;
    }

    .typing.active {
      display: block;
    }

    .dots {
      display: inline-flex;
      gap: 8px;
      align-items: center;
      height: 18px;
    }

    .dots span {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: #bfeeff;
      display: inline-block;
      animation: blink 1.1s infinite ease-in-out;
    }

    .dots span:nth-child(2) { animation-delay: 0.18s; }
    .dots span:nth-child(3) { animation-delay: 0.36s; }

    @keyframes blink {
      0%, 80%, 100% { opacity: 0.2; transform: scale(0.9); }
      40% { opacity: 1; transform: scale(1.1); }
    }

    @media (max-width: 860px) {
      .shell {
        grid-template-columns: 1fr;
      }

      .sidebar {
        border-right: none;
        border-bottom: 1px solid var(--border);
      }

      .message {
        max-width: 90%;
      }
    }
  </style>
</head>
<body>
  <div class="shell">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-badge">J</div>
        <h2>JARVIS</h2>
      </div>

      <div class="system">
        <div class="status-title">System</div>

        <div class="status-row">
          <span>Core Link</span>
          <span class="dot"></span>
        </div>

        <div class="status-row">
          <span>Power</span>
          <span class="status-value">Stable</span>
        </div>

        <div class="status-row">
          <span>Mode</span>
          <span class="status-value">Assist</span>
        </div>

        <div class="status-row">
          <span>Latency</span>
          <span class="status-value">12ms</span>
        </div>
      </div>

      <div class="quick-actions">
        <button class="quick-btn" data-input="Status report">Status report</button>
        <button class="quick-btn" data-input="Project plan">Project plan</button>
        <button class="quick-btn" data-input="Help me code">Help me code</button>
        <button class="quick-btn" data-input="What time is it?">What time is it?</button>
      </div>
    </aside>

    <main class="main">
      <div class="topbar">
        <div class="title">AI Assistant</div>
        <div class="meta">Online • Secure • Adaptive</div>
      </div>

      <section id="chat" class="chat">
        <div class="message jarvis">
          <span class="label">JARVIS</span><br>
          Salam. Mən JARVISəm. Layihəniz, kodunuz, ideyanız və ya iş planınız üçün kömək edə bilərəm.
        </div>
      </section>

      <div id="typing" class="typing" aria-live="polite">
        <div class="dots">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>

      <div class="composer">
        <div class="quick-pills">
          <button class="pill" type="button">Status</button>
          <button class="pill" type="button">Plan</button>
          <button class="pill" type="button">Debug</button>
          <button class="pill" type="button">Code</button>
          <button class="pill" type="button">Time</button>
        </div>

        <form id="form">
          <input id="input" type="text" placeholder="JARVIS-ə soruş..." autocomplete="off" />
          <button class="send" type="submit">Göndər</button>
        </form>
      </div>
    </main>
  </div>

  <script>
    const chat = document.getElementById("chat");
    const form = document.getElementById("form");
    const input = document.getElementById("input");
    const typing = document.getElementById("typing");

    function addMessage(text, sender) {
      const el = document.createElement("div");
      el.className = `message ${sender}`;
      const label = sender === "user" ? "YOU" : "JARVIS";
      el.innerHTML = `<span class="label">${label}</span><br>${text}`;
      chat.appendChild(el);
      chat.scrollTop = chat.scrollHeight;
    }

    function setTyping(show) {
      typing.classList.toggle("active", show);
    }

    function botReply(userText) {
      const t = userText.toLowerCase().trim();

      if (!t) return "Sualınızı yazın.";

      if (t.includes("salam") || t.includes("hello") || t.includes("hi")) {
        return "Salam. Mən JARVISəm. Necə kömək edə bilərəm?";
      }

      if (t.includes("vaxt") || t.includes("time")) {
        const now = new Date();
        return `Hazırkı vaxt: ${now.toLocaleTimeString("az-AZ")} — ${now.toLocaleDateString("az-AZ")}.`;
      }

      if (t.includes("status") || t.includes("durum")) {
        return "Sistem hazırdır. Server status: online, enerji sabit, modul aktiv, optimizasiya rejimindədir.";
      }

      if (t.includes("plan") || t.includes("layihə")) {
        return "İş planı: 1) Məqsəd müəyyənləşdirin. 2) Arxitektura qurun. 3) Minimal nümunə yaradın. 4) Test edin. 5) Optimallaşdırın.";
      }

      if (t.includes("kod") || t.includes("code") || t.includes("program")) {
        return "Kod yazarkən: məqsəd aydın olmalıdır, funksiyanı kiçik hissələrə bölün, error handling əlavə edin və test edin. İstəsəniz, konkret kod yaza bilərəm.";
      }

      if (t.includes("debug") || t.includes("xəta") || t.includes("error")) {
        return "Xəta aradan qaldırmaq üçün əvvəl problemi təkrarlayın, sonra logları yoxlayın, sonra minimal reproducer yaradın və sonunda kök səbəbi tapın.";
      }

      if (t.includes("yardım") || t.includes("help")) {
        return "Mənə bunlardan birini yazın: status, plan, kod, debug, vaxt, layihə, optimizasiya.";
      }

      if (t.includes("optimi") || t.includes("performans")) {
        return "Optimizasiya üçün: gereksiz dövrləri azaltın, cache istifadə edin, IO əməliyyatlarını minimuma endirin və ölçmə (benchmark) aparın.";
      }

      if (t.includes("sağol") || t.includes("bye")) {
        return "Rahatlıqla. Yenidən yazın, kömək etməyə hazıram.";
      }

      return `Anladım: "${userText}". Bu demo versiyada real AI backend yoxdur, amma mən sizə struktur, plan, debug və kod məsləhəti verə bilərəm. Daha konkret yazın, məsələn: "Layihə planı", "Debug et", "Kod yaz", "Optimizasiya et".`;
    }

    form.addEventListener("submit", (e) => {
      e.preventDefault();
      const value = input.value.trim();
      if (!value) return;

      addMessage(value, "user");
      input.value = "";

      setTyping(true);

      setTimeout(() => {
        const reply = botReply(value);
        setTyping(false);
        addMessage(reply, "jarvis");
      }, 700);
    });

    document.querySelectorAll(".quick-btn").forEach((btn) => {
      btn.addEventListener("click", () => {
        const val = btn.dataset.input;
        input.value = val;
        input.focus();
      });
    });

    document.querySelectorAll(".pill").forEach((pill) => {
      pill.addEventListener("click", () => {
        const val = pill.textContent.trim();
        input.value = val;
        input.focus();
      });
    });
  </script>
</body>
</html>