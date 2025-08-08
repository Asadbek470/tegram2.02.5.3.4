# tegram
TEGRAM
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>OpenTegram</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0; padding: 0;
      display: flex; height: 100vh;
    }
    #contacts {
      width: 30%;
      background-color: #f2f2f2;
      border-right: 1px solid #ccc;
      overflow-y: auto;
      padding: 10px;
    }
    #chat {
      flex: 1;
      display: flex;
      flex-direction: column;
      padding: 10px;
    }
    #chatHeader {
      font-weight: bold;
      padding: 10px 0;
      border-bottom: 1px solid #ccc;
    }
    #chatMessages {
      flex: 1;
      padding: 10px;
      overflow-y: auto;
    }
    #chatInput {
      display: flex;
      border-top: 1px solid #ccc;
      padding: 10px 0;
    }
    #chatInput input {
      flex: 1;
      padding: 10px;
      font-size: 1em;
    }
    #chatInput button {
      padding: 10px;
      font-size: 1em;
    }
    .contact {
      padding: 10px;
      cursor: pointer;
      border-bottom: 1px solid #ccc;
    }
    .contact:hover {
      background-color: #ddd;
    }
  </style>
</head>
<body>

  <div id="contacts">
    <h3>Контакты</h3>
    <div class="contact" onclick="openChat('Папа')">Папа</div>
    <div class="contact" onclick="openChat('Мама')">Мама</div>
    <div class="contact" onclick="openChat('Бабушка')">Бабушка</div>
    <div class="contact" onclick="openChat('Сестра')">Сестра</div>
    <div class="contact" onclick="openChat('Брат')">Брат</div>
  </div>

  <div id="chat">
    <div id="chatHeader">Выберите контакт</div>
    <div id="chatMessages"></div>
    <div id="chatInput">
      <input type="text" id="messageInput" placeholder="Введите сообщение..." />
      <button onclick="sendMessage()">Отправить</button>
    </div>
  </div>

  <script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
  <script>
    const socket = io("http://localhost:3000"); // Обязательно: сервер должен быть запущен
    let currentContact = "";

    function openChat(contact) {
      currentContact = contact;
      document.getElementById("chatHeader").textContent = `Чат с ${contact}`;
      document.getElementById("chatMessages").innerHTML = "";
      socket.emit("joinChat", contact);
    }

    function sendMessage() {
      const input = document.getElementById("messageInput");
      const text = input.value.trim();
      if (!text || !currentContact) return;

      const msg = {
        sender: "Вы",
        text: text,
        contact: currentContact
      };

      socket.emit("newMessage", msg);
      appendMessage(msg);
      input.value = "";
    }

    function appendMessage(msg) {
      const div = document.createElement("div");
      div.textContent = `${msg.sender}: ${msg.text}`;
      document.getElementById("chatMessages").appendChild(div);
    }

    socket.on("newMessage", (msg) => {
      if (msg.contact === currentContact) {
        appendMessage(msg);
      }
    });

    socket.on("allMessages", (msgs) => {
      const box = document.getElementById("chatMessages");
      box.innerHTML = "";
      msgs.filter(m => m.contact === currentContact).forEach(msg => appendMessage(msg));
    });
  </script>
</body>
</html>

