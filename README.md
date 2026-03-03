<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Business Finder</title>

<style>
body{
  margin:0;
  font-family:'Segoe UI', Arial;
  background: linear-gradient(135deg,#141e30,#243b55);
  color:white;
  text-align:center;
}

header{
  padding:25px;
  font-size:34px;
  font-weight:bold;
  letter-spacing:2px;
  background:rgba(255,255,255,0.05);
  backdrop-filter: blur(10px);
  box-shadow:0 5px 20px rgba(0,0,0,0.5);
  cursor:pointer;
}

.container{
  width:90%;
  max-width:600px;
  margin:30px auto;
}

.card{
  background:rgba(255,255,255,0.08);
  backdrop-filter: blur(12px);
  padding:25px;
  border-radius:20px;
  box-shadow:0 15px 35px rgba(0,0,0,0.6);
  margin-bottom:25px;
}

select{
  width:100%;
  padding:12px;
  margin:15px 0;
  border-radius:12px;
  border:none;
  font-size:16px;
}

button{
  width:100%;
  padding:12px;
  border:none;
  border-radius:12px;
  font-size:16px;
  font-weight:bold;
  cursor:pointer;
  background: linear-gradient(45deg,#ff512f,#dd2476);
  color:white;
}

.business{
  background:white;
  color:black;
  padding:15px;
  margin:15px 0;
  border-radius:15px;
  box-shadow:0 10px 25px rgba(0,0,0,0.4);
}

.small{
  font-size:13px;
  color:gray;
}
</style>
</head>

<body>

<header id="appTitle">🌍 Business Finder</header>

<div class="container">

<div class="card">
<h3>Find Businesses in Your State</h3>
<select id="stateSelect"></select>
<button onclick="findBusiness()">🔎 Find Now</button>
</div>

<div id="result"></div>

</div>

<script>

let data = JSON.parse(localStorage.getItem("kFinderData")) || {states:{}};

function loadStates(){
  let select=document.getElementById("stateSelect");
  select.innerHTML="<option>Select State</option>";

  for(let state in data.states){
    select.innerHTML+=`<option>${state}</option>`;
  }
}

function findBusiness(){
  let state=document.getElementById("stateSelect").value;
  let result=document.getElementById("result");
  result.innerHTML="";

  if(!data.states[state]){
    result.innerHTML="<p>No businesses found.</p>";
    return;
  }

  let found=false;

  for(let district in data.states[state]){
    for(let category in data.states[state][district]){
      data.states[state][district][category].forEach(b=>{
        result.innerHTML+=`
          <div class="business">
            <b>${b}</b><br>
            <span class="small">${district} • ${category}</span>
          </div>
        `;
        found=true;
      });
    }
  }

  if(!found){
    result.innerHTML="<p>No businesses available.</p>";
  }
}

loadStates();


// 🔐 LONG PRESS ADMIN ENTRY (3 Seconds)

let timer;
let title = document.getElementById("appTitle");

title.addEventListener("mousedown", startPress);
title.addEventListener("touchstart", startPress);

title.addEventListener("mouseup", cancelPress);
title.addEventListener("mouseleave", cancelPress);
title.addEventListener("touchend", cancelPress);

function startPress(){
  timer = setTimeout(function(){
    let pass = prompt("Enter Admin Password");
    if(pass === "k012"){
      window.location.href="admin.html";
    } else {
      alert("Wrong Password");
    }
  },3000);
}

function cancelPress(){
  clearTimeout(timer);
}

</script>

</body>
</html>
