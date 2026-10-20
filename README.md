<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TX-PREM | FRP Tools & Reseller Portal</title>
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
    }

    body {
      background: #07111f;
      color: #f1f5f9;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    header {
      padding: 15px 20px;
      background: #0d1b2e;
      border-bottom: 1px solid #233650;
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: relative;
    }

    .logo-container {
      text-align: left;
    }

    .logo {
      color: #38bdf8;
      font-size: 24px;
      font-weight: 900;
    }

    .logo span { color: #f8fafc; }

    header p {
      color: #94a3b8;
      font-size: 12px;
      margin-top: 2px;
    }

    .menu-btn {
      background: transparent;
      border: none;
      color: #38bdf8;
      font-size: 28px;
      cursor: pointer;
      display: none;
      padding: 5px 10px;
    }

    .nav-drawer {
      position: absolute;
      top: 100%;
      right: 0;
      width: 250px;
      background: #0d1b2e;
      border: 1px solid #233650;
      border-top: none;
      border-radius: 0 0 0 12px;
      display: none;
      flex-direction: column;
      z-index: 100;
      box-shadow: -5px 8px 15px rgba(0,0,0,0.5);
    }

    .nav-drawer.active {
      display: flex;
    }

    .nav-drawer button {
      background: transparent;
      color: #f1f5f9;
      text-align: left;
      padding: 14px 20px;
      border-bottom: 1px solid #1e2d42;
      border-radius: 0;
      font-size: 14px;
      font-weight: normal;
    }

    .nav-drawer button:hover {
      background: #172b40;
      color: #38bdf8;
    }

    main {
      width: 100%;
      max-width: 850px;
      margin: 0 auto;
      padding: 22px 15px 45px;
      flex: 1;
    }

    .hero {
      text-align: center;
      padding: 15px 5px;
    }

    .hero h1 {
      font-size: 26px;
      margin-bottom: 8px;
    }

    .hero h1 span { color: #38bdf8; }

    .hero p {
      color: #94a3b8;
      line-height: 1.5;
      font-size: 14px;
    }

    .panel, .card {
      background: #0d1b2e;
      border: 1px solid #233650;
      border-radius: 15px;
      padding: 20px;
      margin-bottom: 18px;
    }

    .tabs {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }

    .tabs button { flex: 1; }

    button {
      border: 0;
      padding: 12px;
      border-radius: 9px;
      background: #0284c7;
      color: white;
      font-size: 14px;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s;
    }

    button:hover { background: #0369a1; }
    button.secondary { background: #233650; }
    button.secondary:hover { background: #334861; }
    button:disabled { opacity: 0.6; cursor: not-allowed; }

    input {
      display: block;
      width: 100%;
      margin: 8px 0 15px;
      padding: 12px;
      border-radius: 9px;
      border: 1px solid #334861;
      background: #07111f;
      color: white;
      font-size: 14px;
    }

    label {
      color: #cbd5e1;
      font-size: 13px;
    }

    .password-row {
      display: flex;
      gap: 8px;
      align-items: stretch;
      margin: 8px 0 15px;
    }

    .password-row input {
      flex: 1;
      margin: 0;
    }

    .toggle-password {
      min-width: 70px;
      padding: 10px;
      background: #233650;
      font-size: 12px;
    }

    h2 {
      margin-bottom: 15px;
      font-size: 20px;
      color: #f8fafc;
    }

    h3 { 
      margin-bottom: 10px; 
      color: #38bdf8;
    }

    .message {
      margin-top: 15px;
      padding: 12px;
      border-radius: 8px;
      background: #172b40;
      color: #bae6fd;
      line-height: 1.5;
      overflow-wrap: anywhere;
      font-size: 13px;
    }

    .hidden { display: none !important; }

    .balance {
      background: #102b3f;
      border: 1px solid #155e75;
      border-radius: 12px;
      padding: 18px;
      margin: 18px 0;
    }

    .credits {
      font-size: 32px;
      font-weight: bold;
      color: #38bdf8;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 15px;
    }

    .card p {
      color: #94a3b8;
      line-height: 1.5;
      font-size: 13px;
    }

    .small {
      color: #94a3b8;
      font-size: 12px;
      line-height: 1.5;
      margin-top: 10px;
    }

    .full-width { width: 100%; }

    .link-btn {
      background: none;
      border: none;
      color: #38bdf8;
      font-size: 13px;
      cursor: pointer;
      padding: 0;
      text-decoration: underline;
      margin-top: 5px;
    }

    .payment-methods {
      margin-top: 15px;
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .pay-badge {
      background: #172b40;
      border: 1px solid #334861;
      padding: 6px 12px;
      border-radius: 6px;
      font-size: 12px;
      color: #e2e8f0;
    }

    footer {
      text-align: center;
      padding: 20px;
      color: #64748b;
      border-top: 1px solid #233650;
      font-size: 12px;
      background: #0d1b2e;
      margin-top: auto;
    }
  </style>
</head>
<body>

<header>
  <div class="logo-container">
    <div class="logo">TX-PREM <span>🚀</span></div>
    <p>FRP Tools & Reseller Portal</p>
  </div>
  
  <button id="menuBtn" class="menu-btn" onclick="toggleMenu()" aria-label="Toggle Navigation">☰</button>
  
  <nav id="navDrawer" class="nav-drawer">
    <button onclick="navigateTo('account')">1. Account Settings</button>
    <button onclick="navigateTo('license')">2. Tool Licenses</button>
    <button onclick="navigateTo('balance')">3. Credit Balance</button>
    <button onclick="navigateTo('rent')">4. Rent Tools</button>
    <button onclick="logout()" style="color: #f87171;">5. Log Out</button>
  </nav>
</header>

<main>
  <section class="hero">
    <h1>Welcome to <span>TX-PREM</span></h1>
    <p>Manage your reseller account, credit balance, and tool activations in one place.</p>
  </section>

  <!-- AUTH SECTION -->
  <section id="authSection" class="panel">
    <div id="authTabs" class="tabs">
      <button id="loginTab" type="button" onclick="showForm('login')">Login</button>
      <button id="registerTab" type="button" class="secondary" onclick="showForm('register')">Register</button>
    </div>

    <!-- LOGIN FORM -->
    <form id="loginForm">
      <h2>Sign In</h2>

      <label for="loginEmail">Email Address</label>
      <input id="loginEmail" type="email" placeholder="Enter your email" required autocomplete="email">

      <label for="loginPassword">Password</label>
      <div class="password-row">
        <input id="loginPassword" type="password" placeholder="Enter your password" required autocomplete="current-password">
        <button type="button" class="toggle-password" onclick="togglePassword('loginPassword', this)">Show</button>
      </div>

      <button id="loginButton" type="submit" class="full-width">Login</button>
      
      <div style="text-align: right; margin-top: 10px;">
        <button type="button" class="link-btn" onclick="showForm('forgot')">Forgot Password?</button>
      </div>
    </form>

    <!-- REGISTER FORM -->
    <form id="registerForm" class="hidden">
      <h2>Create Account</h2>

      <label for="regUsername">Username</label>
      <input id="regUsername" type="text" placeholder="Example: user123" minlength="3" maxlength="30" required autocomplete="username">

      <label for="regEmail">Email Address</label>
      <input id="regEmail" type="email" placeholder="Enter your email" required autocomplete="email">

      <label for="regPassword">Password (Min. 8 characters)</label>
      <div class="password-row">
        <input id="regPassword" type="password" placeholder="Create password" minlength="8" required autocomplete="new-password">
        <button type="button" class="toggle-password" onclick="togglePassword('regPassword', this)">Show</button>
      </div>

      <label for="regConfirmPassword">Confirm Password</label>
      <div class="password-row">
        <input id="regConfirmPassword" type="password" placeholder="Repeat password" minlength="8" required autocomplete="new-password">
        <button type="button" class="toggle-password" onclick="togglePassword('regConfirmPassword', this)">Show</button>
      </div>

      <button id="registerButton" type="submit" class="full-width">Create Account</button>
    </form>

    <!-- FORGOT PASSWORD FORM -->
    <form id="forgotForm" class="hidden">
      <h2>Reset Password Request</h2>
      <p class="small" style="margin-bottom: 15px;">Enter your registered Gmail / Email address below to receive the password reset link.</p>

      <label for="forgotEmail">Email Address</label>
      <input id="forgotEmail" type="email" placeholder="Enter your email address" required autocomplete="email">

      <button id="forgotButton" type="submit" class="full-width">Send Reset Link</button>
      
      <div style="text-align: center; margin-top: 15px;">
        <button type="button" class="link-btn" onclick="showForm('login')">Back to Login</button>
      </div>
    </form>

    <!-- NEW PASSWORD FORM (Triggered after clicking reset link from Gmail) -->
    <form id="resetPasswordForm" class="hidden">
      <h2>Set New Password</h2>
      <p class="small" style="margin-bottom: 15px;">Enter your new password below and confirm it to regain access.</p>

      <label for="newPassword">New Password (Min. 8 characters)</label>
      <div class="password-row">
        <input id="newPassword" type="password" placeholder="Enter new password" minlength="8" required>
        <button type="button" class="toggle-password" onclick="togglePassword('newPassword', this)">Show</button>
      </div>

      <label for="confirmNewPassword">Confirm New Password</label>
      <div class="password-row">
        <input id="confirmNewPassword" type="password" placeholder="Repeat new password" minlength="8" required>
        <button type="button" class="toggle-password" onclick="togglePassword('confirmNewPassword', this)">Show</button>
      </div>

      <button id="resetPasswordBtn" type="submit" class="full-width">Update Password & Login</button>
    </form>

    <div id="authMessage" class="message hidden"></div>
  </section>

  <!-- DASHBOARD -->
  <section id="dashboardSection" class="hidden">
    
    <!-- ACCOUNT SECTION -->
    <div id="accountSection" class="panel nav-section">
      <h2>1. Account Settings</h2>
      <p class="small">View and update your profile information.</p>
      <br>

      <p style="margin-bottom: 8px;"><strong>Current Email:</strong> <span id="dashAccountEmail">--</span></p>

      <form id="updateProfileForm" style="margin-top: 15px;">
        <h3>Change Username</h3>
        <label for="updateUsername">Username</label>
        <input id="updateUsername" type="text" placeholder="New username" required>
        <button type="submit" class="secondary">Update Username</button>
      </form>

      <hr style="border-color: #233650; margin: 20px 0;">

      <form id="changePasswordForm">
        <h3>Change Password</h3>
        <label for="changePassCurrent">New Password</label>
        <div class="password-row">
          <input id="changePassCurrent" type="password" placeholder="Enter new password" minlength="8" required>
          <button type="button" class="toggle-password" onclick="togglePassword('changePassCurrent', this)">Show</button>
        </div>
        <button type="submit" class="secondary">Update Password</button>
      </form>
    </div>

    <!-- LICENSES SECTION (ALPHABETICAL) -->
    <div id="licenseSection" class="panel nav-section hidden">
      <h2>2. Tool Licenses & Activations</h2>
      <p class="small" style="margin-bottom: 15px;">Available FRP and servicing tools listed alphabetically.</p>

      <div id="toolsGrid" class="grid">
        <div class="card">
          <h3>AMT Tool (Android Multi Tool)</h3>
          <p>Complete solution for Xiaomi, Vivo, Oppo, and Samsung FRP/Unlocks.</p>
          <div class="small" style="color: #38bdf8;">Status: Available | Contact Admin to Purchase</div>
        </div>

        <div class="card">
          <h3>Chimera Tool</h3>
          <p>Advanced tool for Samsung, Huawei, and MTK/Qualcomm devices.</p>
          <div class="small" style="color: #38bdf8;">Status: Available | Instant License</div>
        </div>

        <div class="card">
          <h3>TSM Tool (Tanzania Super Module)</h3>
          <p>Customized repair and unlocking module for local mobile devices.</p>
          <div class="small" style="color: #38bdf8;">Status: Available | Auto Activation</div>
        </div>

        <div class="card">
          <h3>UnlockTool</h3>
          <p>Popular multi-brand tool for Apple, Samsung, Xiaomi, and Tecno FRP/Bypass.</p>
          <div class="small" style="color: #38bdf8;">Status: Available | 3/6/12 Month License</div>
        </div>
      </div>
    </div>

    <!-- BALANCE SECTION -->
    <div id="balanceSection" class="panel nav-section hidden">
      <h2>3. Reseller Credit Balance</h2>
      <div class="balance">
        <p>Your Current Credits</p>
        <div class="credits" id="userCredits">Loading...</div>
      </div>

      <h3>How to Top Up / Add Credits</h3>
      <p class="small">To add credits via automated SMS verification, transfer funds to the verified number below:</p>
      
      <div style="background: #07111f; padding: 15px; border-radius: 8px; margin: 15px 0; border: 1px solid #233650;">
        <p style="font-size: 16px; font-weight: bold; color: #38bdf8;">Receiver Number: +255628354099</p>
        <p class="small">The system checks incoming payment confirmation messages automatically.</p>
      </div>

      <p class="small"><strong>Supported Gateways:</strong></p>
      <div class="payment-methods">
        <span class="pay-badge">Tigo Pesa</span>
        <span class="pay-badge">Vodacom M-Pesa</span>
        <span class="pay-badge">HaloPesa</span>
        <span class="pay-badge">Airtel Money</span>
        <span class="pay-badge">Binance Pay (Crypto)</span>
        <span class="pay-badge">PayPal</span>
      </div>
    </div>

    <!-- RENT SECTION -->
    <div id="rentSection" class="panel nav-section hidden">
      <h2>4. Tool Rental Services</h2>
      <p class="small">Rent digital tools on a daily or hourly basis.</p>
      <br>

      <div class="grid">
        <div class="card">
          <h3>Rent UnlockTool (2 Hours)</h3>
          <p>Quick access for single device flashing & FRP bypass.</p>
          <button class="full-width" style="margin-top: 10px;" onclick="alert('Contact +255628354099 to confirm rent slot.')">Rent Now</button>
        </div>

        <div class="card">
          <h3>Rent Chimera Tool (24 Hours)</h3>
          <p>Full license access valid for 24 hours.</p>
          <button class="full-width" style="margin-top: 10px;" onclick="alert('Contact +255628354099 to confirm rent slot.')">Rent Now</button>
        </div>
      </div>
    </div>

    <div id="dashboardMessage" class="message hidden"></div>
  </section>
</main>

<footer>
  <p>TX-PREM 🚀 | FRP Tools & Reseller Portal</p>
  <p style="margin-top: 6px;">Human Rights & Technical Support Contact: <strong>+255628354099</strong></p>
</footer>

<script>
  const SUPABASE_URL = "https://ybhgxtnvemvqrysknvmp.supabase.co";
  const SUPABASE_KEY = "sb_publishable_5SrVUvBqMzzXHuKE8ph6cA_uZB_W2yj";

  let supabaseClient = null;

  const authSection = document.getElementById("authSection");
  const dashboardSection = document.getElementById("dashboardSection");
  const authMessage = document.getElementById("authMessage");
  const dashboardMessage = document.getElementById("dashboardMessage");
  const menuBtn = document.getElementById("menuBtn");
  const navDrawer = document.getElementById("navDrawer");

  function showMessage(element, msg) {
    element.textContent = msg;
    element.classList.remove("hidden");
  }

  function hideMessage(element) {
    element.textContent = "";
    element.classList.add("hidden");
  }

  function togglePassword(inputId, btn) {
    const input = document.getElementById(inputId);
    const show = input.type === "password";
    input.type = show ? "text" : "password";
    btn.textContent = show ? "Hide" : "Show";
  }

  function toggleMenu() {
    navDrawer.classList.toggle("active");
  }

  function showForm(formName) {
    hideMessage(authMessage);
    document.getElementById("loginForm").classList.toggle("hidden", formName !== "login");
    document.getElementById("registerForm").classList.toggle("hidden", formName !== "register");
    document.getElementById("forgotForm").classList.toggle("hidden", formName !== "forgot");
    document.getElementById("resetPasswordForm").classList.toggle("hidden", formName !== "reset");
    
    document.getElementById("authTabs").classList.toggle("hidden", formName === "reset" || formName === "forgot");
    if(formName === 'login' || formName === 'register') {
      document.getElementById("loginTab").classList.toggle("secondary", formName !== "login");
      document.getElementById("registerTab").classList.toggle("secondary", formName !== "register");
    }
  }

  function navigateTo(sectionName) {
    navDrawer.classList.remove("active");
    const sections = document.querySelectorAll(".nav-section");
    sections.forEach(s => s.classList.add("hidden"));

    const target = document.getElementById(sectionName + "Section");
    if (target) target.classList.remove("hidden");
  }

  async function startApp() {
    try {
      if (!window.supabase) throw new Error("Supabase SDK failed to load.");
      supabaseClient = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

      supabaseClient.auth.onAuthStateChange(async (event, session) => {
        if (event === "PASSWORD_RECOVERY") {
          authSection.classList.remove("hidden");
          dashboardSection.classList.add("hidden");
          showForm("reset");
          showMessage(authMessage, "Please set and confirm your new password below.");
        } else if (session) {
          showDashboard(session.user);
        } else {
          showLogin();
        }
      });

      const { data } = await supabaseClient.auth.getSession();
      if (data.session) showDashboard(data.session.user);

    } catch (err) {
      showMessage(authMessage, "Connection error: " + err.message);
    }
  }

  function showDashboard(user) {
    authSection.classList.add("hidden");
    dashboardSection.classList.remove("hidden");
    menuBtn.style.display = "block";
    document.getElementById("dashAccountEmail").textContent = user.email || "";
    loadProfile(user);
  }

  function showLogin() {
    dashboardSection.classList.add("hidden");
    authSection.classList.remove("hidden");
    menuBtn.style.display = "none";
    navDrawer.classList.remove("active");
    showForm("login");
  }

  document.getElementById("loginForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    hideMessage(authMessage);
    const email = document.getElementById("loginEmail").value.trim();
    const password = document.getElementById("loginPassword").value;

    try {
      const { data, error } = await supabaseClient.auth.signInWithPassword({ email, password });
      if (error) throw error;
      showDashboard(data.user);
    } catch (err) {
      showMessage(authMessage, "Login failed: " + err.message);
    }
  });

  document.getElementById("registerForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    hideMessage(authMessage);

    const username = document.getElementById("regUsername").value.trim();
    const email = document.getElementById("regEmail").value.trim();
    const password = document.getElementById("regPassword").value;
    const confirmPass = document.getElementById("regConfirmPassword").value;

    if (password !== confirmPass) {
      showMessage(authMessage, "Passwords do not match. Please verify and try again.");
      return;
    }

    try {
      const { data, error } = await supabaseClient.auth.signUp({
        email, password, options: { data: { username } }
      });
      if (error) throw error;
      
      if(data.session) {
        showDashboard(data.user);
      } else {
        showMessage(authMessage, "Registration completed successfully! Please check your Gmail to confirm your email address.");
      }
    } catch (err) {
      showMessage(authMessage, "Registration failed: " + err.message);
    }
  });

  document.getElementById("forgotForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    hideMessage(authMessage);
    const email = document.getElementById("forgotEmail").value.trim();

    try {
      const { error } = await supabaseClient.auth.resetPasswordForEmail(email, {
        redirectTo: window.location.href
      });
      if (error) throw error;
      showMessage(authMessage, "A password reset link has been sent to your Gmail/Email address. Please check your inbox.");
    } catch (err) {
      showMessage(authMessage, "Error: " + err.message);
    }
  });

  document.getElementById("resetPasswordForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    hideMessage(authMessage);

    const newPassword = document.getElementById("newPassword").value;
    const confirmNewPassword = document.getElementById("confirmNewPassword").value;

    if (newPassword !== confirmNewPassword) {
      showMessage(authMessage, "Passwords do not match. Please repeat the exact same password.");
      return;
    }

    try {
      const { error } = await supabaseClient.auth.updateUser({ password: newPassword });
      if (error) throw error;
      
      showMessage(authMessage, "Password updated successfully! Logging you in...");
      setTimeout(() => {
        const { data } = supabaseClient.auth.getSession();
        if(data && data.session) {
          showDashboard(data.session.user);
        } else {
          showForm("login");
        }
      }, 1500);

    } catch (err) {
      showMessage(authMessage, "Failed to update password: " + err.message);
    }
  });

  async function loadProfile(user) {
    try {
      const { data, error } = await supabaseClient
        .from("reseller_profiles")
        .select("username, credits")
        .eq("id", user.id)
        .single();

      if (error) throw error;
      document.getElementById("userCredits").textContent = Number(data.credits || 0).toLocaleString("en-US");
      if(data.username) document.getElementById("updateUsername").value = data.username;
    } catch (err) {
      document.getElementById("userCredits").textContent = "0.00";
    }
  }

  document.getElementById("updateProfileForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    hideMessage(dashboardMessage);
    const username = document.getElementById("updateUsername").value.trim();

    try {
      const { data: { user } } = await supabaseClient.auth.getUser();
      const { error } = await supabaseClient
        .from("reseller_profiles")
        .update({ username })
        .eq("id", user.id);

      if (error) throw error;
      showMessage(dashboardMessage, "Username updated successfully!");
    } catch (err) {
      showMessage(dashboardMessage, "Failed to update username: " + err.message);
    }
  });

  document.getElementById("changePasswordForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    hideMessage(dashboardMessage);
    const newPassword = document.getElementById("changePassCurrent").value;

    try {
      const { error } = await supabaseClient.auth.updateUser({ password: newPassword });
      if (error) throw error;
      showMessage(dashboardMessage, "Password changed successfully!");
      document.getElementById("changePasswordForm").reset();
    } catch (err) {
      showMessage(dashboardMessage, "Failed to change password: " + err.message);
    }
  });

  async function logout() {
    if (supabaseClient) await supabaseClient.auth.signOut();
    showLogin();
  }

  startApp();
</script>
</body>
</html>
