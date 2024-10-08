
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub_Non_Followers</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 0;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
        }
        label {
            display: block;
            margin: 10px 0 5px;
        }
        input[type="text"], input[type="password"] {
            width: 100%;
            padding: 8px;
            margin: 5px 0 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            background-color: #007bff;
            color: #fff;
            font-size: 16px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .result {
            margin-top: 20px;
        }
        .link {
            margin-top: 20px;
            display: block;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>GitHub_Non_Followers</h1>
        <label for="username">GitHub Username:</label>
        <input type="text" id="username" placeholder="GitHub Username">
        <label for="token">Personal Access Token:</label>
        <input type="password" id="token" placeholder="Token">
        <button onclick="checkNonFollowers()">Check Non-Followers</button>
        <button onclick="openTokenPage()">Create a Token Here if You Don't Have One</button>
        <div class="result" id="result"></div>
    </div>

    <script>
        function checkNonFollowers() {
            const username = document.getElementById('username').value;
            const token = document.getElementById('token').value;

            fetch('/check_non_followers', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ username: username, token: token })
            })
            .then(response => response.json())
            .then(data => {
                console.log('API Response:', data);

                if (data.error) {
                    document.getElementById('result').innerText = `Error: ${data.error}`;
                } else {
                    const nonFollowers = data.non_followers || [];
                    const nonFollowersList = nonFollowers.join(', ');
                    document.getElementById('result').innerText = `Non-Followers (${data.count} person): ${nonFollowersList}`;
                }
            })
            .catch(error => {
                document.getElementById('result').innerText = `An error occurred: ${error}`;
            });
        }

        function openTokenPage() {
            window.open('https://github.com/settings/tokens', '_blank');
        }
    </script>
</body>
</html>
