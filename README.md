# Flask Base Project

A clean, minimal starting point for Flask applications with a structured layout and ready-to-use styling.

---

## Project Structure

```
project/
├── run.py                 # Main Flask application entry point
├── requirements.txt        # Python dependencies
├── README.md              # This file
├── templates/
│   └── index.html         # Base HTML template with demo components
└── static/
    ├── css/
    │   └── style.css      # Compact, themed styling (buttons, tables, forms, etc.)
    └── images/
        └── favicon.svg    # Default favicon
└── services/
    ├── __init__.py
    └── example_service.py # Place for business logic and worker functions
```

---

## Quick Start

### 1. Clone or Copy

**Option A: Clone the base**
```bash
git clone https://github.com/bsnellen/Coding_Base.git my_project
cd my_project
rm -rf .git
git init
```

**Option B: Use as GitHub Template**
- Click **"Use this template"** on the GitHub repo page
- Create your new repository from it

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the App

```bash
python run.py
```

Open [http://localhost:5000](http://localhost:5000) in your browser.

---

## Adding New Pages

### Create a new template
1. Add a new file in `templates/` (e.g., `about.html`)
2. Extend the base structure from `index.html`

### Add a route in `run.py`
```python
from flask import render_template

@app.route('/about')
def about():
    return render_template('about.html')
```

---

## Using the Style System

### Colors (CSS Variables)
```css
:root {
    --primary:   #2c3e50;    /* Dark blue */
    --secondary: #34495e;    /* Slightly lighter blue */
    --accent:    #3498db;    /* Bright blue */
    --success:   #27ae60;    /* Green */
    --warning:   #f39c12;    /* Orange */
    --danger:    #e74c3c;    /* Red */
    --light:     #ecf0f1;    /* Light gray */
    --border:    #bdc3c7;    /* Border color */
}
```

### Available Components
- **Buttons**: `.btn`, `.btn-primary`, `.btn-secondary`, `.btn-success`, `.btn-danger`, `.btn-sm`, `.btn-lg`
- **Tables**: `.table`, `.table-striped`
- **Forms**: `.form-group`, `input`, `select`, `textarea` with focus states
- **Cards**: `.card`, `.card-header`, `.card-body`
- **Alerts**: `.alert`, `.alert-success`, `.alert-info`, `.alert-warning`, `.alert-danger`
- **Navbar**: `.navbar`, `.navbar-brand`, `.navbar-nav`
- **Grid**: `.row`, `.col`, `.col-1` through `.col-12`
- **Utilities**: `.text-center`, `.text-right`, `.mt-1` to `.mt-3`, `.mb-1` to `.mb-3`

### Example Usage
```html
<!-- Button -->
<button class="btn btn-primary">Save</button>

<!-- Alert -->
<div class="alert alert-success">Operation completed!</div>

<!-- Card -->
<div class="card">
    <div class="card-header">User Profile</div>
    <div class="card-body">Content goes here</div>
</div>

<!-- Grid -->
<div class="row">
    <div class="col col-6">Left column</div>
    <div class="col col-6">Right column</div>
</div>
```

---

## Adding Business Logic

Place complex functions and services in the `services/` folder:

```python
# services/user_service.py
from typing import List, Dict

def get_users() -> List[Dict]:
    """Example service function to get users."""
    return [{"id": 1, "name": "John Doe"}, {"id": 2, "name": "Jane Smith"}]
```

Then import and use in your routes:
```python
# run.py
from services.user_service import get_users

@app.route('/users')
def users():
    users = get_users()
    return render_template('users.html', users=users)
```

---

## Customizing the Style

Edit `static/css/style.css` to:
- Change color variables at the top
- Modify component styles
- Add new components

---

## Deployment

### Local Development
```bash
# Run with debug mode
python run.py
```

### Production (Gunicorn example)
```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8000 run:app
```

---

## License

Use this base as you see fit for your projects.
