# vamo
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatGPT Interface</title>
    <link href="http://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #1a202c;
        }
    </style>
</head>
<body>
    <div class="container mx-auto px-4 py-5">
        <div class="bg-gray-800 text-white rounded-lg p-6 shadow-lg">
            <h1 class="text-2xl font-bold mb-4">Pippo's Messaging App</h1>
            <div id="chat" class="overflow-y-auto mb-4 h-64">
            </div>
            <form id="user-input-form" class="flex">
                <input id="user-input" class="flex-grow px-4 py-2 bg-gray-900 text-white rounded-l-lg" type="text" placeholder="Enter your fooking question...">
                <button class="px-4 py-2 bg-indigo-600 text-white font-bold rounded-r-lg" type="submit">Send</button>
            </form>
        </div>
    </div>
    <script>
        document.getElementById("user-input-form").addEventListener("submit", async (e) => {
            e.preventDefault();
            const input = document.getElementById("user-input");
            const chat = document.getElementById("chat");
            const prompt = input.value.trim();
            if (prompt.length === 0) return;

            chat.innerHTML += `<p class="text-blue-400 mb-2"><strong>You:</strong> ${prompt}</p>`;
            input.value = "";
            const response = await getChatGPTResponse(prompt);
            chat.innerHTML += `<p class="text-green-400 mb-2"><strong>Babbo:</strong> ${response}</p>`;
            chat.scrollTop = chat.scrollHeight;
        });

        async function getChatGPTResponse(prompt) {
            const apiKey = "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";
            const apiEndpoint = "https://api.openai.com/v1/completions";
            const model = "text-davinci-003";

            const response = await fetch(apiEndpoint, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": `Bearer ${apiKey}`
                },
                body: JSON.stringify({
                    "model": model,
                    "prompt": prompt,
                    "max_tokens": 50
                })
            });

            if (response.ok) {
                const data = await response.json();
                return data.choices[0].text.trim();
            } else {
                return "An error occurred while communicating with the ChatGPT API.";
            }
        }
    </script>
</body>
</html>
