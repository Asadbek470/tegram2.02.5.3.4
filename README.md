#tegramm
uzgramm
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>OpenTegram</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #1e1e1e;
      color: white;
    }
    .splash {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: linear-gradient(135deg, #0088cc, #005577);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 3em;
      animation: fadeOut 2s ease 3s forwards;
      z-index: 1000;
    }
    @keyframes fadeOut {
      to {
        opacity: 0;
        visibility: hidden;
      }
    }
    .container {
      padding: 20px;
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background-color: #2a2a2a;
      padding: 10px;
    }
    .btn {
      background-color: #0088cc;
      border: none;
      padding: 10px 20px;
      color: white;
      cursor: pointer;
      border-radius: 5px;
      margin-left: 5px;
    }
    .chat {
      margin-top: 20px;
    }
    .message {
      background-color: #333;
      padding: 10px;
      margin: 5px 0;
      border-radius: 5px;
    }
    .hidden { display: none; }
  </style>
</head>
<body>
  <div class="splash">OpenTegram</div>

  <div class="header">
    <div>ID: <span id="userIdDisplay"></span></div>
    <div>
      <button class="btn" onclick="showNewChatDialog()">Новый чат</button>
      <button class="btn" onclick="logout()">Выйти</button>
    </div>
  </div>

  <div class="container">
    <div class="chat" id="chatBox"></div>
    <input type="text" id="messageInput" placeholder="Введите сообщение...">
    <button class="btn" onclick="sendMessage()">Отправить</button>
  </div>

  <div id="newChatDialog" class="container hidden">
    <h3>Новый чат</h3>
    <input type="text" id="newChatId" placeholder="Введите ID пользователя">
    <button class="btn" onclick="startChat()">Начать чат</button>
  </div>

  <script>
    let userId = Math.floor(Math.random() * 1000000);
    let activeChatId = null;
    let chats = {};

    document.getElementById('userIdDisplay').innerText = userId;

    function showNewChatDialog() {
      document.getElementById('newChatDialog').classList.remove('hidden');
    }

    function startChat() {
      const id = document.getElementById('newChatId').value.trim();
      if (id && id !== userId.toString()) {
        activeChatId = id;
        if (!chats[activeChatId]) chats[activeChatId] = [];
        renderChat();
        document.getElementById('newChatDialog').classList.add('hidden');
      }
    }

    function sendMessage() {
      const input = document.getElementById('messageInput');
      const message = input.value.trim();
      if (message && activeChatId) {
        chats[activeChatId].push(`Вы: ${message}`);
        input.value = '';
        renderChat();
      }
    }

    function renderChat() {
      const chatBox = document.getElementById('chatBox');
      chatBox.innerHTML = '';
      if (activeChatId && chats[activeChatId]) {
        chats[activeChatId].forEach(msg => {
          const div = document.createElement('div');
          div.className = 'message';
          div.innerText = msg;
          chatBox.appendChild(div);
        });
      }
    }

    function logout() {
      location.reload();
    }
  </script>
</body>
</html>
