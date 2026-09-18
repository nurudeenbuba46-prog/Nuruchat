
‎html
‎<!DOCTYPE html>
‎<html lang="en">
‎<head>
‎<meta charset="UTF-8">
‎<meta name="viewport" content="width=device-width, initial-scale=1.0">
‎<title>NuruChat - Private Invite Only</title>
‎<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
‎<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
‎<style>*{margin:0;padding:0;box-sizing:border-box}body{font-family:Arial;background:#e8f5e9}.header{background:#0a5c36;color:#fff;padding:20px;text-align:center}.container{max-width:420px;margin:0 auto;padding:12px}.card{background:#fff;border-radius:16px;padding:18px;margin:12px 0;box-shadow:0 4px 12px rgba(0,0,0,.08)}.btn{background:#0a5c36;color:#fff;border:0;width:100%;padding:13px;border-radius:10px;font-weight:bold;margin-top:10px;cursor:pointer}.input{width:100%;padding:11px;border:1px solid #ccc;border-radius:9px;margin-top:9px}.hidden{display:none}.admin{background:#fff8db;border:2px solid gold}.code{background:#111;color:#0f0;padding:10px;border-radius:8px;margin-top:8px;font-family:monospace;word-break:break-all}</style>
‎</head>
‎<body>
‎<div class="header"><h2>N NuruChat</h2><p>Private • Invite Only • Free Video 50m</p></div>
‎<div class="container">
‎<div class="card" id="authCard">
‎<h3>🔐 Join - Invite Only</h3>
‎<input class="input" id="userName" placeholder="Your Name">
‎<input class="input" id="inviteCode" placeholder="Invite Code e.g. NURU-A7K9P2">
‎<button class="btn" onclick="register()">Join NuruChat</button>
‎<p id="authMsg" style="text-align:center;color:red;font-size:13px;margin-top:8px"></p>
‎<input class="input" id="adminPass" type="password" placeholder="Admin: admin123" style="margin-top:18px">
‎<button class="btn" style="background:#222" onclick="adminLogin()">Admin Login</button>
‎</div>
‎<div class="card hidden" id="mainCard"><h3 id="welcome"></h3><p style="font-size:12px;background:#e8f5e9;padding:8px;border-radius:8px;margin:8px 0">✅ Free Mode - WiFi Direct - No Data - Video 50m FREE</p><button class="btn" onclick="alert('Scanning 100m... Found Aisha 42m - Mayo Belwa!')">🔍 Find Nearby</button><button class="btn" style="background:green" onclick="alert('Free Video Call via WiFi Direct - NO DATA!')">📹 Free Video Call</button><button class="btn" style="background:#222" onclick="location.reload()">Logout</button></div>
‎<div class="card hidden admin" id="adminCard"><h3>👑 Admin - Nuru Only</h3><input class="input" id="inviteName" placeholder="Name to invite e.g. Aisha"><button class="btn" style="background:gold;color:#000" onclick="genCode()">Generate Invite Code</button><div id="latest"></div><h4 style="margin-top:12px">Invites (Firebase):</h4><div id="inviteList"></div><h4 style="margin-top:12px">Users Joined:</h4><div id="usersList"></div><button class="btn" style="background:#222" onclick="location.reload()">Logout</button></div>
‎</div>
‎<script>
‎const firebaseConfig = {
‎  apiKey: "AIzaSyC6DZjVOcRJK9dwNkidRsG3uiRqdZQE6Uw",
‎  authDomain: "nuruchat.firebaseapp.com",
‎  databaseURL: "https://nuruchat-default-rtdb.firebaseio.com",
‎  projectId: "nuruchat",
‎  storageBucket: "nuruchat.firebasestorage.app",
‎  messagingSenderId: "559461295922",
‎  appId: "1:559461295922:web:50bb9c1d32cd7b7ab4592b",
‎  measurementId: "G-ZR12HGE08W"
‎};
‎firebase.initializeApp(firebaseConfig);
‎const db = firebase.firestore();
‎
‎async function register(){
‎let name=document.getElementById('userName').value.trim();
‎let code=document.getElementById('inviteCode').value.trim().toUpperCase();
‎if(!name||!code){authMsg.innerText='Enter name & code';return;}
‎authMsg.innerText='Checking...';
‎try{
‎let doc=await db.collection('invites').doc(code).get();
‎if(!doc.exists){authMsg.innerText='Invalid code';return;}
‎if(doc.data().used){authMsg.innerText='Used by '+doc.data().usedBy;return;}
‎await db.collection('invites').doc(code).update({used:true,usedBy:name,usedAt:new Date().toISOString()});
‎await db.collection('users').add({name,code,joined:new Date().toISOString()});
‎authCard.classList.add('hidden');mainCard.classList.remove('hidden');
‎welcome.innerText='Welcome '+name+'!';
‎}catch(e){authMsg.innerText=e.message;}
‎}
‎async function adminLogin(){if(adminPass.value!=='admin123'){authMsg.innerText='Wrong password';return;}authCard.classList.add('hidden');adminCard.classList.remove('hidden');loadAdmin();}
‎async function genCode(){let n=inviteName.value||'Guest';let c='NURU-'+Math.random().toString(36).substr(2,6).toUpperCase();await db.collection('invites').doc(c).set({code:c,for:n,used:false,created:new Date().toISOString()});latest.innerHTML='<div class=code>'+c+' for '+n+'</div>';inviteName.value='';loadAdmin();}
‎async function loadAdmin(){let i=await db.collection('invites').orderBy('created','desc').limit(20).get();let h='';i.forEach(d=>{let x=d.data();h+=<p style=font-size:12px;margin:4px 0><b>${x.code}</b> → ${x.for} ${x.used?'':'[ACTIVE]'}</p>});inviteList.innerHTML=h||'No invites';let u=await db.collection('users').orderBy('joined','desc').limit(20).get();let hh='';u.forEach(d=>{hh+=<p style=font-size:12px>${d.data().name} - ${new Date(d.data().joined).toLocaleDateString()}</p>});usersList.innerHTML=hh||'No users';}
‎</script>
‎</body>
‎</html>
‎