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

**Important**: When starting a new project, organize your business logic into **discreet, focused files** in the `services/` folder. Each file should handle a specific domain or feature.

### Service Organization Guidelines

1. **One domain per file**: Create separate files for different business concerns
2. **Keep files small**: Each service file should be focused and manageable
3. **Name clearly**: Use descriptive names like `user_service.py`, `auth_service.py`, `data_processor.py`

### Example Structure
```
services/
├── __init__.py
├── auth_service.py      # Authentication logic
├── user_service.py      # User-related operations
├── data_service.py      # Data processing functions
└── email_service.py     # Email sending functionality
```

### Creating a Service File
```python
# services/user_service.py
from typing import List, Dict, Optional

class UserService:
    """All user-related business logic."""
    
    @staticmethod
    def get_users() -> List[Dict]:
        """Get all users."""
        return [{"id": 1, "name": "John Doe"}, {"id": 2, "name": "Jane Smith"}]
    
    @staticmethod
    def get_user(user_id: int) -> Optional[Dict]:
        """Get a specific user by ID."""
        users = UserService.get_users()
        return next((u for u in users if u["id"] == user_id), None)

# Create an instance for easy importing
user_service = UserService()
```

### Using Services in Routes
```python
# run.py
from services.user_service import user_service

@app.route('/users')
def users():
    users = user_service.get_users()
    return render_template('users.html', users=users)

@app.route('/users/<int:user_id>')
def user_detail(user_id):
    user = user_service.get_user(user_id)
    if user is None:
        return "User not found", 404
    return render_template('user_detail.html', user=user)
```

### Service with External Dependencies
```python
# services/email_service.py
import smtplib
from email.mime.text import MIMEText
from typing import Optional

class EmailService:
    """Email sending functionality."""
    
    def __init__(self, smtp_server: str, smtp_port: int, username: str, password: str):
        self.smtp_server = smtp_server
        self.smtp_port = smtp_port
        self.username = username
        self.password = password
    
    def send_email(self, to: str, subject: str, body: str) -> bool:
        """Send an email."""
        try:
            msg = MIMEText(body)
            msg['Subject'] = subject
            msg['From'] = self.username
            msg['To'] = to
            
            with smtplib.SMTP(self.smtp_server, self.smtp_port) as server:
                server.starttls()
                server.login(self.username, self.password)
                server.send_message(msg)
            return True
        except Exception as e:
            print(f"Error sending email: {e}")
            return False

# Initialize with your SMTP settings (or use environment variables)
email_service = EmailService(
    smtp_server="smtp.example.com",
    smtp_port=587,
    username="your@email.com",
    password="your-password"
)
```

**Best Practice**: Initialize services with configuration in `run.py` or use environment variables for sensitive data.

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
