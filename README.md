<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quick and Easy Word Counter</title>
    <style>
        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            transition: background-color 0.3s, color 0.3s;
            background-color: white; /* Default background is white */
            color: black; /* Default text color is black */
            padding: 0 15px;
        }
        .title {
            font-size: 4em;
            margin-top: 10px;
            margin-bottom: 30px;
            text-align: center;
            font-weight: 600;
        }
        .theme-toggle-container {
            position: absolute;
            top: 20px;
            right: 20px;
            display: flex;
            align-items: center;
        }
        .toggle-switch {
            position: relative;
            width: 50px;
            height: 24px;
            background-color: #ccc;
            border-radius: 15px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .toggle-switch::before {
            content: "";
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: white;
            border-radius: 50%;
            top: 2px;
            left: 2px;
            transition: all 0.3s;
        }
        .toggle-switch.active {
            background-color: #4caf50; /* Green for active */
        }
        .toggle-switch.active::before {
            transform: translateX(26px);
        }
        .container {
            text-align: center;
            background: #ffffff;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            color: black;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        textarea {
            width: 100%;
            height: 150px;
            margin-bottom: 15px;
            font-size: 1.2em;
            padding: 15px;
            border: 1px solid #ccc;
            border-radius: 10px;
            transition: border 0.3s;
            box-sizing: border-box;
        }
        textarea:focus {
            border-color: #4caf50;
            outline: none;
        }
        .counter {
            font-size: 1.5em;
            margin: 10px 0;
        }
        .counter span {
            font-weight: 700;
        }
        .keyword-analysis {
            text-align: left;
            margin-top: 10px;
            font-size: 1.2em;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
        .highlight {
            color: #4caf50;
        }
    </style>
</head>
<body>
    <div class="theme-toggle-container">
        <label for="themeSwitch" class="toggle-switch" id="themeSwitch"></label>
    </div>
    <h1 class="title">Quick and Easy Word Counter</h1>
    <div class="container">
        <textarea id="textInput" placeholder="Type or paste your text here..."></textarea>
        <div class="counter">
            Character Count: <span id="charCount" class="highlight">0</span>
        </div>
        <div class="counter">
            Word Count: <span id="wordCount" class="highlight">0</span>
        </div>
        <div class="counter">
            Average Word Length: <span id="avgWordLength" class="highlight">0</span> characters
        </div>
        <div class="keyword-analysis">
            <strong>Keyword Density:</strong>
            <ul id="keywordDensity"></ul>
        </div>
    </div>

    <script>
        const textInput = document.getElementById('textInput');
        const charCount = document.getElementById('charCount');
        const wordCount = document.getElementById('wordCount');
        const avgWordLength = document.getElementById('avgWordLength');
        const keywordDensity = document.getElementById('keywordDensity');
        const themeSwitch = document.getElementById('themeSwitch');
        const body = document.body;

        // Update text statistics
        textInput.addEventListener('input', () => {
            const text = textInput.value.trim();

            // Character count
            charCount.textContent = text.length;

            // Word count
            const words = text ? text.split(/\s+/) : [];
            wordCount.textContent = words.length;

            // Average word length
            const totalWordLength = words.reduce((total, word) => total + word.length, 0);
            avgWordLength.textContent = words.length ? (totalWordLength / words.length).toFixed(2) : 0;

            // Keyword density
            const wordFrequency = {};
            words.forEach(word => {
                word = word.toLowerCase();
                wordFrequency[word] = (wordFrequency[word] || 0) + 1;
            });

            const sortedKeywords = Object.entries(wordFrequency).sort((a, b) => b[1] - a[1]);
            keywordDensity.innerHTML = sortedKeywords
                .slice(0, 5) // Display the top 5 keywords
                .map(([word, count]) => `<li>${word}: ${count} (${((count / words.length) * 100).toFixed(1)}%)</li>`)
                .join('');
        });

        // Toggle between white and dark grey themes
        themeSwitch.addEventListener('click', () => {
            themeSwitch.classList.toggle('active');
            const isWhite = body.style.backgroundColor === 'white';
            body.style.backgroundColor = isWhite ? '#333' : 'white'; // Toggle to dark grey
            body.style.color = isWhite ? 'white' : 'black'; // Adjust text color accordingly
        });
    </script>
</body>
</html>
