[index.html](https://github.com/user-attachments/files/22134321/index.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Fiche Brocante / Bourse</title>

<!-- Leaflet (carte) -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<style>
  :root{
    --bg:#f6f8fb; --ink:#1f2937; --card:#ffffff; --line:#e5e7eb;
    --accent:#2ecc71; --accent-2:#2a74ff; --pill:#18b368;
    --gold:#f4b84a; --warn:#e91e63;
    --day: 32px;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;background:var(--bg);color:var(--ink)}
  .wrap{max-width:1160px;margin:auto;padding:22px}
  .head{display:flex;align-items:center;gap:12px;margin-bottom:12px}
  .head h1{margin:0;font-size:1.4rem}
  .grid{display:grid;grid-template-columns:1.15fr 1fr 0.9fr;gap:18px}
  @media (max-width:1000px){.grid{grid-template-columns:1fr}}
  .card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:14px}

  /* AFFICHE */
  .media .frame{width:100%;height:520px;border:1px solid var(--line);border-radius:10px;background:#fafafa;overflow:hidden}
  .media .frame embed,.media .frame img{width:100%;height:100%;border:0;object-fit:cover;border-radius:10px}

  /* INFOS MILIEU */
  .section{padding:8px 0 10px 0}
  .titleSec{font-weight:700;margin:0 0 10px 0;padding-bottom:8px;border-bottom:1px solid var(--line)}
  .v{line-height:1.55}
  .dateLink{color:var(--gold);text-decoration:none;font-weight:400}

  /* ORGANISATEUR */
  .org{display:flex;flex-direction:column;gap:12px}
  .orgCard{border:1px solid var(--line);border-radius:14px;padding:14px}
  .orgCard .legend{font-weight:700;color:#6b7280;margin-bottom:6px}
  .orgCard h3{margin:0}
  .actions{display:grid;gap:10px;margin-top:12px}
  .btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;padding:10px 14px;border-radius:999px;border:0;background:var(--pill);color:#fff;text-decoration:none;font-weight:600;cursor:pointer}
  .btn.secondary{background:#14a364}
  .btn.replaced{background:#e2e8f0;color:#111;border:1px solid #cfd6e4}

  /* DESCRIPTION */
  .desc h3{margin:0 0 10px 0}
  .desc .box{border:1px solid var(--line);border-radius:10px;padding:12px;background:#fff}

  /* CALENDRIER */
  .calWrap{display:grid;grid-template-columns:1.05fr 1fr;gap:18px}
  @media (max-width:900px){.calWrap{grid-template-columns:1fr}}
  .cal{border:1px solid var(--line);border-radius:12px;padding:10px;display:flex;flex-direction:column;align-items:center}
  .calHead{position:relative;width:100%;display:flex;align-items:center;justify-content:center;margin-bottom:6px}
  .calTitle{font-weight:700;color:#6b7280;text-align:center}
  .navBtn{position:absolute;top:0;bottom:0;margin:auto 0;height:28px;background:#fff;border:1px solid var(--line);border-radius:8px;padding:4px 8px;cursor:pointer}
  .navLeft{left:8px} .navRight{right:8px}
  .dow,.gridDays{width:max-content;margin:0 auto;justify-content:center}
  .dow{display:grid;grid-template-columns:repeat(7,var(--day));gap:4px;margin:4px 0;color:#6b7280;font-weight:600;font-size:.85rem}
  .gridDays{display:grid;grid-template-columns:repeat(7,var(--day));gap:4px}
  .day{width:var(--day);height:var(--day);border:1px solid var(--line);border-radius:999px;display:flex;align-items:center;justify-content:center;background:#fff;cursor:pointer;font-size:.9rem}
  .day:hover{box-shadow:0 0 0 2px #e8eefc inset}
  .muted{color:#9aa2af}
  .hasEvent{background:#fff7e6;border-color:#f2d39a}
  .selected{background:var(--gold);color:#111;border-color:var(--gold)}
  .calInfo{border:1px solid var(--line);border-radius:12px;padding:12px}
  .infoBlock + .infoBlock{margin-top:14px}
  .infoTitle{font-weight:700;margin-bottom:8px;border-bottom:1px solid var(--line);padding-bottom:8px;display:flex;align-items:center;gap:8px}
  .chip{display:inline-flex;align-items:center;gap:8px;color:#c48f00}
  .danger{color:var(--warn);display:flex;align-items:center;gap:8px}

  /* CARTE (sous les dates) */
  .mapCard h3{margin:0 0 10px 0}
  #map{width:100%;height:330px;border-radius:12px;border:1px solid var(--line);overflow:hidden}
  .leaflet-container{border-radius:12px}

  /* ADMIN */
  #toggleAdmin{display:inline-flex;align-items:center;gap:8px;margin-left:auto;background:var(--accent-2);color:#fff;border:none;padding:10px 14px;border-radius:999px;cursor:pointer}
  #login{display:none;background:#fff;border:1px solid var(--line);border-radius:10px;padding:12px;margin:12px 0}
  #panel{display:none;background:#eef4ff;border:1px solid #d6e2ff;border-radius:12px;padding:16px;margin-top:12px}
  #panel h2{margin:0 0 10px 0}
  label{display:block;font-weight:600;color:#374151;margin:8px 0 4px}
  input,textarea{width:100%;padding:9px;border:1px solid #cfd6e4;border-radius:8px}
  .row{display:grid;grid-template-columns:1fr 1fr;gap:10px}
  .row3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px}

  footer{text-align:center;margin-top:18px;color:#6b7280}
</style>
</head>
<body>
<div class="wrap">
  <div class="head">
    <h1 id="titreView">Brocante</h1>
    <button id="toggleAdmin">🔑 Admin</button>
  </div>

  <!-- LOGIN ADMIN -->
  <div id="login">
    <label>Code admin</label>
    <input id="code" type="password" placeholder="Entrez le code" />
    <div style="margin-top:8px;display:flex;gap:8px;align-items:center">
      <button id="okCode" class="btn" style="background:#2ecc71">Valider</button>
      <span id="msg" style="color:#6b7280"></span>
    </div>
  </div>

  <!-- 3 COLONNES (affiche / infos / organisateur) -->
  <div class="grid">
    <section class="card media">
      <div id="mediaContainer" class="frame">
        <div style="height:100%;display:flex;align-items:center;justify-content:center;color:#9aa2af">Aperçu PDF / image</div>
      </div>
    </section>

    <section class="card">
      <div class="section">
        <h3 class="titleSec">Dates</h3>
        <div class="v"><a id="datePrincipale" href="#" class="dateLink">—</a></div>
      </div>
      <div class="section">
        <h3 class="titleSec">Catégorie</h3>
        <div class="v" id="categorieView">—</div>
      </div>
      <div class="section">
        <h3 class="titleSec">Adresse</h3>
        <div class="v" id="adresseView">—</div>
      </div>
    </section>

    <aside class="org">
      <div class="orgCard">
        <div class="legend">Organisateur</div>
        <h3 id="organisateurView">—</h3>
        <div class="actions">
          <a id="btnCall" class="btn" href="#">📞 Appeler</a>
          <a id="btnMail" class="btn" href="#">✉️ Écrire</a>
          <a id="btnAnnounce" class="btn secondary" href="#">📢 Annoncer</a>
        </div>
      </div>
    </aside>
  </div>

  <!-- DESCRIPTION -->
  <section class="card desc" style="margin-top:18px">
    <h3>L’événement :</h3>
    <div id="descriptionView" class="box">Description non renseignée.</div>
  </section>

  <!-- CALENDRIER -->
  <section class="card" style="margin-top:18px">
    <h3 style="margin:0 0 10px 0">Dates & Informations tarifaires :</h3>
    <div class="calWrap">
      <div class="cal">
        <div class="calHead">
          <button id="prevMonth" class="navBtn navLeft">‹</button>
          <div class="calTitle" id="calTitle">—</div>
          <button id="nextMonth" class="navBtn navRight">›</button>
        </div>
        <div class="dow"><div>lu</div><div>ma</div><div>me</div><div>je</div><div>ve</div><div>sa</div><div>di</div></div>
        <div id="calGrid" class="gridDays"></div>
      </div>

      <div class="calInfo">
        <div class="infoBlock">
          <div class="infoTitle"><span class="chip">🕒</span>Horaires :</div>
          <div id="infoDate" style="color:#6b7280;margin-bottom:8px"></div>
          <div id="infoHours" style="font-weight:600"></div>
          <div id="infoWarn" class="danger" style="display:none;margin-top:6px">⚠️ Cet événement n’a pas lieu ce jour-là.</div>
        </div>
        <div class="infoBlock">
          <div class="infoTitle"><span class="chip">ℹ️</span>Tarifs :</div>
          <div id="infoPrice">Gratuit</div>
        </div>
      </div>
    </div>
  </section>

  <!-- CARTE (DÉPLACÉE SOUS LES DATES) -->
  <section class="card mapCard" style="margin-top:18px">
    <h3>Localisation :</h3>
    <div id="map"></div>
  </section>

  <footer id="footerView">© 2025 – Fiche événement</footer>

  <!-- PANNEAU ADMIN -->
  <div id="panel" style="display:none">
    <h2>Éditer la fiche</h2>

    <div class="row">
      <div><label>Titre</label><input id="titre" placeholder="Brocante" /></div>
      <div><label>Organisateur</label><input id="organisateur" placeholder="comite des fetes" /></div>
    </div>

    <div class="row">
      <div><label>Date principale (ex : Le 21 septembre)</label><input id="date1" placeholder="Le 21 septembre" /></div>
      <div><label>Catégorie</label><input id="categorie" placeholder="Brocante" /></div>
    </div>

    <label>Adresse (utilise &lt;br&gt; pour des retours à la ligne)</label>
    <textarea id="adresse" rows="3"></textarea>

    <label>Description (section “L’événement”)</label>
    <textarea id="description" rows="5"></textarea>

    <!-- Coordonnées carte -->
    <div class="row3">
      <div><label>Latitude</label><input id="lat" placeholder="43.296" /></div>
      <div><label>Longitude</label><input id="lng" placeholder="5.369" /></div>
      <div><label>Zoom (2–19)</label><input id="zoom" type="number" min="2" max="19" placeholder="15" /></div>
    </div>

    <div class="row3">
      <div><label>Téléphone (pour “Appeler”)</label><input id="tel" placeholder="+33 6 12 34 56 78" /></div>
      <div><label>Texte “Écrire” (clic)</label><input id="mailReplace" placeholder="Merci de contacter un administrateur du site" /></div>
      <div><label>Texte “Annoncer” (clic)</label><input id="announceReplace" placeholder="J'annonce rien" /></div>
    </div>

    <label>Texte “Appeler” (si différent du numéro)</label>
    <input id="callReplace" placeholder="(laisser vide = affiche le numéro)" />

    <div class="row">
      <div><label>Horaires (ex : 09:00 - 17:00)</label><input id="hours" placeholder="09:00 - 17:00" /></div>
      <div><label>Tarif</label><input id="price" placeholder="Gratuit" /></div>
    </div>

    <label>Dates d’événement (AAAA-MM-JJ, séparées par des virgules)</label>
    <input id="datesList" placeholder="2025-09-21,2025-10-12" />

    <label>Lien “Annoncer” (facultatif)</label>
    <input id="announce" placeholder="https://..." />

    <label>URL média (image ou PDF)</label>
    <input id="mediaUrl" placeholder="https://...pdf ou https://...jpg" />

    <label>OU fichier média local (image / PDF)</label>
    <input id="mediaFile" type="file" accept="application/pdf,image/*" />
    <div style="color:#6b7280;margin-top:4px">Le fichier est stocké localement (localStorage).</div>

    <label>Texte pied de page</label>
    <input id="footerTxt" placeholder="© 2025 – Fiche événement" />

    <div style="margin-top:12px;display:flex;gap:8px">
      <button id="save" class="btn" style="background:#2ecc71">💾 Enregistrer</button>
      <button id="clear" class="btn" style="background:#fff;color:#111;border:1px solid var(--line)">🧹 Réinitialiser</button>
    </div>
  </div>
</div>

<script>
/* Helpers */
const $=id=>document.getElementById(id);
const KEY="fiche_brocante_map_below_dates_v1";
const months=["janvier","février","mars","avril","mai","juin","juillet","août","septembre","octobre","novembre","décembre"];
const weekdays=["dimanche","lundi","mardi","mercredi","jeudi","vendredi","samedi"];
const pad=n=>n<10?"0"+n:""+n;
const cap=s=>s.charAt(0).toUpperCase()+s.slice(1);
const fmtLong=d=>`${cap(weekdays[d.getDay()])} ${pad(d.getDate())} ${cap(months[d.getMonth()])} ${d.getFullYear()}`;

/* State */
let state={
  titre:"Brocante",
  organisateur:"comite des fetes",
  date1:"Le 21 septembre",
  categorie:"Brocante",
  adresse:"salle polyvalente<br>route du Chef-Lieu<br>73420 Drumettaz-Clarafond",
  description:"Description non renseignée.",
  tel:"", email:"",
  callReplace:"", mailReplace:"Merci de contacter un administrateur du site",
  announceReplace:"J'annonce rien", announce:"",
  mediaDataUrl:"", mediaUrl:"",
  hours:"09:00 - 17:00", price:"Gratuit",
  dates:["2025-09-21"],
  footer:"© 2025 – Fiche événement",
  lat:45.658, lng:5.903, zoom:15
};
try{const raw=localStorage.getItem(KEY); if(raw){Object.assign(state, JSON.parse(raw));}}catch(e){}

/* Calendar model */
let cal={year:new Date().getFullYear(), month:new Date().getMonth(), selected:null};

/* Leaflet map */
let map=null, marker=null;
function initMap(){
  if(map) return;
  map = L.map('map',{scrollWheelZoom:true}).setView([state.lat, state.lng], state.zoom||15);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap'
  }).addTo(map);
  marker = L.marker([state.lat, state.lng], {title:'Position de l’événement'}).addTo(map);

  // clic -> déplacer le marqueur + MAJ admin
  map.on('click', (e)=>{
    const {lat,lng}=e.latlng;
    marker.setLatLng([lat,lng]);
    state.lat=+lat.toFixed(6);
    state.lng=+lng.toFixed(6);
    $('lat').value=state.lat;
    $('lng').value=state.lng;
    persist();
  });
}
function updateMap(){
  if(!map){ initMap(); return; }
  map.setView([state.lat, state.lng], state.zoom||15);
  marker.setLatLng([state.lat, state.lng]);
}

/* Render */
function render(){
  $('titreView').textContent=state.titre||"Brocante";
  $('organisateurView').textContent=state.organisateur||"—";
  $('datePrincipale').textContent=state.date1||"—";
  $('categorieView').textContent=state.categorie||"—";
  $('adresseView').innerHTML=state.adresse||"—";
  $('descriptionView').innerHTML=state.description||"Description non renseignée.";
  $('footerView').textContent=state.footer||"";

  // reset boutons
  $('btnCall').textContent="📞 Appeler"; $('btnCall').classList.remove('replaced');
  $('btnMail').textContent="✉️ Écrire";  $('btnMail').classList.remove('replaced');
  $('btnAnnounce').textContent="📢 Annoncer"; $('btnAnnounce').classList.remove('replaced');

  // media
  const cont=$('mediaContainer'); cont.innerHTML="";
  const src=state.mediaDataUrl||state.mediaUrl;
  if(!src){cont.innerHTML=`<div style="height:100%;display:flex;align-items:center;justify-content:center;color:#9aa2af">Aperçu PDF / image</div>`;}
  else if(/^data:application\/pdf|\.pdf(\?|$)/i.test(src)){const emb=document.createElement('embed'); emb.type="application/pdf"; emb.src=src; cont.appendChild(emb);}
  else{const img=document.createElement('img'); img.src=src; img.alt="Affiche"; cont.appendChild(img);}

  // admin fill
  $('titre').value=state.titre; $('organisateur').value=state.organisateur; $('date1').value=state.date1;
  $('categorie').value=state.categorie; $('adresse').value=state.adresse; $('description').value=state.description;
  $('lat').value=state.lat; $('lng').value=state.lng; $('zoom').value=state.zoom;
  $('tel').value=state.tel; $('mailReplace').value=state.mailReplace; $('announceReplace').value=state.announceReplace; $('callReplace').value=state.callReplace;
  $('hours').value=state.hours; $('price').value=state.price; $('datesList').value=state.dates.join(',');
  $('announce').value=state.announce; $('mediaUrl').value=state.mediaUrl; $('footerTxt').value=state.footer;

  // calendrier
  renderCalendar();
  const def=cal.selected||state.dates[0]||`${cal.year}-${pad(cal.month+1)}-${pad(new Date().getDate())}`;
  const [y,m,d]=def.split('-').map(n=>parseInt(n,10));
  updateInfoPanel(new Date(y,m-1,d));

  // carte (sous les dates)
  initMap();
  updateMap();
}
function persist(){ localStorage.setItem(KEY, JSON.stringify(state)); }

/* Calendar */
function renderCalendar(){
  const grid=$('calGrid'); grid.innerHTML="";
  $('calTitle').textContent=`${months[cal.month]} ${cal.year}`;
  const first=new Date(cal.year,cal.month,1);
  const start=(first.getDay()+6)%7;
  const days=new Date(cal.year,cal.month+1,0).getDate();
  for(let i=0;i<start;i++){const e=document.createElement('div'); e.className='day muted'; grid.appendChild(e);}
  for(let d=1; d<=days; d++){
    const cell=document.createElement('button'); cell.type='button'; cell.className='day'; cell.textContent=d;
    const ds=`${cal.year}-${pad(cal.month+1)}-${pad(d)}`;
    if(state.dates.includes(ds)) cell.classList.add('hasEvent');
    if(cal.selected===ds) cell.classList.add('selected');
    cell.addEventListener('click',()=>{cal.selected=ds; updateInfoPanel(new Date(cal.year,cal.month,d)); renderCalendar();});
    grid.appendChild(cell);
  }
}
function updateInfoPanel(dateObj){
  const ds=`${dateObj.getFullYear()}-${pad(dateObj.getMonth()+1)}-${pad(dateObj.getDate())}`;
  $('infoDate').textContent=fmtLong(dateObj);
  const ok=state.dates.includes(ds);
  $('infoHours').textContent= ok ? (state.hours||"") : "";
  $('infoWarn').style.display= ok ? "none" : "block";
  $('infoPrice').textContent=state.price||"Gratuit";
}
$('prevMonth').onclick=()=>{cal.month--; if(cal.month<0){cal.month=11; cal.year--;} renderCalendar(); updateInfoPanel(new Date(cal.year,cal.month,1));};
$('nextMonth').onclick=()=>{cal.month++; if(cal.month>11){cal.month=0; cal.year++;} renderCalendar(); updateInfoPanel(new Date(cal.year,cal.month,1));};

/* Boutons droite */
$('btnCall').addEventListener('click',e=>{e.preventDefault(); const t=$('callReplace').value.trim()||state.callReplace.trim()||state.tel||"—"; e.currentTarget.textContent=t; e.currentTarget.classList.add('replaced');});
$('btnMail').addEventListener('click',e=>{e.preventDefault(); const t=$('mailReplace').value.trim()||state.mailReplace||"Merci de contacter un administrateur du site"; e.currentTarget.textContent=t; e.currentTarget.classList.add('replaced');});
$('btnAnnounce').addEventListener('click',e=>{e.preventDefault(); const t=$('announceReplace').value.trim()||state.announceReplace||"J'annonce rien"; e.currentTarget.textContent=t; e.currentTarget.classList.add('replaced');});

/* Admin */
$('toggleAdmin').onclick=()=>{$('login').style.display = ($('panel').style.display==="block")?"none":"block";};
$('okCode').onclick=()=>{ if($('code').value.trim()==="2210"){ $('msg').textContent="✅ Code accepté"; $('login').style.display="none"; $('panel').style.display="block"; } else { $('msg').textContent="❌ Code incorrect"; } };

/* Save / Reset / Upload */
$('save').onclick=()=>{
  state.titre=$('titre').value.trim(); state.organisateur=$('organisateur').value.trim();
  state.date1=$('date1').value.trim(); state.categorie=$('categorie').value.trim();
  state.adresse=$('adresse').value.trim(); state.description=$('description').value.trim();
  state.lat=parseFloat($('lat').value)||state.lat; state.lng=parseFloat($('lng').value)||state.lng;
  let z=parseInt($('zoom').value,10); if(!isNaN(z)) state.zoom=Math.min(19,Math.max(2,z));
  state.tel=$('tel').value.trim(); state.mailReplace=$('mailReplace').value.trim();
  state.announceReplace=$('announceReplace').value.trim(); state.callReplace=$('callReplace').value.trim();
  state.hours=$('hours').value.trim(); state.price=$('price').value.trim();
  state.dates=$('datesList').value.split(',').map(s=>s.trim()).filter(Boolean);
  state.announce=$('announce').value.trim(); state.mediaUrl=$('mediaUrl').value.trim();
  state.footer=$('footerTxt').value.trim();
  persist(); render();
};
$('clear').onclick=()=>{ if(confirm("Réinitialiser la fiche locale ?")){localStorage.removeItem(KEY); location.reload();} };
$('mediaFile').addEventListener('change',e=>{
  const f=e.target.files[0]; if(!f) return;
  const r=new FileReader();
  r.onload=()=>{ state.mediaDataUrl=r.result; persist(); render(); };
  r.readAsDataURL(f); e.target.value="";
});

/* Init */
if(state.dates.length){const [y,m,d]=state.dates[0].split('-').map(n=>parseInt(n,10)); cal.year=y; cal.month=m-1; cal.selected=state.dates[0];}
render();
</script>
</body>
</html>
