# Gg5
Medicina 
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Doctora vs Superbacterias</title>

<style>
body {
    background: linear-gradient(to bottom, #020617, #0f172a);
    color: white;
    font-family: Arial;
    text-align: center;
}

h1 {
    color: #38bdf8;
}

.bar {
    width: 300px;
    height: 20px;
    background: #1e293b;
    margin: auto;
    border-radius: 10px;
    overflow: hidden;
}

.fill {
    height: 100%;
    background: #22c55e;
}

.enemyBar .fill {
    background: #ef4444;
}

button {
    padding: 10px;
    margin: 8px;
    font-size: 16px;
    border-radius: 10px;
    border: none;
    cursor: pointer;
}

button:hover {
    transform: scale(1.1);
}

#log {
    margin-top: 15px;
    min-height: 40px;
}
</style>

</head>
<body>

<h1>👩‍⚕️ Doctora vs Superbacterias 🦠</h1>

<p id="mission"></p>

<h3>Doctora</h3>
<div class="bar"><div id="doctorHP" class="fill"></div></div>

<h3 id="enemyName"></h3>
<div class="bar enemyBar"><div id="enemyHP" class="fill"></div></div>

<div>
    <button onclick="attack('paracetamol')">💊 Paracetamol</button>
    <button onclick="attack('amoxicilina')">🧪 Amoxicilina</button>
    <button onclick="attack('dexametasona')">⚡ Dexametasona</button>
</div>

<p id="log"></p>

<script>

let mission = 1;
let maxMissions = 7;

let doctor = {
    hp: 100,
    maxHp: 100
};

let enemy = newEnemy();

function newEnemy() {
    return {
        name: "🦠 Superbacteria Nivel " + mission,
        hp: 60 + mission * 10,
        maxHp: 60 + mission * 10,
        resistance: Math.random()
    };
}

function updateUI() {
    document.getElementById("mission").innerText = "Misión " + mission + " / 7";

    document.getElementById("doctorHP").style.width =
        (doctor.hp / doctor.maxHp * 100) + "%";

    document.getElementById("enemyHP").style.width =
        (enemy.hp / enemy.maxHp * 100) + "%";

    document.getElementById("enemyName").innerText = enemy.name;
}

function attack(type) {

    let damage = 0;
    let text = "";

    if(type === "paracetamol") {
        damage = rand(5,10);
        text = "Alivias síntomas 🔹";
    }

    if(type === "amoxicilina") {
        damage = rand(15,25);

        if(enemy.resistance > 0.5) {
            damage *= 0.5;
            text = "⚠️ Resistencia antibiótica!";
        } else {
            text = "💥 Ataque antibiótico efectivo!";
        }
    }

    if(type === "dexametasona") {
        enemy.resistance -= 0.2;
        doctor.hp += 5;
        text = "⚡ Inflamación reducida + curación leve";
        updateUI();
        document.getElementById("log").innerText = text;
        return;
    }

    enemy.hp -= damage;

    // Contraataque
    let enemyDamage = rand(5,15) + mission;
    doctor.hp -= enemyDamage;

    text += "\nDaño: " + damage + " | Recibes: " + enemyDamage;

    checkGame();
    updateUI();

    document.getElementById("log").innerText = text;
}

function checkGame() {

    if(doctor.hp <= 0) {
        alert("💀 Has caído en combate clínico...");
        location.reload();
    }

    if(enemy.hp <= 0) {
        mission++;

        if(mission > maxMissions) {
            alert("🏆 ¡Eres leyenda médica! Has vencido todas las superbacterias.");
            location.reload();
        }

        enemy = newEnemy();
        doctor.hp += 20;
        if(doctor.hp > doctor.maxHp) doctor.hp = doctor.maxHp;

        document.getElementById("log").innerText =
            "🔬 Nueva mutación detectada...";
    }
}

function rand(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

updateUI();

</script>

</body>
</html>
