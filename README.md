# Velandev-BOOTSTRAP-Assignment
# Chat App Web UI

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Chat App UI</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        :root {
            --sidebar-width: 300px;
            --primary: #4f8cff;
            --bg: #f5f6fa;
            --bubble-user: #e1f0ff;
            --bubble-me: #4f8cff;
            --bubble-me-text: #fff;
            --border: #e0e0e0;
        }
        * { box-sizing: border-box; }
        body {
            margin: 0;
            font-family: 'Segoe UI', Arial, sans-serif;
            background: var(--bg);
            height: 100vh;
            display: flex;
            flex-direction: column;
        }
        .container {
            display: flex;
            height: 100vh;
            width: 100vw;
        }
        .sidebar {
            width: var(--sidebar-width);
            background: #fff;
            border-right: 1px solid var(--border);
            display: flex;
            flex-direction: column;
            padding: 0;
            transition: width 0.2s;
        }
        .sidebar-header {
            padding: 1rem;
            font-weight: bold;
            font-size: 1.2rem;
            border-bottom: 1px solid var(--border);
            background: var(--bg);
        }
        .search-box {
            padding: 0.75rem 1rem;
            border-bottom: 1px solid var(--border);
        }
        .search-box input {
            width: 100%;
            padding: 0.5rem;
            border-radius: 20px;
            border: 1px solid var(--border);
            outline: none;
            font-size: 1rem;
        }
        .contacts {
            flex: 1;
            overflow-y: auto;
            padding: 0;
            margin: 0;
            list-style: none;
        }
        .contact {
            display: flex;
            align-items: center;
            padding: 0.75rem 1rem;
            cursor: pointer;
            border-bottom: 1px solid var(--border);
            transition: background 0.15s;
        }
        .contact:hover {
            background: #f0f4fa;
        }
        .avatar {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            background: #dbeafe;
            margin-right: 0.75rem;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #4f8cff;
            font-size: 1.1rem;
        }
        .status-dot {
            position: absolute;
            bottom: 4px;
            right: 4px;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            border: 2px solid #fff;
            background: #44e37b;
        }
        .contact-info {
            flex: 1;
            min-width: 0;
        }
        .contact-name {
            font-weight: 500;
            font-size: 1rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .contact-last {
            font-size: 0.9rem;
            color: #888;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .chat-area {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: var(--bg);
            min-width: 0;
        }
        .chat-header {
            padding: 1rem;
            border-bottom: 1px solid var(--border);
            background: #fff;
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        .chat-header .avatar {
            margin-right: 0.5rem;
        }
        .chat-header .contact-name {
            font-size: 1.1rem;
            font-weight: bold;
        }
        .chat-thread {
            flex: 1;
            overflow-y: auto;
            padding: 1.5rem 1rem 1rem 1rem;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }
        .message-row {
            display: flex;
            align-items: flex-end;
            gap: 0.5rem;
        }
        .message-row.me {
            flex-direction: row-reverse;
        }
        .message-bubble {
            max-width: 65%;
            padding: 0.75rem 1rem;
            border-radius: 18px;
            font-size: 1rem;
            background: var(--bubble-user);
            color: #222;
            position: relative;
            word-break: break-word;
        }
        .message-row.me .message-bubble {
            background: var(--bubble-me);
            color: var(--bubble-me-text);
            border-bottom-right-radius: 6px;
        }
        .message-row:not(.me) .message-bubble {
            border-bottom-left-radius: 6px;
        }
        .message-time {
            font-size: 0.8rem;
            color: #aaa;
            margin: 0 0.5rem;
            align-self: flex-end;
        }
        .message-input-section {
            display: flex;
            align-items: center;
            padding: 1rem;
            background: #fff;
            border-top: 1px solid var(--border);
            gap: 0.5rem;
        }
        .emoji-picker-btn {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            margin-right: 0.5rem;
        }
        .message-input {
            flex: 1;
            padding: 0.7rem 1rem;
            border-radius: 20px;
            border: 1px solid var(--border);
            font-size: 1rem;
            outline: none;
        }
        .send-btn {
            background: var(--primary);
            color: #fff;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            font-size: 1.3rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background 0.2s;
        }
        .send-btn:hover {
            background: #2563eb;
        }
        /* Emoji picker popup */
        .emoji-picker {
            position: absolute;
            bottom: 60px;
            left: 20px;
            background: #fff;
            border: 1px solid var(--border);
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            padding: 0.5rem;
            display: none;
            z-index: 10;
            max-width: 250px;
            flex-wrap: wrap;
            gap: 0.3rem;
        }
        .emoji-picker span {
            font-size: 1.3rem;
            cursor: pointer;
            margin: 0.2rem;
            transition: transform 0.1s;
        }
        .emoji-picker span:hover {
            transform: scale(1.2);
        }
        /* Responsive */
        @media (max-width: 800px) {
            .sidebar {
                width: 70px;
                min-width: 70px;
                padding: 0;
            }
            .sidebar-header,
            .search-box,
            .contact-info,
            .contact-last {
                display: none;
            }
            .contact {
                justify-content: center;
                padding: 0.75rem 0;
            }
        }
        @media (max-width: 600px) {
            .container {
                flex-direction: column;
            }
            .sidebar {
                width: 100vw;
                min-width: 0;
                height: 60px;
                flex-direction: row;
                border-right: none;
                border-bottom: 1px solid var(--border);
            }
            .contacts {
                display: flex;
                flex-direction: row;
                overflow-x: auto;
                overflow-y: hidden;
                height: 60px;
            }
            .contact {
                flex-direction: column;
                align-items: center;
                border-bottom: none;
                border-right: 1px solid var(--border);
                padding: 0.3rem 0.5rem;
            }
            .chat-area {
                flex: 1;
                min-height: 0;
            }
            .chat-header {
                padding: 0.7rem;
            }
            .chat-thread {
                padding: 1rem 0.5rem 0.5rem 0.5rem;
            }
            .message-input-section {
                padding: 0.7rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Sidebar -->
        <aside class="sidebar">
            <div class="sidebar-header">Chats</div>
            <div class="search-box">
                <input type="text" id="searchInput" placeholder="Search contacts...">
            </div>
            <ul class="contacts" id="contactsList">
                <li class="contact" data-name="Vishal">
                    <div class="avatar">A<span class="status-dot"></span></div>
                    <div class="contact-info">
                        <div class="contact-name">Vishal</div>
                        <div class="contact-last">Hey, how are you?</div>
                    </div>
                </li>
                <li class="contact" data-name="Nidish">
                    <div class="avatar">B</div>
                    <div class="contact-info">
                        <div class="contact-name">Nidish</div>
                        <div class="contact-last">See you at 5!</div>
                    </div>
                </li>
                <li class="contact" data-name="Dakshina">
                    <div class="avatar">C<span class="status-dot"></span></div>
                    <div class="contact-info">
                        <div class="contact-name">Dakshina</div>
                        <div class="contact-last">👍</div>
                    </div>
                </li>
            </ul>
        </aside>
        <!-- Chat Area -->
        <main class="chat-area">
            <div class="chat-header">
                <div class="avatar">A<span class="status-dot"></span></div>
                <div>
                    <div class="contact-name" id="chatContactName">Vishal</div>
                    <div class="contact-last" id="chatContactStatus">Online</div>
                </div>
            </div>
            <div class="chat-thread" id="chatThread">
                <div class="message-row">
                    <div class="avatar" style="width:32px;height:32px;font-size:1rem;">A</div>
                    <div class="message-bubble">Hi there! 👋</div>
                    <div class="message-time">09:00</div>
                </div>
                <div class="message-row me">
                    <div class="avatar" style="width:32px;height:32px;font-size:1rem;">Me</div>
                    <div class="message-bubble">Hello Vishal! 😊</div>
                    <div class="message-time">09:01</div>
                </div>
                <div class="message-row">
                    <div class="avatar" style="width:32px;height:32px;font-size:1rem;">A</div>
                    <div class="message-bubble">How's your day?</div>
                    <div class="message-time">09:02</div>
                </div>
            </div>
            <form class="message-input-section" id="messageForm" autocomplete="off">
                <button type="button" class="emoji-picker-btn" id="emojiBtn" title="Pick emoji">😊</button>
                <input type="text" class="message-input" id="messageInput" placeholder="Type a message..." required>
                <button type="submit" class="send-btn" title="Send">&#9658;</button>
                <div class="emoji-picker" id="emojiPicker">
                    <span>😀</span><span>😂</span><span>😍</span><span>😊</span><span>👍</span>
                    <span>😎</span><span>🥳</span><span>😢</span><span>😡</span><span>🙏</span>
                    <span>🎉</span><span>❤</span><span>🔥</span><span>👏</span><span>🤔</span>
                </div>
            </form>
        </main>
    </div>
    <script>
        // Contact search
        document.getElementById('searchInput').addEventListener('input', function() {
            const val = this.value.toLowerCase();
            document.querySelectorAll('.contact').forEach(c => {
                const name = c.dataset.name.toLowerCase();
                c.style.display = name.includes(val) ? '' : 'none';
            });
        });

        // Contact switching (demo)
        document.querySelectorAll('.contact').forEach(contact => {
            contact.addEventListener('click', function() {
                document.querySelectorAll('.contact').forEach(c => c.classList.remove('active'));
                this.classList.add('active');
                // Demo: update chat header and thread
                const name = this.querySelector('.contact-name').textContent;
                document.getElementById('chatContactName').textContent = name;
                document.getElementById('chatContactStatus').textContent = this.querySelector('.status-dot') ? 'Online' : 'Offline';
                // Demo: clear and add a welcome message
                const thread = document.getElementById('chatThread');
                thread.innerHTML = `
                    <div class="message-row">
                        <div class="avatar" style="width:32px;height:32px;font-size:1rem;">${name[0]}</div>
                        <div class="message-bubble">Say hi to ${name}!</div>
                        <div class="message-time">${new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</div>
                    </div>
                `;
            });
        });

        // Emoji picker
        const emojiBtn = document.getElementById('emojiBtn');
        const emojiPicker = document.getElementById('emojiPicker');
        const messageInput = document.getElementById('messageInput');
        emojiBtn.addEventListener('click', function(e) {
            e.stopPropagation();
            emojiPicker.style.display = emojiPicker.style.display === 'flex' ? 'none' : 'flex';
            emojiPicker.style.flexDirection = 'row';
        });
        emojiPicker.addEventListener('click', function(e) {
            if (e.target.tagName === 'SPAN') {
                messageInput.value += e.target.textContent;
                emojiPicker.style.display = 'none';
                messageInput.focus();
            }
        });
        document.body.addEventListener('click', function() {
            emojiPicker.style.display = 'none';
        });
        emojiPicker.addEventListener('click', function(e) {
            e.stopPropagation();
        });

        // Send message
        document.getElementById('messageForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const text = messageInput.value.trim();
            if (!text) return;
            const thread = document.getElementById('chatThread');
            const now = new Date();
            const time = now.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
            const msg = document.createElement('div');
            msg.className = 'message-row me';
            msg.innerHTML = `
                <div class="avatar" style="width:32px;height:32px;font-size:1rem;">Me</div>
                <div class="message-bubble">${text.replace(/</g, "&lt;")}</div>
                <div class="message-time">${time}</div>
            `;
            thread.appendChild(msg);
            messageInput.value = '';
            thread.scrollTop = thread.scrollHeight;
        });
    </script>
</body>
</html>
```
