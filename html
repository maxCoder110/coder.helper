<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Code Mastery</title>
    <style>
        :root {
            --bg-color: #050510;
            --neon-blue: #00f3ff;
            --neon-pink: #ff00ea;
            --neon-green: #39ff14;
            --text-color: #ffffff;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            font-family: 'Courier New', Courier, monospace;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        h1 {
            text-align: center;
            font-size: 3rem;
            text-shadow: 0 0 10px var(--neon-blue), 0 0 20px var(--neon-blue);
            margin-bottom: 40px;
        }

        /* Neon Button Styles */
        .neon-btn {
            background: transparent;
            color: var(--neon-blue);
            border: 2px solid var(--neon-blue);
            padding: 15px 30px;
            font-size: 1.2rem;
            font-family: inherit;
            cursor: pointer;
            border-radius: 5px;
            margin: 10px;
            transition: 0.3s;
            box-shadow: 0 0 5px var(--neon-blue), inset 0 0 5px var(--neon-blue);
        }

        .neon-btn:hover {
            background: var(--neon-blue);
            color: var(--bg-color);
            box-shadow: 0 0 20px var(--neon-blue), inset 0 0 10px var(--neon-blue);
        }

        .python-btn {
            color: var(--neon-green);
            border-color: var(--neon-green);
            box-shadow: 0 0 5px var(--neon-green), inset 0 0 5px var(--neon-green);
        }

        .python-btn:hover {
            background: var(--neon-green);
            color: var(--bg-color);
            box-shadow: 0 0 20px var(--neon-green), inset 0 0 10px var(--neon-green);
        }

        /* Layout Containers */
        .screen { display: none; width: 100%; max-width: 1200px; }
        .active-screen { display: block; }

        /* Level Grid */
        .level-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            width: 100%;
        }

        .level-card {
            border: 1px solid var(--neon-pink);
            padding: 20px;
            text-align: center;
            cursor: pointer;
            box-shadow: 0 0 5px var(--neon-pink);
            transition: 0.3s;
            position: relative;
        }

        .level-card:hover {
            box-shadow: 0 0 15px var(--neon-pink);
            transform: translateY(-2px);
        }

        /* Workspace Layout */
        .workspace {
            display: flex;
            gap: 20px;
            height: 60vh;
            margin-top: 20px;
        }

        .editor-container, .preview-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            border: 2px solid var(--neon-blue);
            box-shadow: 0 0 10px var(--neon-blue);
            padding: 10px;
            border-radius: 5px;
            background: #0a0a1a;
        }

        .target-code {
            color: #888;
            margin-bottom: 10px;
            white-space: pre-wrap;
            user-select: none;
            flex: 1;
            overflow-y: auto;
        }

        .target-code span.correct { color: var(--neon-green); text-shadow: 0 0 5px var(--neon-green); }
        .target-code span.incorrect { color: #ff0044; background: rgba(255,0,68,0.3); }

        textarea {
            flex: 1;
            background: transparent;
            color: transparent; /* Hide text, show caret */
            caret-color: white;
            border: none;
            font-family: inherit;
            font-size: 1rem;
            resize: none;
            outline: none;
            z-index: 2;
        }

        /* We overlay the target text and textarea so typing feels natural */
        .typing-area {
            position: relative;
            flex: 2;
            display: flex;
            flex-direction: column;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
            background: white; /* Browsers expect white bg for standard HTML render */
        }
        
        .header-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

    </style>
</head>
<body>

    <div id="main-menu" class="screen active-screen">
        <h1>NEON_CODE</h1>
        <div style="display: flex; justify-content: center; gap: 20px;">
            <button class="neon-btn" onclick="showLevelSelect('html')">HTML Track</button>
            <button class="neon-btn python-btn" onclick="showLevelSelect('python')">Python Track</button>
        </div>
        <div style="text-align: center; margin-top: 40px;">
            <button class="neon-btn" style="border-color: var(--neon-pink); color: var(--neon-pink);" onclick="alert('AI Integration Endpoint Required. (Coming Soon)')">Generate Custom AI Level</button>
        </div>
    </div>

    <div id="level-select" class="screen">
        <div class="header-controls">
            <button class="neon-btn" onclick="showScreen('main-menu')">← Back to Menu</button>
            <h2 id="track-title" style="text-shadow: 0 0 10px var(--neon-blue);">HTML Levels</h2>
        </div>
        <div class="level-grid" id="level-grid-container">
            </div>
    </div>

    <div id="workspace" class="screen">
        <div class="header-controls">
            <button class="neon-btn" onclick="showScreen('level-select')">← Levels</button>
            <h2 id="current-level-title">Level 1.1</h2>
            <button class="neon-btn" onclick="runCode()">Run Code ▶</button>
        </div>
        
        <div class="workspace">
            <div class="editor-container typing-area">
                <div id="target-display" class="target-code"></div>
                <textarea id="user-input" autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></textarea>
            </div>
            <div class="preview-container">
                <h3 style="margin-top: 0; color: #888;">Live Preview</h3>
                <iframe id="live-preview"></iframe>
            </div>
        </div>
    </div>

    <script>
        // --- DATA STRUCTURE ---
        // 10 Levels, 3 Sub-levels each. Here is the framework.
        const courseData = {
            html: [
                {
                    level: 1, title: "Level 1: The Foundations",
                    sublevels: [
                        { id: "1.1", title: "Level 1.1: Hello World", tooltip: "Learn the basic structure of a webpage using tags.", code: "<h1>Hello World!</h1>\n<p>Welcome to the Neon Web.</p>" },
                        { id: "1.2", title: "Level 1.2: Bold & Italic", tooltip: "Use formatting tags to style your text.", code: "<h1>Formatting</h1>\n<p>This is <b>bold</b> and this is <i>italic</i>.</p>" },
                        { id: "1.3", title: "Level 1.3: Creating Lists", tooltip: "Organize data using unordered lists.", code: "<ul>\n  <li>HTML</li>\n  <li>CSS</li>\n  <li>Python</li>\n</ul>" }
                    ]
                },
                // Add Levels 2 through 10 here following the same structure
            ],
            python: [
                {
                    level: 1, title: "Level 1: Python Basics",
                    sublevels: [
                        { id: "1.1", title: "Level 1.1: Print Statements", tooltip: "Output text to the console.", code: "print('Welcome to the Python Track')\nprint('Initializing...')" },
                        { id: "1.2", title: "Level 1.2: Variables", tooltip: "Store data in variables.", code: "player_name = 'Bruce'\nlevel = 1\nprint(player_name, level)" },
                        { id: "1.3", title: "Level 1.3: Basic Loops", tooltip: "Use a for loop to repeat actions.", code: "for i in range(3):\n    print('Neon system active')" }
                    ]
                }
            ]
        };

        let currentTrack = 'html';
        let currentTargetCode = '';

        // --- DOM ELEMENTS ---
        const userInput = document.getElementById('user-input');
        const targetDisplay = document.getElementById('target-display');
        const livePreview = document.getElementById('live-preview');

        // --- NAVIGATION LOGIC ---
        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(screenId).classList.add('active-screen');
        }

        function showLevelSelect(track) {
            currentTrack = track;
            showScreen('level-select');
            
            document.getElementById('track-title').innerText = track.toUpperCase() + " Levels";
            const grid = document.getElementById('level-grid-container');
            grid.innerHTML = ''; // Clear previous

            // Populate the grid based on data
            courseData[track].forEach(levelData => {
                levelData.sublevels.forEach(sub => {
                    const card = document.createElement('div');
                    card.className = 'level-card';
                    // The title attribute creates the native hover tooltip you requested
                    card.title = sub.tooltip; 
                    card.innerHTML = `<h3>${sub.title}</h3><p style="font-size: 0.8rem; color: #aaa;">Hover to see mission</p>`;
                    card.onclick = () => loadLevel(sub);
                    grid.appendChild(card);
                });
            });
        }

        function loadLevel(sublevel) {
            showScreen('workspace');
            document.getElementById('current-level-title').innerText = sublevel.title;
            currentTargetCode = sublevel.code;
            userInput.value = '';
            livePreview.srcdoc = ''; // Reset preview
            updateTypingDisplay();
        }

        // --- TYPING MECHANIC ---
        userInput.addEventListener('input', updateTypingDisplay);

        function updateTypingDisplay() {
            const typed = userInput.value;
            let displayHTML = '';
            let isPerfect = true;

            for (let i = 0; i < currentTargetCode.length; i++) {
                if (i < typed.length) {
                    if (typed[i] === currentTargetCode[i]) {
                        displayHTML += `<span class="correct">${currentTargetCode[i]}</span>`;
                    } else {
                        displayHTML += `<span class="incorrect">${currentTargetCode[i]}</span>`;
                        isPerfect = false;
                    }
                } else {
                    displayHTML += currentTargetCode[i];
                }
            }

            // Handle newlines properly in HTML
            targetDisplay.innerHTML = displayHTML;

            // Auto-run if completed perfectly
            if (typed === currentTargetCode) {
                runCode();
                alert("Level Complete! Great typing.");
            }
        }

        // --- LIVE COMPILER ---
        function runCode() {
            const codeToRun = userInput.value;
            if (currentTrack === 'html') {
                // For HTML, we inject it directly into the iframe
                livePreview.srcdoc = codeToRun;
            } else if (currentTrack === 'python') {
                // Python requires a backend or Pyodide. For this learning tool, we mock the console output.
                livePreview.srcdoc = `<body style="background: #111; color: #39ff14; font-family: monospace; padding: 20px;">
                    <h3>> Python Console Simulated Output:</h3>
                    <p>Executing...</p>
                    <pre>${codeToRun}</pre>
                </body>`;
            }
        }
    </script>
</body>
</html>
