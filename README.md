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
        }
        .text-container {
            margin: 20px 0;
            padding: 10px;
            border: 1px solid #ccc;
            background-color: #fff;
            cursor: text;
            white-space: pre-wrap; /* Maintain text format */
        }
        .text-container p {
            margin: 10px 0;
        }
        #entryCount {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 18px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Text Selector and Copier</h1>
    <div class="input-container">
        <textarea id="inputText" rows="10" cols="50" placeholder="Paste your text here..."></textarea><br>
        <button onclick="processText()">OK</button>
    </div>
    <div id="output" class="text-container" contenteditable="true"></div>
    <div id="entryCount">Total Entries: 0</div>

    <script>
        function countOccurrences(text, word) {
            const regex = new RegExp(`\\b${word}\\b`, 'gi');
            return (text.match(regex) || []).length;
        }

        function updateCount() {
            const outputContainer = document.getElementById('output');
            const count = countOccurrences(outputContainer.innerText, 'professor');
            document.getElementById('entryCount').innerText = `Total Entries: ${count}`;
        }

        function processText() {
            const inputText = document.getElementById('inputText').value;
            const paragraphs = inputText.split('\n\n');
            const outputContainer = document.getElementById('output');
            outputContainer.innerHTML = '<p id="cursorStart">Place your cursor here</p>';

            paragraphs.forEach(paragraph => {
                if (paragraph.trim() !== '') {
                    const p = document.createElement('p');
                    p.innerHTML = paragraph.replace(/\n/g, '<br>');
                    outputContainer.appendChild(p);

                    // Add a gap after every email for smooth cursor movement
                    if (paragraph.includes('@')) {
                        const gap = document.createElement('div');
                        gap.innerHTML = '<br><br>';
                        outputContainer.appendChild(gap);
                    }
                }
            });

            updateCount();
        }

        function cutParagraph(paragraph) {
            const selection = window.getSelection();
            const range = document.createRange();
            range.selectNodeContents(paragraph);
            selection.removeAllRanges();
            selection.addRange(range);

            // Copy the text with formatting
            const tempDiv = document.createElement('div');
            tempDiv.appendChild(range.cloneContents());
            const textToCopy = tempDiv.innerHTML;
            document.body.appendChild(tempDiv);

            const tempTextarea = document.createElement('textarea');
            tempTextarea.style.position = 'fixed';
            tempTextarea.style.opacity = '0';
            tempTextarea.value = paragraph.innerHTML.replace(/<br>/g, '\n');

            document.body.appendChild(tempTextarea);
            tempTextarea.select();
            document.execCommand('copy');
            document.body.removeChild(tempTextarea);
            document.body.removeChild(tempDiv);

            // Remove the paragraph
            paragraph.remove();

            // Update the count
            updateCount();
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

        document.getElementById('output').addEventListener('click', function(event) {
            if (event.target.id === 'cursorStart') {
                document.getElementById('cursorStart').innerHTML = ''; // Remove the placeholder text
                startMonitoring(); // Start monitoring cursor after clicking in the text container
            }
        });
    </script>
</body>
</html>
