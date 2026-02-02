<html lang="en">
<head>
  <title>Peter AI</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg: #0f172a;
      --surface: #1e293b;
      --primary: #3b82f6;
      --text: #e2e8f0;
      --user-bg: #1d4ed8;
      --ai-bg: #334155;
    }
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    header {
      background: linear-gradient(135deg, #1e3a8a, #1e40af);
      padding: 1.2rem 1.5rem;
      text-align: center;
      border-bottom: 1px solid rgba(255,255,255,0.1);
      box-shadow: 0 4px 20px rgba(0,0,0,0.3);
    }
    header h1 {
      font-size: 1.6rem;
      margin-bottom: 0.3rem;
      letter-spacing: 0.5px;
    }
    header p {
      font-size: 0.9rem;
      opacity: 0.9;
    }
    main {
      flex: 1;
      padding: 1rem;
      max-width: 900px;
      margin: 0 auto;
      width: 100%;
    }
    #chat {
      height: 60vh;
      overflow-y: auto;
      border-radius: 12px;
      padding: 0.8rem 1rem;
      background: var(--surface);
      border: 1px solid rgba(255,255,255,0.1);
      box-shadow: 0 2px 12px rgba(0,0,0,0.2);
      display: flex;
      flex-direction: column;
    }
    #chat::before {
      content: "";
      flex: 1;
    }
    .msg {
      max-width: 75%;
      padding: 0.7rem 1rem;
      border-radius: 16px 16px 4px 16px;
      margin-bottom: 0.7rem;
      font-size: 0.95rem;
      line-height: 1.4;
      opacity: 0;
      transform: translateY(10px);
      animation: slideIn 0.3s ease-out forwards;
    }
    .user {
      align-self: flex-end;
      background: var(--user-bg);
      color: white;
      border-radius: 16px 16px 16px 4px;
    }
    .ai {
      align-self: flex-start;
      background: var(--ai-bg);
      border-radius: 16px 16px 4px 16px;
    }
    @keyframes slideIn {
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }
    #typing {
      align-self: flex-start;
      background: var(--ai-bg);
      padding: 0.6rem 0.9rem;
      border-radius: 12px;
      font-size: 0.9rem;
      margin-bottom: 0.5rem;
      display: none;
    }
    .dots span {
      display: inline-block;
      width: 6px;
      height: 6px;
      background: #94a3b8;
      border-radius: 50%;
      margin: 0 2px;
      opacity: 0.6;
      animation: pulse 1.4s infinite ease-in-out;
    }
    .dots span:nth-child(1) { animation-delay: 0s; }
    .dots span:nth-child(2) { animation-delay: 0.2s; }
    .dots span:nth-child(3) { animation-delay: 0.4s; }
    @keyframes pulse {
      0%, 80%, 100% { transform: scale(0.8); opacity: 0.6; }
      40% { transform: scale(1.1); opacity: 0.9; }
    }
    footer {
      padding: 0.8rem;
      text-align: center;
      color: #94a3b8;
      font-size: 0.85rem;
    }
    #input-area {
      display: flex;
      gap: 0.5rem;
      margin-top: 0.8rem;
    }
    #input-area input {
      flex: 1;
      padding: 0.8rem 1rem;
      border: 1px solid rgba(255,255,255,0.15);
      border-radius: 10px;
      background: rgba(255,255,255,0.05);
      color: var(--text);
      font-size: 1rem;
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
    }
    #input-area input:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(59,130,246,0.3);
    }
    #input-area button {
      padding: 0.8rem 1.3rem;
      background: linear-gradient(135deg, #3b82f6, #1d4ed8);
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-weight: 500;
      transition: transform 0.15s, box-shadow 0.15s;
    }
    #input-area button:hover {
      transform: translateY(-1px);
      box-shadow: 0 4px 12px rgba(59,130,246,0.4);
    }
    #input-area button:active {
      transform: translateY(0);
    }
    @media (max-width: 500px) {
      header h1 { font-size: 1.4rem; }
      #chat { height: 55vh; }
      .msg { max-width: 90%; }
    }
  </style>
</head>
<body>
  <header>
    <h1>Peter AI</h1>
    <p>Ask me anything in any language 🌍</p>
  </header>

  <main>
    <div id="chat">
      <div id="typing" class="msg ai">
        <div class="dots">
          <span></span><span></span><span></span>
        </div>
      </div>
    </div>
    <div id="input-area">
      <input type="text" id="user-input" placeholder="Type your message here..." autocomplete="off" />
      <button onclick="send()">Send</button>
    </div>
  </main>

  <footer>
    Peter AI – ChatGPT‑style assistant
  </footer>

  <script>
    const chat = document.getElementById("chat");
    const input = document.getElementById("user-input");
    const typing = document.getElementById("typing");

    // === CONFIG (you change these) ===
    const API_KEY = "YOUR_API_KEY"; // e.g., OpenAI, DeepSeek, etc.
    const API_ENDPOINT = "YOUR_API_ENDPOINT"; // e.g., "https://api.openai.com/v1/chat/completions"

    async function askAI(prompt) {
      try {
        const res = await fetch(API_ENDPOINT, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${API_KEY}`
          },
          body: JSON.stringify({
            model: "gpt-4o", // or whatever model you use
            messages: [{ role: "user", content: prompt }],
            max_tokens: 1000,
            temperature: 0.7
          })
        });
        const data = await res.json();
        return data.choices?.[0]?.message?.content || "No response.";
      } catch (err) {
        return "Error contacting AI. Check console.";
      }
    }

    function addMsg(text, isUser) {
      const div = document.createElement("div");
      div.className = `msg ${isUser ? "user" : "ai"}`;
      div.textContent = text;
      chat.appendChild(div);
      chat.scrollTop = chat.scrollHeight;
    }

    function showTyping() {
      typing.style.display = "block";
      chat.scrollTop = chat.scrollHeight;
    }
    function hideTyping() {
      typing.style.display = "none";
    }

    async function send() {
      const text = input.value.trim();
      if (!text) return;

      addMsg(text, true);
      input.value = "";

      showTyping();

      const reply = await askAI(text);
      hideTyping();
      addMsg(reply, false);
    }

    input.addEventListener("keypress", (e) => {
      if (e.key === "Enter") send();
    });

    window.addEventListener("load", () => {
      addMsg("Welcome to Peter AI! I can answer questions in any language, explain code, help with studies, and more. Ask me anything!", false);
    });
  </script>
</body>
</html>
