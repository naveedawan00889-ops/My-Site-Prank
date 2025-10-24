<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Prank Page</title>
<style>
  html,body{height:100%;margin:0;font-family:Arial,Helvetica,sans-serif;}
  #hacked{position:fixed;inset:0;background:#050505;color:#ff4d4d;display:flex;align-items:center;justify-content:center;flex-direction:column;z-index:9999;padding:20px}
  h1{font-size:56px;margin:0 0 6px;letter-spacing:6px}
  #sub{color:#ddd;margin:6px 0 12px}
  .bar{width:80%;max-width:760px;background:#222;height:14px;border-radius:10px;overflow:hidden}
  .fill{height:100%;width:0%;background:linear-gradient(90deg,#ff4d4d,#ffb3b3);transition:width .9s linear}
  #warnModal{position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);background:#fff;color:#111;padding:18px;border-radius:10px;box-shadow:0 10px 30px rgba(0,0,0,.45);z-index:10010;display:none;max-width:92%;width:420px}
  #warnModal h3{margin:0 0 8px}
  .row{display:flex;gap:10px;justify-content:center;margin-top:12px}
  .btn{padding:10px 12px;border-radius:8px;border:none;cursor:pointer;font-weight:700}
  .btn-stay{background:#111;color:#fff}
  .btn-leave{background:#ff4d4d;color:#fff}
  #site{display:none;padding:32px;text-align:center}
  #esc-hint{position:fixed;right:8px;top:8px;font-size:11px;color:#777;opacity:.6;z-index:10011}
</style>
</head>
<body>

<div id="esc-hint" aria-hidden="true">Press ESC to exit</div>

<div id="hacked" role="dialog" aria-live="assertive" aria-label="Hacked message">
  <h1>HACKED</h1>
  <p id="sub">Suspicious activity detected. Securing system...</p>
  <div class="bar" aria-hidden="true"><div class="fill" id="fill"></div></div>
  <div style="color:#cfcfcf;margin-top:12px;font-size:13px">This is a harmless prank. Nothing is harmed. (Press ESC to exit)</div>
</div>

<div id="warnModal" role="alertdialog" aria-hidden="true">
  <h3>Wait — prank still running</h3>
  <p>The prank is scheduled for 5 minutes. Do you want to leave now or stay?</p>
  <div class="row">
    <button class="btn btn-stay" id="stayBtn">Stay (Keep watching)</button>
    <button class="btn btn-leave" id="leaveBtn">Leave (Exit)</button>
  </div>
</div>

<div id="site">
  <h2>Welcome back</h2>
  <p>The prank has ended — everything is fine.</p>
</div>

<script>
(function(){
  const DURATION = 300; // 5 minutes
  const fill = document.getElementById('fill');
  const hacked = document.getElementById('hacked');
  const site = document.getElementById('site');
  const sub = document.getElementById('sub');
  const warnModal = document.getElementById('warnModal');
  const stayBtn = document.getElementById('stayBtn');
  const leaveBtn = document.getElementById('leaveBtn');

  let elapsed = 0;
  let timer = null;
  let allowExit = false;

  function startTimer(){
    timer = setInterval(() => {
      elapsed++;
      const pct = Math.min(100, Math.round((elapsed/DURATION)*100));
      fill.style.width = pct + '%';
      if(elapsed === 10) sub.textContent = "Alerting security team...";
      if(elapsed === 60) sub.textContent = "Analyzing system state...";
      if(elapsed === 180) sub.textContent = "Finalizing checks...";
      if(elapsed === 240) sub.textContent = "Almost done...";
      if(elapsed >= DURATION) finish();
    },1000);
  }

  function finish(){
    clearInterval(timer);
    hacked.style.display = 'none';
    site.style.display = 'block';
    fill.style.width = '0%';
    allowExit = true;
    // remove handlers
    window.removeEventListener('popstate', onPopState);
    window.removeEventListener('beforeunload', onBeforeUnload);
  }

  startTimer();

  // ensure there are history entries so Back triggers popstate
  try {
    history.replaceState({prank:'start'}, '', '#prankstart');
    history.pushState({prank:'1'}, '', '#prank1');
    history.pushState({prank:'2'}, '', '#prank2');
  } catch(e){ /* ignore */ }

  function showModal(){
    warnModal.style.display = 'block';
    warnModal.setAttribute('aria-hidden','false');
  }
  function hideModal(){
    warnModal.style.display = 'none';
    warnModal.setAttribute('aria-hidden','true');
  }

  function onPopState(e){
    if(allowExit) return;
    // show warning modal instead of leaving immediately
    showModal();
    // push another state to keep page from actually going back on first press
    try { history.pushState({prank:'stay'}, '', '#stay'); } catch(e){}
  }

  function onBeforeUnload(e){
    if(allowExit) return;
    e.preventDefault();
    e.returnValue = '';
    return '';
  }

  window.addEventListener('popstate', onPopState);
  window.addEventListener('beforeunload', onBeforeUnload);

  stayBtn.addEventListener('click', () => { hideModal(); });
  leaveBtn.addEventListener('click', () => {
    allowExit = true;
    hideModal();
    window.removeEventListener('popstate', onPopState);
    window.removeEventListener('beforeunload', onBeforeUnload);
    try { history.back(); } catch(e){}
  });

  // ESC exit
  document.addEventListener('keydown', (e) => {
    if(e.key === 'Escape' || e.key === 'Esc'){ finish(); }
  });

  // mobile double-tap escape hint
  const escHint = document.getElementById('esc-hint');
  let cornerTaps = 0;
  escHint.addEventListener('click', () => {
    cornerTaps++;
    if(cornerTaps >= 2){ finish(); }
    setTimeout(()=> cornerTaps = 0, 1600);
  });

})();
</script>

</body>
</html>
