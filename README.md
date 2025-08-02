<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tetum Translator | Tradutor Tetum</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }
        header {
            background-color: #FF0000;
            color: white;
            padding: 1rem;
            text-align: center;
        }
        .container {
            max-width: 800px;
            margin: 2rem auto;
            padding: 1rem;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .language-selector, .translation-box {
            margin-bottom: 1rem;
        }
        textarea {
            width: 100%;
            min-height: 100px;
            padding: 0.5rem;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #FF0000;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1rem;
        }
        button:hover {
            background-color: #CC0000;
        }
        footer {
            text-align: center;
            padding: 1rem;
            background-color: #333;
            color: white;
        }
    </style>
</head>
<body>
    <header>
        <h1>Tetum Translator</h1>
        <p>Tradutor ba Lingua Tetum</p>
    </header>
    
    <div class="container">
        <div class="language-selector">
            <label for="source-language">Translate from:</label>
            <select id="source-language">
                <option value="en">English</option>
                <option value="pt">Portuguese</option>
                <option value="id">Indonesian</option>
                <option value="es">Spanish</option>
                <option value="fr">French</option>
                <!-- Add more languages as needed -->
            </select>
        </div>
        
        <div class="translation-box">
            <textarea id="source-text" placeholder="Type text to translate..."></textarea>
        </div>
        
        <button id="translate-btn">Translate to Tetum</button>
        
        <div class="translation-box">
            <h3>Translation:</h3>
            <textarea id="target-text" readonly placeholder="Translation will appear here..."></textarea>
        </div>
        
        <div class="phrasebook">
            <h3>Common Phrases:</h3>
            <select id="common-phrases">
                <option value="">Select a common phrase</option>
                <option value="Hello">Hello</option>
                <option value="Thank you">Thank you</option>
                <option value="How are you?">How are you?</option>
                <option value="Good morning">Good morning</option>
                <option value="I don't understand">I don't understand</option>
            </select>
            <button id="insert-phrase">Insert Phrase</button>
        </div>
    </div>
    
    <footer>
        <p>© 2023 Tetum Translator | Timor-Leste</p>
    </footer>

    <script>
        // This would connect to a translation API in a real implementation
        document.getElementById('translate-btn').addEventListener('click', function() {
            const sourceText = document.getElementById('source-text').value;
            const sourceLang = document.getElementById('source-language').value;
            
            // In a real app, this would call a translation API
            // For demo purposes, we'll use a simple mock translation
            const mockTranslations = {
                "Hello": "Ola",
                "Thank you": "Obrigadu",
                "How are you?": "Diak ka lae?",
                "Good morning": "Bondia",
                "I don't understand": "Ha'u la komprende"
            };
            
            let translatedText = mockTranslations[sourceText] || 
                               `[Translation from ${sourceLang} to Tetum would appear here for: "${sourceText}"]`;
            
            document.getElementById('target-text').value = translatedText;
        });
        
        // Insert common phrases
        document.getElementById('insert-phrase').addEventListener('click', function() {
            const phrase = document.getElementById('common-phrases').value;
            if (phrase) {
                document.getElementById('source-text').value = phrase;
            }
        });
    </script>
</body>
</html>
// Replace this in your <script> section
async function translateText(text, sourceLang) {
    // Note: You'll need to get an API key from Google Cloud
    const API_KEY = 'YOUR_API_KEY';
    const url = `https://translation.googleapis.com/language/translate/v2?key=${API_KEY}`;
    
    const response = await fetch(url, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            q: text,
            source: sourceLang,
            target: 'tet',  // Tetum language code
        }),
    });
    
    const data = await response.json();
    return data.data.translations[0].translatedText;
}

document.getElementById('translate-btn').addEventListener('click', async function() {
    const sourceText = document.getElementById('source-text').value;
    const sourceLang = document.getElementById('source-language').value;
    
    if (sourceText.trim() === '') return;
    
    try {
        const translatedText = await translateText(sourceText, sourceLang);
        document.getElementById('target-text').value = translatedText;
    } catch (error) {
        document.getElementById('target-text').value = "Error in translation. Please try again.";
        console.error(error);
    }
});
