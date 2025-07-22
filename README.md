# MAC Tool

A simple Python-Flask web application to check and validate MAC addresses against a whitelist. Useful for basic network filtering or device validation.

---

## Features

- Validate user-entered MAC addresses
- Store allowed MACs in a text file
- Lightweight Flask interface
- Easy to run and customize

---

##Program

#app.py
from flask import Flask, render_template, request

app = Flask(__name__)

def load_whitelist():
    try:
        with open('whitelist.txt', 'r') as f:
            return set(line.strip().upper() for line in f.readlines())
    except FileNotFoundError:
        return set()

@app.route('/', methods=['GET', 'POST'])
def index():
    status = None
    mac_input = ""

    if request.method == 'POST':
        mac_input = request.form.get('mac_address', '').strip().upper()
        whitelist = load_whitelist()

        if mac_input in whitelist:
            status = "Allowed"
        else:
            status = "Blocked"

    return render_template('index.html', status=status, mac_input=mac_input)

if __name__ == '__main__':
    import webbrowser
    edge_path = "C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe %s"
    webbrowser.get(edge_path).open("http://127.0.0.1:5000")
    app.run(debug=False)

#index.html

<!DOCTYPE html>
<html>
<head>
    <title>MAC Address Checker</title>
    <style>
        body { font-family: Arial; padding: 30px; background-color: #f4f4f4; }
        h1 { color: #0b5394; }
        form { margin-bottom: 20px; }
        input[type=text] { padding: 8px; width: 300px; }
        input[type=submit] { padding: 8px 15px; }
        .result { font-size: 18px; font-weight: bold; margin-top: 10px; }
        .allowed { color: green; }
        .blocked { color: red; }
    </style>
</head>
<body>
    <h1>MAC Address Filtering Tool</h1>

    <form method="POST">
        <label for="mac_address">Enter MAC Address:</label><br><br>
        <input type="text" id="mac_address" name="mac_address" placeholder="AA:BB:CC:DD:EE:FF" required>
        <input type="submit" value="Check">
    </form>

    {% if status %}
        <div class="result {{ 'allowed' if status == 'Allowed' else 'blocked' }}">
            Result: {{ status }} ({{ mac_input }})
        </div>
    {% endif %}
</body>
</html>
---
