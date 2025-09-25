<!doctype html>
<html lang="az">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Car Parking Multiplayer - Avtomobil Satışı</title>
<style>
:root{
  --bg:#0f1724; --card:#0b1220; --accent:#f97316; --muted:#9aa4b2; --glass: rgba(255,255,255,0.04);
  --radius:14px; --max-width:1100px;
  --shadow: 0 6px 18px rgba(2,6,23,0.6);
}
*{box-sizing:border-box}
body{
  margin:0; font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  background: linear-gradient(180deg,#071029 0%, #071733 100%); color:#e6eef6; -webkit-font-smoothing:antialiased;
  padding:30px; display:flex; justify-content:center;
}
.container{width:100%; max-width:var(--max-width)}
header{display:flex; align-items:center; justify-content:space-between; gap:20px; margin-bottom:28px}
.brand{display:flex; align-items:center; gap:14px}
.logo{width:52px; height:52px; border-radius:12px; background:linear-gradient(135deg,var(--accent),#ef4444); display:flex; align-items:center; justify-content:center; font-weight:700; color:white; box-shadow:var(--shadow)}
h1{font-size:20px; margin:0}
.subtitle{color:var(--muted); font-size:13px}
.search-row{display:flex; gap:12px; align-items:center}
.search{background:var(--card); border-radius:12px; padding:8px 12px; display:flex; gap:8px; align-items:center; box-shadow:var(--shadow)}
.search input{background:transparent; border:0; outline:0; color:inherit; font-size:14px}
.btn{background:var(--accent); color:#08101a; padding:9px 14px; border-radius:10px; font-weight:600; border:0; cursor:pointer; transition: all .2s ease}
.btn:hover{background:#fb923c; transform:scale(1.05)}
main{display:grid; grid-template-columns:1fr 320px; gap:22px}
.list{background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent); padding:18px; border-radius:16px}
.filters{background:var(--card); padding:16px; border-radius:16px; box-shadow:var(--shadow)}
.filters h3{margin:0 0 12px 0}
.filters label{display:block; font-size:13px; color:var(--muted); margin-bottom:8px}
.filters select, .filters input{width:100%; padding:10px; border-radius:10px; border:0; background:var(--glass); color:inherit}
.grid{display:grid; gap:16px; grid-template-columns:repeat(auto-fill, minmax(240px,1fr)); margin-top:12px}
.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent); border-radius:12px; overflow:hidden; box-shadow:var(--shadow); transition:transform .18s ease, box-shadow .18s ease; position:relative}
.card:hover{transform:translateY(-6px); box-shadow:0 10px 25px rgba(0,0,0,0.5)}
.thumb{height:150px; width:100%; object-fit:cover; display:block; background:#1e2a44; display:flex; align-items:center; justify-content:center; color:#9aa4b2; font-size:14px;}
.card-body{padding:12px}
.car-title{display:flex; justify-content:space-between; align-items:center; gap:8px}
.car-title h4{margin:0; font-size:16px}
.price{background:rgba(255,255,255,0.04); padding:6px 10px; border-radius:10px; font-weight:700}
.meta{color:var(--muted); font-size:13px; margin-top:8px}
.contact{margin-top:6px; font-size:13px; color:#93c5fd}
.cta-row{display:flex; justify-content:space-between; gap:10px; margin-top:12px}
.btn-outline{background:transparent; border:1px solid rgba(255,255,255,0.06); padding:8px 10px; border-radius:10px; color:var(--muted); cursor:pointer; transition: all .2s ease}
.btn-outline:hover{background:var(--glass); color:white}
.sidebar-card{padding:16px; border-radius:12px}
.stats{display:flex; gap:10px; margin-top:12px}
.stat{flex:1; background:var(--glass); padding:12px; border-radius:10px; text-align:center}
.stat b{display:block; font-size:18px}
footer{margin-top:18px; color:var(--muted); font-size:13px; text-align:center}
.fade-in{opacity:0; transform:translateY(10px); transition:all .6s ease}
.fade-in.visible{opacity:1; transform:translateY(0)}
@media (max-width:900px){
  main{grid-template-columns:1fr}
  .search-row{flex-direction:column-reverse; align-items:stretch}
  aside.filters{order:-1; margin-bottom:20px}
}
</style>
</head>
<body>
<div class="container">
<header>
  <div class="brand">
    <div class="logo">CP</div>
    <div>
      <h1>Car Parking Multiplayer — Avtomobil Satışı</h1>
      <div class="subtitle">Sadə HTML, CSS & JS ilə təqdimat səhifəsi</div>
    </div>
  </div>
  <div class="search-row">
    <div class="search">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" aria-hidden>
        <path d="M21 21l-4.35-4.35" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"></path>
        <circle cx="11" cy="11" r="6" stroke="currentColor" stroke-width="1.5"></circle>
      </svg>
      <input id="searchInput" placeholder="Avtomobil, marka, il axtar..." />
    </div>
    <button class="btn" id="newAdBtn">Yeni elan</button>
  </div>
</header>

<main>
<section class="list">
  <div style="display:flex; align-items:center; justify-content:space-between">
    <h2 style="margin:0">Elanlar</h2>
    <div style="color:var(--muted); font-size:13px">Tapılan: <strong id="count">0</strong></div>
  </div>
  <div class="grid" id="carGrid" style="margin-top:14px"></div>
</section>

<aside class="filters">
  <div class="sidebar-card">
    <h3>Filtrlər</h3>
    <label>Marka</label>
    <select id="brandFilter">
      <option value="">Hamısı</option>
      <option>Ronaldo</option>
      <option>Pista</option>
      <option>City</option>
    </select>

    <label style="margin-top:10px">Qiymət (max)</label>
    <input type="range" min="1" max="15" value="15" id="priceFilter">
    <span id="priceValue" style="font-size:13px; color:var(--muted)">15 ₼</span>

    <label style="margin-top:10px">Yük/tam</label>
    <select id="gearFilter">
      <option value="">Hamısı</option>
      <option value="Avtomatik">Cizimli</option>
      <option value="Manual">Normal</option>
    </select>

    <div style="margin-top:14px; display:flex; gap:8px">
      <button class="btn" id="applyFilters">Tətbiq et</button>
      <button class="btn-outline" id="resetFilters">Sıfırla</button>
    </div>

    <div class="stats">
      <div class="stat"><b id="totalAds">0</b><span style="font-size:12px; color:var(--muted)">Elan</span></div>
      <div class="stat"><b id="newAds">0</b><span style="font-size:12px; color:var(--muted)">Yeni</span></div>
    </div>
  </div>
</aside>
</main>

<footer>
  <div>Bu səhifə yalnız təqdimat məqsədi ilədir — Car Parking Multiplayer üçün məzmun.</div>
  <div style="margin-top:10px; display:flex; justify-content:center; align-items:center; flex-direction:column">
    <div id="qrcode"></div>
    <span style="font-size:12px; color:var(--muted); margin-top:4px">Səhifəni mobil ilə açmaq üçün QR kod</span>
  </div>
</footer>
</div>

<script>
const cars = [
  {title:"", price:"", gear:"Avtomatik", brand:"", img:"", phone:""},
  {title:"", price:"", gear:"Manual", brand:"", img:"", phone:""},
  {title:"", price:"", gear:"Avtomatik", brand:"", img:"", phone:""},
  {title:"", price:"", gear:"Manual", brand:"", img:"", phone:""}
];

const carGrid = document.getElementById("carGrid");
const countEl = document.getElementById("count");
const totalAdsEl = document.getElementById("totalAds");
const newAdsEl = document.getElementById("newAds");
const searchInput = document.getElementById("searchInput");
const brandFilter = document.getElementById("brandFilter");
const priceFilter = document.getElementById("priceFilter");
const priceValue = document.getElementById("priceValue");
const gearFilter = document.getElementById("gearFilter");

function getGearLabel(gear){
  if(gear === "Avtomatik") return "Cizimli";
  if(gear === "Manual") return "Normal";
  return gear;
}

function renderCars(list){
  carGrid.innerHTML="";
  list.forEach((car,i)=>{
    const card=document.createElement("article");
    card.className="card fade-in";
    card.innerHTML=`
      <div class="thumb">Şəkil əlavə edin</div>
      <div class="card-body">
        <div class="car-title">
          <h4>${car.title || "Başlıq yoxdur"}</h4>
          <div class="price">${car.price ? car.price+" ₼" : "Qiymət yoxdur"}</div>
        </div>
        <div class="meta">Gear: ${getGearLabel(car.gear)}</div>
        <div class="contact">Əlaqə: ${car.phone || "Telefon yoxdur"}</div>
        <div class="cta-row">
          <button class="btn-outline">Ətraflı</button>
          <button class="btn">Sifariş et</button>
        </div>
      </div>`;
    carGrid.appendChild(card);
    setTimeout(()=>card.classList.add("visible"),150*i);
  });
  countEl.textContent=list.length;
  totalAdsEl.textContent=cars.length;
  newAdsEl.textContent=list.filter(c=>c.gear==="Avtomatik").length;
}

function applyFilters(){
  const filtered = cars.filter(car=>{
    const search = searchInput.value.toLowerCase();
    if(search && !(car.title.toLowerCase().includes(search) || car.brand.toLowerCase().includes(search))) return false;
    if(brandFilter.value && car.brand!==brandFilter.value) return false;
    if(gearFilter.value && car.gear!==gearFilter.value) return false;
    if(car.price && car.price > parseInt(priceFilter.value)) return false;
    return true;
  });
  renderCars(filtered);
}

searchInput.addEventListener("input",applyFilters);
brandFilter.addEventListener("change",applyFilters);
gearFilter.addEventListener("change",applyFilters);
priceFilter.addEventListener("input",()=>{
  priceValue.textContent = priceFilter.value + " ₼";
  applyFilters();
});
document.getElementById("applyFilters").addEventListener("click",applyFilters);
document.getElementById("resetFilters").addEventListener("click",()=>{
  searchInput.value="";
  brandFilter.value="";
  gearFilter.value="";
  priceFilter.value=15;
  priceValue.textContent="15 ₼";
  renderCars(cars);
});

renderCars(cars);
</script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script>
window.addEventListener("load", function() {
  var qrContainer = document.getElementById("qrcode");
  if(qrContainer) {
    new QRCode(qrContainer, {
      text: window.location.href,
      width: 120,
      height: 120,
      colorDark : "#000000",
      colorLight : "#ffffff",
      correctLevel : QRCode.CorrectLevel.H
    });
  }
});
</script>
</body>
</html>
