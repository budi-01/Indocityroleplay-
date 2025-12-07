<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Indo City RP</title>
<style>
body { margin:0; overflow:hidden; font-family:sans-serif; }
#gui {
    position: absolute; top: 10px; left: 10px; color: white; 
    background: rgba(0,0,0,0.5); padding: 10px; border-radius:5px;
    z-index:100;
}
#commands {
    position: absolute; bottom: 10px; left: 10px; width:300px;
    z-index:100;
}
</style>
</head>
<body>
<canvas id="gameCanvas"></canvas>

<div id="gui">
    <div>Uang: <span id="money">0</span></div>
    <div>Health: <span id="health">200</span></div>
    <div>Vehicle: <span id="vehicle">None</span></div>
</div>

<div id="commands">
    <input type="text" id="cmdInput" placeholder="Ketik command /..." style="width:100%;padding:5px;">
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r152/three.min.js"></script>
<script>
// ===== Player & Role =====
const roles = {warga:{money:40000},polisi:{money:500000},ems:{money:500000},militer:{money:500000}};
class Player{
    constructor(name,role){
        this.name=name;this.role=role;
        this.money=roles[role].money;this.health=200;
        this.vehicle=null;this.position={x:0,y:0,z:0};
    }
    buy(price){if(this.money>=price){this.money-=price;updateGUI(this);}else console.log("Uang tidak cukup!");}
    eat(){this.buy(25);}
    treatment(){this.buy(25);}
}
function updateGUI(player){
    document.getElementById("money").innerText=player.money;
    document.getElementById("health").innerText=player.health;
    document.getElementById("vehicle").innerText=player.vehicle||"None";
}

// ===== Three.js Scene =====
const scene=new THREE.Scene();
scene.background=new THREE.Color(0x87CEEB);
const camera=new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1,1000);
camera.position.set(0,8,20);
const renderer=new THREE.WebGLRenderer({canvas:document.getElementById("gameCanvas")});
renderer.setSize(window.innerWidth,window.innerHeight);

// ===== Ground =====
const ground=new THREE.Mesh(new THREE.PlaneGeometry(200,200),new THREE.MeshBasicMaterial({color:0x228B22}));
ground.rotation.x=-Math.PI/2;scene.add(ground);

// ===== Vehicles =====
const vehicles={car:30000,hypercar:30000,jet:400000,tank:500000};
function spawnVehicle(player,type){
    if(player.money>=vehicles[type]){
        player.money-=vehicles[type];player.vehicle=type;updateGUI(player);
        let geo=new THREE.BoxGeometry(2,1,4);
        let mat=new THREE.MeshBasicMaterial({color:0xffff00});
        let v=new THREE.Mesh(geo,mat);
        v.position.set(player.position.x,0.5,player.position.z);
        v.name=type;scene.add(v);
        console.log(`${type} berhasil spawn!`);
    }else console.log("Uang tidak cukup!");
}

// ===== NPC =====
function spawnBot(jumlah){
    for(let i=0;i<jumlah;i++){
        let b=new THREE.Mesh(new THREE.BoxGeometry(1,2,1),new THREE.MeshBasicMaterial({color:0xff0000}));
        b.position.set(Math.random()*50-25,1,Math.random()*50-25);b.name="Bot"+(i+1);scene.add(b);
        console.log(`Bot ${i+1} spawn!`);
    }
}
function spawnGangster(jumlah){
    for(let i=0;i<jumlah;i++){
        let g=new THREE.Mesh(new THREE.BoxGeometry(1,2,1),new THREE.MeshBasicMaterial({color:0x0000ff}));
        g.position.set(Math.random()*50-25,1,Math.random()*50-25);g.name="Gangster"+(i+1);scene.add(g);
        console.log(`Gangster ${i+1} spawn!`);
    }
}

// ===== Command System =====
function executeCommand(cmd,player){
    cmd=cmd.toLowerCase();
    if(cmd==="/sit") console.log("Player duduk");
    else if(cmd==="/tepuk") console.log("Player tepuk tangan");
    else if(cmd==="/clear") console.log("Inventory dihapus");
    else if(cmd==="/spawn car") spawnVehicle(player,'car');
    else if(cmd==="/spawn hypercar") spawnVehicle(player,'hypercar');
    else if(cmd==="/spawn jet") spawnVehicle(player,'jet');
    else if(cmd==="/spawn tank") spawnVehicle(player,'tank');
    else if(cmd.startsWith("/spawn bot")) spawnBot(parseInt(cmd.split(" ")[2])||1);
    else if(cmd.startsWith("/spawn gangster")) spawnGangster(parseInt(cmd.split(" ")[2])||1);
    else console.log("Perintah tidak dikenal");
}

// ===== Kota Sederhana =====
function createCity(){
    for(let i=0;i<5;i++){
        let h=5+Math.random()*15;
        let b=new THREE.Mesh(new THREE.BoxGeometry(3,h,3),new THREE.MeshBasicMaterial({color:0x808080}));
        b.position.set(i*5, h/2, 0);scene.add(b);
    }
}
function createSmallCity(){
    for(let i=0;i<5;i++){
        let h=2+Math.random()*3;
        let b=new THREE.Mesh(new THREE.BoxGeometry(2,h,2),new THREE.MeshBasicMaterial({color:0xaaaaaa}));
        b.position.set(i*5, h/2, 10);scene.add(b);
    }
}
function createDesertCity(){
    for(let i=0;i<5;i++){
        let h=2+Math.random()*3;
        let b=new THREE.Mesh(new THREE.BoxGeometry(2,h,2),new THREE.MeshBasicMaterial({color:0xdeb887}));
        b.position.set(i*5, h/2, -10);scene.add(b);
    }
}
function createBandara(){
    let runway=new THREE.Mesh(new THREE.PlaneGeometry(50,5),new THREE.MeshBasicMaterial({color:0x333333}));
    runway.rotation.x=-Math.PI/2; runway.position.set(0,0.01,20);scene.add(runway);
}

// ===== Spawn Semua Kota =====
createCity(); createSmallCity(); createDesertCity(); createBandara();

// ===== Animate =====
function animate(){requestAnimationFrame(animate);renderer.render(scene,camera);}animate();

// ===== Spawn Player =====
let player1=new Player("Lutfi","militer");updateGUI(player1);spawnVehicle(player1,'jet');

// ===== Command Input =====
document.getElementById("cmdInput").addEventListener("keydown",(e)=>{
    if(e.key==="Enter"){executeCommand(e.target.value,player1);e.target.value="";}
});
</script>
</body>
</html>
