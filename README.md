<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text Selector and Copier</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #ADD8E6; /* Light blue background color */
            padding: 20px;
            position: relative;
        }
        .input-container {
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            flex-direction: column;
        }
        .text-container {
            margin: 20px 0;
            padding: 10px;
            border: 1px solid #ccc;
            background-color: #fff;
            cursor: text;
            white-space: pre-wrap; /* Maintain text format */
            position: relative;
        }
        .text-container p {
            margin: 10px 0;
        }
        .copy-button, .clear-button {
            background-color: #4CAF50; /* Green */
            border: none;
            color: white;
            padding: 10px 20px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            cursor: pointer;
            transition-duration: 0.4s;
            margin: 5px;
        }
        .copy-button:hover, .clear-button:hover {
            background-color: white;
            color: black;
            border: 2px solid #4CAF50;
        }
        .clear-button {
            background-color: #f44336; /* Red */
        }
        .clear-button:hover {
            border: 2px solid #f44336;
        }
        #adCount, #dailyAdCount, #remainingTime, #countryCount {
            margin-top: 10px;
            font-size: 18px;
            font-weight: bold;
        }
        #remainingTime {
            position: absolute;
            right: 20px;
            top: 20px;
        }
        .font-controls {
            margin-bottom: 10px;
        }
        #okButton {
            background-color: #4CAF50; /* Green */
            border: none;
            color: white;
            padding: 15px 32px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin: 4px 2px;
            cursor: pointer;
            transition-duration: 0.4s;
        }
        #okButton:hover {
            background-color: white;
            color: black;
            border: 2px solid #4CAF50;
        }
        #cursorStart {
            font-weight: bold;
            color: red;
        }
        #credits {
            position: absolute;
            bottom: 20px;
            right: 20px;
            font-size: 16px;
        }
        #credits a {
            color: #0000EE;
            text-decoration: none;
        }
        #credits a:hover {
            text-decoration: underline;
        }
        .error {
            color: red;
            font-weight: bold;
        }
        .login-container {
            margin-bottom: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .login-container input {
            margin: 5px 0;
            padding: 10px;
            font-size: 16px;
        }
        #loginButton {
            background-color: #4CAF50; /* Green */
            border: none;
            color: white;
            padding: 10px 20px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin: 10px 0;
            cursor: pointer;
            transition-duration: 0.4s;
        }
        #loginButton:hover {
            background-color: white;
            color: black;
            border: 2px solid #4CAF50;
        }
        .hourglass {
            width: 24px;
            height: 24px;
            background-image: url('https://upload.wikimedia.org/wikipedia/commons/4/4e/Simpleicons_Interface_hourglass.svg');
            background-size: cover;
            display: inline-block;
            margin-left: 10px;
        }
    </style>
</head>
<body>
    <h1>Text Selector and Copier</h1>
    <div class="login-container">
        <input type="text" id="username" placeholder="Enter your name">
        <input type="password" id="password" placeholder="Enter your password">
        <button id="loginButton" onclick="login()">Login</button>
    </div>
    <div class="font-controls" style="display:none;">
        <label for="fontStyle">Font Style:</label>
        <select id="fontStyle" onchange="updateFont()">
            <option value="Arial">Arial</option>
            <option value="Times New Roman">Times New Roman</option>
            <option value="Courier New">Courier New</option>
            <option value="Georgia">Georgia</option>
            <option value="Calibri Light">Calibri Light</option>
        </select>
        <label for="fontSize">Font Size:</label>
        <input type="number" id="fontSize" value="16" onchange="updateFont()">px
    </div>
    <div class="input-container" style="display:none;">
        <textarea id="inputText" rows="10" cols="50" placeholder="Paste your text here..."></textarea>
        <button id="okButton" onclick="processText()">OK</button>
    </div>
    <div id="adCount" style="display:none;">Total Advertisements: 0</div>
    <div id="dailyAdCount" style="display:none;">Total Ads Today: 0</div>
    <div id="remainingTime" style="display:none;">Remaining Time: <span id="time"></span><div class="hourglass"></div></div>
    <div id="countryCount" style="display:none;">Country Counts:</div>
    <button class="copy-button" style="display:none;" onclick="copyRemainingText()">Copy Remaining Text</button>
    <button class="clear-button" style="display:none;" onclick="clearAllText()">Clear All</button>
    <div id="output" class="text-container" style="display:none;" contenteditable="true"></div>
    <div id="credits">
        This page is developed by <a href="https://prakashsharma19.github.io/prakash/" target="_blank">Prakash</a>
    </div>

    <script>
        const countryList = [
            "Afghanistan", "Albania", "Algeria", "Andorra", "Angola", "Antigua and Barbuda", "Argentina", "Armenia", "Australia", "Austria",
            "Azerbaijan", "Bahamas", "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium", "Belize", "Benin", "Bhutan",
            "Bolivia", "Bosnia and Herzegovina", "Botswana", "Brazil", "Brunei", "Bulgaria", "Burkina Faso", "Burundi", "Cabo Verde", "Cambodia",
            "Cameroon", "Canada", "Central African Republic", "Chad", "Chile", "China", "Colombia", "Comoros", "Congo", "Costa Rica",
            "Croatia", "Cuba", "Cyprus", "Czech Republic", "Denmark", "Djibouti", "Dominica", "Dominican Republic", "Ecuador", "Egypt",
            "El Salvador", "Equatorial Guinea", "Eritrea", "Estonia", "Eswatini", "Ethiopia", "Fiji", "Finland", "France", "Gabon",
            "Gambia", "Georgia", "Germany", "Ghana", "Greece", "Grenada", "Guatemala", "Guinea", "Guinea-Bissau", "Guyana",
            "Haiti", "Honduras", "Hungary", "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel",
            "Italy", "Jamaica", "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
            "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg", "Madagascar", "Malawi",
            "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands", "Mauritania", "Mauritius", "Mexico", "Micronesia", "Moldova",
            "Monaco", "Mongolia", "Montenegro", "Morocco", "Mozambique", "Myanmar", "Namibia", "Nauru", "Nepal", "Netherlands",
            "New Zealand", "Nicaragua", "Niger", "Nigeria", "North Korea", "North Macedonia", "Norway", "Oman", "Pakistan", "Palau",
            "Palestine", "Panama", "Papua New Guinea", "Paraguay", "Peru", "Philippines", "Poland", "Portugal", "Qatar", "Romania",
            "Russia", "Rwanda", "Saint Kitts and Nevis", "Saint Lucia", "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Sao Tome and Principe", "Saudi Arabia", "Senegal",
            "Serbia", "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands", "Somalia", "South Africa", "South Korea",
            "South Sudan", "Spain", "Sri Lanka", "Sudan", "Suriname", "Sweden", "Switzerland", "Syria", "Taiwan", "Tajikistan",
            "Tanzania", "Thailand", "Timor-Leste", "Togo", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey", "Turkmenistan", "Tuvalu",
            "Uganda", "Ukraine", "United Arab Emirates", "United Kingdom", "United States", "Uruguay", "Uzbekistan", "Vanuatu", "Vatican City", "Venezuela",
            "Vietnam", "Yemen", "Zambia", "Zimbabwe"
        ];

        let currentUser = null;
        let dailyAdCount = 0;
        let startTime = Date.now();
        let cutCount = 0;
        let adTimestamps = [];

        // Function to save text to localStorage for the current user
        function saveText() {
            const inputText = document.getElementById('inputText').value;
            const outputText = document.getElementById('output').innerHTML;
            if (currentUser) {
                localStorage.setItem(`savedInput_${currentUser}`, inputText);
                localStorage.setItem(`savedOutput_${currentUser}`, outputText);
                localStorage.setItem(`dailyAdCount_${currentUser}`, dailyAdCount);
                localStorage.setItem(`lastCutTime_${currentUser}`, Date.now());
            }
        }

        // Function to load text from localStorage for the current user
        function loadText() {
            if (currentUser) {
                const savedInput = localStorage.getItem(`savedInput_${currentUser}`);
                const savedOutput = localStorage.getItem(`savedOutput_${currentUser}`);
                const savedDailyAdCount = localStorage.getItem(`dailyAdCount_${currentUser}`);
                if (savedInput) {
                    document.getElementById('inputText').value = savedInput;
                }
                if (savedOutput) {
                    document.getElementById('output').innerHTML = savedOutput;
                }
                if (savedDailyAdCount) {
                    dailyAdCount = parseInt(savedDailyAdCount, 10);
                }
                updateCounts();
            }
        }

        function countOccurrences(text, word) {
            const regex = new RegExp(`\\b${word}\\b`, 'gi');
            return (text.match(regex) || []).length;
        }

        function countCountryOccurrences(text) {
            const lines = text.split('\n');
            const countryCounts = {};

            for (let i = 0; i < lines.length - 1; i++) {
                const line = lines[i].trim();
                const nextLine = lines[i + 1].trim();

                if (nextLine.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/)) { // Check if next line is an email
                    countryList.forEach(country => {
                        if (line.includes(country)) {
                            countryCounts[country] = (countryCounts[country] || 0) + 1;
                        }
                    });
                }
            }
            return countryCounts;
        }

        function highlightErrors(text) {
            return text.replace(/(\w+\?\w+)/g, '<span class="error">$1</span>');
        }

        function updateCounts() {
            const outputContainer = document.getElementById('output');
            const text = outputContainer.innerText;
            const adCount = countOccurrences(text, 'professor');
            document.getElementById('adCount').innerText = `Total Advertisements: ${adCount}`;
            document.getElementById('dailyAdCount').innerText = `Total Ads Today: ${dailyAdCount}`;

            const countryCounts = countCountryOccurrences(text);
            let countryCountText = 'Country Counts:<br>';
            for (const country in countryCounts) {
                countryCountText += `<b>${country}</b>: ${countryCounts[country]}; `;
            }
            document.getElementById('countryCount').innerHTML = countryCountText.trim();

            updateRemainingTime(adCount);
        }

        function updateRemainingTime(adCount) {
            if (cutCount === 0) return;

            const now = Date.now();
            const elapsedTime = (now - startTime) / 1000; // in seconds
            const averageTimePerCut = elapsedTime / cutCount;

            const remainingAds = adCount - dailyAdCount;
            const remainingTimeInSeconds = remainingAds * averageTimePerCut;

            const hours = Math.floor(remainingTimeInSeconds / 3600);
            const minutes = Math.floor((remainingTimeInSeconds % 3600) / 60);

            document.getElementById('time').innerText = hours > 0 ? `${hours}h ${minutes}m` : `${minutes}m`;
        }

        function processText() {
            const inputText = document.getElementById('inputText').value;
            const paragraphs = inputText.split('\n\n');
            const outputContainer = document.getElementById('output');
            outputContainer.innerHTML = '<p id="cursorStart">Place your cursor here</p>';

            let index = 0;
            function processChunk() {
                const chunkSize = 100; // Number of paragraphs to process in one go
                const end = Math.min(index + chunkSize, paragraphs.length);
                for (; index < end; index++) {
                    const paragraph = paragraphs[index];
                    if (paragraph.trim() !== '') {
                        const p = document.createElement('p');
                        p.innerHTML = highlightErrors(paragraph.replace(/\n/g, '<br>'));
                        outputContainer.appendChild(p);

                        // Add a gap after each paragraph for smooth cursor movement
                        const gap = document.createElement('div');
                        gap.innerHTML = '<br><br>'; // Add larger gap
                        outputContainer.appendChild(gap);
                    }
                }
                if (index < paragraphs.length) {
                    requestAnimationFrame(processChunk);
                } else {
                    updateCounts();
                    saveText(); // Save text to localStorage for the current user
                }
            }
            requestAnimationFrame(processChunk);
        }

        function cutParagraph(paragraph) {
            const textToCopy = paragraph.innerText;
            const selection = window.getSelection();
            const range = document.createRange();
            range.selectNodeContents(paragraph);
            selection.removeAllRanges();
            selection.addRange(range);

            // Copy the text without formatting
            const tempTextarea = document.createElement('textarea');
            tempTextarea.style.position = 'fixed';
            tempTextarea.style.opacity = '0';
            tempTextarea.value = textToCopy;
            document.body.appendChild(tempTextarea);
            tempTextarea.select();
            document.execCommand('copy');
            document.body.removeChild(tempTextarea);

            // Remove the paragraph and cleanup
            paragraph.remove();
            cleanupSpaces();

            // Update the input textarea
            const inputText = document.getElementById('inputText').value;
            const updatedText = inputText.replace(new RegExp(`(?:\\n)?${textToCopy}(?:\\n)?`), '');
            document.getElementById('inputText').value = updatedText;

            // Update the daily ad count
            dailyAdCount++;
            cutCount++;

            // Save the timestamp for the cut operation
            adTimestamps.push(Date.now());

            // Update the count and save changes
            updateCounts();
            saveText();
        }

        function cleanupSpaces() {
            const outputContainer = document.getElementById('output');
            const paragraphs = outputContainer.querySelectorAll('p, div');
            paragraphs.forEach(paragraph => {
                if (!paragraph.innerText.trim()) {
                    paragraph.remove();
                }
            });
        }

        function handleCursorMovement(event) {
            const selection = window.getSelection();
            if (selection.rangeCount > 0) {
                const range = selection.getRangeAt(0);
                const container = range.commonAncestorContainer;

                // Check if the cursor is inside a paragraph containing "Professor"
                let paragraph = container;
                while (paragraph && paragraph.nodeName !== 'P') {
                    paragraph = paragraph.parentNode;
                }

                if (paragraph && paragraph.textContent.includes('Professor')) {
                    cutParagraph(paragraph);

                    // Set focus back to the text container after cutting
                    document.getElementById('output').focus();
                }
            }
        }

        function startMonitoring() {
            document.addEventListener('keyup', handleCursorMovement);
        }

        function updateFont() {
            const fontStyle = document.getElementById('fontStyle').value;
            const fontSize = document.getElementById('fontSize').value;
            document.getElementById('output').style.fontFamily = fontStyle;
            document.getElementById('output').style.fontSize = `${fontSize}px`;
        }

        function copyRemainingText() {
            const outputContainer = document.getElementById('output');
            const remainingText = outputContainer.innerText;
            const tempTextarea = document.createElement('textarea');
            tempTextarea.style.position = 'fixed';
            tempTextarea.style.opacity = '0';
            tempTextarea.value = remainingText;
            document.body.appendChild(tempTextarea);
            tempTextarea.select();
            document.execCommand('copy');
            document.body.removeChild(tempTextarea);
        }

        function clearAllText() {
            document.getElementById('inputText').value = '';
            document.getElementById('output').innerHTML = '';
            document.getElementById('adCount').innerText = 'Total Advertisements: 0';
            document.getElementById('dailyAdCount').innerText = 'Total Ads Today: 0';
            document.getElementById('countryCount').innerText = 'Country Counts:';
            if (currentUser) {
                localStorage.removeItem(`savedInput_${currentUser}`);
                localStorage.removeItem(`savedOutput_${currentUser}`);
                localStorage.removeItem(`dailyAdCount_${currentUser}`);
            }
            dailyAdCount = 0;
            cutCount = 0;
            adTimestamps = [];
        }

        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;

            if (username && password) {
                currentUser = `${username}_${password}`;
                document.querySelector('.login-container').style.display = 'none';
                document.querySelector('.font-controls').style.display = 'block';
                document.querySelector('.input-container').style.display = 'block';
                document.getElementById('adCount').style.display = 'block';
                document.getElementById('dailyAdCount').style.display = 'block';
                document.getElementById('remainingTime').style.display = 'block';
                document.getElementById('countryCount').style.display = 'block';
                document.querySelector('.copy-button').style.display = 'block';
                document.querySelector('.clear-button').style.display = 'block';
                document.getElementById('output').style.display = 'block';
                loadText();
            } else {
                alert('Please enter both username and password.');
            }
        }

        document.getElementById('output').addEventListener('click', function(event) {
            if (event.target.id === 'cursorStart') {
                // Start monitoring cursor after clicking in the text container
                startMonitoring();
            }
        });

        // Check for daily reset of ad count
        function checkDailyReset() {
            const lastCutTime = localStorage.getItem(`lastCutTime_${currentUser}`);
            if (lastCutTime) {
                const lastCutDate = new Date(parseInt(lastCutTime, 10));
                const currentDate = new Date();
                if (lastCutDate.toDateString() !== currentDate.toDateString()) {
                    dailyAdCount = 0;
                    localStorage.setItem(`dailyAdCount_${currentUser}`, dailyAdCount);
                }
            }
        }

        setInterval(checkDailyReset, 60000); // Check every minute

        // No need to loadText on page load since it will be handled on login
    </script>
</body>
</html>
