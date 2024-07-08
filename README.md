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
    </style>
</head>
<body>
    <h1>Text Selector and Copier</h1>
    <div class="input-container">
        <textarea id="inputText" rows="10" cols="50" placeholder="Paste your text here..."></textarea><br>
        <button onclick="processText()">OK</button>
    </div>
    <div id="output" class="text-container" contenteditable="true"></div>

    <script>
        function processText() {
            const inputText = document.getElementById('inputText').value;
            const paragraphs = inputText.split('\n\n');
            const outputContainer = document.getElementById('output');
            outputContainer.innerHTML = '';

            paragraphs.forEach(paragraph => {
                if (paragraph.trim() !== '') {
                    const p = document.createElement('p');
                    p.textContent = paragraph.trim();
                    outputContainer.appendChild(p);
                }
            });
        }

        document.addEventListener('click', function(event) {
            const outputContainer = document.getElementById('output');

            if (event.target.closest('.text-container') && event.target.nodeName === 'P' && event.shiftKey) {
                const paragraph = event.target;
                const range = document.createRange();
                range.selectNodeContents(paragraph);
                const selection = window.getSelection();
                selection.removeAllRanges();
                selection.addRange(range);
                
                // Automatically copy and cut the text
                document.execCommand('copy');
                paragraph.remove();
            }
        });
    </script>
</body>
</html>
