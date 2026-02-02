<!DOCTYPE html>
<html lang="en">
<head>
  <title>Peter AI</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    body {
      font-family: system-ui, sans-serif;
      margin: 0;
      background: #f4f6f8;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }
    header {
      background: #101010;
      color: #fff;
      padding: 1rem 1.5rem;
      text-align: center;
    }
    main {
      flex: 1;
      padding: 1rem;
      max-width: 800px;
      margin: 0 auto;
      width: 100%;
    }
    #chat {
      height: 60vh;
      overflow-y: auto;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 0.8rem;
      background: #fff;
      margin-bottom: 0.8rem;
    }
    .msg {
      margin-bottom: 0.6rem;
      padding: 0.5rem 0.8rem;
      border-radius: 16px;
      max-width: 80%;
    }
    .user {
      background: #e3f2fd;
      margin-left: auto;
      text-align: right;
    }
    .ai {
      background: #f0f0f0;
      margin-right: auto;
    }
    footer {
      padding: 1rem;
      text-align: center;
      color: #666;
      font-size: 0.9rem;
    }
    #input-area {
      display: flex;
      gap: 0.5rem;
    }
    #input-area input {
      flex: 1;
      padding: 0.7rem;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 1rem;
    }
    #input-area button {
      padding: 0.7rem 1.2rem;
      background: #007bff;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    #input-area button:hover {
      background: #0056b3;
    }
  </style>
</head>
<body>
  <header>
    <h1>Peter AI</h1>
    <p>Ask me anything in any language</p>
  </header>

  <main>
    <div id="chat"></div>
    <div id="input-area">
      <input type="text" id="user-input" placeholder="Type your message here..." />
      <button onclick="send()">Send</button>
    </div>
  </main>

  <footer>
    Peter AI – a simple multilingual chat demo
  </footer>

  <script>
    const chat = document.getElementById("chat");
    const input = document.getElementById("user-input");

    function addMsg(text, isUser) {
      const div = document.createElement("div");
      div.className = `msg ${isUser ? "user" : "ai"}`;
      div.textContent = text;
      chat.appendChild(div);
      chat.scrollTop = chat.scrollHeight;
    }

    function send() {
      const text = input.value.trim();
      if (!text) return;

      addMsg(text, true);
      input.value = "";

      // Simple “AI” replies (you can replace this with a real API later)
      setTimeout(() => {
        const lower = text.toLowerCase();
        let reply;

        if (lower.includes("hi") || lower.includes("hello") || lower.includes("नमस्ते") || lower.includes("مرحبا")) {
          reply = "Hello! How can I help you today? आपकी कैसे मदद कर सकता हूँ? كيف يمكنني مساعدتك اليوم؟";
        } else if (lower.includes("name")) {
          reply = "My name is Peter AI. I'm here to chat with you in any language.";
        } else if (lower.includes("help")) {
          reply = "I can answer questions, explain concepts, and chat in many languages. Just type in your language!";
        } else {
          reply = "I understand many languages. Could you rephrase or ask something else?";
        }

        addMsg(reply, false);
      }, 300);
    }

    // Allow pressing Enter to send
    input.addEventListener("keypress", (e) => {
      if (e.key === "Enter") send();
    });

    // Welcome message on load
    window.addEventListener("load", () => {
      addMsg("Welcome to Peter AI! Type your message in any language and press Send.", false);
    });
  </script>
</body>
</html>
