<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Academia do Rafael Guedes</title>

  <style>
    :root{
      --bg:#0f1720;
      --card:#111827;
      --accent:#10b981;
      --glass: rgba(255,255,255,0.03);
      --text:#e6edf3;
      --muted:#9aa4b2;
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, Arial;
      background:
        radial-gradient(circle at 10% 10%, rgba(16,185,129,0.06), transparent 10%),
        linear-gradient(180deg, #071024 0%, #071428 60%);
      color:var(--text);
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
    }

    .container{width:100%; max-width:880px}
    .card{
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:14px;
      padding:28px;
      box-shadow: 0 10px 30px rgba(2,6,23,0.6);
      border: 1px solid rgba(255,255,255,0.03);
    }

    .card-header h1{
      margin:0;
      font-size:20px;
      letter-spacing:0.2px;
    }
    .sub{
      margin:6px 0 16px 0;
      color:var(--muted);
      font-size:13px;
    }

    .login-form{
      display:flex;
      gap:10px;
      align-items:center;
      margin-bottom:18px;
    }
    .login-form input{
      flex:1;
      padding:12px 14px;
      border-radius:10px;
      border:1px solid rgba(255,255,255,0.04);
      background:var(--glass);
      color:var(--text);
      outline:none;
      font-size:15px;
    }
    .login-form input:focus{
      box-shadow:0 0 0 4px rgba(16,185,129,0.06);
      border-color:rgba(16,185,129,0.4);
    }
    .login-form button{
      padding:11px 16px;
      border-radius:10px;
      border:none;
      background:var(--accent);
      color:#042018;
      font-weight:600;
      cursor:pointer;
      transition:transform .12s ease, box-shadow .12s;
    }
    .login-form button:hover{
      transform:translateY(-2px);
      box-shadow:0 8px 20px rgba(16,185,129,0.12);
    }

    .resultado{
      margin-top:6px;
      display:flex;
      gap:18px;
      align-items:center;
      min-height:120px;
    }

    .avatar-wrap{
      width:120px;
      height:120px;
      border-radius:16px;
      background:linear-gradient(135deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      display:flex;
      align-items:center;
      justify-content:center;
      position:relative;
      overflow:visible;
      flex-shrink:0;
      transition:transform .35s ease, opacity .25s ease;
    }
    .avatar-wrap.hidden{opacity:0; transform:translateY(6px) scale(.98); pointer-events:none}
    .avatar-wrap.visible{opacity:1; transform:translateY(0) scale(1); pointer-events:auto}

    .avatar-wrap img{
      width:92px;
      height:92px;
      object-fit:cover;
      border-radius:50%;
      border:4px solid rgba(255,255,255,0.06);
      box-shadow: 0 8px 30px rgba(2,6,23,0.6);
      animation: float 3s ease-in-out infinite;
    }

    .joia{
      position:absolute;
      bottom:12px;
      right:8px;
      font-size:28px;
      opacity:0;
      transform: translateY(12px) scale(.6) rotate(-14deg);
      transition: all .45s cubic-bezier(.2,.9,.2,1);
      pointer-events:none;
      filter: drop-shadow(0 6px 12px rgba(16,185,129,0.14));
    }
    .joia.show{
      opacity:1;
      transform: translateY(0) scale(1) rotate(0deg);
    }

    .mensagem{
      flex:1;
      font-size:18px;
      font-weight:600;
      line-height:1.3;
    }

    @keyframes float {
      0%{ transform: translateY(0) }
      50%{ transform: translateY(-6px) }
      100%{ transform: translateY(0) }
    }

    @media (max-width:520px){
      .resultado{flex-direction:column; align-items:flex-start}
      .avatar-wrap{width:96px;height:96px}
    }
  </style>
</head>

<body>
  <main class="container">
    <div class="card">
      <header class="card-header">
        <h1>Login - Academia do Rafael Guedes</h1>
        <p class="sub">Digite seu nome e receba uma saudação personalizada 🎉</p>
      </header>

      <form class="login-form" onsubmit="return false;">
        <input id="nome" type="text" placeholder="Digite seu nome..." autocomplete="off" />
        <button type="button" id="btnEntrar">Entrar</button>
      </form>

      <section class="resultado">
        <div class="avatar-wrap hidden" id="avatarWrap">
          <img src="personagem.png" alt="Avatar" id="avatar" onerror="this.style.display='none';"/>
          <div class="joia" id="joia">👍</div>
        </div>
        <div class="mensagem" id="mensagem"></div>
      </section>
    </div>
  </main>

  <script>
    const btn = document.getElementById("btnEntrar");
    const nomeInput = document.getElementById("nome");
    const mensagem = document.getElementById("mensagem");
    const avatarWrap = document.getElementById("avatarWrap");
    const joia = document.getElementById("joia");

    const dono = "Rafael Guedes de Melo Santana";

    function escapeHtml(text) {
      const map = { '&':'&amp;', '<':'&lt;', '>':'&gt;', '"':'&quot;', "'":'&#039;' };
      return text.replace(/[&<>"']/g, m => map[m]);
    }

    function mostrarMensagem(nomeDigitado) {
      const nome = nomeDigitado.trim();
      if (!nome) {
        mensagem.textContent = "Digite um nome válido.";
        avatarWrap.classList.add("hidden");
        joia.classList.remove("show");
        return;
      }

      mensagem.innerHTML = `😄 <strong>Seja bem-vindo à Academia do ${dono}, ${escapeHtml(nome)}!</strong>`;

      avatarWrap.classList.remove("hidden");
      avatarWrap.classList.add("visible");

      joia.classList.remove("show");
      setTimeout(() => joia.classList.add("show"), 250);
    }

    btn.addEventListener("click", () => {
      mostrarMensagem(nomeInput.value);
    });

    nomeInput.addEventListener("keydown", (e) => {
      if (e.key === "Enter") mostrarMensagem(nomeInput.value);
    });
  </script>
</body>
</html>
