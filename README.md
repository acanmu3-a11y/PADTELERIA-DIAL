# PADTELERIA-DIAL
Pastelería y repostería casera y personalizadas
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Pastelería DIAL — ESPECIALISTA EN REPOSTERÍA CASERA</title>
  <style>
    :root{
      --bg:#fffaf6;
      --card:#ffffff;
      --accent:#8b5e3c;
      --muted:#666;
      --glass: rgba(255,255,255,0.6);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{height:100%;margin:0;background:linear-gradient(180deg,#fffaf6 0%, #fff 100%);color:#222}
    header{
      padding:28px 20px 12px;
      text-align:center;
      border-bottom:1px solid rgba(0,0,0,0.04);
    }
    h1{margin:0;font-size:28px;letter-spacing:1px;color:var(--accent)}
    .subtitle{margin-top:6px;font-weight:700;color:#333;font-size:13px}
    .tagline{margin-top:6px;color:var(--muted);font-size:13px}
    main{max-width:1100px;margin:24px auto;padding:0 16px}
    .grid{
      display:grid;
      grid-template-columns:repeat(auto-fill,minmax(220px,1fr));
      gap:18px;
    }
    .card{
      background:var(--card);
      border-radius:12px;
      box-shadow:0 6px 18px rgba(30,30,30,0.06);
      overflow:hidden;
      display:flex;
      flex-direction:column;
      cursor:default;
      transition:transform .18s ease, box-shadow .18s ease;
    }
    .card:hover{transform:translateY(-6px);box-shadow:0 12px 28px rgba(30,30,30,0.09)}
    .thumb{
      width:100%;
      height:150px;
      object-fit:cover;
      display:block;
      background:#eee;
    }
    .card-body{padding:12px 14px 16px;flex:1;display:flex;flex-direction:column}
    .title{font-weight:700;font-size:15px;margin:0 0 6px}
    .meta{font-size:13px;color:var(--muted);margin-top:auto}
    .btn{
      margin-top:10px;
      background:transparent;
      border:1px solid rgba(0,0,0,0.06);
      padding:8px 10px;border-radius:8px;font-size:13px;cursor:pointer;
    }

    /* modal */
    .modal-backdrop{position:fixed;inset:0;background:rgba(0,0,0,0.45);display:none;align-items:center;justify-content:center;padding:20px;z-index:40}
    .modal{background:linear-gradient(180deg,#fff 0%,#fffaf0 100%);max-width:720px;width:100%;border-radius:12px;box-shadow:0 20px 50px rgba(0,0,0,0.35);overflow:hidden}
    .modal-header{display:flex;align-items:center;justify-content:space-between;padding:14px 18px;border-bottom:1px solid rgba(0,0,0,0.05)}
    .modal-title{font-weight:800;color:var(--accent)}
    .close{background:transparent;border:0;font-size:20px;cursor:pointer;color:#555}
    .modal-content{display:flex;gap:12px;padding:18px;flex-direction:row;align-items:flex-start}
    .modal-img{width:38%;min-width:180px;height:180px;object-fit:cover;border-radius:8px}
    .modal-right{flex:1}
    .section-title{font-weight:700;margin:0 0 8px}
    .ingredients{background:var(--glass);padding:10px;border-radius:8px;border:1px solid rgba(0,0,0,0.03)}
    .ingredients ul{margin:0;padding:0 16px;line-height:1.6;color:#222}
    footer{max-width:1100px;margin:28px auto 60px;padding:0 16px;text-align:center;color:var(--muted);font-size:13px}
    @media(max-width:720px){
      .modal-content{flex-direction:column}
      .modal-img{width:100%;height:180px}
    }
  </style>
</head>
<body>
  <header>
    <h1>Pastelería DIAL</h1>
    <div class="subtitle">ESPECIALISTA EN REPOSTERÍA CASERA</div>
    <div class="tagline">Tartas y postres caseros — descubre ingredientes y sabores</div>
  </header>

  <main>
    <section class="grid" id="gallery" aria-live="polite">
      <!-- Cards se generan por JS -->
    </section>
  </main>

  <!-- Modal -->
  <div id="modalBackdrop" class="modal-backdrop" role="dialog" aria-modal="true" aria-hidden="true">
    <div class="modal" role="document">
      <div class="modal-header">
        <div class="modal-title" id="modalTitle">Título</div>
        <button class="close" id="modalClose" aria-label="Cerrar ventana">✕</button>
      </div>
      <div class="modal-content">
        <img id="modalImg" class="modal-img" alt="">
        <div class="modal-right">
          <p class="section-title">Ingredientes (sin cantidades)</p>
          <div class="ingredients" id="modalIngredients">
            <!-- lista -->
          </div>
          <p style="margin-top:12px;color:var(--muted);font-size:13px">Si quieres la receta completa con cantidades, puedes solicitárnosla.</p>
        </div>
      </div>
    </div>
  </div>

  <footer>
    © <span id="year"></span> Pastelería DIAL — Repostería casera artesanal
  </footer>

  <script>
    // Datos: nombre, imagen (placeholder Unsplash), y array de ingredientes (sin cantidades).
    const items = [
      {
        id: 'cheesecake',
        name: 'Cheesecake clásico',
        img: 'https://source.unsplash.com/featured/?cheesecake',
        ingredients: ['Queso crema', 'Galletas', 'Mantequilla', 'Huevos', 'Azúcar', 'Extracto de vainilla', 'Ralladura de limón (opcional)']
      },
      {
        id: 'brownie',
        name: 'Brownie de chocolate',
        img: 'https://source.unsplash.com/featured/?brownie,chocolate',
        ingredients: ['Chocolate negro', 'Mantequilla', 'Azúcar', 'Huevos', 'Harina', 'Cacao en polvo', 'Nueces (opcional)']
      },
      {
        id: 'tarta-manzana',
        name: 'Tarta de manzana',
        img: 'https://source.unsplash.com/featured/?apple,pie',
        ingredients: ['Manzanas', 'Masa quebrada o brisa', 'Azúcar', 'Mantequilla', 'Canela', 'Jugo de limón']
      },
      {
        id: 'flan',
        name: 'Flan de huevo',
        img: 'https://source.unsplash.com/featured/?flan,custard',
        ingredients: ['Huevos', 'Leche', 'Azúcar', 'Esencia de vainilla', 'Caramelo']
      },
      {
        id: 'tiramisu',
        name: 'Tiramisú',
        img: 'https://source.unsplash.com/featured/?tiramisu',
        ingredients: ['Queso mascarpone', 'Bizcochos de soletilla', 'Café', 'Huevos', 'Azúcar', 'Cacao en polvo', 'Licor (opcional)']
      },
      {
        id: 'mousse-chocolate',
        name: 'Mousse de chocolate',
        img: 'https://source.unsplash.com/featured/?chocolate,mousse',
        ingredients: ['Chocolate negro', 'Huevos (claras y yemas separadas)', 'Azúcar', 'Nata para montar', 'Extracto de vainilla']
      },
      {
        id: 'panna-cotta',
        name: 'Panna cotta',
        img: 'https://source.unsplash.com/featured/?panna,cotta',
        ingredients: ['Nata líquida', 'Leche', 'Azúcar', 'Gelatina o agar-agar', 'Vainilla', 'Salsa de frutas para acompañar']
      },
      {
        id: 'creme-brulee',
        name: 'Crème brûlée',
        img: 'https://source.unsplash.com/featured/?creme,brulee',
        ingredients: ['Nata', 'Yemas de huevo', 'Azúcar', 'Vainilla', 'Azúcar para la capa caramelizada']
      },
      {
        id: 'tarta-limon-merengue',
        name: 'Tarta de limón y merengue',
        img: 'https://source.unsplash.com/featured/?lemon,meringue',
        ingredients: ['Masa quebrada', 'Limones (zumo y ralladura)', 'Huevos', 'Azúcar', 'Mantequilla', 'Claras para merengue']
      },
      {
        id: 'eclairs',
        name: 'Eclairs / profiteroles',
        img: 'https://source.unsplash.com/featured/?eclair,profiteroles',
        ingredients: ['Masa choux (harina, mantequilla, agua, huevos)', 'Crema pastelera', 'Cobertura de chocolate']
      },
      {
        id: 'tarta-zanahoria',
        name: 'Tarta de zanahoria',
        img: 'https://source.unsplash.com/featured/?carrot,cake',
        ingredients: ['Zanahorias ralladas', 'Harina', 'Azúcar', 'Huevos', 'Aceite', 'Especias (canela, nuez moscada)', 'Frosting de queso crema']
      },
      {
        id: 'helado-casero',
        name: 'Helado casero (sin máquinas)',
        img: 'https://source.unsplash.com/featured/?icecream,homemade',
        ingredients: ['Nata para montar', 'Leche condensada o azúcar', 'Sabor (chocolate, vainilla, fruta)']
      }
    ];

    const gallery = document.getElementById('gallery');
    const modalBackdrop = document.getElementById('modalBackdrop');
    const modalTitle = document.getElementById('modalTitle');
    const modalImg = document.getElementById('modalImg');
    const modalIngredients = document.getElementById('modalIngredients');
    const modalClose = document.getElementById('modalClose');

    // Generar tarjetas
    items.forEach(item => {
      const card = document.createElement('article');
      card.className = 'card';
      card.innerHTML = `
        <img class="thumb" src="${item.img}" alt="${item.name}">
        <div class="card-body">
          <h3 class="title">${item.name}</h3>
          <div class="meta">Pincha la imagen para ver ingredientes</div>
        </div>
      `;
      const img = card.querySelector('.thumb');
      img.style.cursor = 'pointer';
      img.addEventListener('click', () => openModal(item));
      gallery.appendChild(card);
    });

    // Abrir modal
    function openModal(item){
      modalTitle.textContent = item.name;
      modalImg.src = item.img;
      modalImg.alt = item.name;
      // construir lista sin cantidades
      const ul = document.createElement('ul');
      item.ingredients.forEach(ing => {
        const li = document.createElement('li');
        li.textContent = ing;
        ul.appendChild(li);
      });
      modalIngredients.innerHTML = '';
      modalIngredients.appendChild(ul);
      modalBackdrop.style.display = 'flex';
      modalBackdrop.setAttribute('aria-hidden','false');
      // focus trap simple
      modalClose.focus();
    }

    // Cerrar modal
    function closeModal(){
      modalBackdrop.style.display = 'none';
      modalBackdrop.setAttribute('aria-hidden','true');
    }

    modalClose.addEventListener('click', closeModal);
    modalBackdrop.addEventListener('click', (e)=>{
      if(e.target === modalBackdrop) closeModal();
    });
    document.addEventListener('keydown', (e)=> {
      if(e.key === 'Escape' && modalBackdrop.style.display === 'flex') closeModal();
    });

    // Año en footer
    document.getElementById('year').textContent = new Date().getFullYear();
  </script>
</body>
</html>
