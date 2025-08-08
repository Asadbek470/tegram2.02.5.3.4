# tegram2
TEGRAM
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Open XI CHAT</title>
    <!-- Подключаем Tailwind CSS через CDN для стилей -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Используем шрифт Inter -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1c2128;
            color: #ffffff;
        }
    </style>
</head>
<body class="flex flex-col md:flex-row h-screen w-screen bg-[#1c2128] text-white">

    <!-- Основной контейнер приложения -->
    <div id="app-container" class="flex flex-col md:flex-row h-screen w-screen bg-[#1c2128] text-white">

        <!-- Список чатов (левая панель) -->
        <div id="chat-list-container" class="flex-1 overflow-y-auto bg-[#242931] border-r border-[#2c333a] md:w-1/3 transition-transform duration-300 ease-in-out md:translate-x-0">
            <header class="bg-[#2a313b] p-4 flex items-center justify-between shadow-md">
                <h1 class="text-xl font-bold">Open XI CHAT</h1>
                <button id="new-chat-button" class="bg-[#3e81b6] text-white py-1 px-3 text-sm rounded-full transition-colors duration-200 hover:bg-[#346e9c]">Новый чат</button>
            </header>
            <div class="p-4 text-sm text-gray-400 border-b border-[#2c333a] flex items-center justify-between">
                <div>Ваш ID:
                    <span id="user-id-display" class="font-mono text-white break-words ml-2"></span>
                </div>
                <button id="copy-id-button" class="ml-2 px-2 py-1 bg-[#3e81b6] text-white rounded-md text-xs transition-opacity duration-200 hover:opacity-80">
                    Копировать
                </button>
            </div>
            <div id="chats-list" class="p-4">
                <!-- Здесь будут отображаться чаты -->
                <div class="p-4 text-center text-gray-500">Загрузка чатов...</div>
            </div>
            <footer class="bg-[#2a313b] p-4 flex justify-around items-center shadow-inner">
                <!-- Иконки контактов, чатов и настроек -->
                <button class="flex flex-col items-center text-gray-500 transition-colors duration-200 hover:text-white">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M12 2C6.486 2 2 6.486 2 12s4.486 10 10 10 10-4.486 10-10S17.514 2 12 2zM12 20c-4.411 0-8-3.589-8-8s3.589-8 8-8 8 3.589 8 8-3.589 8-8 8z"/><path d="M13 7h-2v5H7v2h4v4h2v-4h4v-2h-4z"/></svg>
                    <span class="text-xs mt-1">Контакты</span>
                </button>
                <button class="flex flex-col items-center text-blue-500 transition-colors duration-200">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M12 2C6.486 2 2 6.486 2 12s4.486 10 10 10 10-4.486 10-10S17.514 2 12 2zM12 20c-4.411 0-8-3.589-8-8s3.589-8 8-8 8 3.589 8 8-3.589 8-8 8z"/><path d="M9 10h6v4H9z"/></svg>
                    <span class="text-xs mt-1">Чаты</span>
                </button>
                <button class="flex flex-col items-center text-gray-500 transition-colors duration-200 hover:text-white">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M12 2C6.486 2 2 6.486 2 12s4.486 10 10 10 10-4.486 10-10S17.514 2 12 2zM12 20c-4.411 0-8-3.589-8-8s3.589-8 8-8 8 3.589 8 8-3.589 8-8 8z"/><path d="M11 7h2v10h-2zM7 11h10v2H7z"/></svg>
                    <span class="text-xs mt-1">Настройки</span>
                </button>
            </footer>
        </div>

        <!-- Окно чата (правая панель) -->
        <div id="chat-window-container" class="absolute top-0 left-0 w-full h-full bg-[#242931] flex flex-col md:relative md:w-2/3 transition-transform duration-300 ease-in-out translate-x-full md:translate-x-0">
            <header class="bg-[#2a313b] p-4 flex items-center shadow-md">
                <button id="back-to-chats" class="text-blue-500 mr-4 md:hidden transition-transform duration-200 hover:scale-110">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M15.41 16.59L10.83 12l4.58-4.59L14 6l-6 6 6 6z"/></svg>
                </button>
                <div class="flex-1 flex items-center gap-3">
                    <div id="chat-avatar" class="w-10 h-10 bg-blue-500 rounded-full flex items-center justify-center text-lg font-bold"></div>
                    <h2 id="chat-name" class="text-lg font-semibold"></h2>
                </div>
                <div class="flex gap-4">
                    <button class="text-gray-400 transition-transform duration-200 hover:scale-110">
                         <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M17.41 11.59l-4.29-4.29a1 1 0 00-1.41 0L7.41 11.59a1 1 0 000 1.41l4.29 4.29a1 1 0 001.41 0l4.29-4.29a1 1 0 000-1.41z"/></svg>
                    </button>
                </div>
            </header>
            <main id="messages-container" class="flex-1 p-4 overflow-y-auto space-y-3">
                <div id="empty-chat-message" class="text-center text-gray-500">
                    Выберите чат, чтобы начать общение.
                </div>
            </main>
            <footer class="bg-[#2a313b] p-3 flex items-center gap-3">
                <input
                    type="text"
                    id="new-message-input"
                    placeholder="Сообщение..."
                    class="flex-1 bg-[#323841] border border-[#3e4651] rounded-full px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#3e81b6]"
                />
                <button id="send-message-button" class="p-2 bg-[#3e81b6] rounded-full text-white transition-transform duration-200 hover:scale-110">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/></svg>
                </button>
            </footer>
        </div>

        <!-- Модальное окно для создания нового чата -->
        <div id="new-chat-modal" class="fixed inset-0 bg-black bg-opacity-70 flex justify-center items-center z-50 opacity-0 invisible transition-opacity duration-300">
            <div class="bg-[#242931] rounded-2xl p-6 w-11/12 max-w-sm flex flex-col gap-4 shadow-xl">
                <h2 class="text-2xl font-bold mb-4 text-center">Создать новый чат</h2>
                <input type="text" id="new-chat-contact-id" placeholder="Введите ID пользователя" class="bg-[#323841] border border-[#3e4651] rounded-lg px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 mb-2" />
                <p id="new-chat-error-message" class="text-sm text-red-400 text-center hidden"></p>
                <button id="create-chat-button" class="flex items-center gap-4 p-4 rounded-xl bg-[#2a313b] transition-colors duration-200 hover:bg-[#2c333a] focus:outline-none">
                    <div class="w-12 h-12 bg-gray-500 rounded-full flex items-center justify-center text-xl text-white">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24" width="24" height="24"><path d="M12 2C6.486 2 2 6.486 2 12s4.486 10 10 10 10-4.486 10-10S17.514 2 12 2zM12 20c-4.411 0-8-3.589-8-8s3.589-8 8-8 8 3.589 8 8-3.589 8-8 8z"/><path d="M13 7h-2v5H7v2h4v4h2v-4h4v-2h-4z"/></svg>
                    </div>
                    <span class="font-semibold text-lg">Создать чат</span>
                </button>
                <button id="cancel-new-chat" class="mt-4 p-2 text-gray-400 hover:text-white transition-colors duration-200">
                    Отмена
                </button>
            </div>
        </div>

    </div>

    <!-- Модальное окно для отображения сообщений (замены alert) -->
    <div id="message-modal" class="fixed inset-0 bg-black bg-opacity-70 flex justify-center items-center z-50 opacity-0 invisible transition-opacity duration-300">
        <div class="bg-[#242931] rounded-2xl p-6 w-11/12 max-w-sm flex flex-col gap-4 shadow-xl text-center">
            <h2 id="message-modal-title" class="text-xl font-bold"></h2>
            <p id="message-modal-text" class="text-sm text-gray-300"></p>
            <button id="message-modal-ok-button" class="mt-4 p-2 bg-[#3e81b6] text-white rounded-lg transition-colors duration-200 hover:bg-[#346e9c]">ОК</button>
        </div>
    </div>

    <!-- Firebase SDK - должен быть в самом конце тела -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, collection, query, where, onSnapshot, addDoc, serverTimestamp, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Глобальные переменные, предоставленные средой Canvas
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        // Переменные для хранения состояния приложения
        let db;
        let auth;
        let userId = '';
        let currentChat = null;
        let unsubscribeChats = null;
        let unsubscribeMessages = null;

        // DOM элементы
        const chatListContainer = document.getElementById('chat-list-container');
        const chatWindowContainer = document.getElementById('chat-window-container');
        const chatsList = document.getElementById('chats-list');
        const userIdDisplay = document.getElementById('user-id-display');
        const copyIdButton = document.getElementById('copy-id-button');
        const newChatButton = document.getElementById('new-chat-button');
        const backToChatsButton = document.getElementById('back-to-chats');
        const chatAvatar = document.getElementById('chat-avatar');
        const chatName = document.getElementById('chat-name');
        const messagesContainer = document.getElementById('messages-container');
        const emptyChatMessage = document.getElementById('empty-chat-message');
        const newMessageInput = document.getElementById('new-message-input');
        const sendMessageButton = document.getElementById('send-message-button');
        const newChatModal = document.getElementById('new-chat-modal');
        const newChatContactIdInput = document.getElementById('new-chat-contact-id');
        const newChatErrorMessage = document.getElementById('new-chat-error-message');
        const createChatButton = document.getElementById('create-chat-button');
        const cancelNewChatButton = document.getElementById('cancel-new-chat');
        const messageModal = document.getElementById('message-modal');
        const messageModalTitle = document.getElementById('message-modal-title');
        const messageModalText = document.getElementById('message-modal-text');
        const messageModalOkButton = document.getElementById('message-modal-ok-button');

        // Функция для отображения кастомного модального окна
        function showMessageModal(title, text) {
            messageModalTitle.textContent = title;
            messageModalText.textContent = text;
            messageModal.classList.remove('opacity-0', 'invisible');
            messageModal.classList.add('opacity-100', 'visible');
        }

        // Функция для скрытия кастомного модального окна
        function hideMessageModal() {
            messageModal.classList.remove('opacity-100', 'visible');
            messageModal.classList.add('opacity-0', 'invisible');
        }

        // Инициализация Firebase и аутентификация
        async function initFirebase() {
            const app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);

            // Аутентификация
            try {
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                console.error("Ошибка аутентификации:", error);
            }

            onAuthStateChanged(auth, (user) => {
                if (user) {
                    userId = user.uid;
                    userIdDisplay.textContent = userId;
                    fetchChats();
                } else {
                    userIdDisplay.textContent = 'Ошибка. Перезагрузите страницу.';
                }
            });
        }

        // Функция для получения и отображения чатов
        function fetchChats() {
            if (unsubscribeChats) {
                unsubscribeChats(); // Отписываемся от предыдущего слушателя
            }

            const chatsRef = collection(db, `artifacts/${appId}/public/data/chats`);
            const q = query(chatsRef, where('participants', 'array-contains', userId));

            chatsList.innerHTML = `<div class="p-4 text-center text-gray-500">Загрузка чатов...</div>`;

            unsubscribeChats = onSnapshot(q, (snapshot) => {
                const chatsData = snapshot.docs.map(doc => {
                    const data = doc.data();
                    const otherParticipants = data.participants.filter(p => p !== userId);
                    let chatName = data.name;
                    if (data.type === 'contact' && otherParticipants.length > 0) {
                        chatName = otherParticipants[0]; // Отображаем ID другого пользователя
                    }
                    return {
                        id: doc.id,
                        ...data,
                        name: chatName
                    };
                });
                renderChatList(chatsData);
            }, (error) => {
                console.error("Ошибка получения чатов:", error);
                chatsList.innerHTML = `<div class="p-4 text-center text-red-500">Ошибка загрузки чатов.</div>`;
            });
        }

        // Функция для отрисовки списка чатов
        function renderChatList(chats) {
            chatsList.innerHTML = '';
            if (chats.length === 0) {
                chatsList.innerHTML = `<div class="p-4 text-center text-gray-500">Начните, создав новый чат!</div>`;
                return;
            }

            chats.forEach(chat => {
                const chatElement = document.createElement('div');
                chatElement.className = "p-4 border-b border-[#2c333a] flex items-center gap-4 cursor-pointer transition-colors duration-200 hover:bg-[#2c333a]";
                chatElement.onclick = () => openChat(chat);

                let avatarColor = 'bg-blue-500';
                if (chat.type === 'group') avatarColor = 'bg-green-500';
                if (chat.type === 'channel') avatarColor = 'bg-purple-500';

                chatElement.innerHTML = `
                    <div class="w-12 h-12 rounded-full flex-shrink-0 flex items-center justify-center text-xl font-bold ${avatarColor}">
                        ${chat.name.charAt(0).toUpperCase()}
                    </div>
                    <div class="flex-1 min-w-0">
                        <div class="flex justify-between items-center">
                            <span class="font-semibold truncate">${chat.name}</span>
                            <span class="text-xs text-gray-400">
                                ${chat.lastMessageTimestamp ? chat.lastMessageTimestamp.toDate().toLocaleTimeString() : ''}
                            </span>
                        </div>
                        <p class="text-sm text-gray-400 truncate">
                            Последнее сообщение...
                        </p>
                    </div>
                `;
                chatsList.appendChild(chatElement);
            });
        }

        // Функция для открытия чата
        function openChat(chat) {
            currentChat = chat;
            chatName.textContent = chat.name;
            chatAvatar.textContent = chat.name.charAt(0).toUpperCase();

            // Показываем окно чата и скрываем список чатов на мобильных
            chatListContainer.style.transform = 'translateX(-100%)';
            chatWindowContainer.style.transform = 'translateX(0)';

            fetchMessages();
        }

        // Функция для получения и отображения сообщений
        function fetchMessages() {
            if (unsubscribeMessages) {
                unsubscribeMessages(); // Отписываемся от предыдущего слушателя
            }

            messagesContainer.innerHTML = `<div class="p-4 text-center text-gray-500">Загрузка сообщений...</div>`;

            const messagesRef = collection(db, `artifacts/${appId}/public/data/chats/${currentChat.id}/messages`);

            unsubscribeMessages = onSnapshot(messagesRef, (snapshot) => {
                const messagesData = snapshot.docs.map(doc => ({
                    id: doc.id,
                    ...doc.data()
                }));
                // Сортируем сообщения по времени
                messagesData.sort((a, b) => a.timestamp?.seconds - b.timestamp?.seconds);
                renderMessages(messagesData);
            }, (error) => {
                console.error("Ошибка получения сообщений:", error);
                messagesContainer.innerHTML = `<div class="p-4 text-center text-red-500">Ошибка загрузки сообщений.</div>`;
            });
        }

        // Функция для отрисовки сообщений
        function renderMessages(messages) {
            messagesContainer.innerHTML = '';
            if (messages.length === 0) {
                messagesContainer.innerHTML = `<div class="p-4 text-center text-gray-500">Отправьте первое сообщение!</div>`;
                return;
            }

            messages.forEach(msg => {
                const messageElement = document.createElement('div');
                const isSentByMe = msg.senderId === userId;
                messageElement.className = `message-bubble p-3 rounded-lg max-w-[80%] break-words ${isSentByMe ? 'sent bg-[#3e81b6] ml-auto border-b-2 border-r-2 border-blue-600' : 'received bg-[#484d53] mr-auto border-b-2 border-l-2 border-gray-600'}`;
                messageElement.textContent = msg.text;
                messagesContainer.appendChild(messageElement);
            });

            // Прокручиваем вниз, чтобы увидеть последнее сообщение
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        // Функция для отправки сообщения
        async function sendMessage() {
            const messageText = newMessageInput.value.trim();
            if (!db || !currentChat || !messageText) return;

            try {
                await addDoc(collection(db, `artifacts/${appId}/public/data/chats/${currentChat.id}/messages`), {
                    text: messageText,
                    senderId: userId,
                    timestamp: serverTimestamp()
                });
                newMessageInput.value = ''; // Очищаем поле ввода
            } catch (error) {
                console.error("Ошибка отправки сообщения:", error);
            }
        }

        // Функция для создания нового чата
        async function createNewChat() {
            const contactId = newChatContactIdInput.value.trim();
            if (!contactId || contactId === userId) {
                newChatErrorMessage.textContent = 'Введите действительный ID контакта, который отличается от вашего.';
                newChatErrorMessage.classList.remove('hidden');
                return;
            }
            newChatErrorMessage.classList.add('hidden');

            try {
                // Проверяем, существует ли уже такой чат
                const existingChatQuery = query(collection(db, `artifacts/${appId}/public/data/chats`), where('participants', 'in', [[userId, contactId], [contactId, userId]]));
                const existingChatSnapshot = await getDocs(existingChatQuery);

                if (!existingChatSnapshot.empty) {
                    const existingChat = existingChatSnapshot.docs[0];
                    showMessageModal('Ошибка', `Чат с этим пользователем уже существует.`);
                    openChat({ id: existingChat.id, ...existingChat.data(), name: existingChat.data().participants.filter(p => p !== userId)[0] });
                    closeNewChatModal();
                    return;
                }

                await addDoc(collection(db, `artifacts/${appId}/public/data/chats`), {
                    type: 'contact',
                    name: `Чат с ${contactId.substring(0, 5)}...`,
                    participants: [userId, contactId],
                    createdAt: serverTimestamp()
                });
                showMessageModal('Успешно', 'Чат создан!');
                closeNewChatModal();
            } catch (error) {
                console.error("Ошибка создания чата:", error);
                showMessageModal('Ошибка', 'Ошибка при создании чата. Попробуйте еще раз.');
            }
        }

        // Функция для открытия модального окна нового чата
        function openNewChatModal() {
            newChatModal.classList.remove('opacity-0', 'invisible');
            newChatModal.classList.add('opacity-100', 'visible');
        }

        // Функция для закрытия модального окна нового чата
        function closeNewChatModal() {
            newChatModal.classList.remove('opacity-100', 'visible');
            newChatModal.classList.add('opacity-0', 'invisible');
            newChatContactIdInput.value = '';
            newChatErrorMessage.classList.add('hidden');
        }

        // Функция для копирования ID пользователя
        function copyUserId() {
            navigator.clipboard.writeText(userId).then(() => {
                const originalText = copyIdButton.textContent;
                copyIdButton.textContent = 'Скопировано!';
                setTimeout(() => {
                    copyIdButton.textContent = originalText;
                }, 2000);
            }).catch(err => {
                console.error('Не удалось скопировать ID', err);
            });
        }

        // Обработчики событий
        newChatButton.addEventListener('click', openNewChatModal);
        cancelNewChatButton.addEventListener('click', closeNewChatModal);
        createChatButton.addEventListener('click', createNewChat);
        backToChatsButton.addEventListener('click', () => {
            chatListContainer.style.transform = 'translateX(0)';
            chatWindowContainer.style.transform = 'translateX(100%)';
            if (unsubscribeMessages) {
                unsubscribeMessages(); // Отписываемся от слушателя сообщений
            }
        });
        sendMessageButton.addEventListener('click', sendMessage);
        newMessageInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
        copyIdButton.addEventListener('click', copyUserId);
        messageModalOkButton.addEventListener('click', hideMessageModal);

        // Инициализируем приложение
        initFirebase();
    </script>
</body>
</html>
