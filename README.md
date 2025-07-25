# Skill Craft Task1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caesar Cipher Tool</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
            animation: slideIn 0.5s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
            font-size: 2.5em;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
        }

        .cipher-info {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 30px;
            text-align: center;
            font-size: 0.9em;
        }

        .input-group {
            margin-bottom: 25px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
            font-size: 1.1em;
        }

        textarea, input[type="number"] {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            font-family: inherit;
            transition: all 0.3s ease;
            resize: vertical;
        }

        textarea:focus, input[type="number"]:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
            transform: translateY(-2px);
        }

        textarea {
            min-height: 120px;
        }

        .shift-input {
            max-width: 150px;
        }

        .button-group {
            display: flex;
            gap: 15px;
            margin: 30px 0;
            flex-wrap: wrap;
        }

        button {
            flex: 1;
            min-width: 120px;
            padding: 15px 25px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .encrypt-btn {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }

        .decrypt-btn {
            background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
            color: white;
        }

        .clear-btn {
            background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            color: #333;
        }

        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }

        button:active {
            transform: translateY(-1px);
        }

        .result-section {
            margin-top: 30px;
        }

        .result-box {
            background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
            padding: 20px;
            border-radius: 10px;
            border: 2px solid #f0a500;
            min-height: 120px;
            font-family: 'Courier New', monospace;
            font-size: 16px;
            line-height: 1.6;
            white-space: pre-wrap;
            word-wrap: break-word;
            color: #333;
        }

        .alphabet-display {
            margin-top: 20px;
            padding: 15px;
            background: rgba(102, 126, 234, 0.1);
            border-radius: 10px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            text-align: center;
        }

        .alphabet-row {
            margin: 5px 0;
            letter-spacing: 3px;
        }

        @media (max-width: 600px) {
            .container {
                padding: 25px;
                margin: 10px;
            }
            
            h1 {
                font-size: 2em;
            }
            
            .button-group {
                flex-direction: column;
            }
            
            button {
                min-width: auto;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔐 Caesar Cipher</h1>
        
        <div class="cipher-info">
            The Caesar Cipher shifts each letter by a fixed number of positions in the alphabet. 
            Use positive numbers to encrypt, negative to decrypt, or use the dedicated buttons!
        </div>

        <div class="input-group">
            <label for="inputText">Enter your message:</label>
            <textarea id="inputText" placeholder="Type your message here..."></textarea>
        </div>

        <div class="input-group">
            <label for="shiftValue">Shift value (1-25):</label>
            <input type="number" id="shiftValue" class="shift-input" value="3" min="1" max="25">
        </div>

        <div class="button-group">
            <button class="encrypt-btn" onclick="encryptText()">🔒 Encrypt</button>
            <button class="decrypt-btn" onclick="decryptText()">🔓 Decrypt</button>
            <button class="clear-btn" onclick="clearAll()">🗑️ Clear All</button>
        </div>

        <div class="result-section">
            <label>Result:</label>
            <div id="result" class="result-box">Your encrypted/decrypted text will appear here...</div>
        </div>

        <div class="alphabet-display">
            <div class="alphabet-row">Original: A B C D E F G H I J K L M N O P Q R S T U V W X Y Z</div>
            <div class="alphabet-row" id="shiftedAlphabet">Shifted:  A B C D E F G H I J K L M N O P Q R S T U V W X Y Z</div>
        </div>
    </div>

    <script>
        function caesarCipher(text, shift, isEncrypt = true) {
            // If decrypting, reverse the shift
            if (!isEncrypt) {
                shift = -shift;
            }
            
            // Normalize shift to be within 0-25 range
            shift = ((shift % 26) + 26) % 26;
            
            let result = '';
            
            for (let i = 0; i < text.length; i++) {
                let char = text[i];
                
                if (char.match(/[a-zA-Z]/)) {
                    // Determine if uppercase or lowercase
                    let isUpperCase = char === char.toUpperCase();
                    
                    // Convert to uppercase for processing
                    char = char.toUpperCase();
                    
                    // Get character code (A=65, B=66, etc.)
                    let charCode = char.charCodeAt(0);
                    
                    // Shift the character (A=0, B=1, etc.)
                    let shifted = (charCode - 65 + shift) % 26;
                    
                    // Convert back to character
                    let newChar = String.fromCharCode(shifted + 65);
                    
                    // Maintain original case
                    if (!isUpperCase) {
                        newChar = newChar.toLowerCase();
                    }
                    
                    result += newChar;
                } else {
                    // Keep non-alphabetic characters unchanged
                    result += char;
                }
            }
            
            return result;
        }

        function encryptText() {
            const inputText = document.getElementById('inputText').value;
            const shiftValue = parseInt(document.getElementById('shiftValue').value);
            
            if (!inputText.trim()) {
                alert('Please enter some text to encrypt!');
                return;
            }
            
            if (isNaN(shiftValue) || shiftValue < 1 || shiftValue > 25) {
                alert('Please enter a valid shift value between 1 and 25!');
                return;
            }
            
            const encrypted = caesarCipher(inputText, shiftValue, true);
            document.getElementById('result').textContent = encrypted;
            updateAlphabetDisplay(shiftValue);
        }

        function decryptText() {
            const inputText = document.getElementById('inputText').value;
            const shiftValue = parseInt(document.getElementById('shiftValue').value);
            
            if (!inputText.trim()) {
                alert('Please enter some text to decrypt!');
                return;
            }
            
            if (isNaN(shiftValue) || shiftValue < 1 || shiftValue > 25) {
                alert('Please enter a valid shift value between 1 and 25!');
                return;
            }
            
            const decrypted = caesarCipher(inputText, shiftValue, false);
            document.getElementById('result').textContent = decrypted;
            updateAlphabetDisplay(-shiftValue);
        }

        function clearAll() {
            document.getElementById('inputText').value = '';
            document.getElementById('result').textContent = 'Your encrypted/decrypted text will appear here...';
            document.getElementById('shiftValue').value = '3';
            updateAlphabetDisplay(0);
        }

        function updateAlphabetDisplay(shift) {
            const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
            let shifted = '';
            
            if (shift === 0) {
                shifted = alphabet;
            } else {
                // Normalize shift
                shift = ((shift % 26) + 26) % 26;
                
                for (let i = 0; i < 26; i++) {
                    let newIndex = (i + shift) % 26;
                    shifted += alphabet[newIndex];
                }
            }
            
            // Format with spaces
            const formattedShifted = shifted.split('').join(' ');
            document.getElementById('shiftedAlphabet').innerHTML = `Shifted:  ${formattedShifted}`;
        }

        // Initialize alphabet display
        updateAlphabetDisplay(0);

        // Add event listener for Enter key
        document.getElementById('inputText').addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                encryptText();
            }
        });

        // Update alphabet display when shift value changes
        document.getElementById('shiftValue').addEventListener('input', function() {
            const shift = parseInt(this.value) || 0;
            updateAlphabetDisplay(shift);
        });
    </script>
</body>
</html>

