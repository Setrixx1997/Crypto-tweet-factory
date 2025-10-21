[crypto-tweet-factory.html_1 (1).html](https://github.com/user-attachments/files/23020912/crypto-tweet-factory.html_1.1.html)
<!doctype html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Crypto Tweet Factory</title>
<style>
:root{
  --bg:#050708;--panel:#0b1115;--line:#13212c;
  --muted:#8aa1b1;--neon:#4ade80;--blue:#60aaff;
  --tooltip-bg:#0f1a12;--tooltip-border:#1d2d23;
}
*{box-sizing:border-box}
body{
  margin:0;background:linear-gradient(180deg,#050708,#081019);
  color:#e6eef8;font-family:Inter,system-ui,Segoe UI,Roboto,Arial;
  min-height:100vh;display:flex;align-items:center;justify-content:center;padding:22px;
}
.wrap{
  width:min(980px,96vw);
  background:linear-gradient(180deg,rgba(255,255,255,.03),rgba(255,255,255,.02));
  border:1px solid var(--line);border-radius:16px;padding:18px;
  box-shadow:0 12px 36px rgba(0,0,0,.35);
}
header{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
h1{margin:0;font-size:20px;color:var(--neon)}
.right{display:flex;gap:8px;align-items:center}

/* Tip button + tooltip */
.tipWrap{position:relative;display:inline-block}
.tipBtn{
  background:var(--neon);color:#001c09;font-weight:800;border:none;border-radius:10px;
  padding:8px 14px;cursor:pointer;transition:transform .15s ease,opacity .15s ease;
}
.tipBtn:hover{transform:scale(1.05);opacity:.92}
.tipWrap[data-tip]:hover:after{
  content:attr(data-tip);position:absolute;top:120%;right:0;white-space:nowrap;
  background:var(--tooltip-bg);color:#cff8df;border:1px solid var(--tooltip-border);
  border-radius:8px;padding:6px 10px;font-size:12px;box-shadow:0 0 18px rgba(74,222,128,.2);
}

/* Saved tweets button */
.savedBtn{
  background:transparent;border:1px solid var(--line);color:#cfeaff;border-radius:10px;
  padding:8px 12px;font-weight:800;cursor:pointer
}

/* Upgrade button */
.upgBtn{
  background:#213b2a;color:#bfffd3;border:none;border-radius:10px;padding:8px 12px;font-weight:800;cursor:pointer;
}

/* small badge to show premium */
.premiumBadge{background:#223a20;color:#bfffd3;padding:6px 10px;border-radius:10px;font-weight:800;border:1px solid #12321a}

/* topbar */
.topbar{display:flex;gap:8px;flex-wrap:wrap;margin:10px 0}
.input{flex:1 1 360px;background:#0d151c;color:#dfe8f4;border:1px solid #0f1820;
  padding:10px 12px;border-radius:12px;outline:none}
select{background:#0b141b;color:#dfe8f4;border:1px solid #13212c;
  padding:8px 10px;border-radius:10px;font-weight:700}
.btn{border:none;border-radius:10px;padding:10px 16px;font-weight:800;cursor:pointer;font-size:15px}
.btn.gen{background:var(--neon);color:#001c09}
.btn.tweet{background:var(--blue);color:#001426}
.btn.copy,.btn.back,.btn.toggle{background:transparent;border:1px solid var(--line);color:#cfeaff}
#tweet-box{
  background:var(--panel);border:1px solid var(--line);
  border-radius:12px;padding:24px;margin:12px 0;text-align:center;
  font-size:18px;line-height:1.45;box-shadow:0 0 12px rgba(74,222,128,0.2);
  min-height:96px;display:flex;align-items:center;justify-content:center;
}
.actions{display:flex;gap:8px;flex-wrap:wrap;justify-content:center;margin-top:8px}
.countdown{margin-top:6px;text-align:center;color:#9ec1ff;font-size:13px;min-height:18px}

/* Modal / overlays */
.modalOverlay{position:fixed;inset:0;background:rgba(0,0,0,.6);display:none;align-items:center;justify-content:center;z-index:50}
.modal{width:min(780px,94vw);max-height:80vh;overflow:auto;background:#0b1115;border:1px solid #13212c;border-radius:14px;box-shadow:0 30px 80px rgba(0,0,0,.55)}
.modal header{display:flex;justify-content:space-between;align-items:center;padding:14px;border-bottom:1px solid #13212c}
.modal h2{margin:0;font-size:16px;color:#9fe6b8}
.modal .body{padding:12px}
.modal .row{display:flex;gap:10px;align-items:flex-start;justify-content:space-between;background:#0e151a;border:1px solid #17242f;border-radius:10px;padding:10px;margin-bottom:8px}
.modal .text{white-space:pre-wrap;line-height:1.35}
.modal .del{background:transparent;border:1px solid #243748;color:#cfeaff;border-radius:8px;padding:6px 10px;cursor:pointer}
.modal .footer{display:flex;justify-content:space-between;align-items:center;padding:12px;border-top:1px solid #13212c}
.modal .clear{background:transparent;border:1px solid #243748;color:#cfeaff;border-radius:10px;padding:8px 12px;cursor:pointer}
.modal .close{background:#4ade80;color:#001c09;border:none;border-radius:10px;padding:8px 14px;font-weight:800;cursor:pointer}
.empty{color:#8aa1b1;background:#0e151a;border:1px dashed #223342;border-radius:10px;padding:14px;text-align:center}

/* wallet list inside upgrade modal */
.walletRow{display:flex;gap:10px;align-items:center;padding:10px;border-radius:10px;background:#081016;border:1px solid #12313b;margin-bottom:8px}
.walletRow .info{flex:1}
.walletRow .actions{display:flex;gap:8px}

/* premium notice */
.pNotice{background:#07170f;border:1px solid #183424;color:#9ff3c1;padding:10px;border-radius:10px;margin-bottom:10px;text-align:center}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>Crypto Tweet Factory</h1>
    <div class="right">
      <button id="openSaved" class="savedBtn">📁 Saved Tweets</button>
      <div id="premiumWrap"></div>
      <div class="tipWrap" id="tipWrap" data-tip="Click to copy wallet address">
        <button id="tipBtn" class="tipBtn">💰 Tip SOL</button>
      </div>
      <button id="openUpgrade" class="upgBtn">💎 Upgrade</button>
    </div>
  </header>

  <div class="topbar">
    <input id="search" class="input" placeholder="Type a keyword (e.g., PEPE, Solana, Elon)">
    <select id="tone">
      <option value="funny">Funny 😆</option>
      <option value="sarcastic">Sarcastic 😏</option>
      <option value="motivational">Motivational 💪</option>
    </select>
    <select id="length">
      <option value="short" selected>Short (≤150)</option>
      <option value="long">Long (200–250)</option>
    </select>
    <button id="searchGen" class="btn gen">Generate</button>
  </div>

  <div id="tweet-box">Loading tweet...</div>
  <div class="countdown" id="countdown"></div>

  <div class="actions">
    <button id="back" class="btn back">⬅ Back</button>
    <button id="generate" class="btn gen">Generate</button>
    <button id="tweet" class="btn tweet">Tweet</button>
    <button id="copy" class="btn copy">Copy</button>
    <button id="auto" class="btn toggle">Auto Mode : OFF</button>
    <select id="speed" class="speedSelect">
      <option value="3000">3s</option>
      <option value="5000" selected>5s</option>
      <option value="10000">10s</option>
      <option value="15000">15s</option>
    </select>
  </div>
</div>

<!-- Saved Tweets Modal -->
<div id="modalOverlay" class="modalOverlay" aria-hidden="true">
  <div class="modal" role="dialog" aria-modal="true" aria-labelledby="savedTitle">
    <header>
      <h2 id="savedTitle">Saved Tweets</h2>
      <span style="color:#8aa1b1;font-size:12px">Stored locally in your browser</span>
    </header>
    <div class="body" id="savedBody"></div>
    <div class="footer">
      <button id="clearAll" class="clear">🧹 Clear All</button>
      <button id="closeModal" class="close">✖ Close</button>
    </div>
  </div>
</div>

<!-- Upgrade Modal -->
<div id="upgradeOverlay" class="modalOverlay" aria-hidden="true">
  <div class="modal" role="dialog" aria-modal="true" aria-labelledby="upgradeTitle">
    <header>
      <h2 id="upgradeTitle">Upgrade to Premium</h2>
      <span style="color:#8aa1b1;font-size:12px">Pay the requested amount and mark as paid below</span>
    </header>
    <div class="body">
      <div class="pNotice">
        <strong>Manual upgrade flow:</strong> Send the requested payment manually from your wallet. After sending, paste the transaction ID (TXID) below and click "Mark as Paid".
      </div>

      <!-- Wallet options -->
      <div id="wallets">
        <!-- Phantom (SOL) -->
        <div class="walletRow">
          <div class="info">
            <strong>Phantom (SOL)</strong>
            <div style="font-size:13px;color:#9fb">Recommended for SOL payments</div>
            <div style="margin-top:6px;font-size:13px;color:#bfc">Send <strong>0.2 SOL</strong> to this address:</div>
            <div style="font-family:monospace;margin-top:6px;color:#cfeaff">77M7waPc3o6uEpwHxnNRzJ9Ga47gnFPZVJKu3AP1mRjv</div>
          </div>
          <div class="actions">
            <button class="btn copyWallet" data-wallet="phantom">Copy</button>
            <button class="btn openWallet" data-wallet="phantom">Open Phantom</button>
          </div>
        </div>

        <!-- Trust Wallet (Note: Trust is EVM and may not support SOL natively) -->
        <div class="walletRow">
          <div class="info">
            <strong>Trust Wallet (EVM)</strong>
            <div style="font-size:13px;color:#9fb">Trust is primarily EVM – for SOL use Phantom. You can still pay via ETH if you accept ETH payments.</div>
            <div style="margin-top:6px;font-size:13px;color:#bfc">Send <strong>0.02 ETH</strong> to this ETH address (if you accept ETH):</div>
            <div style="font-family:monospace;margin-top:6px;color:#cfeaff">0xYourEthAddressHere</div>
          </div>
          <div class="actions">
            <button class="btn copyWallet" data-wallet="trusteth">Copy</button>
            <button class="btn openWallet" data-wallet="trust">Open Trust</button>
          </div>
        </div>

        <!-- MetaMask (EVM) -->
        <div class="walletRow">
          <div class="info">
            <strong>MetaMask (EVM)</strong>
            <div style="font-size:13px;color:#9fb">MetaMask is for ETH/Layer-2 chains; cannot send SOL natively.</div>
            <div style="margin-top:6px;font-size:13px;color:#bfc">Send <strong>0.02 ETH</strong> to:</div>
            <div style="font-family:monospace;margin-top:6px;color:#cfeaff">0xYourEthAddressHere</div>
          </div>
          <div class="actions">
            <button class="btn copyWallet" data-wallet="metamask">Copy</button>
            <button class="btn openWallet" data-wallet="metamask">Open MetaMask</button>
          </div>
        </div>
      </div>

      <hr style="border:none;border-top:1px solid #13212c;margin:12px 0">

      <div style="margin-bottom:8px">After you've sent the payment, paste the transaction ID (TXID) here and click <strong>Mark as Paid</strong>. This will locally unlock Premium in your browser.</div>
      <input id="txid" placeholder="Enter TXID (transaction id)" style="width:100%;padding:10px;border-radius:10px;border:1px solid #13212c;background:#07121a;color:#dfe8f4;margin-bottom:8px" />
      <div style="display:flex;gap:8px;justify-content:flex-end">
        <button id="markPaid" class="btn gen">Mark as Paid</button>
        <button id="closeUpgrade" class="close">Close</button>
      </div>
    </div>
  </div>
</div>

<script>
// ===== Config & addresses =====
const WALLET_SOL = "77M7waPc3o6uEpwHxnNRzJ9Ga47gnFPZVJKu3AP1mRjv";
const WALLET_ETH = "0xYourEthAddressHere"; // replace with real ETH address if you want ETH option
const STORAGE_KEY = "ctf_v9_memory";
const PREMIUM_KEY = "ctf_premium";
const PREMIUM_AMOUNT_SOL = 0.2; // suggested amount

// ===== Small helper functions =====
function by(id){return document.getElementById(id)}
function rnd(a){return a[Math.floor(Math.random()*a.length)]}
function clamp(str,max){return str.length>max?str.slice(0,max-1)+"…":str;}

// ===== UI elements =====
const tipWrap = by("tipWrap"), tipBtn = by("tipBtn"), openSaved = by("openSaved"),
      openUpgrade = by("openUpgrade"), upgradeOverlay = by("upgradeOverlay"),
      modalOverlay = by("modalOverlay"), savedBody = by("savedBody"),
      clearAll = by("clearAll"), closeModal = by("closeModal"),
      txidInput = by("txid"), markPaid = by("markPaid"), closeUpgrade = by("closeUpgrade"),
      copyWalletBtns = document.querySelectorAll(".copyWallet"), openWalletBtns = document.querySelectorAll(".openWallet"),
      premiumWrap = by("premiumWrap");

// ===== Memory & premium state =====
function loadMem(){ try{ const r = localStorage.getItem(STORAGE_KEY); return r?new Set(JSON.parse(r)):new Set(); }catch{return new Set(); } }
function saveMem(s){ try{ localStorage.setItem(STORAGE_KEY, JSON.stringify([...s])); }catch{} }
let memory = loadMem();
function remember(t){ memory.add(t); saveMem(memory); }

function isPremium(){ return localStorage.getItem(PREMIUM_KEY) === "true"; }
function setPremium(val){ if(val) localStorage.setItem(PREMIUM_KEY,"true"); else localStorage.removeItem(PREMIUM_KEY); renderPremiumBadge(); renderSavedList(); }

// show badge if premium
function renderPremiumBadge(){
  premiumWrap.innerHTML = "";
  if(isPremium()){
    const b = document.createElement("div"); b.className = "premiumBadge"; b.textContent = "⭐ Premium";
    premiumWrap.appendChild(b);
  }
}

// ===== Basic core generator (same as before) =====
const intros=["Honestly","Some days","Not gonna lie","Lately","Tell me why","Apparently","It's wild that","Sometimes I feel like","Just realized","You ever notice"];
const subjects=["the market","crypto","this coin","my portfolio","the charts","everything on-chain","my trades","my patience","these bags","my screen time"];
const actions=["keeps testing me","just wants chaos","never lets me rest","makes zero sense","is playing games","won’t calm down","acts like it’s personal","keeps reminding me I know nothing","is allergic to stability","has its own mood swings"];
const reactions=["and yet here I am still watching.","but I’m still here pretending I understand.","and I can’t even be mad anymore.","and I swear it’s funny now.","but deep down I love the drama.","and I keep thinking I’ve learned something.","so I’ll just keep coping in HD.","while acting like it’s all part of my plan.","and I’m taking it personally.","but this is the life I chose."];
const addons=["Trying to stay calm but my coffee’s doing the heavy lifting.","At this point I’m trading emotions more than tokens.","Every dip feels like character development.","Still waiting for a green candle that respects me.","My phone battery fears price alerts now.","Maybe the real bull run was the coping we did along the way."];

function rndArr(a){ return a[Math.floor(Math.random()*a.length)]; }
function buildSentence(keyword,len){
  const kw=(keyword||"").trim(); const kwInsert = kw ? ` ${kw}` : "";
  const spice = Math.random()<0.35 ? ["honestly","apparently","somehow","for reasons unknown","low-key"][Math.floor(Math.random()*5)]+" " : "";
  if(len==="short"){
    let s = `${rndArr(intros)} ${spice}${rndArr(subjects)}${kwInsert} ${rndArr(actions)} ${rndArr(reactions)}`;
    return clamp(s,150);
  } else {
    let s = `${rndArr(intros)} ${spice}${rndArr(subjects)}${kwInsert} ${rndArr(actions)} ${rndArr(reactions)} ${rndArr(addons)}`;
    return clamp(s,250);
  }
}

// ===== Storage helpers & saved modal rendering =====
function loadMemory(){ try{ const r = localStorage.getItem(STORAGE_KEY); return r?new Set(JSON.parse(r)):new Set(); }catch{return new Set(); } }
function saveMemorySet(s){ try{ localStorage.setItem(STORAGE_KEY, JSON.stringify([...s])); }catch{} }
function renderSavedList(){
  savedBody.innerHTML = "";
  const items = [...memory];
  if(items.length === 0){
    const e = document.createElement("div"); e.className="empty"; e.textContent="No saved tweets yet. Generate some and they’ll appear here.";
    savedBody.appendChild(e); return;
  }
  items.forEach(txt=>{
    const row = document.createElement("div"); row.className="row";
    const p = document.createElement("div"); p.className="text"; p.textContent = txt;
    const actions = document.createElement("div");
    actions.style.display = "flex";
    actions.style.gap = "8px";
    // delete
    const del = document.createElement("button"); del.className="del"; del.textContent="🗑 Delete";
    del.onclick = ()=>{ memory.delete(txt); saveMemorySet(memory); renderSavedList(); };
    // export (premium-only)
    if(isPremium()){
      const exp = document.createElement("button"); exp.className="del"; exp.textContent="⬇ Export";
      exp.onclick = ()=> { downloadText(txt.replace(/\n/g,' '), 'tweet.txt'); };
      actions.appendChild(exp);
    }
    actions.appendChild(del);
    row.appendChild(p); row.appendChild(actions);
    savedBody.appendChild(row);
  });
}

// download helper
function downloadText(text, filename){
  const blob = new Blob([text], {type: 'text/plain;charset=utf-8'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href = url; a.download = filename; document.body.appendChild(a);
  a.click(); a.remove(); URL.revokeObjectURL(url);
}

// ===== Premium helper (export all) =====
function exportAll(){
  if(!isPremium()){ alert("This is a Premium feature. Upgrade to export all saved tweets."); return; }
  const arr = [...memory];
  if(arr.length===0){ alert("No saved tweets to export."); return; }
  const txt = arr.join("\n\n");
  downloadText(txt, "saved-tweets.txt");
}

// ===== Wallet UI functions (copy/open) =====
function tryOpenSolana(uri){
  // Try open via window.open; may be blocked in some browsers
  try{ const w = window.open(uri); if(!w) window.location.href = uri; }catch(e){ window.location.href = uri; }
}
function copyToClipboard(text){ return navigator.clipboard.writeText(text); }

// copy/open handlers
copyWalletBtns.forEach(btn=>{
  btn.addEventListener('click', async (e)=>{
    const w = btn.getAttribute('data-wallet');
    if(w === "phantom"){
      await copyToClipboard(WALLET_SOL);
      alert("SOL address copied to clipboard: " + WALLET_SOL);
    } else if(w === "trusteth" || w === "metamask"){
      await copyToClipboard(WALLET_ETH);
      alert("ETH address copied to clipboard: " + WALLET_ETH);
    }
  });
});
openWalletBtns.forEach(btn=>{
  btn.addEventListener('click',(e)=>{
    const w = btn.getAttribute('data-wallet');
    if(w === "phantom"){
      // try open solana URI to prompt wallet apps to open
      tryOpenSolana(`solana:${WALLET_SOL}?amount=${encodeURIComponent(PREMIUM_AMOUNT_SOL)}`);
    } else if(w === "trust"){
      // open trustwallet link (deep link) – best effort
      // trustwallet://send?address=<address>&asset=ETH
      window.open(`https://link.trustwallet.com/send?address=${WALLET_ETH}`, "_blank");
    } else if(w === "metamask"){
      // metamask deep links are limited; open metamask website instruction
      window.open("https://metamask.io/", "_blank");
    }
  });
});

// ===== Mark as paid flow (manual) =====
markPaid.addEventListener('click', ()=>{
  const tx = txidInput.value.trim();
  if(!tx){
    alert("Please paste a transaction ID (TXID) that you received after payment.");
    return;
  }
  // NOTE: This is manual: we don't verify the TXID. We simply mark local machine as premium.
  if(confirm("By clicking OK you confirm you've sent the payment. This will mark this browser as Premium.")){
    localStorage.setItem(PREMIUM_KEY, "true");
    renderPremiumBadge();
    renderSavedList();
    alert("Marked as Premium locally. Premium features unlocked in this browser.");
    upgradeOverlay.style.display = "none";
  }
});
closeUpgrade.addEventListener('click', ()=>{ upgradeOverlay.style.display = "none"; });

// ===== Modal open/close for saved tweets =====
openSaved.addEventListener('click', ()=>{
  modalOverlay.style.display = "flex"; renderSavedList();
});
closeModal.addEventListener('click', ()=>{ modalOverlay.style.display = "none"; });
modalOverlay.addEventListener('click', (e)=>{ if(e.target === modalOverlay) modalOverlay.style.display = "none"; });
clearAll.addEventListener('click', ()=>{ if(confirm("Delete ALL saved tweets? This cannot be undone.")){ memory = new Set(); saveMemorySet(memory); renderSavedList(); } });

// ===== core generator + UI (same behavior as before with uniqueness checks) =====
const STORAGE_MEM = STORAGE_KEY;
function loadMemorySet(){ try{ const r = localStorage.getItem(STORAGE_MEM); return r?new Set(JSON.parse(r)):new Set(); }catch{return new Set(); } }
function saveMemorySet2(s){ try{ localStorage.setItem(STORAGE_MEM, JSON.stringify([...s])); }catch{} }
memory = loadMemorySet();

let current="", history=[], autoTimer=null, countTimer=null;
let INTERVAL = 5000; // default 5s

const tweetBox = document.getElementById("tweet-box"), searchEl = document.getElementById("search"),
toneEl = document.getElementById("tone"), lenEl = document.getElementById("length"),
countdown = document.getElementById("countdown"), btnGen = document.getElementById("generate"),
btnSearch = document.getElementById("searchGen"), btnTweet = document.getElementById("tweet"),
btnCopy = document.getElementById("copy"), btnAuto = document.getElementById("auto"),
btnBack = document.getElementById("back"), speedSel = document.getElementById("speed");

function isDup(t){ return memory.has(t); }
function remember2(t){ memory.add(t); saveMemorySet2(memory); }

function genUnique(){
  const kw = searchEl.value; const len = lenEl.value;
  let attempts = 0; let t = "";
  do{
    t = buildSentence(kw, len);
    attempts++;
    if(attempts > 60){ t = t + " (still learning)"; break; }
  } while(isDup(t));
  return t;
}

function showTweet(t){ tweetBox.textContent = t; }
function updateBack(){ btnBack.disabled = !history.length; btnBack.style.opacity = history.length ? 1 : 0.5; }

function generate(){
  if(current){ history.push(current); if(history.length>10) history.shift(); }
  const t = genUnique();
  current = t; showTweet(t); remember2(t); updateBack(); resetCountdown();
}
function back(){ if(!history.length) return; current = history.pop(); showTweet(current); updateBack(); resetCountdown(); }

function resetCountdown(){
  clearInterval(countTimer);
  if(!autoTimer){ countdown.textContent = ""; return; }
  let sec = Math.ceil(INTERVAL/1000);
  countdown.textContent = "Next tweet in: " + sec + " s";
  countTimer = setInterval(()=>{ sec--; countdown.textContent = "Next tweet in: "+sec+" s"; if(sec<=0){ clearInterval(countTimer); countdown.textContent = "Generating…"; } }, 1000);
}
function setAuto(on){
  if(on){ if(autoTimer) return; btnAuto.textContent = "Auto Mode : ON"; autoTimer = setInterval(generate, INTERVAL); }
  else { btnAuto.textContent = "Auto Mode : OFF"; clearInterval(autoTimer); autoTimer = null; }
  resetCountdown();
}

// speed selector changes interval; if not premium and choosing <5000 disallow
speedSel.addEventListener('change', ()=>{
  const val = parseInt(speedSel.value,10) || 5000;
  if(val < 5000 && !isPremium()){
    alert("Shorter intervals (<5s) are Premium. Click Upgrade to unlock.");
    // revert to 5000
    speedSel.value = "5000";
    return;
  }
  INTERVAL = val;
  if(autoTimer){ setAuto(false); setAuto(true); }
});

// buttons
btnGen.onclick = generate;
btnSearch.onclick = generate;
btnTweet.onclick = ()=> window.open("https://twitter.com/intent/tweet?text=" + encodeURIComponent(current), "_blank");
btnCopy.onclick = async ()=> { await navigator.clipboard.writeText(current); btnCopy.textContent = "Copied!"; setTimeout(()=>btnCopy.textContent = "Copy", 1200); };
btnAuto.onclick = ()=> setAuto(!autoTimer);
btnBack.onclick = back;

// upgrade button opens modal
openUpgrade.addEventListener('click', ()=>{ upgradeOverlay.style.display = "flex"; });

// when marking premium via UI, set flag (already handled above via markPaid)
// also add quick "Unlock for free" dev helper? (no — security)

/* initialize */
renderPremiumBadge();
generate();
updateBack();
setAuto(false);

// Expose exportAll to global for use if needed
window.exportAllSavedTweets = exportAll;

// small UX: if user is premium, allow exportAll button to appear in saved modal
// we already render export buttons per item; also add global export holder
if(isPremium()){
  // nothing extra needed; save modal provides per-item export; we also expose function exportAllSavedTweets
}
</script>
</body>
</html>
