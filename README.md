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
            const outputContainer = document.getElementById('output');
            outputContainer.textContent = inputText;
        }

        document.addEventListener('click', function(event) {
            const outputContainer = document.getElementById('output');
            const selection = window.getSelection();

            if (event.target.closest('#output') && selection.rangeCount && event.shiftKey) {
                const range = selection.getRangeAt(0);
                const startNode = range.startContainer;
                let endNode = range.endContainer;

                while (endNode !== outputContainer && endNode.nodeName !== 'BR' && endNode.textContent.trim() === '') {
                    endNode = endNode.parentNode;
                }

                const newRange = document.createRange();
                newRange.setStart(startNode, range.startOffset);
                newRange.setEnd(endNode, endNode.textContent.length);

                selection.removeAllRanges();
                selection.addRange(newRange);

                document.execCommand('copy');
                document.execCommand('cut');
            }
        });
    </script>
</body>
</html>
