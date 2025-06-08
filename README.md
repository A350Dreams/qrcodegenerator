<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>QR Code Generator</title>
  <script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>
  <link rel="icon" type="image/png" href="https://cdn.discordapp.com/attachments/1377572968766636084/1381350183232868526/Screenshot_2025-06-08_201214.png?ex=6847321b&is=6845e09b&hm=15d562784c0143ff95e5c42122c8b3dda0303e62fb795fb4e447de90f95acfb5&">
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #040f80;
      color: rgb(118, 116, 116);
      margin: 0;
      padding: 0;
    }

    header {
      background: #000;
      padding: 1rem 2rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .left-header {
      display: flex;
      align-items: center;
    }

    .logo {
      height: 50px;
      margin-right: 1rem;
    }

    .nav-links a {
      margin-left: 1rem;
      color: white;
      text-decoration: none;
      font-weight: bold;
      cursor: pointer;
    }

    main {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
    }

    input, button {
      padding: 10px;
      margin: 10px;
      border-radius: 5px;
      border: none;
    }

    button {
      background-color: #00b4d8;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }

    #qrcode {
      margin-top: 20px;
    }

    .popup {
      background: #ffffff;
      border: 1px solid #fff;
      padding: 1rem;
      position: absolute;
      top: 80px;
      right: 20px;
      display: none;
      z-index: 10;
      border-radius: 10px;
      color: black;
    }
  </style>
</head>
<body>

<header>
  <div class="left-header">
    <img src="https://cdn.discordapp.com/attachments/1377572968766636084/1381347535981121686/4826B8F3-D245-4FBB-8278-F4BC558B1B4E.png?ex=68472fa4&is=6845de24&hm=7f68747bcfdcb7916bce21e7cfb58e9a4aaa8a1c3365e38b2db77d3210f8042c&" alt="Logo" class="logo">
    <h2 style="margin: 0;">QR Code Generator</h2>
  </div>
  <div class="nav-links" id="nav">
    <a onclick="toggleForm('login')">Login</a>
    <a onclick="toggleForm('register')">Register</a>
  </div>
</header>

<!-- Login/Register Forms -->
<div class="popup" id="loginForm">
  <h3>Login</h3>
  <input type="text" id="loginUser" placeholder="Username"><br>
  <input type="password" id="loginPass" placeholder="Password"><br>
  <button onclick="login()">Login</button>
</div>

<div class="popup" id="registerForm">
  <h3>Register</h3>
  <input type="text" id="registerUser" placeholder="Username"><br>
  <input type="password" id="registerPass" placeholder="Password"><br>
  <button onclick="register()">Register</button>
</div>

<main>
  <h1>Generate your QR Code</h1>
  <input type="text" id="qrText" placeholder="Enter text or URL" />
  <button onclick="generateQR()">Generate</button>
  <div id="qrcode"></div>
</main>

<script>
  function toggleForm(formId) {
    document.getElementById("loginForm").style.display = formId === "login" ? "block" : "none";
    document.getElementById("registerForm").style.display = formId === "register" ? "block" : "none";
  }

  function register() {
    const user = document.getElementById("registerUser").value;
    const pass = document.getElementById("registerPass").value;
    if (!user || !pass) return alert("Fill both fields.");
    let users = JSON.parse(localStorage.getItem("users")) || {};
    if (users[user]) return alert("User already exists.");
    users[user] = pass;
    localStorage.setItem("users", JSON.stringify(users));
    alert("Registered! Now log in.");
    toggleForm();
  }

  function login() {
    const user = document.getElementById("loginUser").value;
    const pass = document.getElementById("loginPass").value;
    const users = JSON.parse(localStorage.getItem("users")) || {};
    if (users[user] === pass) {
      localStorage.setItem("currentUser", user);
      updateNav();
      document.getElementById("loginForm").style.display = "none";
    } else {
      alert("Invalid login.");
    }
  }

  function logout() {
    localStorage.removeItem("currentUser");
    updateNav();
  }

  function updateNav() {
    const nav = document.getElementById("nav");
    const user = localStorage.getItem("currentUser");
    if (user) {
      nav.innerHTML = `Welcome, <strong>${user}</strong> <a onclick="logout()">Logout</a>`;
    } else {
      nav.innerHTML = `
        <a onclick="toggleForm('login')">Login</a>
        <a onclick="toggleForm('register')">Register</a>
      `;
    }
  }

  function generateQR() {
    const qrDiv = document.getElementById("qrcode");
    qrDiv.innerHTML = "";
    const text = document.getElementById("qrText").value;
    if (text.trim() === "") return;
    QRCode.toCanvas(document.createElement("canvas"), text, { width: 200 }, (err, canvas) => {
      if (err) console.error(err);
      qrDiv.appendChild(canvas);
    });
  }

  updateNav();
</script>

</body>
</html>
