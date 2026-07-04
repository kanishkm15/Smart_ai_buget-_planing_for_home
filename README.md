# Smart_ai_buget-_planing_for_home
Calculator 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BuildSmart AI – Construction Budget Estimator</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root{
  --blue:#3B82F6; --purple:#8B5CF6; --emerald:#10B981; --orange:#F59E0B;
  --bg:#F7F8FC; --bg-alt:#ffffff;
  --ink:#12131A; --ink-soft:#5B5F73; --ink-faint:#9296A8;
  --card:rgba(255,255,255,0.72); --card-solid:#ffffff;
  --border:rgba(18,19,26,0.08);
  --shadow:0 8px 30px rgba(30,35,60,0.08);
  --shadow-lg:0 20px 60px rgba(30,35,60,0.14);
  --radius:20px;
  --grad-1:linear-gradient(135deg,var(--blue),var(--purple));
  --grad-2:linear-gradient(135deg,var(--emerald),var(--blue));
  --grad-3:linear-gradient(135deg,var(--orange),var(--purple));
  --grad-4:linear-gradient(135deg,var(--purple),#EC4899);
  --sidebar-w:260px;
}
[data-theme="dark"]{
  --bg:#0B0D14; --bg-alt:#12141F;
  --ink:#F1F2F7; --ink-soft:#B4B7C7; --ink-faint:#6E7286;
  --card:rgba(24,26,38,0.65); --card-solid:#171926;
  --border:rgba(255,255,255,0.08);
  --shadow:0 8px 30px rgba(0,0,0,0.35);
  --shadow-lg:0 20px 60px rgba(0,0,0,0.5);
}
*{box-sizing:border-box; margin:0; padding:0;}
html{scroll-behavior:smooth;}
body{
  font-family:'Inter',sans-serif; background:var(--bg); color:var(--ink);
  overflow-x:hidden; transition:background .4s ease, color .4s ease;
  min-height:100vh;
}
h1,h2,h3,h4,.display{font-family:'Space Grotesk',sans-serif;}
.mono{font-family:'JetBrains Mono',monospace;}
a{color:inherit; text-decoration:none;}
button{font-family:inherit; cursor:pointer; border:none;}
::selection{background:var(--purple); color:#fff;}

/* ---------- background blobs ---------- */
.bg-blobs{position:fixed; inset:0; z-index:-1; overflow:hidden; pointer-events:none;}
.blob{position:absolute; border-radius:50%; filter:blur(70px); opacity:.35; animation:float 18s ease-in-out infinite;}
.blob1{width:420px; height:420px; background:var(--blue); top:-120px; left:-100px;}
.blob2{width:380px; height:380px; background:var(--purple); top:30%; right:-140px; animation-delay:-6s;}
.blob3{width:320px; height:320px; background:var(--emerald); bottom:-100px; left:20%; animation-delay:-11s;}
.blob4{width:260px; height:260px; background:var(--orange); bottom:10%; right:10%; animation-delay:-3s;}
@keyframes float{
  0%,100%{transform:translate(0,0) scale(1);}
  33%{transform:translate(30px,-40px) scale(1.08);}
  66%{transform:translate(-25px,25px) scale(0.94);}
}

/* ---------- layout ---------- */
.app{display:flex; min-height:100vh;}

/* Sidebar */
.sidebar{
  width:var(--sidebar-w); flex-shrink:0; padding:28px 18px;
  background:var(--card); backdrop-filter:blur(20px); -webkit-backdrop-filter:blur(20px);
  border-right:1px solid var(--border); position:sticky; top:0; height:100vh;
  display:flex; flex-direction:column; z-index:100; transition:transform .3s ease, background .4s;
}
.brand{display:flex; align-items:center; gap:10px; padding:6px 10px 26px;}
.brand-icon{
  width:38px; height:38px; border-radius:12px; background:var(--grad-1);
  display:flex; align-items:center; justify-content:center; color:#fff; font-size:16px;
  box-shadow:0 6px 16px rgba(139,92,246,0.35);
}
.brand-text .t1{font-weight:700; font-size:15px; line-height:1.1;}
.brand-text .t2{font-size:11px; color:var(--ink-faint); letter-spacing:.04em;}

.nav-group{margin-bottom:6px;}
.nav-item{
  display:flex; align-items:center; gap:12px; padding:11px 14px; border-radius:12px;
  color:var(--ink-soft); font-size:14px; font-weight:500; cursor:pointer;
  transition:all .2s ease; margin-bottom:3px; position:relative; background:transparent; width:100%; text-align:left;
}
.nav-item i{width:18px; text-align:center; font-size:15px;}
.nav-item:hover{background:rgba(139,92,246,0.08); color:var(--ink);}
.nav-item.active{background:var(--grad-1); color:#fff; box-shadow:0 8px 20px rgba(59,130,246,0.3);}
.sidebar-bottom{margin-top:auto; padding-top:16px; border-top:1px solid var(--border);}
.theme-toggle{
  display:flex; align-items:center; justify-content:space-between; padding:10px 14px;
  border-radius:12px; background:var(--bg-alt); border:1px solid var(--border); font-size:13px; font-weight:600;
}
.switch{width:40px; height:22px; border-radius:20px; background:var(--border); position:relative; cursor:pointer; transition:.3s;}
.switch::after{content:''; position:absolute; width:16px; height:16px; border-radius:50%; background:#fff; top:3px; left:3px; transition:.3s; box-shadow:0 2px 4px rgba(0,0,0,.2);}
[data-theme="dark"] .switch{background:var(--grad-1);}
[data-theme="dark"] .switch::after{left:21px;}

/* Main */
.main{flex:1; min-width:0;}
.topbar{
  display:flex; align-items:center; justify-content:space-between; padding:18px 34px;
  position:sticky; top:0; z-index:60; background:var(--card); backdrop-filter:blur(18px);
  border-bottom:1px solid var(--border);
}
.topbar h2{font-size:19px; font-weight:600;}
.topbar .sub{font-size:12.5px; color:var(--ink-faint); margin-top:2px;}
.top-actions{display:flex; align-items:center; gap:14px;}
.icon-btn{width:38px; height:38px; border-radius:11px; background:var(--bg-alt); border:1px solid var(--border); display:flex; align-items:center; justify-content:center; color:var(--ink-soft); transition:.2s;}
.icon-btn:hover{color:var(--purple); transform:translateY(-1px);}
.avatar{width:38px; height:38px; border-radius:11px; background:var(--grad-2); display:flex; align-items:center; justify-content:center; color:#fff; font-weight:700; font-size:13px;}
.menu-btn{display:none; width:38px; height:38px; border-radius:10px; background:var(--bg-alt); border:1px solid var(--border); align-items:center; justify-content:center;}

.page{display:none; padding:32px 34px 60px; animation:fadeUp .5s ease;}
.page.active{display:block;}
@keyframes fadeUp{from{opacity:0; transform:translateY(14px);} to{opacity:1; transform:translateY(0);}}

/* ---------- cards / glass ---------- */
.glass{
  background:var(--card); backdrop-filter:blur(16px); -webkit-backdrop-filter:blur(16px);
  border:1px solid var(--border); border-radius:var(--radius); box-shadow:var(--shadow);
  transition:transform .25s ease, box-shadow .25s ease;
}
.glass:hover{box-shadow:var(--shadow-lg);}
.grid{display:grid; gap:20px;}
.g2{grid-template-columns:repeat(2,1fr);}
.g3{grid-template-columns:repeat(3,1fr);}
.g4{grid-template-columns:repeat(4,1fr);}
.g6{grid-template-columns:repeat(6,1fr);}

/* ---------- HOME / hero ---------- */
.hero{position:relative; padding:64px 40px; border-radius:28px; overflow:hidden; margin-bottom:36px;
  background:linear-gradient(120deg, rgba(59,130,246,0.08), rgba(139,92,246,0.08) 45%, rgba(16,185,129,0.08));
  border:1px solid var(--border);
}
.hero-inner{display:flex; align-items:center; gap:50px; position:relative; z-index:2;}
.hero-copy{flex:1;}
.eyebrow{display:inline-flex; align-items:center; gap:8px; font-size:12.5px; font-weight:600; color:var(--purple);
  background:rgba(139,92,246,0.1); padding:6px 14px; border-radius:30px; margin-bottom:18px;}
.eyebrow i{font-size:11px;}
.hero h1{font-size:44px; line-height:1.08; font-weight:700; letter-spacing:-.02em; margin-bottom:16px;}
.hero h1 span{background:var(--grad-1); -webkit-background-clip:text; background-clip:text; color:transparent;}
.hero p.lead{font-size:16.5px; color:var(--ink-soft); max-width:480px; margin-bottom:28px; line-height:1.6;}
.btn-row{display:flex; gap:14px;}
.btn{position:relative; overflow:hidden; padding:13px 26px; border-radius:13px; font-weight:600; font-size:14.5px; display:inline-flex; align-items:center; gap:9px; transition:transform .2s ease, box-shadow .2s ease;}
.btn-primary{background:var(--grad-1); color:#fff; box-shadow:0 10px 28px rgba(59,130,246,0.35);}
.btn-primary:hover{transform:translateY(-2px); box-shadow:0 14px 34px rgba(59,130,246,0.45);}
.btn-ghost{background:var(--bg-alt); color:var(--ink); border:1px solid var(--border);}
.btn-ghost:hover{transform:translateY(-2px); border-color:var(--purple);}
.ripple{position:absolute; border-radius:50%; background:rgba(255,255,255,0.5); transform:scale(0); animation:ripple .6s ease-out;}
@keyframes ripple{to{transform:scale(4); opacity:0;}}

.hero-visual{flex:0 0 300px; height:280px; position:relative;}
.crane{position:absolute; inset:0; display:flex; align-items:center; justify-content:center;}
.building-stack{display:flex; align-items:flex-end; gap:8px;}
.blk{border-radius:10px 10px 0 0; animation:rise 2.2s cubic-bezier(.2,.8,.2,1) both;}
.blk1{width:34px; height:90px; background:var(--grad-1); animation-delay:.1s;}
.blk2{width:34px; height:150px; background:var(--grad-2); animation-delay:.3s;}
.blk3{width:34px; height:110px; background:var(--grad-3); animation-delay:.5s;}
.blk4{width:34px; height:190px; background:var(--grad-4); animation-delay:.2s;}
.blk5{width:34px; height:70px; background:var(--grad-1); animation-delay:.6s;}
@keyframes rise{from{transform:scaleY(0); opacity:0;} to{transform:scaleY(1); opacity:1;} }
.floaty{position:absolute; font-size:22px; color:var(--orange); animation:bob 3.4s ease-in-out infinite;}
.floaty.f2{top:10%; right:0; color:var(--emerald); animation-delay:-1s;}
.floaty.f3{bottom:6%; left:2%; color:var(--purple); animation-delay:-2s;}
@keyframes bob{0%,100%{transform:translateY(0);} 50%{transform:translateY(-14px);}}

.feat-title{font-size:22px; font-weight:700; margin:6px 0 20px;}
.feat-card{padding:24px; cursor:default;}
.feat-icon{width:46px; height:46px; border-radius:13px; display:flex; align-items:center; justify-content:center; color:#fff; font-size:18px; margin-bottom:14px;}
.feat-card h4{font-size:15.5px; margin-bottom:6px;}
.feat-card p{font-size:13px; color:var(--ink-soft); line-height:1.5;}

/* ---------- stat / dashboard cards ---------- */
.stat-card{padding:22px; position:relative; overflow:hidden;}
.stat-top{display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:14px;}
.stat-icon{width:40px; height:40px; border-radius:12px; display:flex; align-items:center; justify-content:center; color:#fff; font-size:16px;}
.stat-trend{font-size:11.5px; font-weight:700; padding:4px 9px; border-radius:20px;}
.trend-up{color:var(--emerald); background:rgba(16,185,129,0.12);}
.stat-val{font-size:26px; font-weight:700; font-family:'Space Grotesk',sans-serif;}
.stat-label{font-size:12.5px; color:var(--ink-faint); margin-top:4px;}

.section-head{display:flex; align-items:center; justify-content:space-between; margin:34px 0 18px;}
.section-head h3{font-size:18px; font-weight:700;}
.section-head .view-all{font-size:12.5px; color:var(--purple); font-weight:600;}

.report-row{display:flex; align-items:center; justify-content:space-between; padding:14px 18px; border-bottom:1px solid var(--border);}
.report-row:last-child{border-bottom:none;}
.report-row .rl{display:flex; align-items:center; gap:12px;}
.report-ico{width:36px; height:36px; border-radius:10px; background:var(--grad-1); display:flex; align-items:center; justify-content:center; color:#fff; font-size:13px;}
.report-row .rname{font-size:13.5px; font-weight:600;}
.report-row .rdate{font-size:11.5px; color:var(--ink-faint);}
.badge{font-size:11px; font-weight:700; padding:4px 10px; border-radius:20px; background:rgba(16,185,129,0.12); color:var(--emerald);}

/* ---------- forms ---------- */
.form-card{padding:30px;}
.form-grid{display:grid; grid-template-columns:repeat(2,1fr); gap:20px;}
.field{display:flex; flex-direction:column; gap:7px;}
.field.full{grid-column:1/-1;}
.field label{font-size:12.5px; font-weight:600; color:var(--ink-soft);}
.field input[type=text], .field input[type=number], .field input[type=date], .field select{
  padding:12px 14px; border-radius:11px; border:1.5px solid var(--border); background:var(--bg-alt); color:var(--ink);
  font-size:13.5px; font-family:inherit; transition:.2s;
}
.field input:focus, .field select:focus{outline:none; border-color:var(--purple); box-shadow:0 0 0 4px rgba(139,92,246,0.12);}
.chip-group{display:flex; flex-wrap:wrap; gap:9px;}
.chip{padding:9px 16px; border-radius:11px; border:1.5px solid var(--border); background:var(--bg-alt); font-size:13px; font-weight:600; color:var(--ink-soft); transition:.2s;}
.chip:hover{border-color:var(--purple);}
.chip.selected{background:var(--grad-1); color:#fff; border-color:transparent;}
.toggle-row{display:flex; align-items:center; justify-content:space-between; padding:12px 16px; border:1.5px solid var(--border); border-radius:12px; background:var(--bg-alt);}
.toggle-row span{font-size:13px; font-weight:600;}
.mini-switch{width:38px; height:21px; border-radius:20px; background:var(--border); position:relative; transition:.25s;}
.mini-switch::after{content:''; width:15px; height:15px; border-radius:50%; background:#fff; position:absolute; top:3px; left:3px; transition:.25s; box-shadow:0 1px 3px rgba(0,0,0,.25);}
.mini-switch.on{background:var(--grad-2);}
.mini-switch.on::after{left:20px;}

.divider-label{display:flex; align-items:center; gap:12px; margin:6px 0 4px; grid-column:1/-1;}
.divider-label span{font-size:12px; font-weight:700; color:var(--ink-faint); text-transform:uppercase; letter-spacing:.06em; white-space:nowrap;}
.divider-label::after{content:''; height:1px; background:var(--border); flex:1;}

.predict-btn{width:100%; margin-top:26px; padding:16px; border-radius:14px; background:var(--grad-1); color:#fff; font-weight:700; font-size:15px; display:flex; align-items:center; justify-content:center; gap:10px; box-shadow:0 14px 30px rgba(59,130,246,0.32); transition:.2s;}
.predict-btn:hover{transform:translateY(-2px);}
.predict-btn:disabled{opacity:.75; cursor:progress;}
.spinner{width:16px; height:16px; border-radius:50%; border:2.5px solid rgba(255,255,255,.4); border-top-color:#fff; animation:spin .7s linear infinite; display:none;}
.spinner.show{display:inline-block;}
@keyframes spin{to{transform:rotate(360deg);}}

.result-wrap{display:none; margin-top:30px;}
.result-wrap.show{display:block; animation:fadeUp .5s ease;}
.confidence-band{display:flex; align-items:center; gap:16px; padding:18px 22px; border-radius:16px; background:linear-gradient(90deg, rgba(16,185,129,0.1), rgba(59,130,246,0.1)); margin-bottom:22px; border:1px solid var(--border);}
.conf-ring{position:relative; width:56px; height:56px; flex-shrink:0;}
.conf-ring svg{transform:rotate(-90deg);}
.conf-ring .ct{position:absolute; inset:0; display:flex; align-items:center; justify-content:center; font-size:12px; font-weight:700;}
.result-grid{grid-template-columns:repeat(3,1fr);}
.result-card{padding:20px; text-align:left;}
.result-card .rc-icon{width:36px; height:36px; border-radius:10px; display:flex; align-items:center; justify-content:center; color:#fff; font-size:14px; margin-bottom:10px;}
.result-card .rc-val{font-size:20px; font-weight:700; font-family:'Space Grotesk',sans-serif;}
.result-card .rc-label{font-size:12px; color:var(--ink-faint); margin-top:3px;}

/* skeleton */
.skeleton{background:linear-gradient(90deg, var(--border) 25%, rgba(139,92,246,0.15) 50%, var(--border) 75%); background-size:200% 100%; animation:shimmer 1.3s infinite; border-radius:10px;}
@keyframes shimmer{0%{background-position:200% 0;} 100%{background-position:-200% 0;}}

/* ---------- analytics ---------- */
.chart-card{padding:22px;}
.chart-card h4{font-size:14.5px; font-weight:700; margin-bottom:14px;}
.chart-box{position:relative; height:230px;}
.breakdown-row{padding:13px 0; border-bottom:1px solid var(--border);}
.breakdown-row:last-child{border-bottom:none;}
.br-top{display:flex; justify-content:space-between; font-size:13px; font-weight:600; margin-bottom:8px;}
.br-top .amt{color:var(--ink-faint); font-weight:500; font-size:12px;}
.progress{height:8px; border-radius:20px; background:var(--border); overflow:hidden;}
.progress-fill{height:100%; border-radius:20px; width:0; transition:width 1.1s cubic-bezier(.2,.8,.2,1);}

/* ---------- house design ---------- */
.style-bar{display:flex; flex-wrap:wrap; gap:10px; margin-bottom:24px;}
.gallery-card{border-radius:18px; overflow:hidden; cursor:default;}
.gallery-thumb{height:170px; display:flex; align-items:center; justify-content:center; position:relative; color:#fff; font-size:34px; overflow:hidden;}
.gallery-thumb img{width:100%; height:100%; object-fit:cover; display:block; position:relative; z-index:1; transition:transform .5s ease, opacity .4s ease; opacity:0;}
.gallery-thumb img.loaded{opacity:1;}
.gallery-card:hover .gallery-thumb img{transform:scale(1.06);}
.gallery-thumb .thumb-fallback{position:absolute; inset:0; display:flex; align-items:center; justify-content:center; z-index:0;}
.gallery-thumb::after{content:attr(data-tag); position:absolute; top:10px; left:10px; font-size:10.5px; font-weight:700; background:rgba(0,0,0,0.45); padding:4px 10px; border-radius:20px; letter-spacing:.03em; z-index:2; color:#fff;}
.gallery-thumb::before{content:''; position:absolute; inset:0; background:linear-gradient(180deg, rgba(0,0,0,0) 55%, rgba(0,0,0,0.35) 100%); z-index:1; pointer-events:none;}
.gallery-body{padding:14px 16px;}
.gallery-body h5{font-size:13.5px; font-weight:700; margin-bottom:3px;}
.gallery-body p{font-size:11.5px; color:var(--ink-faint);}

/* ---------- ML page ---------- */
.ml-stat{padding:20px; text-align:center;}
.ml-stat .mv{font-size:24px; font-weight:700; font-family:'Space Grotesk',sans-serif;}
.ml-stat .ml{font-size:11.5px; color:var(--ink-faint); margin-top:4px;}

/* ---------- report ---------- */
.report-doc{padding:40px; max-width:820px; margin:0 auto;}
.report-doc .rd-head{display:flex; justify-content:space-between; align-items:center; border-bottom:2px solid var(--border); padding-bottom:20px; margin-bottom:24px;}
.report-doc h2{font-size:22px;}
.report-doc .rd-grid{display:grid; grid-template-columns:1fr 1fr; gap:10px 26px; font-size:13px; margin-bottom:22px;}
.report-doc .rd-grid div span{display:block; font-size:11px; color:var(--ink-faint); margin-bottom:2px;}
.report-doc table{width:100%; border-collapse:collapse; font-size:12.5px; margin-top:8px;}
.report-doc th{text-align:left; padding:9px 10px; background:var(--bg-alt); font-size:11px; color:var(--ink-faint); text-transform:uppercase; letter-spacing:.04em;}
.report-doc td{padding:9px 10px; border-bottom:1px solid var(--border);}

/* ---------- settings ---------- */
.settings-row{display:flex; align-items:center; justify-content:space-between; padding:18px 6px; border-bottom:1px solid var(--border);}
.settings-row:last-child{border-bottom:none;}
.settings-row h5{font-size:13.5px; font-weight:600;}
.settings-row p{font-size:12px; color:var(--ink-faint); margin-top:3px;}

/* ---------- chatbot ---------- */
.chat-fab{position:fixed; bottom:26px; right:26px; width:60px; height:60px; border-radius:50%; background:var(--grad-1); color:#fff; display:flex; align-items:center; justify-content:center; font-size:22px; box-shadow:0 14px 30px rgba(59,130,246,0.4); z-index:200; transition:.25s;}
.chat-fab:hover{transform:scale(1.08) rotate(-4deg);}
.chat-panel{position:fixed; bottom:98px; right:26px; width:340px; max-height:480px; border-radius:20px; background:var(--card-solid); border:1px solid var(--border); box-shadow:var(--shadow-lg); z-index:200; display:none; flex-direction:column; overflow:hidden;}
.chat-panel.open{display:flex; animation:fadeUp .3s ease;}
.chat-head{padding:16px 18px; background:var(--grad-1); color:#fff; display:flex; align-items:center; gap:10px;}
.chat-head .ci{width:32px; height:32px; border-radius:10px; background:rgba(255,255,255,.2); display:flex; align-items:center; justify-content:center;}
.cha
