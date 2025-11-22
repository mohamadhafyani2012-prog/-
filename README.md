<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>ريستورو بيدزا — ميني صفحة</title>
  <style>
    :root{--accent:#d84315;--dark:#222;--muted:#666}
    *{box-sizing:border-box}
    body{font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif; margin:0; background:#fff8f5; color:var(--dark);}
    header{background:linear-gradient(90deg,#fff 0%, #fff8f5 100%); padding:18px 20px; display:flex;align-items:center;justify-content:space-between; border-bottom:1px solid #f2d9d2}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:56px;height:56px;border-radius:12px;background:var(--accent);display:flex;align-items:center;justify-content:center;color:#fff;font-weight:700;font-size:20px}
    nav a{margin-left:14px;text-decoration:none;color:var(--muted)}
    .hero{padding:30px 20px; display:flex; gap:20px; align-items:center; justify-content:space-between}
    .hero-left{max-width:640px}
    .title{font-size:28px;margin:0 0 8px}
    .subtitle{color:var(--muted); margin:0 0 16px}
    .cta{background:var(--accent); color:#fff; border:none;padding:12px 18px;border-radius:10px; cursor:pointer}
    .menu{padding:10px 20px 80px}
    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px}
    .card{background:#fff;border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(0,0,0,0.06);display:flex;flex-direction:column;gap:8px}
    .thumb{height:140px;border-radius:10px;background-size:cover;background-position:center}
    .name{font-weight:600}
    .desc{font-size:13px;color:var(--muted)}
    .row{display:flex;justify-content:space-between;align-items:center;margin-top:6px}
    .price{font-weight:700}
    .add{background:linear-gradient(90deg,#ff7a42,#d84315);color:#fff;border:none;padding:8px 10px;border-radius:8px;cursor:pointer}
    /* cart sidebar */
    .cart-toggle{position:fixed;left:16px;bottom:18px;background:var(--dark);color:#fff;padding:10px 12px;border-radius:999px;box-shadow:0 8px 20px rgba(0,0,0,0.12);cursor:pointer}
    .cart{position:fixed;left:0;top:0;height:100vh;width:360px;background:#fff;box-shadow:6px 0 30px rgba(0,0,0,0.08);transform:translateX(-380px);transition:transform .28s;display:flex;flex-direction:column}
    .cart.open{transform:translateX(0)}
    .cart .head{padding:18px;border-bottom:1px solid #f2f2f2;font-weight:700}
    .items{flex:1;overflow:auto;padding:12px}
    .item{display:flex;gap:10px;padding:10px 6px;border-bottom:1px dashed #f3eded}
    .item .it-name{font-weight:600}
    .checkout{padding:12px;border-top:1px solid #f2f2f2}
    .btn-primary{background:var(--accent);color:#fff;padding:10px;border-radius:10px;border:none;width:100%;cursor:pointer}
    /* responsive tweaks */
    @media (max-width:720px){
      .hero{flex-direction:column;align-items:flex-start}
      header{padding:12px}
      .brand .logo{width:48px;height:48px}
      .cart{width:100%;}
    }
  </style>
</head>
<body>
  <header>
    <div class="brand">
      <div class="logo">PB</div>
      <div>
        <div style="font-weight:800">ريستورو بيدزا</div>
        <div style="font-size:12px;color:var(--muted)">بيتزا طازجة — توصيل سريع</div>
      </div>
    </div>
    <nav>
      <a href="#menu">القائمة</a>
      <a href="#about">من نحن</a>
      <a href="#contact">اتصل</a>
    </nav>
  </header>

  <section class="hero">
    <div class="hero-left">
      <h1 class="title">بيتزا إيطالية بطبقات مغرية</h1>
      <p class="subtitle">حضرنا وصفات بيتزا أصلية بمكونات طازجة — جرب المارجريتا الكلاسيكية أو بيتزا الشيف الخاصة.</p>
      <button class="cta" onclick="scrollToMenu()">اطّلع على القائمة</button>
    </div>
    <div class="hero-right">
      <img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='240' height='160'><rect width='100%' height='100%' fill='%23fdebe6' rx='12'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-size='16' fill='%23d84315'>صورة بيتزا</text></svg>" alt="pizza" style="border-radius:12px;">
    </div>
  </section>

  <main class="menu" id="menu">
    <h2 style="margin:6px 0 14px">قائمة البيتزا</h2>
    <div class="grid" id="products"></div>
  </main>

  <button class="cart-toggle" onclick="toggleCart()">عربة (<span id="count">0</span>)</button>
  <aside class="cart" id="cart">
    <div class="head">عربة الطلب</div>
    <div class="items" id="cart-items"></div>
    <div class="checkout">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px"><div>المجموع</div><div id="total">0 د.م</div></div>
      <button class="btn-primary" onclick="checkout()">أطلب الآن</button>
    </div>
  </aside>

  <script>
    // بيانات مبدئية للبيتزا
    const products = [
      {id:1,name:'مارغريتا',desc:'جبنة موزاريلا، صلصة طماطم، ريحان طازج',price:48,img:'https://images.unsplash.com/photo-1601924582975-9b6c1f8f1f7d?auto=format&fit=crop&w=800&q=60'},
      {id:2,name:'بيبروني سبيشيال',desc:'شرائح بيبروني حارة، جبنة',price:59,img:'https://images.unsplash.com/photo-1548365328-9f3413b208a1?auto=format&fit=crop&w=800&q=60'},
      {id:3,name:'أربياتا تشيكن',desc:'دجاج مشوي، صلصة حارة، بصل',price:65,img:'https://images.unsplash.com/photo-1607274462830-1a9f7bdf3f7f?auto=format&fit=crop&w=800&q=60'},
      {id:4,name:'فورسيزوني',desc:'خضروات موسمية، زيتون، مشروم',price:52,img:'https://images.unsplash.com/photo-1571091718767-18a5f6e0f0e8?auto=format&fit=crop&w=800&q=60'},
    ];

    const cart = [];

    function formatPrice(n){ return n + ' د.م' }

    function renderProducts(){
      const container = document.getElementById('products');
      container.innerHTML = '';
      products.forEach(p=>{
        const c = document.createElement('div'); c.className='card';
        c.innerHTML = `
          <div class="thumb" style="background-image:url(${p.img})"></div>
          <div class="name">${p.name}</div>
          <div class="desc">${p.desc}</div>
          <div class="row">
            <div class="price">${formatPrice(p.price)}</div>
            <button class="add" onclick="addToCart(${p.id})">أضف</button>
          </div>`;
        container.appendChild(c);
      });
    }

    function addToCart(id){
      const prod = products.find(p=>p.id===id);
      const existing = cart.find(i=>i.id===id);
      if(existing){ existing.qty += 1; }
      else cart.push({id:prod.id,name:prod.name,price:prod.price,qty:1});
      updateCartUI();
    }

    function updateCartUI(){
      const items = document.getElementById('cart-items');
      items.innerHTML = '';
      let total=0, count=0;
      cart.forEach(it=>{
        total += it.price*it.qty; count += it.qty;
        const div = document.createElement('div'); div.className='item';
        div.innerHTML = `<div style="flex:1"><div class="it-name">${it.name}</div><div style='font-size:13px;color:var(--muted)'>${it.qty} × ${formatPrice(it.price)}</div></div><div style='display:flex;flex-direction:column;gap:6px;align-items:flex-end'><button onclick='inc(${it.id})' style='padding:6px;border-radius:8px;border:none;background:#f3f3f3;cursor:pointer'>+</button><button onclick='dec(${it.id})' style='padding:6px;border-radius:8px;border:none;background:#f3f3f3;cursor:pointer'>−</button></div>`;
        items.appendChild(div);
      });
      document.getElementById('total').innerText = formatPrice(total);
      document.getElementById('count').innerText = count;
    }

    function inc(id){ const it = cart.find(x=>x.id===id); if(it){ it.qty++; updateCartUI(); } }
    function dec(id){ const idx=cart.findIndex(x=>x.id===id); if(idx>-1){ cart[idx].qty--; if(cart[idx].qty<=0) cart.splice(idx,1); updateCartUI(); } }

    function toggleCart(){ document.getElementById('cart').classList.toggle('open'); }

    function checkout(){
      if(cart.length===0){ alert('العربة فارغة — أضف بيتزا أولاً.'); return; }
      const name = prompt('اسمك؟'); if(!name) return;
      const phone = prompt('رقم الهاتف للتواصل؟'); if(!phone) return;
      const addr = prompt('عنوان التوصيل (شارع/مدينة)'); if(!addr) return;
      // محاكاة إرسال الطلب
      const order = {customer:name,phone,addr,items:cart.slice(),total:cart.reduce((s,i)=>s+i.price*i.qty,0)};
      console.log('ORDER', order);
      alert('شكراً! تم تسجيل الطلب — سنتصل بك للتأكيد.\nمجموع الطلب: ' + formatPrice(order.total));
      // تفريغ العربة
      cart.length = 0; updateCartUI(); toggleCart();
    }

    function scrollToMenu(){ document.getElementById('menu').scrollIntoView({behavior:'smooth'}); }

    // تحميل المبدئي
    renderProducts();
    updateCartUI();
  </script>
</body>
</html>
