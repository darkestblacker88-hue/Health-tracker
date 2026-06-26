# Health-tracker
#This health tracker is track your health and sleep and this is give you options that is four option fighting, physic, #calisthenics, strength what's your choice


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Cybernetic Core v7.1</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-dark: #06090e; /* Intermediate Slate-Black */
            --card-base: rgba(12, 19, 30, 0.9); /* Premium intermediate contrast pane */
            --neon-blue: #00f0ff; /* Neon Cyber Blue */
            --neon-red: #ff2a2a; /* Futuristic Cyber Red */
            --white-shadow: rgba(255, 255, 255, 0.12); /* Soft intermediate white glow */
            --text-main: #f1f5f9;
            --text-muted: #64748b;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Rajdhani', sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            overflow-y: auto; /* FIXED: Now you can scroll easily! */
            min-height: 100vh;
            perspective: 1000px;
        }

        .cyber-grid {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-image: linear-gradient(rgba(0, 240, 255, 0.01) 1px, transparent 1px),
                              linear-gradient(90deg, rgba(0, 240, 255, 0.01) 1px, transparent 1px);
            background-size: 40px 40px;
            z-index: -1;
            opacity: 0.5;
        }

        /* --- CASCADING BOOT ANIMATION --- */
        @keyframes boxPopUp {
            0% {
                opacity: 0;
                transform: translateY(50px) rotateX(-15deg) rotateY(10deg) scale(0.95);
                filter: blur(4px);
            }
            100% {
                opacity: 1;
                transform: translateY(0) rotateX(-8deg) rotateY(8deg) scale(1);
                filter: blur(0);
            }
        }

        /* --- SETUP SCREEN --- */
        .setup-container {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
            transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
        }

        .setup-card {
            background: var(--card-base);
            border: 1px solid var(--neon-blue);
            backdrop-filter: blur(15px);
            box-shadow: 0 15px 40px rgba(0, 240, 255, 0.15), 0 0 25px var(--white-shadow);
            padding: 45px;
            border-radius: 24px;
            max-width: 480px;
            width: 100%;
            text-align: center;
        }

        .cyber-title {
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: 2.3rem;
            background: linear-gradient(135deg, var(--neon-blue), var(--neon-red));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: 2px;
            text-transform: uppercase;
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 22px;
            text-align: left;
        }

        .form-group label {
            display: block;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.85rem;
            color: var(--neon-blue);
            margin-bottom: 8px;
            letter-spacing: 1px;
        }

        .form-control, select {
            width: 100%;
            background: rgba(6, 9, 14, 0.9);
            border: 1px solid rgba(0, 240, 255, 0.25);
            padding: 14px;
            color: var(--text-main);
            font-size: 1.1rem;
            border-radius: 14px;
            outline: none;
        }

        .cyber-btn {
            font-family: 'Orbitron', sans-serif;
            background: transparent;
            border: 1px solid var(--neon-blue);
            color: var(--neon-blue);
            padding: 16px 32px;
            font-size: 1.1rem;
            font-weight: 700;
            text-transform: uppercase;
            cursor: pointer;
            letter-spacing: 2px;
            width: 100%;
            border-radius: 14px;
            transition: all 0.3s ease;
        }

        .cyber-btn:hover {
            background: linear-gradient(135deg, var(--neon-blue), var(--neon-red));
            color: #000;
            box-shadow: 0 0 25px var(--neon-blue), 0 0 20px var(--white-shadow);
            transform: translateY(-2px);
        }

        /* --- DASHBOARD WRAPPER --- */
        .dashboard-container {
            display: none;
            padding: 40px;
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            transition: opacity 0.4s ease;
        }

        .dash-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(0, 240, 255, 0.1);
            padding-bottom: 15px;
            margin-bottom: 40px;
        }

        .user-meta-info {
            font-family: 'Orbitron', sans-serif;
            font-size: 0.95rem;
            color: var(--text-muted);
        }

        .user-meta-info span {
            color: var(--neon-blue);
        }

        /* MAIN GRID */
        .grid-layout {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
            transform-style: preserve-3d;
            transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
        }

        /* Standing 3D Tilted Boxes */
        .dash-box {
            background: var(--card-base);
            border: 1px solid rgba(0, 240, 255, 0.15);
            padding: 35px;
            border-radius: 24px;
            cursor: pointer;
            backdrop-filter: blur(10px);
            box-shadow: 
                -8px 12px 25px rgba(0, 0, 0, 0.5), 
                4px -4px 15px var(--white-shadow),
                0 0 10px rgba(255, 42, 42, 0.03);
            opacity: 0;
            transform-style: preserve-3d;
            transform: rotateX(-8deg) rotateY(8deg);
            transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
        }

        /* Triggering fluid entry cascades */
        .grid-layout .dash-box:nth-child(1) { animation: boxPopUp 0.6s 0.1s forwards; }
        .grid-layout .dash-box:nth-child(2) { animation: boxPopUp 0.6s 0.2s forwards; }
        .grid-layout .dash-box:nth-child(3) { animation: boxPopUp 0.6s 0.3s forwards; }
        .grid-layout .dash-box:nth-child(4) { animation: boxPopUp 0.6s 0.4s forwards; }
        .grid-layout .dash-box:nth-child(5) { animation: boxPopUp 0.6s 0.5s forwards; }

        .dash-box:hover {
            border-color: var(--neon-red);
            transform: translateY(-15px) rotateX(0deg) rotateY(0deg) translateZ(30px) scale(1.03);
            box-shadow: 
                0 25px 45px rgba(0, 0, 0, 0.7), 
                0 0 35px var(--white-shadow), 
                0 0 20px rgba(255, 42, 42, 0.25);
        }

        .dash-box h3 {
            font-family: 'Orbitron', sans-serif;
            color: var(--neon-blue);
            font-size: 1.3rem;
            margin-bottom: 12px;
            letter-spacing: 0.5px;
            transform: translateZ(20px);
        }

        .dash-box p {
            color: var(--text-main);
            opacity: 0.75;
            font-size: 1.05rem;
            line-height: 1.5;
            transform: translateZ(10px);
        }

        /* --- DEEP-DIVE MODULE VIEW (COMPLETE INTERFACE CHANGE) --- */
        .module-view {
            display: none;
            width: 100%;
            background: var(--bg-dark);
            z-index: 10;
            padding: 20px 0;
            box-sizing: border-box;
            opacity: 0;
            transform: scale(0.95);
            transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
        }

        .module-view.active {
            display: block;
            opacity: 1;
            transform: scale(1);
        }

        /* Upper Left Back Controls */
        .back-btn {
            font-family: 'Orbitron', sans-serif;
            background: transparent;
            border: 1px solid var(--neon-red);
            color: var(--neon-red);
            padding: 8px 18px;
            font-size: 0.9rem;
            font-weight: 700;
            cursor: pointer;
            border-radius: 10px;
            margin-bottom: 30px;
            transition: all 0.3s;
            box-shadow: 0 0 10px rgba(255, 42, 42, 0.1);
        }

        .back-btn:hover {
            background: var(--neon-red);
            color: #000;
            box-shadow: 0 0 20px var(--neon-red), 0 0 15px var(--white-shadow);
        }

        .content-panel {
            background: var(--card-base);
            border: 1px solid var(--neon-blue);
            padding: 45px;
            border-radius: 28px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.8), 0 0 35px var(--white-shadow);
        }

        .module-title {
            font-family: 'Orbitron', sans-serif;
            color: var(--neon-blue);
            font-size: 1.8rem;
            margin-bottom: 20px;
            border-bottom: 1px dashed rgba(0, 240, 255, 0.2);
            padding-bottom: 10px;
            text-transform: uppercase;
        }

        /* --- LINE-BY-LINE WRITING TRANSITION --- */
        @keyframes textLineWrite {
            from { opacity: 0; transform: translateY(15px); filter: blur(2px); }
            to { opacity: 1; transform: translateY(0); filter: blur(0); }
        }

        .data-display p, .data-display h4, .data-display ul li {
            opacity: 0;
        }

        .module-view.active .data-display p { animation: textLineWrite 0.4s 0.1s forwards; }
        .module-view.active .data-display h4 { animation: textLineWrite 0.4s 0.2s forwards; }
        .module-view.active .data-display ul li:nth-child(1) { animation: textLineWrite 0.4s 0.3s forwards; }
        .module-view.active .data-display ul li:nth-child(2) { animation: textLineWrite 0.4s 0.4s forwards; }
        .module-view.active .data-display ul li:nth-child(3) { animation: textLineWrite 0.4s 0.5s forwards; }
        .module-view.active .data-display ul li:nth-child(4) { animation: textLineWrite 0.4s 0.6s forwards; }
        .module-view.active .data-display ul li:nth-child(5) { animation: textLineWrite 0.4s 0.7s forwards; }

        .data-display h4 {
            color: var(--neon-blue);
            margin: 25px 0 12px 0;
            font-family: 'Orbitron', sans-serif;
            font-size: 1.15rem;
        }

        .data-display ul li {
            position: relative;
            padding-left: 25px;
            margin-bottom: 12px;
            list-style: none;
            font-size: 1.15rem;
        }

        .data-display ul li::before {
            content: "◆";
            position: absolute;
            left: 5px;
            color: var(--neon-red);
            text-shadow: 0 0 5px var(--neon-red);
        }

        .upload-zone {
            border: 2px dashed rgba(0, 240, 255, 0.3);
            padding: 40px;
            text-align: center;
            cursor: pointer;
            border-radius: 16px;
            transition: 0.3s;
        }
        .upload-zone:hover {
            border-color: var(--neon-blue);
            box-shadow: 0 0 25px var(--white-shadow);
        }
        #preview-img {
            max-width: 240px;
            margin-top: 20px;
            border: 2px solid var(--neon-blue);
            border-radius: 12px;
            display: none;
        }
    </style>
</head>
<body>

    <div class="cyber-grid"></div>

    <div class="setup-container" id="setup-screen">
        <div class="setup-card">
            <h2 class="cyber-title">COACH NETWORK</h2>
            
            <div class="form-group">
                <label>OPERATIONAL OBJECTIVE</label>
                <select id="user-goal">
                    <option value="Fighting">Fighting Masterclass</option>
                    <option value="Calisthenics">Calisthenics Skill Tree</option>
                    <option value="Strength">Hypertrophy & Strength</option>
                </select>
            </div>

            <div class="form-group">
                <label>BIOMETRIC AGE</label>
                <input type="number" id="user-age" class="form-control" value="23">
            </div>

            <div class="form-group">
                <label>SYSTEM MASS (KG)</label>
                <input type="number" id="user-weight" class="form-control" value="68">
            </div>

            <button class="cyber-btn" onclick="initializeDashboard()">CONNECT INTERFACE</button>
        </div>
    </div>


    <div class="dashboard-container" id="main-dashboard">
        <div class="dash-header" id="dash-header">
            <h1 class="cyber-title" style="font-size: 1.5rem; margin:0;">AI CORE STREAM</h1>
            <div class="user-meta-info">
                CORE: <span id="meta-goal">-</span> | AGE: <span id="meta-age">-</span> | WEIGHT: <span id="meta-weight">-</span>KG
            </div>
        </div>

        <div class="grid-layout" id="main-grid">
            
            <div class="dash-box" onclick="expandModule('diet')">
                <h3>DIET & NUTRITION <span>[01]</span></h3>
                <p>AI synchronized macro calibration, caloric tracking, and fluid targets.</p>
            </div>

            <div class="dash-box" onclick="expandModule('condition')">
                <h3>BODY CONDITION <span>[02]</span></h3>
                <p>Upload biological progression visuals. Computer Vision alignment tracking.</p>
            </div>

            <div class="dash-box" onclick="expandModule('mobility')">
                <h3>MOBILITY & STAMINA <span>[03]</span></h3>
                <p>Active dynamic flexibility paths and high-threshold stamina protocols.</p>
            </div>

            <div class="dash-box" onclick="expandModule('skills')">
                <h3>FIGHTING SKILLS <span>[04]</span></h3>
                <p>Advanced combative archives. Spin roundhouses, feints, and tutorials.</p>
            </div>

            <div class="dash-box" onclick="expandModule('sleep')">
                <h3>SLEEP & RECOVERY <span>[05]</span></h3>
                <p>Circadian logs, neurological restoration, and REM deep sleep metrics.</p>
            </div>

        </div>

        <div class="module-view" id="module-deep-view">
            <button class="back-btn" onclick="backToMatrix()">[ ← BACK TO MATRIX ]</button>
            <div class="content-panel">
                <h3 class="module-title" id="panel-title">LOADING COMPONENT...</h3>
                <div class="data-display" id="panel-content"></div>
            </div>
        </div>
    </div>

    <script>
        let userData = { goal: '', age: 0, weight: 0 };

        function initializeDashboard() {
            userData.goal = document.getElementById('user-goal').value;
            userData.age = parseInt(document.getElementById('user-age').value) || 23;
            userData.weight = parseInt(document.getElementById('user-weight').value) || 68;

            document.getElementById('meta-goal').innerText = userData.goal.toUpperCase();
            document.getElementById('meta-age').innerText = userData.age;
            document.getElementById('meta-weight').innerText = userData.weight;

            document.getElementById('setup-screen').style.display = 'none';
            document.getElementById('main-dashboard').style.display = 'block';
        }

        function expandModule(moduleName) {
            const grid = document.getElementById('main-grid');
            const deepView = document.getElementById('module-deep-view');
            const title = document.getElementById('panel-title');
            const content = document.getElementById('panel-content');

            grid.style.opacity = '0';
            setTimeout(() => {
                grid.style.display = 'none';
                deepView.classList.add('active');
                // Scroll straight to the top of the newly loaded module smoothly
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }, 300);

            // Compute AI Arrays
            const calories = Math.round(userData.weight * 33);
            const protein = Math.round(userData.weight * 2);
            const carbs = Math.round(userData.weight * 3.5);
            const fats = Math.round(userData.weight * 0.8);
            const water = (userData.weight * 0.05).toFixed(1);
            const sleepHours = userData.weight > 80 ? "8.5" : "7.5";

            if(moduleName === 'diet') {
                title.innerText = "DATABASE LOG // NUTRITION TARGETS";
                content.innerHTML = `
                    <p>Precise molecular macro arrays calibrated for your <strong>${userData.weight} kg</strong> configuration:</p>
                    <h4>TARGET SCHEDULING:</h4>
                    <ul>
                        <li><strong>Energy Budget Allowance:</strong> ${calories} kcal / Day</li>
                        <li><strong>Protein Structural Allocation:</strong> ${protein}g / Muscle Lock</li>
                        <li><strong>Carbohydrates Glycogen Stack:</strong> ${carbs}g / Core Fuel</li>
                        <li><strong>Essential Lipid Metrics:</strong> ${fats}g / Balance</li>
                        <li><strong>Fluid Intake Protocol:</strong> Minimum ${water} Liters / Day</li>
                    </ul>
                `;
            } 
            else if(moduleName === 'condition') {
                title.innerText = "DATABASE LOG // COMPUTER VISION BIOMETRICS";
                content.innerHTML = `
                    <p>Drop visual progression snapshots below to trigger symmetry tracking algorithms:</p>
                    <div class="upload-zone" onclick="document.getElementById('file-input').click()">
                        <p style="color: var(--neon-blue); font-weight: bold;">[ INITIALIZE SYSTEM TO UPLOAD SCREENSHOT ]</p>
                        <input type="file" id="file-input" style="display:none" onchange="previewFile()">
                        <img id="preview-img" src="" alt="Progress Data">
                    </div>
                `;
            } 
            else if(moduleName === 'mobility') {
                title.innerText = "DATABASE LOG // KINETIC ROUTINES";
                content.innerHTML = `
                    <p>Dynamic flexibility path arrays computed for your <strong>${userData.goal}</strong> framework:</p>
                    <h4>MOBILITY SEQUENCES:</h4>
                    <ul>
                        <li><strong>Deep Cossack Sweeps:</strong> 3 Sets × 12 Reps (Pelvic threshold expanding)</li>
                        <li><strong>Thoracic Bridge Extensions:</strong> 3 Sets × 10 Reps (Spinal alignment)</li>
                    </ul>
                    <h4>VO2 MAX STRENGTH:</h4>
                    <ul>
                        <li><strong>High-Velocity Kinetic Shadow Loops:</strong> 4 Rounds × 3 Minutes</li>
                        <li><strong>Explosive Burpee Launch Chains:</strong> 4 Sets × 15 Reps (Threshold boost)</li>
                    </ul>
                `;
            } 
            else if(moduleName === 'skills') {
                title.innerText = "DATABASE LOG // STRIKING PROTOCOLS";
                content.innerHTML = `
                    <p>Biomechanical evaluation files for high-tier kinetic combative mastery:</p>
                    <h4>COMBAT MASTER REGISTRY:</h4>
                    <ul>
                        <li><strong>360 Degree Spin Roundhouse Kick:</strong> Angular force rotation maps.</li>
                        <li><strong>The Question Mark Kick (Deceptive Array):</strong> Vertical split fake vector.</li>
                        <li><strong>Spinning Hook Kick Knockout Sequence:</strong> Maximum torque damage distribution.</li>
                        <li><strong>Axe Kick Decisive Drop:</strong> Straight overhead guard bypass.</li>
                    </ul>
                `;
            }
            else if(moduleName === 'sleep') {
                title.innerText = "DATABASE LOG // RECOVERY PROTOCOLS";
                content.innerHTML = `
                    <p>Optimized rest metrics for system nervous cooldown adjustments:</p>
                    <h4>BIORHYTHM ASSIGNMENT:</h4>
                    <ul>
                        <li><strong>Calculated System Rest Allocation:</strong> ${sleepHours} Hours / Night</li>
                        <li><strong>Deep REM Recovery Target:</strong> Minimum 2 Hours (Cellular repair sync)</li>
                        <li><strong>Circadian Window Core:</strong> 10:30 PM - 06:00 AM (Melatonin curve optimization)</li>
                        <li><strong>Screen Logoff Directive:</strong> Suspend blue-light displays 45 mins before sleep.</li>
                    </ul>
                `;
            }
        }

        function backToMatrix() {
            const grid = document.getElementById('main-grid');
            const deepView = document.getElementById('module-deep-view');

            deepView.classList.remove('active');
            setTimeout(() => {
                grid.style.display = 'grid';
                setTimeout(() => {
                    grid.style.opacity = '1';
                    window.scrollTo({ top: 0, behavior: 'smooth' });
                }, 50);
            }, 400);
        }

        function previewFile() {
            const preview = document.getElementById('preview-img');
            const file = document.getElementById('file-input').files[0];
            const reader = new FileReader();
            reader.onloadend = function () {
                preview.src = reader.result;
                preview.style.display = "block";
            }
            if (file) { reader.readAsDataURL(file); }
        }
    </script>
</body>
</html>
