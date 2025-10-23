# 🌐 CS50 Lecture 9: Flask

> **Course**: CS50 - Harvard University's Introduction to Computer Science  
> **Lecture**: Week 9 - Flask Web Framework  
> **Focus**: Dynamic web applications with Python, Flask routes, templates, forms, databases, sessions, and APIs

---

## 🎯 Learning Objectives

By the end of this lecture, you will understand:
- Flask framework fundamentals and routing
- Dynamic HTML generation with Jinja templates
- Form handling with GET and POST methods
- Template inheritance and layouts
- SQL database integration with Flask
- Session management and cookies
- Shopping cart implementation
- Building and consuming APIs

---

## 📑 Table of Contents

1. [Introduction to Flask](#-introduction-to-flask)
2. [Forms and User Input](#-forms-and-user-input)
3. [Jinja Templating](#-jinja-templating)
4. [Templates and Layouts](#-templates-and-layouts)
5. [Request Methods](#-request-methods)
6. [Frosh IMs Application](#-frosh-ims-application)
7. [SQLite and Python](#-sqlite-and-python)
8. [Cookies and Sessions](#-cookies-and-sessions)
9. [Shopping Cart](#-shopping-cart)
10. [Shows Database](#-shows-database)
11. [APIs](#-apis)

---

## 🚀 Introduction to Flask

### 🎯 What is Flask?

**Flask** is a **micro-framework** for Python web development that simplifies:
- Handling HTTP requests and responses
- Routing URLs to Python functions
- Rendering HTML templates
- Managing sessions and cookies
- Integrating with databases

### 📊 Static vs. Dynamic Content

**Last Week (Static):**
```bash
http-server  # Serves files as-is
```
- ❌ No server-side logic
- ❌ No database access
- ❌ Manual HTML editing

**This Week (Dynamic):**
```bash
flask run  # Executes Python code
```
- ✅ Server-side Python execution
- ✅ Database integration
- ✅ Dynamic HTML generation

### 🗂️ Flask Project Structure

```
project/
├── app.py                  # Main application (controller)
├── requirements.txt        # Dependencies
├── templates/             # HTML templates (views)
│   ├── layout.html
│   ├── index.html
│   └── greet.html
└── static/               # CSS, JS, images
    ├── styles.css
    └── script.js
```

---

## 📦 Flask Basics

### ⚙️ Setup and Installation

**requirements.txt:**
```
Flask
```

**Install dependencies:**
```bash
pip install -r requirements.txt
```

### 🔧 Minimal Flask Application

**app.py:**
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return "hello, world"
```

**Run the application:**
```bash
flask run
```

**Access at:** `http://localhost:5000/`

### 🎯 Understanding Decorators

**What is `@app.route("/")`?**

A **decorator** wraps one function inside another:
- `@app.route("/")` tells Flask: "Call `index()` when user visits `/`"
- The function name (`index`) can be anything
- The route (`"/"`) determines the URL path

### 🌐 Routes and Paths

| Route | URL | Purpose |
|-------|-----|---------|
| `"/"` | `example.com/` | Home page (index) |
| `"/about"` | `example.com/about` | About page |
| `"/greet"` | `example.com/greet` | Greeting page |
| `"/search"` | `example.com/search` | Search results |

---

## 📝 Forms and User Input

### 🔍 URL Parameters

**Passing data via URL:**
```
http://localhost:5000/?name=David
                      └── query string (GET parameter)
```

**Accessing in Flask:**
```python
from flask import Flask, request

@app.route("/")
def index():
    name = request.args.get("name", "world")
    return f"hello, {name}"
```

**Explanation:**
- `request.args` - Dictionary of GET parameters
- `.get("name", "world")` - Get value with default fallback

### 📋 HTML Forms

**index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Greet</title>
</head>
<body>
    <form action="/greet" method="get">
        <input autocomplete="off" autofocus 
               name="name" placeholder="Name" type="text">
        <button type="submit">Greet</button>
    </form>
</body>
</html>
```

**Form attributes:**
- `action="/greet"` - Where to submit data
- `method="get"` - How to send data (GET or POST)
- `name="name"` - Parameter name in URL

### ⚡ Rendering Templates

**app.py:**
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")
```

**How it works:**
1. Flask looks in `templates/` folder
2. Finds `index.html`
3. Sends it to browser

---

## 🎨 Jinja Templating

### 🔤 Variable Substitution

**Syntax:** `{{ variable }}`

**greet.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Greet</title>
</head>
<body>
    hello, {{ name }}
</body>
</html>
```

**app.py:**
```python
@app.route("/greet")
def greet():
    name = request.args.get("name", "world")
    return render_template("greet.html", name=name)
```

**Output:** `hello, David` (or `hello, world` if no name provided)

### 🔁 Conditionals in Templates

**Syntax:**
```jinja
{% if condition %}
    <!-- content -->
{% else %}
    <!-- alternative -->
{% endif %}
```

**Example:**
```html
<body>
    hello, 
    {% if name %}
        {{ name }}
    {% else %}
        world
    {% endif %}
</body>
```

### 🔄 Loops in Templates

**Syntax:**
```jinja
{% for item in items %}
    <!-- content -->
{% endfor %}
```

**Example:**
```html
<ul>
    {% for student in students %}
        <li>{{ student }}</li>
    {% endfor %}
</ul>
```

**Python:**
```python
students = ["Alice", "Bob", "Charlie"]
return render_template("students.html", students=students)
```

---

## 🏗️ Templates and Layouts

### 📐 Template Inheritance

**Problem:** Repeated HTML structure across pages

**Solution:** Use a base layout template

### 🎯 Creating a Layout

**layout.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>hello</title>
</head>
<body>
    {% block body %}{% endblock %}
</body>
</html>
```

**Key concept:** `{% block body %}` is a placeholder

### 🔗 Extending Layouts

**index.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <form action="/greet" method="post">
        <input autocomplete="off" autofocus 
               name="name" placeholder="Name" type="text">
        <button type="submit">Greet</button>
    </form>
{% endblock %}
```

**greet.html:**
```html
{% extends "layout.html" %}

{% block body %}
    hello, {{ name }}
{% endblock %}
```

### 📱 Mobile-Friendly Meta Tag

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

**Benefits:**
- ✅ Responsive design
- ✅ Proper scaling on mobile devices
- ✅ Better user experience

---

## 🔄 Request Methods

### 📊 GET vs POST

| Feature | GET | POST |
|---------|-----|------|
| **Visibility** | Parameters in URL | Hidden in request body |
| **Use Case** | Search, navigation | Forms, sensitive data |
| **Caching** | Can be cached | Not cached |
| **Bookmarkable** | Yes | No |
| **Data Limit** | ~2000 characters | No practical limit |

### 🔒 POST Request Example

**form.html:**
```html
<form action="/greet" method="post">
    <input name="name" type="text">
    <button type="submit">Submit</button>
</form>
```

**app.py:**
```python
@app.route("/greet", methods=["POST"])
def greet():
    name = request.form.get("name", "world")
    return render_template("greet.html", name=name)
```

**Key differences:**
- `request.form` (POST) vs `request.args` (GET)
- `methods=["POST"]` specifies allowed methods

### 🔀 Supporting Multiple Methods

```python
@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        name = request.form.get("name")
        return render_template("greet.html", name=name)
    else:
        return render_template("index.html")
```

**Flow:**
1. **GET request** → Show form
2. **POST request** → Process form and show result

---

## 🏈 Frosh IMs Application

### 🎯 Registration Form

**index.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Register</h1>
    <form action="/register" method="post">
        <input autocomplete="off" autofocus 
               name="name" placeholder="Name" type="text">
        
        <select name="sport">
            <option value="Basketball">Basketball</option>
            <option value="Soccer">Soccer</option>
            <option value="Ultimate Frisbee">Ultimate Frisbee</option>
        </select>
        
        <button type="submit">Register</button>
    </form>
{% endblock %}
```

### 📋 Form Elements

**Text Input:**
```html
<input name="name" type="text">
```

**Dropdown (Select Menu):**
```html
<select name="sport">
    <option value="Basketball">Basketball</option>
    <option value="Soccer">Soccer</option>
</select>
```

**Checkboxes:**
```html
<input name="sport" type="checkbox" value="Basketball"> Basketball
<input name="sport" type="checkbox" value="Soccer"> Soccer
```

**Radio Buttons:**
```html
<input name="sport" type="radio" value="Basketball"> Basketball
<input name="sport" type="radio" value="Soccer"> Soccer
```

### ⚙️ Processing Registration

**app.py:**
```python
from flask import Flask, render_template, request

app = Flask(__name__)

SPORTS = ["Basketball", "Soccer", "Ultimate Frisbee"]

@app.route("/")
def index():
    return render_template("index.html", sports=SPORTS)

@app.route("/register", methods=["POST"])
def register():
    name = request.form.get("name")
    sport = request.form.get("sport")
    
    if not name or sport not in SPORTS:
        return render_template("failure.html")
    
    return render_template("success.html", name=name, sport=sport)
```

### ✅ Validation

**Server-side validation is critical:**
```python
if not name:
    return render_template("error.html", message="Missing name")
if sport not in SPORTS:
    return render_template("error.html", message="Invalid sport")
```

**🔑 Key Principle:** Never trust client-side validation alone!

---

## 🗄️ SQLite and Python

### 📊 Why Use Databases?

**In-memory storage problems:**
- ❌ Data lost when server restarts
- ❌ Can't share data across requests
- ❌ No persistence

**Database benefits:**
- ✅ Permanent storage
- ✅ Query capabilities
- ✅ Data integrity

### 🔧 CS50 SQL Library

**Installation:**
```bash
pip install cs50
```

**Import:**
```python
from cs50 import SQL
```

### 📦 Connecting to Database

```python
from cs50 import SQL

# Connect to SQLite database
db = SQL("sqlite:///registrants.db")
```

**Database file:** `registrants.db` (created automatically)

### 🏗️ Creating Tables

```python
# Create registrants table
db.execute("""
    CREATE TABLE IF NOT EXISTS registrants (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        sport TEXT NOT NULL
    )
""")
```

### ➕ Inserting Data

```python
@app.route("/register", methods=["POST"])
def register():
    name = request.form.get("name")
    sport = request.form.get("sport")
    
    # Validate
    if not name or sport not in SPORTS:
        return render_template("failure.html")
    
    # Insert into database
    db.execute("INSERT INTO registrants (name, sport) VALUES (?, ?)",
               name, sport)
    
    return render_template("success.html")
```

**🔒 Security:** Use `?` placeholders to prevent SQL injection!

### 🔍 Querying Data

```python
@app.route("/registrants")
def registrants():
    # Fetch all registrants
    rows = db.execute("SELECT * FROM registrants")
    
    # rows is a list of dictionaries
    # [{"id": 1, "name": "Alice", "sport": "Basketball"}, ...]
    
    return render_template("registrants.html", registrants=rows)
```

### 📄 Displaying Data

**registrants.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Registrants</h1>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Sport</th>
            </tr>
        </thead>
        <tbody>
            {% for registrant in registrants %}
                <tr>
                    <td>{{ registrant.name }}</td>
                    <td>{{ registrant.sport }}</td>
                </tr>
            {% endfor %}
        </tbody>
    </table>
{% endblock %}
```

---

## 🍪 Cookies and Sessions

### 🎯 The Stateless Problem

**HTTP is stateless:** Each request is independent

**Challenge:** How to remember logged-in users?

### 🤝 The Hand Stamp Analogy

**Real world:**
1. Show ID at entrance
2. Get hand stamp ✋
3. Show stamp to re-enter (no ID needed)

**Web equivalent:**
1. Log in (show credentials)
2. Receive cookie 🍪
3. Send cookie with each request

### 📦 How Cookies Work

**Server response (after login):**
```http
HTTP/2 200 OK
Set-Cookie: session=abc123xyz
```

**Browser's subsequent requests:**
```http
GET /profile HTTP/2
Cookie: session=abc123xyz
```

### 🔧 Flask Sessions

**Import:**
```python
from flask import session
```

**Configure secret key:**
```python
app = Flask(__name__)
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)
```

**Or simpler:**
```python
app.secret_key = "your-secret-key-here"
```

### 💾 Storing Data in Sessions

**Login route:**
```python
@app.route("/login", methods=["POST"])
def login():
    username = request.form.get("username")
    password = request.form.get("password")
    
    # Validate credentials (check database)
    if valid_credentials(username, password):
        session["user_id"] = get_user_id(username)
        return redirect("/profile")
    
    return render_template("error.html")
```

### 🔓 Accessing Session Data

```python
@app.route("/profile")
def profile():
    # Check if user is logged in
    if "user_id" not in session:
        return redirect("/login")
    
    user_id = session["user_id"]
    user = db.execute("SELECT * FROM users WHERE id = ?", user_id)[0]
    
    return render_template("profile.html", user=user)
```

### 🚪 Logging Out

```python
@app.route("/logout")
def logout():
    session.clear()  # Remove all session data
    return redirect("/")
```

---

## 🛒 Shopping Cart

### 🎯 Shopping Cart Requirements

**Features:**
1. Browse products
2. Add items to cart
3. View cart
4. Persist cart across pages

### 📦 Cart Implementation

**Initialize session:**
```python
from flask import Flask, render_template, request, session
from flask_session import Session

app = Flask(__name__)
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)
```

### 📚 Product Catalog

```python
BOOKS = [
    {"id": 1, "title": "Python Programming", "price": 29.99},
    {"id": 2, "title": "Web Development", "price": 34.99},
    {"id": 3, "title": "Data Science", "price": 39.99}
]
```

### 🏠 Store Page

**store.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Books</h1>
    {% for book in books %}
        <div>
            <h3>{{ book.title }}</h3>
            <p>${{ book.price }}</p>
            <form action="/cart" method="post">
                <input name="id" type="hidden" value="{{ book.id }}">
                <button type="submit">Add to Cart</button>
            </form>
        </div>
    {% endfor %}
{% endblock %}
```

### ➕ Adding to Cart

```python
@app.route("/")
def index():
    return render_template("store.html", books=BOOKS)

@app.route("/cart", methods=["POST"])
def add_to_cart():
    # Get book ID from form
    book_id = request.form.get("id")
    
    # Initialize cart if not exists
    if "cart" not in session:
        session["cart"] = []
    
    # Add book ID to cart
    session["cart"].append(int(book_id))
    
    return redirect("/cart")
```

### 🛒 Viewing Cart

```python
@app.route("/cart", methods=["GET"])
def view_cart():
    # Get cart from session
    cart = session.get("cart", [])
    
    # Get book details
    books_in_cart = []
    for book_id in cart:
        book = next((b for b in BOOKS if b["id"] == book_id), None)
        if book:
            books_in_cart.append(book)
    
    return render_template("cart.html", books=books_in_cart)
```

**cart.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Shopping Cart</h1>
    {% if books %}
        <table>
            <thead>
                <tr>
                    <th>Title</th>
                    <th>Price</th>
                </tr>
            </thead>
            <tbody>
                {% for book in books %}
                    <tr>
                        <td>{{ book.title }}</td>
                        <td>${{ book.price }}</td>
                    </tr>
                {% endfor %}
            </tbody>
        </table>
    {% else %}
        <p>Your cart is empty.</p>
    {% endif %}
{% endblock %}
```

---

## 📺 Shows Database

### 🗄️ Database Schema

**shows.db structure:**
```sql
CREATE TABLE shows (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL UNIQUE,
    genres TEXT NOT NULL
);
```

**Example data:**
```
id | title           | genres
---|-----------------|------------------
1  | The Office      | Comedy
2  | Game of Thrones | Drama, Fantasy
3  | Breaking Bad    | Crime, Drama
```

### 🔍 Searching Shows

**search.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Search for Shows</h1>
    <form action="/search" method="get">
        <input autocomplete="off" autofocus 
               name="q" placeholder="Title" type="search">
        <button type="submit">Search</button>
    </form>
{% endblock %}
```

**app.py:**
```python
@app.route("/search")
def search():
    query = request.args.get("q", "")
    
    if query:
        # Search for shows containing query
        shows = db.execute(
            "SELECT * FROM shows WHERE title LIKE ?",
            f"%{query}%"
        )
    else:
        shows = []
    
    return render_template("results.html", shows=shows)
```

### 📄 Displaying Results

**results.html:**
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Search Results</h1>
    {% if shows %}
        <ul>
            {% for show in shows %}
                <li>
                    <strong>{{ show.title }}</strong> - {{ show.genres }}
                </li>
            {% endfor %}
        </ul>
    {% else %}
        <p>No shows found.</p>
    {% endif %}
{% endblock %}
```

---

## 🌐 APIs (Application Programming Interfaces)

### 🎯 What is an API?

**API:** A way for programs to communicate with each other

**Web API:** Server endpoints that return data (usually JSON) instead of HTML

### 📊 JSON Format

**JavaScript Object Notation (JSON):**
```json
{
    "id": 1,
    "title": "The Office",
    "genres": ["Comedy"],
    "year": 2005
}
```

**Array of objects:**
```json
[
    {"id": 1, "title": "The Office", "year": 2005},
    {"id": 2, "title": "Breaking Bad", "year": 2008}
]
```

### 🔧 Creating an API

**app.py:**
```python
from flask import jsonify

@app.route("/api/shows")
def api_shows():
    shows = db.execute("SELECT * FROM shows")
    return jsonify(shows)
```

**Response:**
```json
[
    {"id": 1, "title": "The Office", "genres": "Comedy"},
    {"id": 2, "title": "Breaking Bad", "genres": "Crime, Drama"}
]
```

### 🔍 API with Search

```python
@app.route("/api/search")
def api_search():
    query = request.args.get("q", "")
    
    if query:
        shows = db.execute(
            "SELECT * FROM shows WHERE title LIKE ?",
            f"%{query}%"
        )
    else:
        shows = []
    
    return jsonify(shows)
```

**Example request:**
```
GET /api/search?q=office
```

**Response:**
```json
[
    {"id": 1, "title": "The Office", "genres": "Comedy"}
]
```

### 🌍 Consuming External APIs

**Using `requests` library:**
```python
import requests

@app.route("/weather")
def weather():
    city = request.args.get("city", "Cambridge")
    
    # Call external weather API
    response = requests.get(
        f"https://api.weather.com/v1/forecast?city={city}",
        headers={"Authorization": "Bearer YOUR_API_KEY"}
    )
    
    data = response.json()
    return render_template("weather.html", weather=data)
```

### 🔐 API Best Practices

1. **Use API Keys:** Authenticate requests
2. **Rate Limiting:** Prevent abuse
3. **Error Handling:** Return meaningful error messages
4. **Documentation:** Explain endpoints and parameters
5. **Versioning:** `/api/v1/shows` for backward compatibility

---

## 💡 Best Practices

### ✅ Flask Application Structure

**Recommended organization:**
```
project/
├── app.py              # Main application
├── requirements.txt    # Dependencies
├── models.py          # Database models (optional)
├── helpers.py         # Utility functions
├── static/
│   ├── styles.css
│   └── script.js
└── templates/
    ├── layout.html
    ├── index.html
    └── error.html
```

### 🔒 Security Best Practices

1. **Never trust user input** - Always validate
2. **Use parameterized queries** - Prevent SQL injection
3. **Hash passwords** - Never store plain text
4. **Set secret keys** - Secure session data
5. **Use HTTPS** - Encrypt data in transit

### 🎨 Template Best Practices

1. **Use template inheritance** - Avoid duplication
2. **Keep logic minimal** - Heavy logic belongs in Python
3. **Pass data explicitly** - Don't rely on global state
4. **Use descriptive variable names**
5. **Comment complex templates**

### 📊 Database Best Practices

1. **Create indexes** - Speed up queries
2. **Use foreign keys** - Maintain data integrity
3. **Normalize data** - Reduce redundancy
4. **Back up databases** - Prevent data loss
5. **Use transactions** - Ensure consistency

---

## 🎯 Key Takeaways

### 1️⃣ **MVC Architecture**

| Component | Purpose | Example |
|-----------|---------|---------|
| **Model** | Data storage | SQLite database |
| **View** | User interface | Jinja templates |
| **Controller** | Business logic | Flask routes |

### 2️⃣ **Flask Fundamentals**

**Minimal Flask app requires:**
- Import Flask
- Create app instance
- Define routes with decorators
- Return responses (HTML/JSON)
- Run with `flask run`

### 3️⃣ **Request Handling**

**GET vs POST:**
- **GET:** Read data, parameters in URL
- **POST:** Write data, parameters hidden

**Access data:**
- GET: `request.args.get("key")`
- POST: `request.form.get("key")`

### 4️⃣ **Jinja Templates**

**Three core features:**
1. **Variables:** `{{ variable }}`
2. **Control flow:** `{% if %}`, `{% for %}`
3. **Inheritance:** `{% extends %}`, `{% block %}`

### 5️⃣ **Sessions and Cookies**

**Purpose:** Remember user state across requests

**Implementation:**
```python
# Store
session["key"] = "value"

# Retrieve
value = session.get("key")

# Clear
session.clear()
```

---

## 🎬 Looking Ahead

### Final Project

**Apply what you've learned:**
- Build a complete web application
- Use Flask, databases, and templates
- Implement user authentication
- Create APIs for data access
- Deploy to the web

### Real-World Applications

**Flask powers:**
- E-commerce sites
- Social networks
- Content management systems
- Data dashboards
- RESTful APIs

---

## 📚 Additional Resources

- **Flask Documentation**: [flask.palletsprojects.com](https://flask.palletsprojects.com)
- **Jinja Documentation**: [jinja.palletsprojects.com](https://jinja.palletsprojects.com)
- **CS50 SQL Library**: [cs50.readthedocs.io](https://cs50.readthedocs.io)
- **SQLite Documentation**: [sqlite.org](https://sqlite.org)
- **HTTP Status Codes**: [httpstatuses.com](https://httpstatuses.com)

---

## 🎓 Practice Exercises

### 🔰 Beginner

1. **Hello Application**: Create `/hello/<name>` route
2. **Form Handler**: Build form that accepts name and age
3. **Template Inheritance**: Create layout with multiple pages

### 🔸 Intermediate

4. **Todo List**: Add, view, delete tasks (session-based)
5. **Search Engine**: Query database and display results
6. **User Registration**: Store users in database

### 🔥 Advanced

7. **Blog Platform**: Posts, comments, authentication
8. **E-commerce Store**: Products, cart, checkout
9. **RESTful API**: Full CRUD operations with JSON

---

## 🎬 Lecture Credits

**Instructor**: David J. Malan  
**University**: Harvard University  
**Course**: CS50x 2025  
**Video**: CS50x 2025 - Lecture 9 - Flask

---

## 📝 Quick Reference

### Flask Routes
```python
# Basic route
@app.route("/")
def index():
    return "Hello"

# Multiple methods
@app.route("/submit", methods=["GET", "POST"])
def submit():
    if request.method == "POST":
        # Process form
        pass
    return render_template("form.html")
```

### Database Operations
```python
# Connect
db = SQL("sqlite:///database.db")

# Create
db.execute("INSERT INTO users (name) VALUES (?)", name)

# Read
users = db.execute("SELECT * FROM users")

# Update
db.execute("UPDATE users SET name = ? WHERE id = ?", name, id)

# Delete
db.execute("DELETE FROM users WHERE id = ?", id)
```

### Session Management
```python
# Set
session["user_id"] = 123

# Get
user_id = session.get("user_id")

# Check
if "user_id" in session:
    # User is logged in
    pass

# Clear
session.clear()
```

---

*Made with 🌐 for CS50 students learning web development with Flask*