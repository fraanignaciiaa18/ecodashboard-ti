# ecodashboard-ti
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>EcoDashboard TI</title>
<style>
:root{--g9:#12392f;--g7:#1b7f64;--g5:#27a98b;--g1:#dff3ea;--bg:#f3f6f4;--tx:#1f2d28;--mu:#5d6b66;--bd:#d9e2de;--rd:#b8392f;--am:#9a6700}
*{box-sizing:border-box}
body{margin:0;font:15px/1.5 system-ui,"Segoe UI",Arial,sans-serif;background:var(--bg);color:var(--tx)}
header{background:var(--g9);color:#fff;position:sticky;top:0;z-index:5;display:flex;flex-wrap:wrap;align-items:center;gap:6px 20px;padding:10px 20px}
header h1{font-size:20px;margin:0;margin-right:auto}
nav{display:flex;flex-wrap:wrap;gap:2px}
nav a{color:#cfeee2;text-decoration:none;padding:6px 10px;border-radius:6px;font-size:14px}
nav a:hover,nav a:focus-visible{background:#ffffff26;outline:none}
.warn{background:#fff4d6;border-bottom:1px solid #ecd28a;color:#5f4400;text-align:center;padding:8px 12px;font-weight:600;font-size:14px}
main{max-width:1200px;margin:0 auto;padding:20px}
section{margin-bottom:30px;scroll-margin-top:70px}
h2{font-size:19px;color:var(--g9);margin:0 0 12px}
h3{margin:0 0 6px;font-size:17px}
.grid{display:grid;gap:12px;grid-template-columns:repeat(auto-fit,minmax(200px,1fr))}
.card{background:#fff;border:1px solid var(--bd);border-radius:10px;padding:14px}
.k{font-size:13px;color:var(--mu)}
.v{font-size:25px;font-weight:700;color:var(--g7)}
.v small{font-size:13px;font-weight:600;color:var(--mu)}
.card.sim{border-color:var(--g5);background:var(--g1)}
.card.al .v{color:var(--rd)}
.tag{display:inline-block;font-size:12px;font-weight:700;padding:2px 8px;border-radius:99px;background:#e6ebe8;color:var(--mu)}
.btn{font:inherit;font-weight:600;border:0;border-radius:8px;padding:9px 16px;cursor:pointer;background:var(--g7);color:#fff}
.btn:hover{background:var(--g9)}
.btn.sec{background:#fff;color:var(--g9);border:1px solid var(--g7)}
.btn.sec:hover{background:var(--g1)}
.btn:focus-visible,.lab:focus-visible{outline:3px solid #f2b100;outline-offset:2px}
header .btn{background:#ffffff1f;border:1px solid #ffffff66}
.lab{text-align:left;font:inherit;color:inherit;cursor:pointer;width:100%}
.lab.sel{border:2px solid var(--g7);background:#f5fbf8}
.bar{height:8px;background:#e6ebe8;border-radius:4px;margin-top:8px;overflow:hidden}
.bar i{display:block;height:100%;background:var(--g5)}
.bd{font-size:12px;font-weight:700;padding:2px 8px;border-radius:99px;margin-left:6px}
.Pendiente{background:#fde3df;color:var(--rd)}
.Atendida{background:#fff0c9;color:var(--am)}
.Resuelta{background:#d5f1e4;color:var(--g7)}
.split{display:grid;gap:12px;grid-template-columns:1fr}
@media(min-width:900px){.split{grid-template-columns:2fr 3fr}}
.tw{overflow-x:auto;max-height:420px;overflow-y:auto}
table{border-collapse:collapse;width:100%;font-size:14px}
th,td{padding:7px 10px;text-align:left;border-bottom:1px solid var(--bd)}
th{background:var(--g9);color:#fff;position:sticky;top:0}
.st{font-weight:600}.Encendido{color:var(--rd)}.En.espera{color:var(--am)}
.s-Encendido{color:var(--rd)}.s-Espera{color:var(--am)}.s-Apagado{color:var(--g7)}
.row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:10px 0}
#msg{min-height:22px;font-weight:600;color:var(--g7)}
.alert{display:flex;flex-wrap:wrap;gap:8px 14px;align-items:center;justify-content:space-between;margin-bottom:8px}
.fx td:first-child{width:34%}
dialog{border:0;border-radius:12px;padding:22px;max-width:420px;box-shadow:0 10px 40px #0005}
dialog::backdrop{background:#0007}
input{font:inherit;padding:8px 10px;border:1px solid var(--bd);border-radius:8px;width:220px}
footer{text-align:center;color:var(--mu);font-size:13px;padding:10px 20px 30px}
</style>
</head>
<body>
<header>
  <h1>🌱 EcoDashboard TI</h1>
  <nav aria-label="Secciones">
    <a href="#panel">Panel</a><a href="#labs">Laboratorios</a><a href="#alertas">Alertas</a><a href="#impacto">Impacto ambiental</a><a href="#ods">ODS</a><a href="#historial">Historial</a>
  </nav>
  <button class="btn" onclick="reiniciar()">Reiniciar simulación</button>
</header>
<main>
<section id="panel"><h2>Panel de indicadores</h2><div class="grid" id="kpis"></div></section>

<section id="labs"><h2>Laboratorios</h2>
  <div class="split">
    <div id="lablist" class="grid" style="grid-template-columns:1fr"></div>
    <div class="card">
      <div id="det"></div>
      <div class="row">
        <button class="btn sec" onclick="selTodos()">Seleccionar / quitar todos</button>
        <button class="btn" id="bap" onclick="apagar()">Simular apagado seguro</button>
        <span id="nsel" class="k"></span>
      </div>
      <div id="msg" role="status" aria-live="polite"></div>
      <div class="tw"><table><thead><tr><th></th><th>Equipo</th><th>Estado</th><th>Consumo inactivo</th></tr></thead><tbody id="tb"></tbody></table></div>
    </div>
  </div>
</section>

<section id="alertas"><h2>Alertas de consumo fuera de horario</h2><div id="als"></div></section>

<section id="impacto"><h2>Impacto ambiental</h2>
  <p class="k">Se distinguen tres tipos de cifras: <b>estimado</b> (supuesto de partida), <b>potencial</b> (escenario ilustrativo del 50 %, no medido) y <b>simulado</b> (cambia con las acciones hechas en esta aplicación; tampoco es medición real).</p>
  <div class="card" style="margin-bottom:12px"><label for="tar"><b>Tarifa eléctrica en CLP/kWh (opcional)</b></label>
    <div class="row"><input id="tar" type="number" min="0" step="any" placeholder="Sin tarifa verificada" oninput="render()"><span class="k">Déjalo vacío si no tienes una tarifa verificada (por ejemplo, la boleta de la sede). No se asume ningún valor.</span></div></div>
  <div class="grid" id="imp"></div>
  <div class="card" style="margin-top:12px"><h3>Fórmulas aplicadas (laboratorio tipo)</h3>
    <div class="tw"><table class="fx"><tbody id="fx"></tbody></table></div>
    <p class="k">Supuestos académicos del Certamen 1: 30 equipos por laboratorio, 15 W en modo inactivo, 118 h/semana fuera de uso, 4 semanas/mes y 0,38 kg CO₂eq/kWh (factor heredado; debe verificarse con Energía Abierta). No se calculan pesos chilenos porque no hay una tarifa verificada.</p>
  </div>
</section>

<section id="ods"><h2>Sustentabilidad y ODS</h2>
  <div class="grid">
    <div class="card"><h3>ODS 7 · Energía asequible y no contaminante</h3><p>Al hacer visible el consumo en espera, la solución busca apoyar un uso más eficiente de la electricidad.</p></div>
    <div class="card"><h3>ODS 12 · Producción y consumo responsables</h3><p>Promueve hábitos de cierre y una gestión más responsable de los equipos de los laboratorios.</p></div>
    <div class="card"><h3>ODS 13 · Acción por el clima</h3><p>Reducir kWh innecesarios podría disminuir emisiones indirectas asociadas a la electricidad.</p></div>
  </div>
  <p class="k">Es una propuesta con datos simulados: aún no existen resultados reales ni comprobados.</p>
</section>

<section id="historial"><h2>Historial de acciones</h2>
  <div class="card tw"><table><thead><tr><th>Fecha y hora</th><th>Laboratorio</th><th>Equipos afectados</th><th>Acción</th></tr></thead><tbody id="log"></tbody></table></div>
</section>
</main>
<footer>EcoDashboard TI · Certamen 2 · Sustentabilidad y Medioambiente · IP Virginio Gómez, sede Los Ángeles</footer>

<dialog id="dlg"><p id="dm"></p><div class="row" style="justify-content:flex-end"><button class="btn sec" id="no">Cancelar</button><button class="btn" id="si">Confirmar</button></div></dialog>

<script>
const P=0.015,H=118,W=4,F=0.38,N=30,OFF="Apagado simulado";
const f=(n,d=1)=>n.toLocaleString("es-CL",{minimumFractionDigits:d,maximumFractionDigits:d});
const $=id=>document.getElementById(id);
const mk=(id,pat)=>({id,nombre:"Laboratorio "+id,eq:Array.from({length:N},(_,i)=>({id:`PC-L${id}-${String(i+1).padStart(2,"0")}`,estado:pat(i)}))});
const init=()=>({labs:[mk(1,i=>i%4==0?"En espera":"Encendido"),mk(2,i=>i%2?"En espera":"Encendido"),mk(3,i=>i%3?"En espera":"Encendido")],al:{1:"Pendiente",2:"Pendiente",3:"Pendiente"},log:[],sel:1,chk:new Set()});
let S=init();
const act=l=>l.eq.filter(e=>e.estado!==OFF).length;
const kwh=n=>n*P*H*W;
const lab=()=>S.labs.find(l=>l.id===S.sel);
const say=t=>{$("msg").textContent=t};
const sc=e=>e==="Encendido"?"s-Encendido":e==="En espera"?"s-Espera":"s-Apagado";

function ask(m,cb){$("dm").textContent=m;$("si").onclick=()=>{$("dlg").close();cb()};$("no").onclick=()=>$("dlg").close();$("dlg").showModal()}
function reg(l,n,txt){S.log.unshift({t:new Date().toLocaleString("es-CL"),lab:l.nombre,n,txt})}

function pick(id){S.sel=id;S.chk.clear();say("");render()}
function tog(id,on){on?S.chk.add(id):S.chk.delete(id);$("nsel").textContent=S.chk.size+" equipo(s) seleccionado(s)"}
function selTodos(){const L=lab(),v=L.eq.filter(e=>e.estado!==OFF);
  if(S.chk.size===v.length)S.chk.clear();else v.forEach(e=>S.chk.add(e.id));render()}

function apagar(){
  const L=lab(),ids=L.eq.filter(e=>S.chk.has(e.id)&&e.estado!==OFF);
  if(!ids.length){say("Selecciona al menos un equipo encendido o en espera.");return}
  ask(`¿Simular el apagado seguro de ${ids.length} equipo(s) del ${L.nombre}? Solo cambia datos dentro de la aplicación; no apaga computadores reales.`,()=>{
    ids.forEach(e=>e.estado=OFF);S.chk.clear();
    const todo=act(L)===0;
    S.al[L.id]=todo?"Resuelta":"Atendida";
    reg(L,ids.length,"Apagado seguro simulado · alerta "+(todo?"resuelta":"atendida"));
    render();say(`Listo: ${ids.length} equipo(s) en "${OFF}". Alerta ${S.al[L.id].toLowerCase()}.`);
  });
}
function atender(id){const L=S.labs.find(l=>l.id===id);S.al[id]="Atendida";reg(L,0,"Alerta marcada como atendida (sin cambiar equipos)");render()}
function ir(id){pick(id);location.hash="#labs"}
function reiniciar(){S=init();$("tar").value="";render();say("Simulación reiniciada: datos iniciales restaurados.")}

function render(){
  const base=S.labs.reduce((a,l)=>a+kwh(l.eq.length),0),cur=S.labs.reduce((a,l)=>a+kwh(act(l)),0);
  const pend=Object.values(S.al).filter(a=>a==="Pendiente").length;
  const pot=base*0.5,red=base-cur,neq=S.labs.length*N,off=S.labs.reduce((a,l)=>a+l.eq.length-act(l),0),pct=off/neq*100,tar=parseFloat($("tar").value)||0;
  const K=(k,v,u,c="")=>`<div class="card ${c}"><div class="k">${k}</div><div class="v">${v} <small>${u}</small></div></div>`;
  $("kpis").innerHTML=K("Laboratorios simulados",S.labs.length,"")+K("Computadores simulados",neq,"")+
    K("Consumo innecesario mensual (estimado, actual)",f(cur),"kWh")+K("Emisiones equivalentes (estimadas, actual)",f(cur*F,2),"kg CO₂eq")+
    K("Ahorro potencial de energía (escenario 50 %)",f(pot),"kWh/mes")+K("Ahorro potencial de emisiones (escenario 50 %)",f(pot*F,2),"kg CO₂eq/mes")+
    K("Alertas pendientes",pend,"",pend?"al":"")+K("Equipos con cierre aplicado (indicador social, simulado)",f(pct,0),"%");

  $("lablist").innerHTML=S.labs.map(l=>{const a=act(l),al=S.al[l.id];
    return `<button class="card lab ${l.id===S.sel?"sel":""}" onclick="pick(${l.id})" aria-pressed="${l.id===S.sel}">
    <b>${l.nombre}</b><span class="bd ${al}">${al==="Resuelta"?"Sin equipos activos":"Alerta "+al.toLowerCase()}</span>
    <div class="k">${l.eq.length} equipos · ${a} activos</div><div>Consumo estimado: <b>${f(kwh(a))} kWh/mes</b></div>
    <div class="bar"><i style="width:${kwh(a)/kwh(N)*100}%"></i></div></button>`}).join("");

  const L=lab(),c=s=>L.eq.filter(e=>e.estado===s).length;
  $("det").innerHTML=`<h3>${L.nombre}</h3><p class="k">${L.eq.length} equipos · ${c("Encendido")} encendidos · ${c("En espera")} en espera · ${c(OFF)} apagados simulados<br>
    Consumo estimado: ${f(kwh(act(L)))} kWh/mes · Emisiones: ${f(kwh(act(L))*F,2)} kg CO₂eq/mes</p>`;
  $("tb").innerHTML=L.eq.map(e=>{const o=e.estado===OFF;
    return `<tr><td><input type="checkbox" aria-label="Seleccionar ${e.id}" ${o?"disabled":""} ${S.chk.has(e.id)?"checked":""} onchange="tog('${e.id}',this.checked)"></td><td>${e.id}</td><td class="st ${sc(e.estado)}">${e.estado}</td><td>${o?0:15} W</td></tr>`}).join("");
  $("nsel").textContent=S.chk.size+" equipo(s) seleccionado(s)";

  $("als").innerHTML=S.labs.map(l=>{const a=act(l),s=S.al[l.id];
    const t=a?`${a} equipos encendidos o en espera fuera del horario de uso.`:"Todos los equipos están en apagado simulado.";
    return `<div class="card alert"><div><b>${l.nombre}</b><span class="bd ${s}">${s}</span><div class="k">${t}</div></div>
    <div class="row" style="margin:0">${s==="Pendiente"?`<button class="btn sec" onclick="atender(${l.id})">Marcar como atendida</button>`:""}${a?`<button class="btn" onclick="ir(${l.id})">Ver equipos</button>`:""}</div></div>`}).join("");

  const sem=N*P*H,mes=sem*W,em=mes*F,ah=mes*0.5;
  const I=(t,k,v,u,c="")=>`<div class="card ${c}"><span class="tag">${t}</span><div class="k">${k}</div><div class="v">${v} <small>${u}</small></div></div>`;
  const eco=tar?I("ECONÓMICO · POTENCIAL","Ahorro económico ilustrativo (50 %)",f(pot*tar,0),"CLP/mes")+I("ECONÓMICO · SIMULADO","Ahorro económico de la simulación",f(red*tar,0),"CLP/mes","sim"):`<div class="card"><span class="tag">ECONÓMICO</span><div class="k">Sin tarifa verificada no se calcula ahorro en pesos chilenos.</div></div>`;
  const soc=I("SOCIAL · SIMULADO","Equipos con cierre aplicado (cumplimiento del protocolo)",f(pct,0),"%","sim");
  $("imp").innerHTML=I("ESTIMADO","Consumo base de los 3 laboratorios",f(base),"kWh/mes")+I("ESTIMADO","Emisiones base",f(base*F,2),"kg CO₂eq/mes")+
    I("POTENCIAL","Ahorro energético ilustrativo (50 %)",f(pot),"kWh/mes")+I("POTENCIAL","Emisiones potencialmente evitables",f(pot*F,2),"kg CO₂eq/mes")+
    I("SIMULADO","Reducción lograda en la simulación",f(red),"kWh/mes","sim")+I("SIMULADO","Emisiones reducidas en la simulación",f(red*F,2),"kg CO₂eq/mes","sim")+eco+soc;
  const R=(a,b,c)=>`<tr><td>${a}</td><td>${b}</td><td><b>${c}</b></td></tr>`;
  $("fx").innerHTML=R("Consumo semanal",`${N} × ${f(P,3)} kW × ${H} h`,f(sem)+" kWh")+R("Consumo mensual",`${f(sem)} × ${W} semanas`,f(mes)+" kWh")+
    R("Emisiones mensuales",`${f(mes)} × ${F.toString().replace(".",",")} kg CO₂eq/kWh`,f(em,2)+" kg CO₂eq")+R("Ahorro potencial (50 %)",`${f(mes)} × 0,50`,f(ah)+" kWh/mes")+
    R("Emisiones evitables potenciales",`${f(ah)} × 0,38`,f(ah*F,2)+" kg CO₂eq/mes")+
    R("Reducción simulada actual",`consumo base − consumo actual = ${f(base)} − ${f(cur)}`,f(red)+" kWh/mes")+R("Ahorro económico (opcional)","kWh × tarifa ingresada",tar?`${f(pot*tar,0)} CLP/mes (potencial)`:"Sin tarifa")+R("Cierre aplicado (social, simulado)","equipos apagados ÷ equipos totales × 100",f(pct,0)+" %");

  $("log").innerHTML=S.log.length?S.log.map(r=>`<tr><td>${r.t}</td><td>${r.lab}</td><td>${r.n}</td><td>${r.txt}</td></tr>`).join(""):`<tr><td colspan="4" class="k">Aún no hay acciones. Selecciona equipos y pulsa «Simular apagado seguro».</td></tr>`;
}
render();
</script>
</body>
</html>
