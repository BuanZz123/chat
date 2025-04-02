<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Realtime Chat</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f1f1f1;
      margin: 0;
    }
    #chat-container {
      width: 100%;
      max-width: 600px;
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    #messages {
      list-style-type: none;
      padding: 0;
      margin: 0;
      max-height: 400px;
      overflow-y: auto;
      margin-bottom: 20px;
    }
    #messages li {
      padding: 8px;
      background-color: #f4f4f4;
      margin-bottom: 5px;
      border-radius: 5px;
    }
    #message-input {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      box-sizing: border-box;
    }
    #send-btn {
      width: 100%;
      padding: 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
    #send-btn:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>
  <div id="chat-container">
    <h1>Realtime Chat</h1>
    <ul id="messages"></ul>
    <input id="message-input" autocomplete="off" placeholder="Type a message..." />
    <button id="send-btn">Send</button>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>

  <script>
    // Cấu hình Firebase (sao chép từ Firebase Console)
const firebaseConfig = {
  apiKey: "AIzaSyD6LDC7ieZpee4e-U15KO6-j6ot12tpdSA",
  authDomain: "ho3u-5f177.firebaseapp.com",
  databaseURL: "https://ho3u-5f177-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "ho3u-5f177",
  storageBucket: "ho3u-5f177.firebasestorage.app",
  messagingSenderId: "264550580305",
  appId: "1:264550580305:web:1176ec65d3b4174b6cb870",
  measurementId: "G-9937KR0WTJ"
};

// Initialize Firebase
const app = firebase.initializeApp(firebaseConfig);
const db = firebase.database(app);

// Lấy các phần tử từ DOM
const messageInput = document.getElementById('message-input');
const sendButton = document.getElementById('send-btn');
const messagesList = document.getElementById('messages');

// Gửi tin nhắn
sendButton.addEventListener('click', () => {
  const message = messageInput.value;
  if (message) {
    // Thêm tin nhắn vào Firebase Realtime Database
    firebase.database().ref('messages').push().set({
      message: message,
      timestamp: Date.now()
    });
    messageInput.value = ''; // Xóa input sau khi gửi
  }
});

// Lắng nghe tin nhắn mới từ Firebase
firebase.database().ref('messages').on('child_added', (snapshot) => {
  const messageData = snapshot.val();
  const messageElement = document.createElement('li');
  messageElement.textContent = messageData.message;
  messagesList.appendChild(messageElement);
});
  </script>
</body>
</html>
