# **MAC Tool**

A simple **Python-Flask** web application to check and validate MAC addresses against a whitelist. Useful for basic network filtering or device validation.

---

## **Features**

- ✅ Validate user-entered MAC addresses  
- ✅ Store allowed MACs in a text file  
- ✅ Lightweight Flask interface  
- ✅ Easy to run and customize  

---

## **Program**

---

### **app.py**

```python
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
