# EcoFinds – Sustainable Second-Hand Marketplace  

## ♻️ Problem  
Every year, millions of usable clothes, electronics, and household items are discarded, ending up in landfills. Students and young people often struggle to find affordable, trustworthy second-hand options.  

## 💡 Our Solution  
**EcoFinds** is a sustainable marketplace that makes it easy to **buy, sell, and exchange second-hand items** while encouraging eco-friendly habits.  
- Simple platform for students to list products  
- EcoPoints system to reward sustainable actions  
- Secure community-based exchange  

from flask import Flask, render_template, request, redirect, url_for
import json

app = Flask(__name__)
DATA_FILE = "items.json"

# Load items
def load_items():
    try:
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    except:
        return []

# Save items
def save_items(items):
    with open(DATA_FILE, "w") as f:
        json.dump(items, f)

@app.route("/", methods=["GET", "POST"])
def index():
    items = load_items()
    if request.method == "POST":
        name = request.form.get("name")
        category = request.form.get("category")
        condition = request.form.get("condition")
        eco_impact = "5 kg CO2 saved!"  # hardcoded for demo
        items.append({"name": name, "category": category, "condition": condition, "eco_impact": eco_impact})
        save_items(items)
        return redirect(url_for("index"))
    return render_template("index.html", items=items)

if __name__ == "__main__":
    app.run(debug=True)
