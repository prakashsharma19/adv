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
        .text-container {
            margin: 20px 0;
            padding: 10px;
            border: 1px solid #ccc;
            background-color: #fff;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Text Selector and Copier</h1>
    <div class="text-container">
        <p>
            This is the first paragraph. When you click anywhere on this text, it will select the entire paragraph.
        </p>
        <p>
            This is the second paragraph. Clicking here will select this paragraph and copy it to the clipboard.
        </p>
    </div>
    <script>
        document.querySelectorAll('.text-container p').forEach(paragraph => {
            paragraph.addEventListener('click', function () {
                const range = document.createRange();
                range.selectNodeContents(this);
                const selection = window.getSelection();
                selection.removeAllRanges();
                selection.addRange(range);
                document.execCommand('copy');
                alert('Text copied to clipboard: ' + this.textContent);
            });
        });
    </script>
</body>
</html>
