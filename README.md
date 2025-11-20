<!DOCTYPE html>
<html>
<head>
    <title>မြတ်မင်းသန့်ရဲ့ website</title>
  <h2>မြတ်မင်းသန့် website မှ အားလုံးကိုကြိုဆိုပါတယ်</h2>
    <style>
        body { font-family: Arial; padding: 20px; }
        #loginBox, #registerBox, #chatBox { border: 1px solid #333; padding: 15px; width: 300px; margin-bottom: 20px; }
        #messages { border: 1px solid #555; height: 200px; overflow-y: scroll; padding: 10px; background: #f2f2f2; }
    </style>
</head>
<body>

<h2>Register</h2>
<div id="registerBox">
    <input id="regUser" type="text" placeholder="Username"><br><br>
    <input id="regPass" type="password" placeholder="Password"><br><br>
    <button onclick="register()">Register</button>
</div>

<h2>Login</h2>
<div id="loginBox">
    <input id="logUser" type="text" placeholder="Username"><br><br>
    <input id="logPass" type="password" placeholder="Password"><br><br>
    <button onclick="login()">Login</button>
</div>

<div id="chatBox" style="display:none;">
    <h3>Public Chat Box</h3>
    <div id="messages"></div><br>
    <input id="msg" type="text" placeholder="Type message" style="width:80%;">
    <button onclick="sendMessage()">Send</button>
</div>

<script>
    // Save Accounts
    function register() {
        let user = document.getElementById("regUser").value;
        let pass = document.getElementById("regPass").value;

        if (user === "" || pass === "") {
            alert("Fill all fields");
            return;
        }

        if (localStorage.getItem(user)) {
            alert("Account already exists");
        } else {
            localStorage.setItem(user, pass);
            alert("Account Registered");
        }
    }

    // Login System
    function login() {
        let user = document.getElementById("logUser").value;
        let pass = document.getElementById("logPass").value;

        if (localStorage.getItem(user) === pass) {
            alert("Login Successful");
            localStorage.setItem("currentUser", user);
            document.getElementById("chatBox").style.display = "block";
        } else {
            alert("Wrong username or password");
        }
    }

    // Load messages
    window.onload = function() {
        loadMessages();
    }

    function loadMessages() {
        let chat = JSON.parse(localStorage.getItem("publicChat") || "[]");
        let msgBox = document.getElementById("messages");
        msgBox.innerHTML = "";
        chat.forEach(m => {
            msgBox.innerHTML += `<p><strong>${m.user}</strong>: ${m.text}</p>`;
        });
    }

    // Send message
    function sendMessage() {
        let text = document.getElementById("msg").value;
        let user = localStorage.getItem("currentUser");

        if (!user) {
            alert("You must login first");
            return;
        }

        let chat = JSON.parse(localStorage.getItem("publicChat") || "[]");
        chat.push({ user: user, text: text });
        localStorage.setItem("publicChat", JSON.stringify(chat));
        document.getElementById("msg").value = "";
        loadMessages();
    }
</script>

</body>
</html>
