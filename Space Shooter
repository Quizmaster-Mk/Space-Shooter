<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Space Shooter</title>
<style>
  body {
    margin: 0;
    background: black;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    overflow: hidden;
  }
  canvas {
    border: 2px solid white;
  }
</style>
</head>
<body>
<canvas id="gameCanvas" width="400" height="600"></canvas>
<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

const ship = { x: 180, y: 550, width: 40, height: 40, speed: 5 };
let bullets = [];
let enemies = [];
let frame = 0;
let score = 0;
let gameOver = false;

// Contrôles
let keys = {};
document.addEventListener("keydown", e => keys[e.key] = true);
document.addEventListener("keyup", e => keys[e.key] = false);

// Tir
function shoot() {
  bullets.push({ x: ship.x + ship.width/2 - 5, y: ship.y, width: 10, height: 20 });
}

// Créer des ennemis
function createEnemy() {
  const x = Math.random() * (canvas.width - 40);
  enemies.push({ x: x, y: -40, width: 40, height: 40, speed: 2 + score/50 });
}

// Collision
function collision(a,b) {
  return a.x < b.x + b.width && a.x + a.width > b.x &&
         a.y < b.y + b.height && a.y + a.height > b.y;
}

// Boucle de jeu
function update() {
  if(gameOver) return;

  ctx.clearRect(0,0,canvas.width,canvas.height);

  // Déplacement vaisseau
  if(keys["ArrowLeft"] && ship.x > 0) ship.x -= ship.speed;
  if(keys["ArrowRight"] && ship.x + ship.width < canvas.width) ship.x += ship.speed;
  if(keys[" "] && frame % 10 === 0) shoot(); // tir toutes les 10 frames

  // Dessiner vaisseau
  ctx.fillStyle = "white";
  ctx.fillRect(ship.x, ship.y, ship.width, ship.height);

  // Déplacer et dessiner balles
  for(let i=bullets.length-1;i>=0;i--){
    let b = bullets[i];
    b.y -= 7;
    ctx.fillStyle="yellow";
    ctx.fillRect(b.x,b.y,b.width,b.height);
    if(b.y + b.height < 0) bullets.splice(i,1);
  }

  // Créer ennemis
  if(frame % 60 === 0) createEnemy();

  // Déplacer et dessiner ennemis
  for(let i=enemies.length-1;i>=0;i--){
    let e = enemies[i];
    e.y += e.speed;
    ctx.fillStyle="red";
    ctx.fillRect(e.x,e.y,e.width,e.height);

    // Collision avec vaisseau
    if(collision(e,ship)){
      gameOver = true;
    }

    // Collision avec balles
    for(let j=bullets.length-1;j>=0;j--){
      if(collision(e,bullets[j])){
        enemies.splice(i,1);
        bullets.splice(j,1);
        score++;
        break;
      }
    }

    // Si ennemi sort du bas
    if(e.y > canvas.height) enemies.splice(i,1);
  }

  // Afficher score
  ctx.fillStyle="white";
  ctx.font="20px Arial";
  ctx.fillText("Score: "+score,10,30);

  frame++;
  requestAnimationFrame(update);
}

update();
</script>
</body>
</html>
