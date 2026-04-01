<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Smart Study Planner</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script src="https://unpkg.com/lucide@latest"></script>

<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Gowun+Dodum:wght@400;700&family=Orbitron:wght@400;700&display=swap');
body{font-family:'Gowun Dodum',sans-serif;background:#F0F9FF;margin:0}
.digital-font{font-family:'Orbitron',sans-serif}
.scrollbar-hide::-webkit-scrollbar{display:none}
</style>
</head>
<body>
<div id="root"></div>

<script type="text/babel">
const firebaseConfig={
apiKey:"AIzaSyBv14g9crV8vGbobK5cdVxwqlrr0EiM0fA",
authDomain:"bokk-55eae.firebaseapp.com",
projectId:"bokk-55eae",
storageBucket:"bokk-55eae.firebasestorage.app",
messagingSenderId:"757990079253",
appId:"1:757990079253:web:f3dbf171b2137f436bc71c"
};
firebase.initializeApp(firebaseConfig);
const auth=firebase.auth();
const db=firebase.firestore();

const {useState,useEffect,useRef}=React;
const APP_ID='planner-app-v1';

function App(){
const [user,setUser]=useState(null);
const [loading,setLoading]=useState(true);
const [seconds,setSeconds]=useState(0);
const [running,setRunning]=useState(false);
const timerRef=useRef(null);

useEffect(()=>{
const unsub=auth.onAuthStateChanged(async u=>{
if(!u) await auth.signInAnonymously();
else{setUser(u);setLoading(false);} });
return()=>unsub();},[]);

useEffect(()=>{
if(running){timerRef.current=setInterval(()=>setSeconds(s=>s+1),1000);} 
return()=>clearInterval(timerRef.current);
},[running]);

useEffect(()=>{setTimeout(()=>lucide.createIcons(),100);});

if(loading) return <div className="h-screen flex items-center justify-center text-xl">Loading planner...</div>

return (
<div className="h-screen flex items-center justify-center">
<div className="w-full max-w-[1100px] bg-white shadow-2xl rounded-xl p-10">

<h1 className="text-3xl font-black mb-6">스마트 스터디 플래너</h1>
<p className="text-xs text-gray-400 mb-6">UID : {user?.uid?.slice(0,10)}</p>

<div className="bg-sky-900 rounded-2xl p-6 text-center text-white">
<div className="text-5xl digital-font mb-4">
{String(Math.floor(seconds/60)).padStart(2,'0')}:{String(seconds%60).padStart(2,'0')}
</div>

<div className="flex gap-4 justify-center">
<button onClick={()=>setRunning(r=>!r)} className="px-6 py-2 bg-sky-400 text-sky-900 rounded-xl">
{running?'PAUSE':'START'}
</button>
<button onClick={()=>setSeconds(0)} className="px-6 py-2 bg-white text-sky-900 rounded-xl">
RESET
</button>
</div>
</div>

</div>
</div>
)}

ReactDOM.createRoot(document.getElementById('root')).render(<App/>);
</script>
</body>
</html>
