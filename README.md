<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Password Generator Pro</title>

<style>
body {
    margin: 0;
    min-height: 100vh;
    background: linear-gradient(135deg, #141e30, #243b55);
    font-family: "Segoe UI", Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
}

.generator {
    width: 420px;
    background: rgba(255,255,255,0.06);
    backdrop-filter: blur(12px);
    border-radius: 20px;
    padding: 25px;
    box-shadow: 0 25px 50px rgba(0,0,0,0.4);
}

h1 {
    text-align: center;
    margin-bottom: 20px;
    font-weight: 600;
}

.output {
    background: #0f172a;
    padding: 15px;
    border-radius: 12px;
    font-size: 16px;
    word-break: break-all;
    text-align: center;
    margin-bottom: 15px;
}

.controls {
    display: grid;
    gap: 10px;
    margin-bottom: 15px;
}

label {
    font-size: 14px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

input[type="range"] {
    width: 100%;
}

button {
    width: 100%;
    padding: 12px;
    background: #38bdf8;
    border: none;
    border-radius: 10px;
    font-weight: 600;
    cursor: pointer;
    transition: 0.3s;
}

button:hover {
    background: #0ea5e9;
    transform: translateY(-2px);
}

.strength {
    margin-top: 10px;
    height: 8px;
    background: #1e293b;
    border-radius: 10px;
    overflow: hidden;
}

.strength-bar {
    height: 100%;
    width: 0%;
    transition: 0.3s;
}
</style>
</head>
<body>

<div class="generator">
    <h1>Password Generator</h1>

    <div class="output" id="passwordOutput">Click generate</div>

    <div class="controls">
        <label>Password Length: <span id="lengthValue">12</span></label>
        <input type="range" id="length" min="6" max="32" value="12">

        <label><input type="checkbox" id="upper" checked> Include Uppercase</label>
        <label><input type="checkbox" id="lower" checked> Include Lowercase</label>
        <label><input type="checkbox" id="numbers" checked> Include Numbers</label>
        <label><input type="checkbox" id="symbols"> Include Symbols</label>
    </div>

    <button onclick="generatePassword()">Generate Password</button>

    <div class="strength">
        <div class="strength-bar" id="strengthBar"></div>
    </div>
</div>

<script>
const lengthSlider = document.getElementById("length");
const lengthValue = document.getElementById("lengthValue");
const output = document.getElementById("passwordOutput");
const strengthBar = document.getElementById("strengthBar");

lengthSlider.oninput = () => {
    lengthValue.textContent = lengthSlider.value;
};

function generatePassword() {
    const length = lengthSlider.value;
    const upper = document.getElementById("upper").checked;
    const lower = document.getElementById("lower").checked;
    const numbers = document.getElementById("numbers").checked;
    const symbols = document.getElementById("symbols").checked;

    let chars = "";
    if (upper) chars += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    if (lower) chars += "abcdefghijklmnopqrstuvwxyz";
    if (numbers) chars += "0123456789";
    if (symbols) chars += "!@#$%^&*()_+{}[]<>?";

    if (!chars) {
        output.textContent = "Select options first";
        return;
    }

    let password = "";
    for (let i = 0; i < length; i++) {
        password += chars.charAt(Math.floor(Math.random() * chars.length));
    }

    output.textContent = password;
    updateStrength(password);
}

function updateStrength(password) {
    let score = 0;
    if (password.length > 10) score++;
    if (/[A-Z]/.test(password)) score++;
    if (/[a-z]/.test(password)) score++;
    if (/[0-9]/.test(password)) score++;
    if (/[^A-Za-z0-9]/.test(password)) score++;

    let width = (score / 5) * 100;
    strengthBar.style.width = width + "%";

    if (width < 40) strengthBar.style.background = "#ef4444";
    else if (width < 70) strengthBar.style.background = "#facc15";
    else strengthBar.style.background = "#22c55e";
}
</script>

</body>
</html>
