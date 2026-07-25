<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Me perdí — ayudame a volver a casa</title>
<style>
  /* ====== EDITÁ ESTOS DATOS ANTES DE SUBIR LA PÁGINA ====== */
  /* Buscá la etiqueta <script id="config"> al final del archivo */

  :root{
    --clay: #b4552f;
    --clay-dark: #7d3b20;
    --cream: #f6efe4;
    --ink: #2b2320;
    --paw: #d9c9ad;
    --green: #3f7a4f;
  }

  *{ box-sizing:border-box; }
  html,body{ margin:0; padding:0; }
  body{
    background: var(--cream);
    color: var(--ink);
    font-family: 'Iowan Old Style','Georgia',serif;
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding: 24px;
  }

  .card{
    max-width: 420px;
    width:100%;
    text-align:center;
  }

  .paw-row{
    font-size: 20px;
    letter-spacing: 10px;
    opacity: .45;
    margin-bottom: 6px;
  }

  .photo{
    width: 148px;
    height: 148px;
    border-radius: 50%;
    object-fit: cover;
    border: 5px solid var(--clay);
    margin: 8px auto 18px;
    display:block;
    box-shadow: 0 6px 18px rgba(0,0,0,.15);
  }

  .photo-placeholder{
    width:148px;height:148px;border-radius:50%;
    margin: 8px auto 18px;
    background: var(--paw);
    border: 5px solid var(--clay);
    display:flex;align-items:center;justify-content:center;
    font-size: 56px;
  }

  h1{
    font-family: 'Georgia', serif;
    font-weight: 700;
    font-size: 28px;
    margin: 0 0 6px;
    color: var(--clay-dark);
  }

  .sub{
    font-size: 16px;
    line-height: 1.5;
    color: #ffffff;
    background: var(--clay-dark);
    padding: 14px 16px;
    border-radius: 12px;
    margin: 0 0 28px;
  }

  button{
    -webkit-appearance:none;
    appearance:none;
    border:none;
    width:100%;
    padding: 18px 20px;
    font-family: inherit;
    font-size: 18px;
    font-weight: 700;
    color: white;
    background: var(--clay);
    border-radius: 14px;
    cursor:pointer;
    box-shadow: 0 4px 0 var(--clay-dark);
    transition: transform .05s ease;
  }
  button:active{
    transform: translateY(3px);
    box-shadow: 0 1px 0 var(--clay-dark);
  }
  button:focus-visible{
    outline: 3px solid var(--green);
    outline-offset: 3px;
  }
  button:disabled{
    opacity:.6;
    cursor:default;
  }

  .status{
    margin-top: 16px;
    font-size: 14px;
    min-height: 20px;
    color: #6b5c4f;
  }

  .fallback{
    margin-top: 22px;
    font-size: 14px;
    color: #6b5c4f;
    display:none;
  }
  .fallback a{
    color: var(--green);
    font-weight:700;
    text-decoration: underline;
  }

  .footer{
    margin-top: 34px;
    font-size: 12px;
    color: #9a8b7c;
  }

  @media (prefers-reduced-motion: no-preference){
    .card{ animation: rise .5s ease both; }
    @keyframes rise{
      from{ opacity:0; transform: translateY(10px); }
      to{ opacity:1; transform:none; }
    }
  }
</style>
</head>
<body>

  <div class="card">
    <div class="paw-row">🐾 🐾 🐾</div>

    <img class="photo" id="dogPhoto" src="" alt="" style="display:none">
    <div class="photo-placeholder" id="dogPlaceholder">🐶</div>

    <h1 id="title">¡Hola! Me llamo <span id="dogNameSpan">tu perro</span></h1>
    <p class="sub" id="subtext">
      Me perdí. Si me encontraste, gracias de corazón — con un toque le avisás
      a mi familia dónde estoy.
    </p>

    <button id="sendBtn">Enviar mi ubicación a la familia</button>
    <div class="status" id="status"></div>

    <div class="fallback" id="fallback">
      No pudimos obtener tu ubicación, pero igual podés
      <a id="fallbackLink" href="#" target="_blank" rel="noopener">avisarles por WhatsApp</a>.
    </div>

    <div class="footer">Collar identificador · escaneá para ayudar a volver a casa</div>
  </div>

<script id="config">
  // ================== EDITÁ SOLO ESTA PARTE ==================
  const CONFIG = {
    dogName: "GALA",                 // nombre del perro
    ownerPhone: "5493425281841",      // tu WhatsApp con código país, sin + ni espacios (ej: 549 + cod area + numero)
    photoUrl: "",                     // link a una foto (opcional, dejalo vacío si no tenés)
    rewardText: ""                    // ej: "Hay recompensa" (opcional, dejalo vacío si no querés)
  };
  // =============================================================

  document.getElementById('dogNameSpan').textContent = CONFIG.dogName;
  document.title = "Me llamo " + CONFIG.dogName + " — ayudame a volver a casa";

  if (CONFIG.photoUrl) {
    const img = document.getElementById('dogPhoto');
    img.src = CONFIG.photoUrl;
    img.alt = CONFIG.dogName;
    img.style.display = 'block';
    document.getElementById('dogPlaceholder').style.display = 'none';
  }

  function buildWaLink(withLocation, coords) {
    let text = `¡Hola! Encontré a ${CONFIG.dogName} 🐾.`;
    if (CONFIG.rewardText) text += ` (${CONFIG.rewardText})`;
    if (withLocation && coords) {
      const mapsUrl = `https://maps.google.com/?q=${coords.lat},${coords.lng}`;
      text += ` Está acá: ${mapsUrl}`;
    } else {
      text += ` No pude compartir la ubicación exacta, pero te escribo para coordinar.`;
    }
    const encoded = encodeURIComponent(text);
    return `https://wa.me/${CONFIG.ownerPhone}?text=${encoded}`;
  }

  const btn = document.getElementById('sendBtn');
  const status = document.getElementById('status');
  const fallback = document.getElementById('fallback');
  const fallbackLink = document.getElementById('fallbackLink');

  fallbackLink.href = buildWaLink(false);

  btn.addEventListener('click', () => {
    if (!CONFIG.ownerPhone || CONFIG.ownerPhone.includes('00000')) {
      status.textContent = 'Falta configurar el número de WhatsApp del dueño en esta página.';
      return;
    }

    btn.disabled = true;
    status.textContent = 'Buscando tu ubicación…';

    if (!('geolocation' in navigator)) {
      status.textContent = '';
      fallback.style.display = 'block';
      btn.disabled = false;
      return;
    }

    navigator.geolocation.getCurrentPosition(
      (pos) => {
        const coords = { lat: pos.coords.latitude, lng: pos.coords.longitude };
        status.textContent = '¡Listo! Abriendo WhatsApp…';
        window.location.href = buildWaLink(true, coords);
        btn.disabled = false;
      },
      () => {
        status.textContent = '';
        fallback.style.display = 'block';
        btn.disabled = false;
      },
      { enableHighAccuracy: true, timeout: 8000 }
    );
  });
</script>
</body>
</html>
