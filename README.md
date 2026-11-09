index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SmoothAI</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background: #0f172a; color: #f8fafc; height: 100vh; display: flex; flex-direction: column; }
        header { padding: 15px; background: #1e293b; border-bottom: 1px solid #334155; display: flex; justify-content: space-between; align-items: center; }
        h1 { font-size: 1.2rem; background: linear-gradient(135deg, #60a5fa, #a855f7); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        select { background: #334155; color: #fff; border: 1px solid #475569; padding: 6px 10px; border-radius: 8px; outline: none; }
        #chat-container { flex: 1; padding: 15px; overflow-y: auto; display: flex; flex-direction: column; gap: 12px; }
        .message { max-width: 85%; padding: 12px 16px; border-radius: 12px; line-height: 1.4; word-wrap: break-word; }
        .user { align-self: flex-end; background: linear-gradient(135deg, #2563eb, #1d4ed8); color: white; border-bottom-right-radius: 2px; }
        .bot { align-self: flex-start; background: #1e293b; border: 1px solid #334155; border-bottom-left-radius: 2px; }
        #input-container { padding: 12px; background: #1e293b; border-top: 1px solid #334155; display: flex; gap: 8px; }
        input { flex: 1; padding: 12px; background: #0f172a; border: 1px solid #334155; border-radius: 8px; color: white; font-size: 1rem; outline: none; }
        button { background: linear-gradient(135deg, #3b82f6, #9333ea); color: white; border: none; padding: 12px 18px; border-radius: 8px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body>
    <header>
        <h1>SmoothAI</h1>
        <select id="modeSelect">
            <option value="general">🤖 General AI</option>
            <option value="coding">💻 Coding Expert</option>
            <option value="writing">✍️ Writing Assistant</option>
            <option value="study">📚 Study Buddy</option>
        </select>
    </header>

    <div id="chat-container">
        <div class="message bot">Hello! I am SmoothAI. How can I assist you today?</div>
    </div>

    <div id="input-container">
        <input type="text" id="userInput" placeholder="Ask SmoothAI anything..." />
        <button onclick="sendMessage()">Send</button>
    </div>

    <script>
        // PASTE YOUR WEB APP URL INSIDE THE QUOTES BELOW:
        const BACKEND_URL = "YOUR_APPS_SCRIPT_WEB_APP_URL_HERE";

        async function sendMessage() {
            const inputField = document.getElementById('userInput');
            const mode = document.getElementById('modeSelect').value;
            const message = inputField.value.trim();
            if (!message) return;

            appendMessage(message, 'user');
            inputField.value = '';

            const botMsgDiv = appendMessage("Typing...", 'bot');

            try {
                const response = await fetch(BACKEND_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                    body: JSON.stringify({ message: message, mode: mode })
                });

                const data = await response.json();
                if (data.status === 'success') {
                    botMsgDiv.innerText = data.reply;
                } else {
                    botMsgDiv.innerText = "Error: " + (data.message || "Could not reach AI.");
                }
            } catch (err) {
                botMsgDiv.innerText = "Network error. Please check your backend Web App URL.";
            }
        }

        function appendMessage(text, sender) {
            const container = document.getElementById('chat-container');
            const msgDiv = document.createElement('div');
            msgDiv.className = `message ${sender}`;
            msgDiv.innerText = text;
            container.appendChild(msgDiv);
            container.scrollTop = container.scrollHeight;
            return msgDiv;
        }
    </script>
</body>
</html>

