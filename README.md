# tegram
TEGRAM
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>OpenTegram - Telegram Clone</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f0f2f5;
    }
    .splash-screen {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: #0088cc;
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      z-index: 9999;
      animation: fadeOut 2s ease forwards;
      animation-delay: 2s;
    }
    @keyframes fadeOut {
      to {
        opacity: 0;
        visibility: hidden;
      }
    }
    .container {
      display: flex;
      height: 100vh;
    }
    .sidebar {
      width: 300px;
      background: rgba(255, 255, 255, 0.8);
      backdrop-filter: blur(10px);
      padding: 1rem;
      overflow-y: auto;
    }
    .main {
      flex: 1;
      padding: 1rem;
    }
    .section-title {
      font-weight: bold;
      margin-top: 1rem;
    }
    .item {
      background: white;
      border-radius: 8px;
      padding: 0.5rem;
      margin-top: 0.5rem;
      cursor: pointer;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .item:hover {
      background: #e6f2fb;
    }
    .chat-window {
      margin-top: 1rem;
      background: white;
      border-radius: 8px;
      padding: 1rem;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    .chat-input {
      display: flex;
      margin-top: 1rem;
    }
    .chat-input input {
      flex: 1;
      padding: 0.5rem;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    .chat-input button {
      margin-left: 0.5rem;
      padding: 0.5rem 1rem;
      border: none;
      background: #0088cc;
      color: white;
      border-radius: 6px;
      cursor: pointer;
    }
    .add-contact {
      margin-top: 1rem;
    }
    .add-contact input {
      width: calc(100% - 1rem);
      margin-bottom: 0.5rem;
      padding: 0.5rem;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    .add-contact button {
      width: 100%;
      padding: 0.5rem;
      border: none;
      background: #0088cc;
      color: white;
      border-radius: 6px;
      cursor: pointer;
    }
    .settings {
      margin-top: 2rem;
    }
    .settings button {
      display: block;
      width: 100%;
      padding: 0.5rem;
      margin-top: 0.5rem;
      border: none;
      background: #ccc;
      border-radius: 6px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="splash-screen">Welcome to OpenTegram</div>
  <div class="container">
    <div class="sidebar">
      <div class="section-title">Contacts</div>
      <div id="contacts"></div>
      <div class="add-contact">
        <input type="text" id="contactName" placeholder="Имя контакта">
        <input type="text" id="contactPhone" placeholder="Номер телефона">
        <button onclick="addContact()">Создать контакт</button>
      </div>
      <div class="settings">
        <div class="section-title">Настройки</div>
        <button onclick="alert('Открытие настроек безопасности')">Безопасность</button>
        <button onclick="alert('Открытие настроек конфиденциальности')">Конфиденциальность</button>
      </div>
    </div>
    <div class="main">
      <h2>Welcome to OpenTegram</h2>
      <div id="chat" class="chat-window"></div>
      <div class="chat-input">
        <input type="text" id="messageInput" placeholder="Type a message...">
        <button onclick="sendMessage()">Send</button>
      </div>
    </div>
  </div>
  <script>
    let contacts = [
      { name: "Папа", phone: "" },
      { name: "Мама", phone: "" },
      { name: "Бабушка", phone: "" },
      { name: "Братья", phone: "" },
      { name: "Сестры", phone: "" }
    ];

    function renderContacts() {
      const container = document.getElementById("contacts");
      container.innerHTML = "";
      contacts.forEach((item, index) => {
        const div = document.createElement("div");
        div.className = "item";
        div.innerHTML = `${item.name} (${item.phone || 'не указан'}) <button onclick="editPhone(${index})" style="float:right">✎</button>`;
        div.onclick = (e) => {
          if (!e.target.matches("button")) openChat(item.name);
        };
        container.appendChild(div);
      });
    }

    function editPhone(index) {
      const newPhone = prompt("Введите новый номер телефона:", contacts[index].phone);
      if (newPhone !== null) {
        contacts[index].phone = newPhone;
        renderContacts();
      }
    }

    function openChat(name) {
      const chat = document.getElementById("chat");
      chat.innerHTML = `<strong>Chat with ${name}</strong><div id="chatMessages" style="margin-top:10px"></div>`;
    }

    function sendMessage() {
      const input = document.getElementById("messageInput");
      const message = input.value.trim();
      if (!message) return;
      const messagesDiv = document.getElementById("chatMessages");
      const msg = document.createElement("div");
      msg.textContent = `You: ${message}`;
      messagesDiv.appendChild(msg);
      input.value = "";
    }

    function addContact() {
      const nameInput = document.getElementById("contactName");
      const phoneInput = document.getElementById("contactPhone");
      const name = nameInput.value.trim();
      const phone = phoneInput.value.trim();
      if (!name || !phone) return alert("Введите имя и номер телефона");
      contacts.push({ name, phone });
      renderContacts();
      nameInput.value = "";
      phoneInput.value = "";
    }

    renderContacts();
  </script>
</body>
</html>
