#tegramm
uzgramm
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AIXI Chat - Family Messenger</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
        }

        :root {
            --primary: #0088cc;
            --primary-dark: #006699;
            --secondary: #25d366;
            --light-bg: #f0f4f8;
            --dark-bg: #0d1418;
            --panel-bg: #ffffff;
            --dark-panel: #1f2c33;
            --text-light: #333333;
            --text-dark: #e9edef;
            --received-bg: #e3f2fd;
            --received-dark: #2a3942;
            --sent-bg: #dcf8c6;
            --sent-dark: #005c4b;
            --input-bg: #f5f5f5;
            --input-dark: #2a3942;
            --border-light: #e0e0e0;
            --border-dark: #374248;
            --sidebar-width: 300px;
            --mom-color: #e91e63;
            --dad-color: #2196f3;
            --grandma-color: #9c27b0;
        }

        body {
            background: linear-gradient(135deg, #1e5799, #207cca, #2989d8, #1e5799);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            transition: background 0.3s ease;
            overflow: hidden;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body.dark-mode {
            background: linear-gradient(135deg, #0c1e2e, #142837, #1c3244, #0c1e2e);
        }

        .app-container {
            display: flex;
            width: 100%;
            max-width: 1000px;
            height: 90vh;
            background: var(--panel-bg);
            border-radius: 20px;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.25);
            overflow: hidden;
            transition: all 0.3s ease;
        }

        .dark-mode .app-container {
            background: var(--dark-panel);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
        }

        /* Sidebar Styles */
        .sidebar {
            width: var(--sidebar-width);
            background: var(--panel-bg);
            border-right: 1px solid var(--border-light);
            display: flex;
            flex-direction: column;
            transition: all 0.3s ease;
            z-index: 10;
        }

        .dark-mode .sidebar {
            background: var(--dark-panel);
            border-right: 1px solid var(--border-dark);
        }

        .sidebar-header {
            padding: 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: var(--primary);
            color: white;
            position: relative;
        }

        .dark-mode .sidebar-header {
            background: var(--primary-dark);
        }

        .user-info {
            display: flex;
            align-items: center;
            flex: 1;
        }

        .user-avatar {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: linear-gradient(45deg, #ff9a9e, #fad0c4);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            font-size: 20px;
            margin-right: 15px;
            border: 2px solid white;
            animation: pulse 2s infinite;
        }

        .user-details {
            display: flex;
            flex-direction: column;
            flex: 1;
        }

        .user-name {
            font-size: 1.1rem;
            font-weight: 600;
        }

        .user-id-display {
            font-size: 0.75rem;
            margin-top: 3px;
            display: flex;
            align-items: center;
            background: rgba(255, 255, 255, 0.2);
            padding: 2px 8px;
            border-radius: 10px;
            max-width: 150px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            cursor: pointer;
            transition: all 0.3s;
        }

        .user-id-display:hover {
            background: rgba(255, 255, 255, 0.3);
        }

        .user-id-display i {
            margin-right: 5px;
            font-size: 0.7rem;
        }

        .user-status {
            font-size: 0.8rem;
            opacity: 0.9;
            margin-top: 3px;
            display: flex;
            align-items: center;
        }

        .status-indicator {
            width: 8px;
            height: 8px;
            background: var(--secondary);
            border-radius: 50%;
            margin-right: 5px;
        }

        .sidebar-icons {
            display: flex;
            gap: 15px;
        }

        .sidebar-icon {
            font-size: 1.2rem;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .sidebar-icon:hover {
            transform: scale(1.1);
        }

        .search-container {
            padding: 15px;
            position: relative;
        }

        .search-input {
            width: 100%;
            padding: 10px 15px 10px 40px;
            border-radius: 20px;
            border: none;
            background: var(--input-bg);
            font-size: 0.9rem;
            outline: none;
            transition: all 0.3s ease;
        }

        .dark-mode .search-input {
            background: var(--input-dark);
            color: var(--text-dark);
        }

        .search-icon {
            position: absolute;
            left: 30px;
            top: 50%;
            transform: translateY(-50%);
            color: #777;
        }

        .contacts-header {
            padding: 15px 20px;
            font-size: 0.9rem;
            color: #777;
            font-weight: 500;
            border-bottom: 1px solid var(--border-light);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .dark-mode .contacts-header {
            border-bottom: 1px solid var(--border-dark);
            color: #aaa;
        }

        .family-badge {
            background: var(--primary);
            color: white;
            padding: 3px 8px;
            border-radius: 10px;
            font-size: 0.7rem;
        }

        .contacts-list {
            flex: 1;
            overflow-y: auto;
        }

        .contact {
            display: flex;
            padding: 12px 20px;
            cursor: pointer;
            transition: background 0.2s;
            border-bottom: 1px solid var(--border-light);
            position: relative;
        }

        .dark-mode .contact {
            border-bottom: 1px solid var(--border-dark);
        }

        .contact:hover {
            background: rgba(0, 0, 0, 0.05);
        }

        .dark-mode .contact:hover {
            background: rgba(255, 255, 255, 0.05);
        }

        .contact.active {
            background: rgba(0, 136, 204, 0.1);
        }

        .dark-mode .contact.active {
            background: rgba(0, 102, 153, 0.2);
        }

        .contact-avatar {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            margin-right: 15px;
            font-size: 18px;
        }

        .contact-info {
            flex: 1;
        }

        .contact-name {
            font-weight: 500;
            margin-bottom: 3px;
            display: flex;
            align-items: center;
        }

        .family-tag {
            font-size: 0.7rem;
            padding: 2px 8px;
            border-radius: 10px;
            margin-left: 8px;
            color: white;
        }

        .mom-tag {
            background: var(--mom-color);
        }

        .dad-tag {
            background: var(--dad-color);
        }

        .grandma-tag {
            background: var(--grandma-color);
        }

        .last-message {
            font-size: 0.85rem;
            color: #777;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 180px;
        }

        .dark-mode .last-message {
            color: #aaa;
        }

        .contact-meta {
            display: flex;
            flex-direction: column;
            align-items: flex-end;
        }

        .message-time {
            font-size: 0.75rem;
            color: #777;
            margin-bottom: 5px;
        }

        .dark-mode .message-time {
            color: #aaa;
        }

        .unread-count {
            background: var(--primary);
            color: white;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7rem;
        }

        .dark-mode .unread-count {
            background: var(--primary-dark);
        }

        /* Chat Container Styles */
        .chat-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .chat-header {
            background: var(--panel-bg);
            color: var(--text-light);
            padding: 15px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid var(--border-light);
            z-index: 5;
        }

        .dark-mode .chat-header {
            background: var(--dark-panel);
            color: var(--text-dark);
            border-bottom: 1px solid var(--border-dark);
        }

        .chat-header-info {
            display: flex;
            align-items: center;
        }

        .chat-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            margin-right: 12px;
            font-size: 16px;
        }

        .chat-title {
            font-weight: 500;
        }

        .chat-id {
            font-size: 0.8rem;
            color: #777;
            margin-top: 2px;
        }

        .dark-mode .chat-id {
            color: #aaa;
        }

        .chat-header-icons {
            display: flex;
            gap: 15px;
        }

        .chat-icon {
            font-size: 1.1rem;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .chat-icon:hover {
            transform: scale(1.1);
        }

        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            background: var(--light-bg);
            display: flex;
            flex-direction: column;
            transition: background 0.3s ease;
        }

        .dark-mode .chat-messages {
            background: var(--dark-bg);
        }

        .message {
            margin-bottom: 15px;
            max-width: 80%;
            clear: both;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 0.3s forwards;
        }

        .message:nth-child(2n) {
            animation-delay: 0.1s;
        }

        .message:nth-child(3n) {
            animation-delay: 0.2s;
        }

        .received {
            align-self: flex-start;
            background: var(--received-bg);
            border-radius: 0 15px 15px 15px;
        }

        .dark-mode .received {
            background: var(--received-dark);
        }

        .sent {
            align-self: flex-end;
            background: var(--sent-bg);
            border-radius: 15px 0 15px 15px;
        }

        .dark-mode .sent {
            background: var(--sent-dark);
        }

        .message-content {
            padding: 12px 15px;
            color: var(--text-light);
            line-height: 1.4;
        }

        .dark-mode .message-content {
            color: var(--text-dark);
        }

        .message-time {
            font-size: 0.7rem;
            color: #888;
            text-align: right;
            margin-top: 3px;
            padding: 0 10px 5px;
        }

        .dark-mode .message-time {
            color: #aaa;
        }

        .chat-input-container {
            padding: 15px;
            background: var(--input-bg);
            border-top: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            transition: all 0.3s ease;
        }

        .dark-mode .chat-input-container {
            background: var(--input-dark);
            border-top: 1px solid var(--border-dark);
        }

        .input-group {
            display: flex;
            flex: 1;
            background: var(--panel-bg);
            border-radius: 30px;
            overflow: hidden;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .dark-mode .input-group {
            background: var(--dark-panel);
        }

        #message-input {
            flex: 1;
            padding: 12px 15px;
            border: none;
            outline: none;
            font-size: 1rem;
            background: transparent;
            color: var(--text-light);
        }

        .dark-mode #message-input {
            color: var(--text-dark);
        }

        #message-input::placeholder {
            color: #999;
        }

        .dark-mode #message-input::placeholder {
            color: #7a8c96;
        }

        #send-button {
            background: var(--primary);
            color: white;
            border: none;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            margin-left: 10px;
            cursor: pointer;
            font-size: 1.2rem;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        #send-button:hover {
            background: var(--primary-dark);
            transform: scale(1.05);
        }

        .action-button {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: transparent;
            border: none;
            color: var(--primary);
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 5px;
            transition: all 0.3s;
        }

        .dark-mode .action-button {
            color: var(--primary-dark);
        }

        .action-button:hover {
            background: rgba(0, 136, 204, 0.1);
            transform: scale(1.1);
        }

        .dark-mode .action-button:hover {
            background: rgba(0, 102, 153, 0.2);
        }

        .language-selector {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 20px;
            padding: 8px 15px;
            display: flex;
            gap: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            z-index: 100;
        }

        .dark-mode .language-selector {
            background: rgba(30, 30, 30, 0.9);
        }

        .lang-btn {
            background: none;
            border: none;
            font-size: 0.9rem;
            padding: 5px 10px;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .lang-btn.active {
            background: var(--primary);
            color: white;
        }

        .dark-mode .lang-btn.active {
            background: var(--primary-dark);
        }

        .modal-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .modal-container.active {
            opacity: 1;
            pointer-events: all;
        }

        .modal-box {
            background: var(--panel-bg);
            border-radius: 20px;
            padding: 30px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 15px 50px rgba(0, 0, 0, 0.3);
            transform: translateY(20px);
            opacity: 0;
            transition: all 0.4s ease;
        }

        .dark-mode .modal-box {
            background: var(--dark-panel);
        }

        .modal-container.active .modal-box {
            transform: translateY(0);
            opacity: 1;
        }

        .modal-title {
            font-size: 1.4rem;
            margin-bottom: 20px;
            text-align: center;
            color: var(--text-light);
        }

        .dark-mode .modal-title {
            color: var(--text-dark);
        }

        .input-group-modal {
            margin-bottom: 25px;
        }

        .input-label {
            display: block;
            margin-bottom: 8px;
            font-size: 0.9rem;
            color: #666;
        }

        .dark-mode .input-label {
            color: #aaa;
        }

        .modal-input {
            width: 100%;
            padding: 12px 15px;
            border-radius: 10px;
            border: 1px solid #ddd;
            font-size: 1rem;
            background: var(--input-bg);
            color: var(--text-light);
        }

        .dark-mode .modal-input {
            background: var(--input-dark);
            border-color: var(--border-dark);
            color: var(--text-dark);
        }

        .modal-input:focus {
            outline: none;
            border-color: var(--primary);
        }

        .modal-buttons {
            display: flex;
            gap: 10px;
        }

        .modal-btn {
            flex: 1;
            padding: 12px;
            border-radius: 10px;
            border: none;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s;
        }

        .modal-submit {
            background: var(--primary);
            color: white;
        }

        .modal-submit:hover {
            background: var(--primary-dark);
        }

        .modal-cancel {
            background: #f0f0f0;
            color: #333;
        }

        .dark-mode .modal-cancel {
            background: #2a3942;
            color: #eee;
        }

        .modal-cancel:hover {
            background: #e0e0e0;
        }

        .dark-mode .modal-cancel:hover {
            background: #374248;
        }

        /* Family Theme */
        .mom-bg {
            background: linear-gradient(45deg, #ff9a9e, #fad0c4);
        }
        
        .dad-bg {
            background: linear-gradient(45deg, #a1c4fd, #c2e9fb);
        }
        
        .grandma-bg {
            background: linear-gradient(45deg, #e0c3fc, #8ec5fc);
        }

        /* Animations */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes pulse {
            0% {
                box-shadow: 0 0 0 0 rgba(0, 136, 204, 0.4);
            }
            70% {
                box-shadow: 0 0 0 10px rgba(0, 136, 204, 0);
            }
            100% {
                box-shadow: 0 0 0 0 rgba(0, 136, 204, 0);
            }
        }

        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .slide-in {
            animation: slideIn 0.4s ease-out;
        }

        @keyframes slideIn {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        /* Responsive */
        @media (max-width: 768px) {
            .app-container {
                height: 95vh;
                border-radius: 15px;
            }
            
            .sidebar {
                width: 70px;
            }
            
            .sidebar-header, .search-container, .contacts-header, .contact-info, .contact-meta {
                display: none;
            }
            
            .contact {
                justify-content: center;
                padding: 15px 10px;
            }
            
            .contact-avatar {
                margin-right: 0;
            }
        }
    </style>
</head>
<body>
    <div class="language-selector">
        <button class="lang-btn active" id="lang-en">EN</button>
        <button class="lang-btn" id="lang-ru">RU</button>
    </div>
    
    <!-- Modal for changing user ID -->
    <div class="modal-container" id="id-change-modal">
        <div class="modal-box">
            <h2 class="modal-title" id="change-id-title">Change Your ID</h2>
            <div class="input-group-modal">
                <label class="input-label">Enter New ID</label>
                <input type="text" class="modal-input" id="new-id-input" placeholder="AXI-XXXX-XXXX">
            </div>
            <div class="modal-buttons">
                <button class="modal-btn modal-cancel" id="id-change-cancel">Cancel</button>
                <button class="modal-btn modal-submit" id="id-change-submit">Update ID</button>
            </div>
        </div>
    </div>
    
    <!-- Modal for adding contacts -->
    <div class="modal-container" id="add-contact-modal">
        <div class="modal-box">
            <h2 class="modal-title" id="add-contact-title">Add Family Member</h2>
            <div class="input-group-modal">
                <label class="input-label">Enter Family Member's ID</label>
                <input type="text" class="modal-input" id="family-id-input" placeholder="AXI-XXXX-XXXX">
            </div>
            <div class="input-group-modal">
                <label class="input-label">Select Relationship</label>
                <select class="modal-input" id="relationship-select">
                    <option value="mom">Mom</option>
                    <option value="dad">Dad</option>
                    <option value="grandma">Grandma</option>
                    <option value="other">Other Family</option>
                </select>
            </div>
            <div class="modal-buttons">
                <button class="modal-btn modal-cancel" id="add-contact-cancel">Cancel</button>
                <button class="modal-btn modal-submit" id="add-contact-submit">Add Member</button>
            </div>
        </div>
    </div>
    
    <div class="app-container">
        <!-- Sidebar with contacts -->
        <div class="sidebar">
            <div class="sidebar-header">
                <div class="user-info">
                    <div class="user-avatar">AX</div>
                    <div class="user-details">
                        <div class="user-name">Alex Ivanov</div>
                        <div class="user-id-display" id="user-id-display">
                            <i class="fas fa-id-card"></i> <span id="current-user-id">AXI-7382-95KF</span>
                        </div>
                    </div>
                </div>
                <div class="sidebar-icons">
                    <div class="sidebar-icon" id="add-contact-btn">
                        <i class="fas fa-user-plus"></i>
                    </div>
                    <div class="sidebar-icon" id="sidebar-theme-toggle">
                        <i class="fas fa-moon"></i>
                    </div>
                </div>
            </div>
            
            <div class="search-container">
                <i class="fas fa-search search-icon"></i>
                <input type="text" class="search-input" placeholder="Search contacts..." id="contact-search">
            </div>
            
            <div class="contacts-header">
                <span id="contacts-header">Family Members</span>
                <span class="family-badge">Family</span>
            </div>
            
            <div class="contacts-list" id="contacts-list">
                <!-- Contacts will be populated dynamically -->
            </div>
        </div>
        
        <!-- Chat Container -->
        <div class="chat-container">
            <div class="chat-header">
                <div class="chat-header-info">
                    <div class="chat-avatar" id="chat-avatar">M</div>
                    <div>
                        <div class="chat-title" id="chat-title">Mom</div>
                        <div class="chat-id" id="chat-id">ID: MOM-5832</div>
                    </div>
                </div>
                <div class="chat-header-icons">
                    <div class="chat-icon">
                        <i class="fas fa-phone-alt"></i>
                    </div>
                    <div class="chat-icon">
                        <i class="fas fa-video"></i>
                    </div>
                    <div class="chat-icon">
                        <i class="fas fa-info-circle"></i>
                    </div>
                </div>
            </div>
            
            <div class="chat-messages" id="chat-messages">
                <!-- Messages will be added here dynamically -->
            </div>
            
            <div class="chat-input-container">
                <button class="action-button">
                    <i class="fas fa-paperclip"></i>
                </button>
                <div class="input-group">
                    <input type="text" id="message-input" placeholder="Type a message...">
                </div>
                <button class="action-button">
                    <i class="fas fa-microphone"></i>
                </button>
                <button id="send-button">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
        </div>
    </div>

    <script>
        // Sample family contacts data
        const familyContacts = [
            { id: 'MOM-5832', name: 'Mom', relationship: 'mom', avatarColor: 'linear-gradient(45deg, #ff9a9e, #fad0c4)', avatarText: 'M', lastMessage: "Don't forget to buy milk", time: "10:30 AM", unread: 0 },
            { id: 'DAD-9274', name: 'Dad', relationship: 'dad', avatarColor: 'linear-gradient(45deg, #4facfe, #00f2fe)', avatarText: 'D', lastMessage: "How's your project going?", time: "Yesterday", unread: 2 },
            { id: 'GRM-1628', name: 'Grandma', relationship: 'grandma', avatarColor: 'linear-gradient(45deg, #d4fc79, #96e6a1)', avatarText: 'G', lastMessage: "Call me when you're free", time: "Last week", unread: 0 }
        ];

        // Language translations
        const translations = {
            en: {
                online: "online",
                contacts: "Family Members",
                typeMessage: "Type a message...",
                connectTitle: "Add Family Member",
                idLabel: "Enter Family Member's ID",
                cancel: "Cancel",
                connect: "Add",
                idPlaceholder: "AXI-XXXX-XXXX",
                mom: "Mom",
                dad: "Dad",
                grandma: "Grandma",
                welcome: "Welcome to AIXI Family Chat!",
                connectInstruction: "Chat with your family members using their unique IDs",
                startChat: "Start chatting with ",
                sentMessage1: "Hello! How are you?",
                receivedMessage1: "I'm doing great! How about you?",
                sentMessage2: "Can we meet this weekend?",
                receivedMessage2: "Sure, let's plan something!",
                sentMessage3: "Don't forget to buy milk",
                receivedMessage3: "Okay, I'll get it on my way home",
                addContact: "Add Family Member",
                searchPlaceholder: "Search contacts...",
                noContacts: "No family members added yet. Add someone to start chatting!",
                changeIdTitle: "Change Your ID",
                newIdPlaceholder: "Enter new ID",
                updateId: "Update ID",
                idChanged: "Your ID has been updated!",
                momTag: "Mom",
                dadTag: "Dad",
                grandmaTag: "Grandma",
                otherTag: "Family"
            },
            ru: {
                online: "в сети",
                contacts: "Члены семьи",
                typeMessage: "Введите сообщение...",
                connectTitle: "Добавить члена семьи",
                idLabel: "Введите ID члена семьи",
                cancel: "Отмена",
                connect: "Добавить",
                idPlaceholder: "AXI-XXXX-XXXX",
                mom: "Мама",
                dad: "Папа",
                grandma: "Бабушка",
                welcome: "Добро пожаловать в AIXI Family Chat!",
                connectInstruction: "Общайтесь с членами вашей семьи, используя их уникальные ID",
                startChat: "Начать чат с ",
                sentMessage1: "Привет! Как дела?",
                receivedMessage1: "Всё отлично! Как ты?",
                sentMessage2: "Можем встретиться на выходных?",
                receivedMessage2: "Конечно, давай что-нибудь запланируем!",
                sentMessage3: "Не забудь купить молоко",
                receivedMessage3: "Хорошо, куплю по дороге домой",
                addContact: "Добавить члена семьи",
                searchPlaceholder: "Поиск контактов...",
                noContacts: "Пока нет членов семьи. Добавьте кого-нибудь, чтобы начать общение!",
                changeIdTitle: "Изменить ваш ID",
                newIdPlaceholder: "Введите новый ID",
                updateId: "Обновить ID",
                idChanged: "Ваш ID был успешно изменён!",
                momTag: "Мама",
                dadTag: "Папа",
                grandmaTag: "Бабушка",
                otherTag: "Семья"
            }
        };

        // Current language
        let currentLang = 'en';
        let currentContact = familyContacts[0];
        let userId = "AXI-7382-95KF";
        
        // DOM Elements
        const chatMessages = document.getElementById('chat-messages');
        const messageInput = document.getElementById('message-input');
        const sendButton = document.getElementById('send-button');
        const themeToggle = document.getElementById('sidebar-theme-toggle');
        const addContactBtn = document.getElementById('add-contact-btn');
        const langEnBtn = document.getElementById('lang-en');
        const langRuBtn = document.getElementById('lang-ru');
        const statusText = document.getElementById('status-text');
        const contactsList = document.getElementById('contacts-list');
        const body = document.body;
        const addContactModal = document.getElementById('add-contact-modal');
        const idChangeModal = document.getElementById('id-change-modal');
        const chatTitle = document.getElementById('chat-title');
        const chatId = document.getElementById('chat-id');
        const chatAvatar = document.getElementById('chat-avatar');
        const contactsHeader = document.getElementById('contacts-header');
        const contactSearch = document.getElementById('contact-search');
        const userIdDisplay = document.getElementById('current-user-id');
        const newIdInput = document.getElementById('new-id-input');
        const idChangeSubmit = document.getElementById('id-change-submit');
        const idChangeCancel = document.getElementById('id-change-cancel');
        const familyIdInput = document.getElementById('family-id-input');
        const relationshipSelect = document.getElementById('relationship-select');
        const addContactSubmit = document.getElementById('add-contact-submit');
        const addContactCancel = document.getElementById('add-contact-cancel');
        
        // Initialize the app
        function initApp() {
            userIdDisplay.textContent = userId;
            renderContacts();
            updateLanguage();
            loadChatHistory(currentContact);
            scrollToBottom();
        }
        
        // Render contacts list
        function renderContacts() {
            contactsList.innerHTML = '';
            
            if (familyContacts.length === 0) {
                contactsList.innerHTML = `<div class="no-contacts">${translations[currentLang].noContacts}</div>`;
                return;
            }
            
            familyContacts.forEach(contact => {
                const contactElement = document.createElement('div');
                contactElement.classList.add('contact');
                if (contact.id === currentContact.id) {
                    contactElement.classList.add('active');
                }
                
                // Get relationship tag
                let tagClass, tagText;
                switch(contact.relationship) {
                    case 'mom':
                        tagClass = 'mom-tag';
                        tagText = translations[currentLang].momTag;
                        break;
                    case 'dad':
                        tagClass = 'dad-tag';
                        tagText = translations[currentLang].dadTag;
                        break;
                    case 'grandma':
                        tagClass = 'grandma-tag';
                        tagText = translations[currentLang].grandmaTag;
                        break;
                    default:
                        tagClass = 'other-tag';
                        tagText = translations[currentLang].otherTag;
                }
                
                contactElement.innerHTML = `
                    <div class="contact-avatar" style="background: ${contact.avatarColor}">${contact.avatarText}</div>
                    <div class="contact-info">
                        <div class="contact-name">
                            ${contact.name}
                            <span class="family-tag ${tagClass}">${tagText}</span>
                        </div>
                        <div class="last-message">${contact.lastMessage}</div>
                    </div>
                    <div class="contact-meta">
                        <div class="message-time">${contact.time}</div>
                        ${contact.unread > 0 ? `<div class="unread-count">${contact.unread}</div>` : ''}
                    </div>
                `;
                
                contactElement.addEventListener('click', () => {
                    currentContact = contact;
                    loadChatHistory(contact);
                    updateActiveContact();
                });
                
                contactsList.appendChild(contactElement);
            });
        }
        
        // Update active contact in the list
        function updateActiveContact() {
            document.querySelectorAll('.contact').forEach(contact => {
                contact.classList.remove('active');
            });
            
            document.querySelectorAll('.contact').forEach(contact => {
                const nameElement = contact.querySelector('.contact-name');
                if (nameElement && nameElement.textContent.includes(currentContact.name)) {
                    contact.classList.add('active');
                }
            });
            
            // Update chat header
            chatTitle.textContent = currentContact.name;
            chatId.textContent = `ID: ${currentContact.id}`;
            chatAvatar.textContent = currentContact.avatarText;
            chatAvatar.style.background = currentContact.avatarColor;
        }
        
        // Load chat history for a contact
        function loadChatHistory(contact) {
            chatMessages.innerHTML = '';
            
            // Welcome message
            addMessage('received', translations[currentLang].welcome);
            addMessage('received', translations[currentLang].connectInstruction);
            
            // Start chat message
            addMessage('received', translations[currentLang].startChat + contact.name);
            
            // Sample conversation
            setTimeout(() => {
                addMessage('sent', translations[currentLang].sentMessage1);
            }, 500);
            
            setTimeout(() => {
                addMessage('received', translations[currentLang].receivedMessage1);
            }, 1200);
            
            setTimeout(() => {
                addMessage('sent', translations[currentLang].sentMessage2);
            }, 2000);
            
            setTimeout(() => {
                addMessage('received', translations[currentLang].receivedMessage2);
            }, 3000);
            
            if (contact.relationship === 'mom') {
                setTimeout(() => {
                    addMessage('sent', translations[currentLang].sentMessage3);
                }, 4000);
                
                setTimeout(() => {
                    addMessage('received', translations[currentLang].receivedMessage3);
                }, 5000);
            }
            
            // Update chat header
            chatTitle.textContent = contact.name;
            chatId.textContent = `ID: ${contact.id}`;
            chatAvatar.textContent = contact.avatarText;
            chatAvatar.style.background = contact.avatarColor;
        }
        
        // Add message to chat
        function addMessage(type, text) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('message', type);
            
            const now = new Date();
            const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
            
            messageElement.innerHTML = `
                <div class="message-content">${text}</div>
                <div class="message-time">${time}</div>
            `;
            
            chatMessages.appendChild(messageElement);
            scrollToBottom();
        }
        
        // Auto-scroll to bottom of chat
        function scrollToBottom() {
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }
        
        // Send message function
        function sendMessage() {
            const message = messageInput.value.trim();
            if (message) {
                addMessage('sent', message);
                messageInput.value = '';
                
                // Simulate reply after 1-2 seconds
                setTimeout(() => {
                    const replies = currentLang === 'en' ? [
                        "Thanks for your message!",
                        "How are you doing today?",
                        "That's interesting!",
                        "Nice to chat with you!",
                        "How can I help you today?",
                        "Let me think about that..."
                    ] : [
                        "Спасибо за ваше сообщение!",
                        "Как у вас дела сегодня?",
                        "Это интересно!",
                        "Приятно с вами общаться!",
                        "Чем я могу вам помочь?",
                        "Дай мне подумать об этом..."
                    ];
                    
                    const reply = replies[Math.floor(Math.random() * replies.length)];
                    addMessage('received', reply);
                }, 1000 + Math.random() * 1000);
            }
        }
        
        // Theme toggle
        themeToggle.addEventListener('click', () => {
            body.classList.toggle('dark-mode');
            const icon = themeToggle.querySelector('i');
            if (body.classList.contains('dark-mode')) {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
        });
        
        // Language switching
        langEnBtn.addEventListener('click', () => {
            if (currentLang !== 'en') {
                currentLang = 'en';
                updateLanguage();
            }
        });
        
        langRuBtn.addEventListener('click', () => {
            if (currentLang !== 'ru') {
                currentLang = 'ru';
                updateLanguage();
            }
        });
        
        // Update UI to current language
        function updateLanguage() {
            const t = translations[currentLang];
            
            // Update buttons
            langEnBtn.classList.toggle('active', currentLang === 'en');
            langRuBtn.classList.toggle('active', currentLang === 'ru');
            
            // Update text elements
            statusText.textContent = t.online;
            messageInput.placeholder = t.typeMessage;
            contactsHeader.textContent = t.contacts;
            contactSearch.placeholder = t.searchPlaceholder;
            document.getElementById('change-id-title').textContent = t.changeIdTitle;
            document.getElementById('add-contact-title').textContent = t.addContact;
            newIdInput.placeholder = t.newIdPlaceholder;
            idChangeSubmit.textContent = t.updateId;
            familyIdInput.placeholder = t.idPlaceholder;
            addContactSubmit.textContent = t.connect;
            
            // Update contact names
            familyContacts[0].name = t.mom;
            familyContacts[1].name = t.dad;
            familyContacts[2].name = t.grandma;
            
            // Re-render contacts
            renderContacts();
            updateActiveContact();
        }
        
        // Show add contact modal
        addContactBtn.addEventListener('click', () => {
            addContactModal.classList.add('active');
        });
        
        // Hide add contact modal
        addContactCancel.addEventListener('click', () => {
            addContactModal.classList.remove('active');
            familyIdInput.value = '';
        });
        
        // Submit new family member
        addContactSubmit.addEventListener('click', () => {
            const familyId = familyIdInput.value.trim();
            const relationship = relationshipSelect.value;
            
            if (familyId) {
                // Get relationship name
                let name;
                switch(relationship) {
                    case 'mom':
                        name = translations[currentLang].mom;
                        break;
                    case 'dad':
                        name = translations[currentLang].dad;
                        break;
                    case 'grandma':
                        name = translations[currentLang].grandma;
                        break;
                    default:
                        name = `Family-${Math.floor(Math.random() * 1000)}`;
                }
                
                // Create gradient based on relationship
                let gradient;
                switch(relationship) {
                    case 'mom':
                        gradient = 'linear-gradient(45deg, #ff9a9e, #fad0c4)';
                        break;
                    case 'dad':
                        gradient = 'linear-gradient(45deg, #4facfe, #00f2fe)';
                        break;
                    case 'grandma':
                        gradient = 'linear-gradient(45deg, #d4fc79, #96e6a1)';
                        break;
                    default:
                        gradient = `linear-gradient(45deg, #${Math.floor(Math.random()*16777215).toString(16)}, #${Math.floor(Math.random()*16777215).toString(16)})`;
                }
                
                const newContact = {
                    id: familyId,
                    name: name,
                    relationship: relationship,
                    avatarColor: gradient,
                    avatarText: name.charAt(0),
                    lastMessage: "New family member added",
                    time: "Just now",
                    unread: 1
                };
                
                familyContacts.push(newContact);
                renderContacts();
                addContactModal.classList.remove('active');
                familyIdInput.value = '';
                
                // Show success message
                addMessage('received', `${translations[currentLang].startChat} ${newContact.name}`);
            }
        });
        
        // Show change ID modal when clicking on user ID
        document.getElementById('user-id-display').addEventListener('click', () => {
            idChangeModal.classList.add('active');
            newIdInput.value = userId;
        });
        
        // Hide change ID modal
        idChangeCancel.addEventListener('click', () => {
            idChangeModal.classList.remove('active');
        });
        
        // Update user ID
        idChangeSubmit.addEventListener('click', () => {
            const newId = newIdInput.value.trim();
            if (newId) {
                userId = newId;
                userIdDisplay.textContent = userId;
                idChangeModal.classList.remove('active');
                
                // Show success message
                const messageElement = document.createElement('div');
                messageElement.classList.add('message', 'received');
                messageElement.innerHTML = `
                    <div class="message-content">${translations[currentLang].idChanged}</div>
                    <div class="message-time">${getCurrentTime()}</div>
                `;
                chatMessages.appendChild(messageElement);
                scrollToBottom();
            }
        });
        
        // Event listeners
        sendButton.addEventListener('click', sendMessage);
        
        messageInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
        
        // Close modals when clicking outside
        document.querySelectorAll('.modal-container').forEach(modal => {
            modal.addEventListener('click', (e) => {
                if (e.target === modal) {
                    modal.classList.remove('active');
                }
            });
        });
        
        // Get current time
        function getCurrentTime() {
            const now = new Date();
            return `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        }
        
        // Initialize the app
        window.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
