<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #222;
            color: white;
            font-family: 'Arial', sans-serif;
        }
        canvas {
            border: 2px solid #fff;
        }
    </style>
    <title>Weapon Arsenal Game</title>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="600"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        const player = {
            x: 50,
            y: 50,
            size: 30,
            color: 'red',
            speed: 5,
            weapon: 'pistol'
        };

        const weapons = {
            pistol: { damage: 10, fireRate: 500 },
            machineGun: { damage: 5, fireRate: 100 }
        };

        let bullets = [];

        function drawPlayer() {
            ctx.fillStyle = player.color;
            ctx.fillRect(player.x, player.y, player.size, player.size);
        }

        function drawBullets() {
            ctx.fillStyle = 'white';
            bullets.forEach(bullet => {
                ctx.fillRect(bullet.x, bullet.y, 5, 5);
            });
        }

        function update() {
            if (keys['ArrowUp']) player.y -= player.speed;
            if (keys['ArrowDown']) player.y += player.speed;
            if (keys['ArrowLeft']) player.x -= player.speed;
            if (keys['ArrowRight']) player.x += player.speed;

            // Limit player within the canvas
            if (player.x < 0) player.x = 0;
            if (player.y < 0) player.y = 0;
            if (player.x + player.size > canvas.width) player.x = canvas.width - player.size;
            if (player.y + player.size > canvas.height) player.y = canvas.height - player.size;

            if (keys['1']) player.weapon = 'pistol';
            if (keys['2']) player.weapon = 'machineGun';

            if (keys['Space']) {
                const bullet = { x: player.x + player.size / 2, y: player.y + player.size / 2, damage: weapons[player.weapon].damage };
                bullets.push(bullet);
            }

            bullets = bullets.filter(bullet => bullet.x < canvas.width);

            bullets.forEach(bullet => {
                bullet.x += 10; // Bullet speed
            });
        }

        function drawWeaponInfo() {
            ctx.fillStyle = 'white';
            ctx.font = '16px Arial';
            ctx.fillText(`Weapon: ${player.weapon.toUpperCase()}`, 10, 20);
        }

        function drawGameInfo() {
            drawWeaponInfo();
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            drawPlayer();
            drawBullets();
            drawGameInfo();

            update();

            requestAnimationFrame(gameLoop);
        }

        const keys = {};

        window.addEventListener('keydown', (e) => {
            keys[e.key] = true;
        });

        window.addEventListener('keyup', (e) => {
            keys[e.key] = false;
        });

        gameLoop();
    </script>
</body>
</html>
