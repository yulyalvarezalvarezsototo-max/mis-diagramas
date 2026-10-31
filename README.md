# mis<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>PixelVault Games</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  body{background:#0b0d17;background-image:radial-gradient(circle at 20% 0,#1a1040 0,transparent 40%),radial-gradient(circle at 90% 10%,#06303a 0,transparent 35%)}
  .glow{box-shadow:0 0 18px rgba(139,92,246,.35)}
  .card{transition:.2s}.card:hover{transform:translateY(-3px);box-shadow:0 0 22px rgba(34,211,238,.35)}
  input,select{color:#e2e8f0}
</style>
</head>
<body class="text-slate-200 min-h-screen font-sans">
<header class="sticky top-0 z-10 backdrop-blur bg-[#0b0d17]/80 border-b border-violet-500/30">
  <div class="max-w-6xl mx-auto px-4 py-3 flex flex-wrap items-center gap-3 justify-between">
    <h1 class="text-2xl font-black tracking-widest bg-gradient-to-r from-violet-400 to-cyan-300 bg-clip-text text-transparent">PIXELVAULT</h1>
    <nav class="flex gap-2">
      <button data-tab="catalogo" class="tab px-4 py-2 rounded-lg font-semibold">🎮 Catálogo</button>
      <button data-tab="arriendos" class="tab px-4 py-2 rounded-lg font-semibold">⏱ Arriendos</button>
    </nav>
    <div class="flex items-center gap-2">
      <span id="pts" class="text-sm text-yellow-300 hidden"></span>
      <button id="vip" class="px-4 py-2 rounded-full border border-yellow-400/50 text-yellow-300 font-bold text-sm"></button>
    </div>
  </div>
</header>

<main class="max-w-6xl mx-auto px-4 py-6 grid lg:grid-cols-3 gap-6">
  <section id="catalogo" class="lg:col-span-2">
    <div class="grid sm:grid-cols-3 gap-2 mb-2">
      <input id="q" placeholder="Buscar juego..." class="sm:col-span-3 px-4 py-3 rounded-xl bg-slate-900 border border-slate-700 focus:border-cyan-400 outline-none">
      <select id="fPlat" class="p-2 rounded-lg bg-slate-900 border border-slate-700"><option value="">Todas las plataformas</option><option>PS5</option><option>Xbox</option><option>Switch</option><option>PC</option></select>
      <select id="fForm" class="p-2 rounded-lg bg-slate-900 border border-slate-700"><option value="">Físico y Digital</option><option>Físico</option><option>Digital</option></select>
      <label class="flex items-center gap-2 text-sm px-2"><input id="fStock" type="checkbox"> Solo con stock</label>
    </div>
    <p id="count" class="text-xs text-slate-500 mb-3"></p>
    <div id="grid" class="grid sm:grid-cols-2 gap-4"></div>
  </section>

  <section id="arriendos" class="lg:col-span-2 hidden">
    <h2 class="text-xl font-bold mb-4 text-cyan-300">Arriendos y Multas <span class="text-xs text-slate-500">(solo juegos físicos)</span></h2>
    <div class="bg-slate-900/80 border border-slate-700 rounded-2xl p-5 space-y-4">
      <div class="grid sm:grid-cols-3 gap-4">
        <label class="block sm:col-span-3 text-sm">Juego<select id="rJuego" class="mt-1 w-full p-2 rounded-lg bg-slate-800 border border-slate-700"></select></label>
        <label class="block text-sm">Días de arriendo<input id="rDias" type="number" min="1" value="3" class="mt-1 w-full p-2 rounded-lg bg-slate-800 border border-slate-700"></label>
        <label class="block text-sm">Días reales hasta devolución<input id="rReal" type="number" min="1" value="3" class="mt-1 w-full p-2 rounded-lg bg-slate-800 border border-slate-700"></label>
        <div class="text-sm text-slate-400 self-end pb-2">Mora: $1.500/día de atraso</div>
      </div>
      <div id="rResumen" class="rounded-xl bg-slate-950 p-4 space-y-1 text-sm"></div>
      <button id="rBtn" class="px-5 py-2 rounded-lg bg-gradient-to-r from-violet-600 to-cyan-500 font-bold glow">Registrar arriendo</button>
      <ul id="rLista" class="text-sm space-y-2"></ul>
    </div>
  </section>

  <aside class="bg-slate-900/80 border border-violet-500/30 rounded-2xl p-5 h-fit lg:sticky lg:top-20 glow">
    <h2 class="text-xl font-bold mb-3">🛒 Carrito</h2>
    <ul id="cart" class="space-y-2 text-sm mb-3"></ul>
    <div class="flex gap-2 mb-3">
      <input id="cupon" placeholder="Cupón (PIXEL10, VAULT5000)" class="flex-1 min-w-0 px-2 py-1 rounded bg-slate-800 border border-slate-700 text-sm uppercase">
      <button id="cuponBtn" class="px-3 rounded bg-slate-700 hover:bg-slate-600 text-sm">Aplicar</button>
    </div>
    <div class="border-t border-slate-700 pt-3 space-y-1 text-sm">
      <div class="flex justify-between"><span>Subtotal</span><span id="sub"></span></div>
      <div class="flex justify-between text-emerald-400"><span id="cLbl">Cupón</span><span id="desc"></span></div>
      <div class="flex justify-between"><span>IVA (19%)</span><span id="iva"></span></div>
      <div class="flex justify-between text-lg font-black text-cyan-300"><span>Total</span><span id="tot"></span></div>
    </div>
    <button id="pay" class="mt-4 w-full py-2 rounded-lg bg-emerald-600 hover:bg-emerald-500 font-bold">Ir a checkout</button>
  </aside>
</main>

<footer class="text-center text-xs text-slate-600 pb-6">Arquitectura: Frontend Web → API Gateway → Microservicios (Catálogo · Ventas · Arriendos · Fidelización) · última respuesta del gateway: <span id="lat">–</span> ms</footer>

<div id="modal" class="fixed inset-0 bg-black/70 hidden items-center justify-center p-4 z-20"><div id="mBody" class="bg-slate-900 border border-violet-500/40 rounded-2xl p-6 w-full max-w-md glow"></div></div>
<div id="toast" class="fixed bottom-5 left-1/2 -translate-x-1/2 px-5 py-3 rounded-xl bg-slate-800 border border-cyan-400 hidden z-30"></div>

<script>
/* ===== Utilidades ===== */
const $ = s => document.querySelector(s);
const clp = n => "$" + Math.round(n).toLocaleString("es-CL");
const toast = m => {const t=$("#toast");t.textContent=m;t.classList.remove("hidden");clearTimeout(t._h);t._h=setTimeout(()=>t.classList.add("hidden"),2400)};

/* ===== API Gateway (enruta a microservicios desacoplados) ===== */
const Gateway = {
  svc:{}, ms:0,
  register(name, service){ this.svc[name]=service },
  call(name, method, ...args){
    const t=performance.now(); const r=this.svc[name][method](...args);
    this.ms=performance.now()-t; $("#lat").textContent=this.ms.toFixed(2); return r;
  }
};

/* ===== Microservicio: Catálogo e Inventario (RF1) ===== */
Gateway.register("catalogo", (()=>{
  const items=[
    {id:1,nombre:"Elden Ring",plat:"PS5",formato:"Físico",precio:39990,arriendo:3500,stock:5},
    {id:2,nombre:"Elden Ring",plat:"PS5",formato:"Digital",precio:36990,arriendo:0,stock:20},
    {id:3,nombre:"Zelda: Tears of the Kingdom",plat:"Switch",formato:"Físico",precio:54990,arriendo:4000,stock:3},
    {id:4,nombre:"Halo Infinite",plat:"Xbox",formato:"Digital",precio:29990,arriendo:0,stock:12},
    {id:5,nombre:"Halo Infinite",plat:"Xbox",formato:"Físico",precio:32990,arriendo:3000,stock:6},
    {id:6,nombre:"Cyberpunk 2077",plat:"PC",formato:"Digital",precio:24990,arriendo:0,stock:15},
    {id:7,nombre:"God of War Ragnarök",plat:"PS5",formato:"Físico",precio:44990,arriendo:3800,stock:2},
    {id:8,nombre:"Forza Horizon 5",plat:"Xbox",formato:"Físico",precio:34990,arriendo:3200,stock:0},
    {id:9,nombre:"Mario Kart 8 Deluxe",plat:"Switch",formato:"Físico",precio:49990,arriendo:3600,stock:7},
    {id:10,nombre:"Baldur's Gate 3",plat:"PC",formato:"Digital",precio:42990,arriendo:0,stock:9},
  ];
  return {
    get:id=>items.find(i=>i.id===id),
    buscar({q="",plat="",formato="",soloStock=false}){
      q=q.toLowerCase();
      return items.filter(i=>i.nombre.toLowerCase().includes(q)&&(!plat||i.plat===plat)&&(!formato||i.formato===formato)&&(!soloStock||i.stock>0));
    },
    rentables:()=>items.filter(i=>i.formato==="Físico"),
    reservar(id,n=1){const i=this.get(id); if(i.stock<n) return false; i.stock-=n; return true},
    liberar(id,n=1){this.get(id).stock+=n}
  };
})());

/* ===== Microservicio: Fidelización VIP (RF4) ===== */
Gateway.register("fidelizacion", {
  cliente:null, // {nombre,email,puntos,activo}
  registrar(nombre,email){this.cliente={nombre,email,puntos:0,activo:true}; return this.cliente},
  toggle(){ if(this.cliente) this.cliente.activo=!this.cliente.activo; return this.cliente },
  esVip(){ return !!(this.cliente&&this.cliente.activo) },
  sumarPuntos(monto){ const p=Math.floor(monto/1000); if(this.cliente) this.cliente.puntos+=p; return this.cliente?p:0 }
});

/* ===== Microservicio: Ventas (RF2) ===== */
Gateway.register("ventas", {
  carrito:{}, cupon:null,
  cupones:{PIXEL10:{tipo:"pct",valor:.10,label:"PIXEL10 (-10%)"},VAULT5000:{tipo:"fijo",valor:5000,label:"VAULT5000 (-$5.000)"}},
  agregar(id){ if(!Gateway.call("catalogo","reservar",id)) return false; this.carrito[id]=(this.carrito[id]||0)+1; return true },
  quitar(id){ Gateway.call("catalogo","liberar",id); if(--this.carrito[id]<=0) delete this.carrito[id] },
  aplicarCupon(code){ const c=this.cupones[code.trim().toUpperCase()]; this.cupon=c||null; return !!c },
  totales(){
    let sub=0; for(const id in this.carrito) sub+=Gateway.call("catalogo","get",+id).precio*this.carrito[id];
    let desc=0; if(this.cupon) desc=Math.min(sub,this.cupon.tipo==="pct"?sub*this.cupon.valor:this.cupon.valor);
    const base=sub-desc, iva=base*0.19; return {sub,desc,iva,total:base+iva};
  },
  checkout(){ const t=this.totales(); const pts=Gateway.call("fidelizacion","sumarPuntos",t.total); this.carrito={}; this.cupon=null; return {...t,pts} }
});

/* ===== Microservicio: Arriendos y Multas (RF3) ===== */
Gateway.register("arriendos", {
  MORA:1500, DESC_VIP:0.10, historial:[],
  calcular(id,dias,real){
    const j=Gateway.call("catalogo","get",id); if(!j) return null;
    const vip=Gateway.call("fidelizacion","esVip");
    const base=j.arriendo*dias, desc=vip?base*this.DESC_VIP:0, atraso=Math.max(0,real-dias), multa=atraso*this.MORA;
    return {j,dias,base,desc,atraso,multa,vip,total:base-desc+multa};
  },
  registrar(id,dias,real){
    const a=this.calcular(id,dias,real); if(!a||!Gateway.call("catalogo","reservar",id)) return null;
    a.pts=Gateway.call("fidelizacion","sumarPuntos",a.total); this.historial.unshift(a); return a;
  }
});

/* ===== UI ===== */
const G=(s,m,...a)=>Gateway.call(s,m,...a);
const colores={PS5:"bg-blue-600",Xbox:"bg-green-600",Switch:"bg-red-600",PC:"bg-slate-600"};

function renderCatalogo(){
  const lista=G("catalogo","buscar",{q:$("#q").value,plat:$("#fPlat").value,formato:$("#fForm").value,soloStock:$("#fStock").checked});
  $("#count").textContent=`${lista.length} producto(s)`;
  $("#grid").innerHTML=lista.map(j=>`
    <div class="card bg-slate-900 border border-slate-700 rounded-2xl p-4">
      <div class="flex justify-between items-start gap-2">
        <h3 class="font-bold leading-tight">${j.nombre}</h3>
        <span class="text-xs px-2 py-1 rounded ${colores[j.plat]}">${j.plat}</span>
      </div>
      <span class="inline-block mt-2 text-xs px-2 py-0.5 rounded border ${j.formato==="Físico"?"border-amber-400 text-amber-300":"border-cyan-400 text-cyan-300"}">${j.formato}</span>
      <p class="text-2xl font-black text-cyan-300 mt-2">${clp(j.precio)}</p>
      <p class="text-sm mt-1 ${j.stock===0?"text-red-400":j.stock<=2?"text-amber-400":"text-slate-400"}">${j.stock===0?"Agotado":j.stock<=2?`¡Últimas ${j.stock} unidades!`:`Stock: ${j.stock}`}</p>
      <button ${j.stock?"":"disabled"} onclick="agregar(${j.id})" class="mt-3 w-full py-2 rounded-lg bg-violet-600 hover:bg-violet-500 font-semibold disabled:opacity-40 disabled:cursor-not-allowed">Agregar al carrito</button>
    </div>`).join("")||`<p class="text-slate-400">Sin resultados.</p>`;
}
function agregar(id){ if(!G("ventas","agregar",id)) toast("Sin stock"); render() }
function quitar(id){ G("ventas","quitar",id); render() }

function renderCarrito(){
  const c=Gateway.svc.ventas.carrito, ids=Object.keys(c);
  $("#cart").innerHTML=ids.length?ids.map(id=>{const j=G("catalogo","get",+id);
    return `<li class="flex justify-between items-center gap-2"><span>${j.nombre} <span class="text-slate-500">${j.plat}·${j.formato[0]} ×${c[id]}</span></span>
    <span class="flex items-center gap-2">${clp(j.precio*c[id])}<button onclick="quitar(${id})" class="px-2 rounded bg-red-600/70">−</button></span></li>`}).join(""):`<li class="text-slate-500">Tu carrito está vacío.</li>`;
  const t=G("ventas","totales"), cu=Gateway.svc.ventas.cupon;
  $("#sub").textContent=clp(t.sub); $("#cLbl").textContent=cu?cu.label:"Cupón"; $("#desc").textContent="−"+clp(t.desc);
  $("#iva").textContent=clp(t.iva); $("#tot").textContent=clp(t.total);
}
$("#cuponBtn").onclick=()=>{ toast(G("ventas","aplicarCupon",$("#cupon").value)?"Cupón aplicado ✔":"Cupón inválido"); render() };

/* Checkout interactivo */
function modal(html){ $("#mBody").innerHTML=html; $("#modal").classList.replace("hidden","flex") }
function cerrar(){ $("#modal").classList.replace("flex","hidden") }
$("#pay").onclick=()=>{
  const t=G("ventas","totales"); if(!t.sub) return toast("El carrito está vacío");
  const cl=Gateway.svc.fidelizacion.cliente;
  modal(`<h3 class="text-xl font-bold mb-3">Checkout</h3>
    <div class="space-y-2 text-sm">
      <input id="coN" placeholder="Nombre" value="${cl?cl.nombre:""}" class="w-full p-2 rounded bg-slate-800 border border-slate-700">
      <input id="coE" placeholder="Email" value="${cl?cl.email:""}" class="w-full p-2 rounded bg-slate-800 border border-slate-700">
      <select id="coP" class="w-full p-2 rounded bg-slate-800 border border-slate-700"><option>Tarjeta de crédito</option><option>Débito</option><option>Transferencia</option></select>
      <p class="text-right text-lg font-black text-cyan-300">Total: ${clp(t.total)}</p>
    </div>
    <div class="flex gap-2 mt-4"><button onclick="cerrar()" class="flex-1 py-2 rounded bg-slate-700">Cancelar</button><button onclick="confirmarPago()" class="flex-1 py-2 rounded bg-emerald-600 font-bold">Confirmar pago</button></div>`);
};
function confirmarPago(){
  const n=$("#coN").value.trim(), e=$("#coE").value.trim();
  if(!n||!/\S+@\S+\.\S+/.test(e)) return toast("Ingresa nombre y email válidos");
  const r=G("ventas","checkout");
  modal(`<h3 class="text-xl font-bold text-emerald-400 mb-2">¡Compra exitosa! 🎉</h3>
    <p class="text-sm text-slate-400 mb-3">Gracias, ${n}. Boleta enviada a ${e}.</p>
    <div class="text-sm space-y-1"><div class="flex justify-between"><span>Subtotal</span><span>${clp(r.sub)}</span></div>
    <div class="flex justify-between"><span>Descuento</span><span>−${clp(r.desc)}</span></div>
    <div class="flex justify-between"><span>IVA 19%</span><span>${clp(r.iva)}</span></div>
    <div class="flex justify-between font-black text-cyan-300"><span>Total</span><span>${clp(r.total)}</span></div></div>
    ${r.pts?`<p class="mt-3 text-yellow-300 text-sm">+${r.pts} puntos VIP acumulados ★</p>`:""}
    <button onclick="cerrar()" class="mt-4 w-full py-2 rounded bg-violet-600">Cerrar</button>`);
  render();
}

/* Arriendos */
function renderArriendo(){
  const sel=$("#rJuego"), cur=sel.value;
  sel.innerHTML=G("catalogo","rentables").map(j=>`<option value="${j.id}" ${j.stock?"":"disabled"}>${j.nombre} (${j.plat}) — ${clp(j.arriendo)}/día — stock ${j.stock}</option>`).join("");
  if(cur) sel.value=cur;
  const a=G("arriendos","calcular",+sel.value,Math.max(1,+$("#rDias").value||1),Math.max(1,+$("#rReal").value||1));
  if(a) $("#rResumen").innerHTML=`
    <div class="flex justify-between"><span>Arriendo (${a.dias} días)</span><span>${clp(a.base)}</span></div>
    <div class="flex justify-between text-yellow-300"><span>Descuento VIP 10%${a.vip?"":" (inactivo)"}</span><span>−${clp(a.desc)}</span></div>
    <div class="flex justify-between ${a.atraso?"text-red-400":"text-slate-500"}"><span>Mora (${a.atraso} día/s × $1.500)</span><span>${clp(a.multa)}</span></div>
    <div class="flex justify-between text-lg font-black text-cyan-300 border-t border-slate-700 pt-2"><span>Total</span><span>${clp(a.total)}</span></div>`;
  $("#rLista").innerHTML=Gateway.svc.arriendos.historial.map(r=>`<li class="p-2 rounded bg-slate-800">${r.j.nombre} · ${r.dias} días${r.vip?" · VIP":""}${r.multa?` · mora ${clp(r.multa)}`:""} — <b>${clp(r.total)}</b></li>`).join("");
}
["rJuego","rDias","rReal"].forEach(id=>$("#"+id).addEventListener("input",renderArriendo));
$("#rBtn").onclick=()=>{
  const r=G("arriendos","registrar",+$("#rJuego").value,Math.max(1,+$("#rDias").value||1),Math.max(1,+$("#rReal").value||1));
  toast(r?`Arriendo registrado${r.pts?` · +${r.pts} pts`:""}`:"Sin stock disponible"); render();
};

/* VIP: registro + toggle */
function renderVip(){
  const f=Gateway.svc.fidelizacion, c=f.cliente, b=$("#vip");
  if(!c){ b.textContent="☆ Registrarse como VIP"; b.className="px-4 py-2 rounded-full border border-yellow-400/50 text-yellow-300 font-bold text-sm"; $("#pts").classList.add("hidden"); return }
  b.textContent=(c.activo?"★":"☆")+" Modo Cliente VIP: "+(c.activo?"ON":"OFF");
  b.className="px-4 py-2 rounded-full border font-bold text-sm "+(c.activo?"bg-yellow-400 text-black border-yellow-400 glow":"border-yellow-400/50 text-yellow-300");
  $("#pts").textContent=`${c.nombre} · ${c.puntos} pts`; $("#pts").classList.remove("hidden");
}
$("#vip").onclick=()=>{
  if(Gateway.svc.fidelizacion.cliente){ G("fidelizacion","toggle"); return render() }
  modal(`<h3 class="text-xl font-bold text-yellow-300 mb-3">Registro Cliente VIP</h3>
    <p class="text-sm text-slate-400 mb-3">10% de descuento automático en arriendos y 1 punto por cada $1.000 gastados.</p>
    <input id="vN" placeholder="Nombre" class="w-full p-2 mb-2 rounded bg-slate-800 border border-slate-700">
    <input id="vE" placeholder="Email" class="w-full p-2 rounded bg-slate-800 border border-slate-700">
    <div class="flex gap-2 mt-4"><button onclick="cerrar()" class="flex-1 py-2 rounded bg-slate-700">Cancelar</button><button onclick="regVip()" class="flex-1 py-2 rounded bg-yellow-400 text-black font-bold">Registrar</button></div>`);
};
function regVip(){
  const n=$("#vN").value.trim(), e=$("#vE").value.trim();
  if(!n||!/\S+@\S+\.\S+/.test(e)) return toast("Nombre y email válidos, por favor");
  G("fidelizacion","registrar",n,e); cerrar(); toast("¡Bienvenido al club VIP! ★"); render();
}

/* Tabs y filtros */
function tab(t){
  ["catalogo","arriendos"].forEach(s=>$("#"+s).classList.toggle("hidden",s!==t));
  document.querySelectorAll(".tab").forEach(b=>b.className="tab px-4 py-2 rounded-lg font-semibold "+(b.dataset.tab===t?"bg-violet-600":"bg-slate-800 hover:bg-slate-700"));
}
document.querySelectorAll(".tab").forEach(b=>b.onclick=()=>tab(b.dataset.tab));
["q","fPlat","fForm","fStock"].forEach(id=>$("#"+id).addEventListener("input",renderCatalogo));
$("#modal").addEventListener("click",e=>{if(e.target.id==="modal")cerrar()});

function render(){renderCatalogo();renderCarrito();renderArriendo();renderVip()}
tab("catalogo"); render();
</script>
</body>
</html>-diagramas
