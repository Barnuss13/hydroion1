<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Hydroion – Prototype</title>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: white;
  min-height: 200vh;
}

/* ===== TOP BAR ===== */
#topBar {
  position: fixed;
  top: 0;
  width: 100%;
  background: #f2f2f2;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  z-index: 1000;
}

#projectName {
  font-weight: bold;
  cursor: pointer;
}

#menu {
  display: flex;
  align-items: center;
}

#menu button {
  margin-left: 10px;
  padding: 6px 14px;
  border: none;
  color: white;
  cursor: pointer;
}

#registerBtn { background: #2ecc71; }
#loginBtn { background: #3498db; }

#clock {
  margin-left: 15px;
  margin-right: 20px;
  font-weight: bold;
  white-space: nowrap;
}

/* ===== CENTER ===== */
#centerText {
  margin-top: 180px;
  text-align: center;
}

#centerText h1 {
  margin-bottom: 10px;
}

/* ===== PANELS ===== */
.panel {
  display: none;
  margin-top: 140px;
  max-width: 420px;
  margin-left: auto;
  margin-right: auto;
  border: 1px solid #ccc;
  padding: 20px;
}

.panel input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
}

.panel button {
  padding: 8px 14px;
  cursor: pointer;
}

/* ===== BOTTOM BAR ===== */
#bottomBar {
  position: fixed;
  bottom: 0;
  width: 100%;
  background: #f2f2f2;
  padding: 15px;
  display: none;
  text-align: center;
}
</style>
</head>

<body>

<!-- ===== TOP BAR ===== -->
<div id="topBar">
  <div id="projectName" onclick="location.reload()">Hydroion</div>

  <div id="menu">
    <span id="userInfo"></span>
    <button id="registerBtn" onclick="openRegister()">Register</button>
    <button id="loginBtn" onclick="openLogin()">Login</button>
    <span id="clock"></span>
  </div>
</div>

<!-- ===== CENTER TEXT ===== -->
<div id="centerText">
  <h1>Hydroion</h1>
  <h3>Prototype</h3>
  <p>
    This interface does not contain real information.<br>
    During the prototype phase, no real data is displayed.
  </p>
</div>

<!-- ===== REGISTER PANEL ===== -->
<div id="registerPanel" class="panel">
  <h3>Register</h3>
  <input id="r_name" placeholder="Full name">
  <input id="r_user" placeholder="Username">
  <input id="r_email" placeholder="Email">
  <input id="r_pass" type="password" placeholder="Password">
  <button onclick="register()">Create account</button>
</div>

<!-- ===== LOGIN PANEL ===== -->
<div id="loginPanel" class="panel">
  <h3>Login</h3>
  <input id="l_user" placeholder="Username or Email">
  <input id="l_pass" type="password" placeholder="Password">
  <button onclick="login()">Login</button>
</div>

<!-- ===== LOGGED PANEL ===== -->
<div id="loggedPanel" class="panel">
  <h3>Welcome</h3>
  <p>You are successfully logged in.</p>
</div>

<!-- ===== BOTTOM BAR ===== -->
<div id="bottomBar">
  Contact: <b>horvath.barnus13@gmail.com</b>
</div>

<script>
/* ===== CLOCK ===== */
setInterval(() => {
  const d = new Date();
  document.getElementById("clock").textContent =
    d.getHours().toString().padStart(2,"0") + ":" +
    d.getMinutes().toString().padStart(2,"0");
}, 1000);

/* ===== SCROLL LOGIC ===== */
window.addEventListener("scroll", () => {
  const atTop = window.scrollY === 0;
  document.getElementById("topBar").style.display = atTop ? "flex" : "none";
  document.getElementById("bottomBar").style.display = atTop ? "none" : "block";
});

/* ===== PANEL CONTROL ===== */
function hideAll() {
  document.querySelectorAll(".panel").forEach(p => p.style.display = "none");
}

function openRegister() {
  hideAll();
  document.getElementById("registerPanel").style.display = "block";
}

function openLogin() {
  hideAll();
  document.getElementById("loginPanel").style.display = "block";
}

/* ===== REGISTER ===== */
function register() {
  const name = r_name.value.trim();
  const user = r_user.value.trim();
  const email = r_email.value.trim();
  const pass = r_pass.value;

  if (!name || !user || !email || !pass) {
    alert("Fill all fields");
    return;
  }

  let users = JSON.parse(localStorage.getItem("users") || "[]");

  if (users.some(u => u.user === user || u.email === email)) {
    alert("Username or email already exists");
    return;
  }

  users.push({ name, user, email, pass });
  localStorage.setItem("users", JSON.stringify(users));

  r_name.value = r_user.value = r_email.value = r_pass.value = "";
  alert("Registration successful");
  hideAll();
}

/* ===== LOGIN ===== */
function login() {
  const id = l_user.value.trim();
  const pass = l_pass.value;

  let users = JSON.parse(localStorage.getItem("users") || "[]");
  const found = users.find(u =>
    (u.user === id || u.email === id) && u.pass === pass
  );

  if (!found) {
    alert("Invalid login");
    return;
  }

  localStorage.setItem("loggedInUser", found.user);
  showLoggedIn(found.user);
}

/* ===== LOGGED IN STATE ===== */
function showLoggedIn(user) {
  hideAll();
  document.getElementById("loggedPanel").style.display = "block";
  document.getElementById("registerBtn").style.display = "none";
  document.getElementById("loginBtn").style.display = "none";
  document.getElementById("userInfo").textContent =
    "Logged in as: " + user;
}

/* ===== AUTO LOGIN ===== */
const logged = localStorage.getItem("loggedInUser");
if (logged) {
  showLoggedIn(logged);
}
</script>

</body>
</html>
