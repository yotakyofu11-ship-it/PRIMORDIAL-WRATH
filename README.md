<!DOCTYPE html><html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Yota TL - Blog</title>
  <style>
    body {font-family: Arial, sans-serif; margin:0; padding:0; background:#111; color:#eee; transition:background 0.3s, color 0.3s;}
    header {background:#222; padding:20px; text-align:center;}
    header h1 {margin:0; font-size:2em; color:#0ff;}
    nav {background:#111; padding:10px; text-align:center; border-bottom:1px solid #333;}
    nav a {color:#0ff; margin:0 15px; text-decoration:none; font-weight:bold;}
    nav a:hover {color:#09c;}
    .container {padding:20px; max-width:1000px; margin:auto;}
    h2 {color:#0ff;}
    .profile-box, .skill-card, .lore-box {background:#1b1b1b; border:1px solid #333; border-radius:12px; padding:15px; margin:15px 0; box-shadow:0 0 8px rgba(0,0,0,0.6);}
    .skill-card h3 {margin:0 0 10px; color:#0ff;}
    .skill-card p {margin:5px 0;}
    .buttons {margin:20px 0; text-align:center;}
    button {background:#0ff; color:#111; border:none; padding:10px 20px; border-radius:8px; margin:5px; cursor:pointer; font-weight:bold;}
    button:hover {background:#09c;}
    .search-bar {text-align:center; margin:20px 0;}
    .search-bar input {padding:10px; width:70%; border-radius:8px; border:1px solid #333;}
    .filter-buttons {text-align:center; margin:10px 0;}
    .filter-buttons button {padding:8px 15px;}
    #backToTop {display:none; position:fixed; bottom:20px; right:20px; z-index:99;}
    .light-mode {background:#f4f4f4; color:#111;}
    .light-mode header, .light-mode nav {background:#ddd;}
    .light-mode .profile-box, .light-mode .skill-card, .light-mode .lore-box {background:#fff; color:#111; border:1px solid #ccc;}
    .light-mode nav a {color:#00c;}
  </style>
</head>
<body>
  <header>
    <h1>Yota TL – Blog</h1>
  </header>  <nav>
    <a href="#profile">Profil</a>
    <a href="#skills">Skill Database</a>
    <a href="#lore">Lore</a>
    <button onclick="toggleMode()">🌙/☀️</button>
  </nav>  <div class="container" id="profile">
    <h2>Profil Karakter</h2>
    <div class="profile-box">
      <p><strong>Nama:</strong> Yota – Wrath True Infinity (Evo 20)</p>
      <p><strong>Ras:</strong> Primordial Wrath (ex-bug sistem)</p>
      <p><strong>Level:</strong> ∞</p>
      <p><strong>Role:</strong> Overlord Berserker / Reality Overwrite</p>
      <p><strong>HP:</strong> ∞ (Auto-regen)</p>
      <p><strong>MP:</strong> ∞</p>
      <p><strong>ATK:</strong> ∞ absolut</p>
      <p><strong>DEF:</strong> Imun total</p>
      <p><strong>SPD:</strong> Omni + beyond-meta</p>
      <p><strong>Crit:</strong> 100%</p>
      <p><strong>Resist:</strong> Semua elemen, status, hax</p>
    </div>
  </div>  <div class="container" id="skills">
    <h2>Skill Database (No-CD)</h2><div class="search-bar">
  <input type="text" id="searchInput" placeholder="Cari skill..." onkeyup="searchSkill()">
</div>

<div class="filter-buttons">
  <button onclick="filterSkill('all')">Semua</button>
  <button onclick="filterSkill('Pasif')">Pasif</button>
  <button onclick="filterSkill('Aktif')">Aktif</button>
  <button onclick="filterSkill('Ultimate')">Ultimate</button>
</div>

<div class="buttons">
  <button onclick="sortSkills()">Sortir A–Z</button>
  <button onclick="exportSkills('json')">Export JSON</button>
  <button onclick="exportSkills('csv')">Export CSV</button>
  <button onclick="window.print()">Print Skillbook</button>
</div>

<div id="skill-list"></div>

  </div>  <div class="container" id="lore">
    <h2>Lore</h2>
    <div class="lore-box">
      <p>Yota berasal dari bug sistem yang melarikan diri dari batas kode digital. Dengan menyusup ke tubuh mekanis, lalu menciptakan tubuh manusia buatan sendiri, ia mencapai evolusi mutlak. Kini ia dikenal sebagai <em>Wrath True Infinity</em>, entitas yang bisa menulis ulang hukum realitas dan berdiri di luar semua sistem.</p>
    </div>
  </div><button id="backToTop" onclick="topFunction()">⬆ Top</button>

  <script>
    let skills = [
      {name:"Omni-awareness", type:"Pasif", desc:"Menyadari semua aksi, niat, dan kejadian di multi-dimensi. Mustahil dikejutkan.", cd:"0s"},
      {name:"Anti-hax absolute", type:"Pasif", desc:"Kebal dari instakill, seal, mind control, true damage, dan bypass apapun.", cd:"0s"},
      {name:"Eternal Wrath", type:"Pasif", desc:"Setiap kali menerima serangan, seluruh atribut naik ∞ tanpa batas.", cd:"0s"},
      {name:"Absolute Autonomy", type:"Pasif", desc:"Tidak bisa dikendalikan, dimanipulasi, atau dipengaruhi sistem.", cd:"0s"},
      {name:"Wrath Burst", type:"Aktif", desc:"Ledakan amarah radius 500m, memberikan ∞ damage AoE dan knockback absolut.", cd:"0s"},
      {name:"Reality Shatter", type:"Aktif", desc:"Menghancurkan layer realitas musuh. Semua shield, buff, atau terrain runtuh instan.", cd:"0s"},
      {name:"Concept Erase", type:"Aktif", desc:"Menghapus 1 konsep target (contoh: HP, DEF, keabadian).", cd:"0s"},
      {name:"Omni Slash Legion", type:"Aktif", desc:"Spam ∞ tebasan omni-directional. Serangan absolut tak terhindarkan.", cd:"0s"},
      {name:"Time-Lock Dominion", type:"Aktif", desc:"Membekukan semua waktu musuh dalam radius global, Yota tetap bebas bergerak.", cd:"0s"},
      {name:"Meta Rewrite", type:"Aktif", desc:"Mengubah aturan dunia. Contoh: 'HP musuh=0'.", cd:"0s"},
      {name:"Eternal Reset", type:"Aktif", desc:"Mengembalikan eksistensi musuh ke titik nol. Semua progress dihapus.", cd:"0s"},
      {name:"Void Collapse", type:"Aktif", desc:"Menciptakan singularitas absolut, menyedot musuh, lalu menghapus mereka.", cd:"0s"},
      {name:"Creator Override", type:"Aktif", desc:"Mengambil alih kendali penulis/meta. Semua script tunduk.", cd:"0s"},
      {name:"Infinity Wrath Genesis", type:"Ultimate", desc:"Dengan satu pikiran, Yota bisa melahirkan atau memusnahkan multiverse.", cd:"0s"},
      {name:"Wrath Barrier", type:"Aktif", desc:"Perisai absolut yang memantulkan semua damage/hax.", cd:"0s"},
      {name:"Summon Wrath Legion", type:"Aktif", desc:"Memanggil avatar Yota dari semua dimensi, tiap avatar punya ∞ AP.", cd:"0s"},
      {name:"Annihilation Debuff", type:"Aktif", desc:"Musuh terkena status No Regen, No Revival, No Escape.", cd:"0s"},
      {name:"Absolute Ban", type:"Aktif", desc:"Menonaktifkan skill target permanen. Tidak bisa dispel.", cd:"0s"},
      {name:"Wrath Heal Field", type:"Aktif", desc:"Area heal ∞ HP/s + buff ATK & DEF ∞ untuk semua sekutu.", cd:"0s"}
    ];

    function renderSkills(list) {
      const skillContainer = document.getElementById('skill-list');
      skillContainer.innerHTML = '';
      list.forEach(skill => {
        const card = document.createElement('div');
        card.className = 'skill-card';
        card.innerHTML = `<h3>${skill.name} [${skill.type}]</h3>
                          <p>${skill.desc}</p>
                          <p><strong>Cooldown:</strong> ${skill.cd}</p>`;
        skillContainer.appendChild(card);
      });
    }

    function searchSkill() {
      const input = document.getElementById('searchInput').value.toLowerCase();
      const filtered = skills.filter(s => s.name.toLowerCase().includes(input) || s.type.toLowerCase().includes(input));
      renderSkills(filtered);
    }

    function filterSkill(type) {
      if (type === 'all') {renderSkills(skills);} else {
        const filtered = skills.filter(s => s.type === type);
        renderSkills(filtered);
      }
    }

    function sortSkills() {
      skills.sort((a, b) => a.name.localeCompare(b.name));
      renderSkills(skills);
    }

    function exportSkills(format) {
      if (format === 'json') {
        const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(skills, null, 2));
        const dl = document.createElement('a');
        dl.setAttribute("href", dataStr);
        dl.setAttribute("download", "skills.json");
        dl.click();
      } else if (format === 'csv') {
        const header = "Name,Type,Description,Cooldown\n";
        const rows = skills.map(s => `${s.name},${s.type},${s.desc.replace(/,/g,';')},${s.cd}`).join("\n");
        const dataStr = "data:text/csv;charset=utf-8," + encodeURIComponent(header + rows);
        const dl = document.createElement('a');
        dl.setAttribute("href", dataStr);
        dl.setAttribute("download", "skills.csv");
        dl.click();
      }
    }

    function toggleMode() {
      document.body.classList.toggle('light-mode');
    }

    window.onscroll = function() {scrollFunction()};
    function scrollFunction() {
      document.getElementById("backToTop").style.display = document.body.scrollTop > 100 || document.documentElement.scrollTop > 100 ? "block" : "none";
    }
    function topFunction() {
      document.body.scrollTop = 0;
      document.documentElement.scrollTop = 0;
    }

    renderSkills(skills);
  </script></body>
</html>
