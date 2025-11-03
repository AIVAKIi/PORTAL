<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Вход</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }

    body {
      background-color: #121212;
      color: #f0f0f0;
      height: 100vh;
      overflow: hidden;
    }

    .screen {
      display: none;
      justify-content: center;
      align-items: center;
      height: 100%;
    }

    .screen.active {
      display: flex;
    }

    .auth-container {
      width: 100%;
      max-width: 400px;
      padding: 40px;
      background-color: #1e1e1e;
      border-radius: 12px;
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.5);
    }

    h1 {
      font-size: 24px;
      margin-bottom: 24px;
      text-align: center;
      color: #ffffff;
    }

    .input-group {
      margin-bottom: 16px;
    }

    input {
      width: 100%;
      padding: 12px 16px;
      background-color: #2d2d2d;
      border: 1px solid #444;
      border-radius: 8px;
      font-size: 16px;
      color: #f0f0f0;
    }

    input:focus {
      outline: none;
      border-color: #007aff;
      box-shadow: 0 0 0 2px rgba(0, 122, 255, 0.3);
    }

    button {
      width: 100%;
      padding: 12px;
      background-color: #007aff;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
    }

    button:hover {
      background-color: #0062cc;
    }

    .error {
      color: #ff6b6b;
      font-size: 14px;
      margin-top: 8px;
      display: none;
    }

   
    #empty-screen {
      background-color: #000;
    }
  </style>
</head>
<body>


  <div id="login-screen" class="screen active">
    <div class="auth-container">
      <h1>Войти</h1>
      <form id="loginForm">
        <div class="input-group">
          <input type="email" id="email" placeholder="Email" required autocomplete="email"/>
        </div>
        <div class="input-group">
          <input type="password" id="password" placeholder="Пароль" required autocomplete="current-password"/>
        </div>
        <button type="submit">Войти</button>
        <div class="error" id="error"></div>
      </form>
    </div>
  </div>

 
  <div id="empty-screen" class="screen"></div>

  <script>
    const TOKEN_KEY = 'auth_token';
    const EXPIRES_AT_KEY = 'token_expires_at';

    function showScreen(id) {
      document.querySelectorAll('.screen').forEach(el => el.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    
    window.addEventListener('DOMContentLoaded', () => {
      const token = localStorage.getItem(TOKEN_KEY);
      const expiresAt = localStorage.getItem(EXPIRES_AT_KEY);

      if (token && expiresAt && Date.now() < parseInt(expiresAt)) {
        showScreen('empty-screen');
      } else {
        
        localStorage.removeItem(TOKEN_KEY);
        localStorage.removeItem(EXPIRES_AT_KEY);
        showScreen('login-screen');
      }
    });

  
    document.getElementById('loginForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const email = document.getElementById('email').value.trim();
      const password = document.getElementById('password').value.trim();
      const errorEl = document.getElementById('error');

      if (email && password) {
        
        const token = btoa(email + ':' + password);
        const expiresAt = Date.now() + 15 * 60 * 1000;

        localStorage.setItem(TOKEN_KEY, token);
        localStorage.setItem(EXPIRES_AT_KEY, expiresAt.toString());

        showScreen('empty-screen');
      } else {
        errorEl.textContent = 'Введите email и пароль';
        errorEl.style.display = 'block';
        setTimeout(() => { errorEl.style.display = 'none'; }, 3000);
      }
    });
  </script>
</body>
</html>
