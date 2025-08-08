#tegramm
uzgramm
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>OpenTegram Chat</title>
  <script type="module">
    // Импорт Firebase SDK
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
    import { getDatabase, ref, push, onChildAdded } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-database.js";

    // Конфигурация Firebase
    const firebaseConfig = {
      apiKey: "AIzaSyAhiffnqKg-ZCvBxucSVXb5UvHiBNVtbZw",
      authDomain: "tegramm-73828.firebaseapp.com",
      databaseURL: "https://tegramm-73828-default-rtdb.firebaseio.com",
      projectId: "tegramm-73828",
      storageBucket: "tegramm-73828.appspot.com",
      messagingSenderId: "841309606863",
      appId: "1:841309606863:web:875c07b87841dd45a0b8ba",
      measurementId: "G-9PCYZR3LK9"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // Получение или генерация уникального ID пользователя
    let myID = localStorage.getItem("myID");
    if (!myID) {
      myID = "user_" + Math.random().toString(36).substring(2, 10);
      localStorage.setItem("myID", myID);
    }
    document.getElementById("my-id").innerText = myID;

    // Переменные
    let currentChatID = null;

    // Поиск по ID
    document.getElementById("searchBtn").onclick = () => {
      currentChatID = document.getElementById("searchInput").value.trim();
      if (!currentChatID) return;
      document.getElementById("chat-with").innerText = "Chat with: " + currentChatID;
      document.getElementById("messages").innerHTML = "";

      const chatPath = getChatPath(myID, currentChatID);
      const chatRef = ref(db, chatPath);

      // Прослушка новых сообщений
      onChildAdded(chatRef, (data) => {
        const msg = data.val();
        const div = document.createElement("div");
        div.textContent = `${msg.sender === myID ? "You" : msg.sender}: ${msg.text}`;
        document.getElementById("messages").appendChild(div);
      });
    };

    // Отправка сообщения
    document.getElementById("sendBtn").onclick = () => {
      const msg = document.getElementById("msgInput").value.trim();
      if (!msg || !currentChatID) return;

      const chatPath = getChatPath(myID, currentChatID);
      const chatRef = ref(db, chatPath);

      push(chatRef, {
        sender: myID,
        text: msg,
        timestamp: Date.now()
      });

      document.getElementById("msgInput").value = "";
    };

    // Генерация уникального пути для пары пользователей
    function getChatPath(id1, id2) {
      return "chats/" + [id1, id2].sort().join("_");
    }
  </script>
</head>
<body>
  <h2>🔐 Your ID: <span id="my-id"></span></h2>
  <input id="searchInput" placeholder="Enter friend's ID" />
  <button id="searchBtn">Start Chat</button>
  <h3 id="chat-with"></h3>

  <div id="messages" style="border:1px solid gray; height:300px; overflow:auto; padding:5px; margin:10px 0;"></div>

  <input id="msgInput" placeholder="Type your message..." />
  <button id="sendBtn">Send</button>
</body>
</html>
