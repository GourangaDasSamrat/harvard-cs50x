# CS50 Lecture 9 - Flask Web Applications
## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Flask combines everything** - Python, SQL, HTML, CSS, JavaScript
- **Dynamic vs static pages** - Server generates HTML on-the-fly
- **Routes handle URLs** - Different paths trigger different functions
- **Templates reuse code** - Don't repeat HTML structure
- **Sessions track users** - Cookies enable login functionality
- **APIs enable communication** - Programs talk to each other via JSON

---

## 🌐 Static vs Dynamic Pages

### Static Pages (Week 8)

**Static**: HTML files pre-written, same for everyone

```html
<!-- Saved as hello.html -->
<!DOCTYPE html>
<html>
<body>
    Hello, world
</body>
</html>
```

**Characteristics**:
- Downloaded by browser exactly as stored
- Same content for all users
- No server-side processing

### Dynamic Pages (This Week!)

**Dynamic**: Server generates HTML based on user input/data

```python
# Server generates different HTML for different users
@app.route("/")
def index():
    name = get_user_name()
    return f"<html><body>Hello, {name}</body></html>"
```

**Characteristics**:
- Created on-the-fly by server
- Personalized for each user
- Can include database data

---

## 🚀 Flask Basics

### What is Flask?

**Flask**: Micro-framework for creating web applications in Python

**Why "micro"?** Minimal core, but extensible with libraries

### File Structure

```
project/
├── app.py              # Main application code
├── requirements.txt    # Required libraries
├── templates/          # HTML templates
│   ├── layout.html
│   ├── index.html
│   └── greet.html
└── static/             # CSS, images, JavaScript
    ├── style.css
    └── cat.jpg
```

### requirements.txt

```
Flask
```

Lists all libraries needed for your app.

**Install with**: `pip install -r requirements.txt`

---

## 📝 Your First Flask App

### Simple Hello World

**app.py**:
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return "hello, world"
```

**Run**: `flask run`

**What it does**:
- Creates Flask application
- `@app.route("/")` - Defines URL route (homepage)
- `def index()` - Function that handles that route
- Returns plain text to browser

### Returning HTML

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return '<!DOCTYPE html><html><body>hello, world</body></html>'
```

**Problem**: Mixing Python and HTML is messy!

---

## 📄 Templates

### Why Templates?

**Separation of concerns**:
- Python (app.py) - Logic
- HTML (templates/) - Presentation

### Using Templates

**templates/index.html**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>hello</title>
</head>
<body>
    hello, {{ name }}
</body>
</html>
```

**app.py**:
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html", name="world")
```

**`{{ name }}`** - Jinja template variable (gets replaced with value)

### Passing Data to Templates

```python
@app.route("/")
def index():
    return render_template("index.html", 
                         name="Alice",
                         age=25,
                         scores=[95, 87, 92])
```

**In template**:
```html
<p>Name: {{ name }}</p>
<p>Age: {{ age }}</p>
<p>First score: {{ scores[0] }}</p>
```

---

## 📋 Routes and Parameters

### URL Parameters (Query Strings)

**URL**: `http://example.com/?name=Alice`

**app.py**:
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    # Get 'name' from URL, default to 'world' if not provided
    name = request.args.get("name", "world")
    return render_template("index.html", name=name)
```

**`request.args.get()`** - Gets value from URL query string

### Multiple Routes

```python
@app.route("/")
def index():
    return render_template("index.html")

@app.route("/greet")
def greet():
    name = request.args.get("name")
    return render_template("greet.html", name=name)
```

**Different paths** trigger different functions!

---

## 📮 Forms and Request Methods

### GET vs POST

**GET**:
- Data visible in URL: `?name=Alice&age=25`
- For retrieving data
- Can be bookmarked
- **Not secure** for passwords!

**POST**:
- Data hidden in request body
- For sending data
- Cannot be bookmarked
- **More secure** for sensitive data

### Basic Form (GET)

**templates/index.html**:
```html
<form action="/greet" method="get">
    <input name="name" placeholder="Name" type="text">
    <button type="submit">Greet</button>
</form>
```

**app.py**:
```python
@app.route("/greet")
def greet():
    name = request.args.get("name")
    return render_template("greet.html", name=name)
```

### POST Form

**templates/index.html**:
```html
<form action="/greet" method="post">
    <input name="name" placeholder="Name" type="text">
    <button type="submit">Greet</button>
</form>
```

**app.py**:
```python
@app.route("/greet", methods=["POST"])
def greet():
    name = request.form.get("name")  # Note: form, not args!
    return render_template("greet.html", name=name)
```

**Key difference**: `request.form.get()` for POST data!

### Handling Both GET and POST

```python
@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        name = request.form.get("name")
        return render_template("greet.html", name=name)
    return render_template("index.html")
```

**One route, two behaviors**:
- GET: Show form
- POST: Process form and show result

---

## 🎨 Template Inheritance

### The Problem: Repeated Code

**index.html**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Site</title>
</head>
<body>
    <h1>Home</h1>
    <p>Welcome!</p>
</body>
</html>
```

**greet.html**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Site</title>  <!-- Repeated! -->
</head>
<body>
    <h1>Greeting</h1>
    <p>Hello!</p>
</body>
</html>
```

**DRY principle violated!** (Don't Repeat Yourself)

### The Solution: Layout Template

**templates/layout.html**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Site</title>
</head>
<body>
    {% block body %}{% endblock %}
</body>
</html>
```

**templates/index.html**:
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Home</h1>
    <p>Welcome!</p>
{% endblock %}
```

**templates/greet.html**:
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Greeting</h1>
    <p>Hello, {{ name }}!</p>
{% endblock %}
```

**Benefits**:
- ✅ No repeated code
- ✅ Change layout once, updates all pages
- ✅ Cleaner, more maintainable

---

## 🔧 Jinja Template Syntax

### Variables

```html
{{ name }}
{{ user.email }}
{{ scores[0] }}
```

### Conditionals

```html
{% if name %}
    Hello, {{ name }}
{% else %}
    Hello, world
{% endif %}
```

### Loops

```html
<ul>
{% for sport in sports %}
    <li>{{ sport }}</li>
{% endfor %}
</ul>
```

**In app.py**:
```python
sports = ["Basketball", "Soccer", "Ultimate Frisbee"]
return render_template("index.html", sports=sports)
```

### Whitespace Control

```html
{% if name -%}
    {{ name }}
{%- else -%}
    world
{%- endif %}
```

**`-`** removes whitespace before/after tag

---

## 📊 Complete Example: Frosh IMs

### Project Structure

```
froshims/
├── app.py
├── requirements.txt
├── templates/
│   ├── layout.html
│   ├── index.html
│   ├── success.html
│   └── failure.html
└── static/
    └── cat.jpg
```

### app.py

```python
from flask import Flask, render_template, request

app = Flask(__name__)

SPORTS = [
    "Basketball",
    "Soccer",
    "Ultimate Frisbee"
]

@app.route("/")
def index():
    return render_template("index.html", sports=SPORTS)

@app.route("/register", methods=["POST"])
def register():
    # Validate submission
    name = request.form.get("name")
    sport = request.form.get("sport")
    
    if not name or sport not in SPORTS:
        return render_template("failure.html")
    
    # Registration successful
    return render_template("success.html")
```

### templates/index.html

```html
{% extends "layout.html" %}

{% block body %}
    <h1>Register</h1>
    <form action="/register" method="post">
        <input name="name" placeholder="Name" type="text">
        
        <select name="sport">
            <option value="">Sport</option>
            {% for sport in sports %}
                <option value="{{ sport }}">{{ sport }}</option>
            {% endfor %}
        </select>
        
        <button type="submit">Register</button>
    </form>
{% endblock %}
```

### Form Input Types

**Radio buttons** (choose one):
```html
{% for sport in sports %}
    <input name="sport" type="radio" value="{{ sport }}"> {{ sport }}
{% endfor %}
```

**Checkboxes** (choose multiple):
```html
{% for sport in sports %}
    <input name="sport" type="checkbox" value="{{ sport }}"> {{ sport }}
{% endfor %}
```

**Get multiple values**:
```python
sports = request.form.getlist("sport")
```

---

## 🗄️ Flask with SQL Database

### Adding SQL

**requirements.txt**:
```
cs50
Flask
```

**app.py**:
```python
from cs50 import SQL
from flask import Flask, render_template, request, redirect

app = Flask(__name__)

# Connect to database
db = SQL("sqlite:///froshims.db")

SPORTS = ["Basketball", "Soccer", "Ultimate Frisbee"]

@app.route("/")
def index():
    return render_template("index.html", sports=SPORTS)

@app.route("/register", methods=["POST"])
def register():
    name = request.form.get("name")
    sport = request.form.get("sport")
    
    # Validate
    if not name:
        return render_template("error.html", message="Missing name")
    if sport not in SPORTS:
        return render_template("error.html", message="Invalid sport")
    
    # Insert into database
    db.execute("INSERT INTO registrants (name, sport) VALUES(?, ?)",
               name, sport)
    
    # Redirect to registrants page
    return redirect("/registrants")

@app.route("/registrants")
def registrants():
    # Get all registrants from database
    rows = db.execute("SELECT * FROM registrants")
    return render_template("registrants.html", registrants=rows)
```

### Displaying Database Data

**templates/registrants.html**:
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

**Access columns**: `registrant.name` or `registrant["name"]`

### Deregistering

**templates/registrants.html** (add column):
```html
<td>
    <form action="/deregister" method="post">
        <input name="id" type="hidden" value="{{ registrant.id }}">
        <button type="submit">Deregister</button>
    </form>
</td>
```

**app.py**:
```python
@app.route("/deregister", methods=["POST"])
def deregister():
    id = request.form.get("id")
    if id:
        db.execute("DELETE FROM registrants WHERE id = ?", id)
    return redirect("/registrants")
```

**Hidden input** passes ID without showing it to user!

---

## 🍪 Sessions and Cookies

### The Problem

**Without sessions**: Server can't tell users apart!

**Need**: Way to remember who's logged in

### What Are Sessions?

**Session**: Server-side storage for user data
**Cookie**: Small piece of data browser stores and sends with each request

**Flow**:
1. User logs in
2. Server creates session
3. Server sends cookie to browser with session ID
4. Browser includes cookie with every request
5. Server uses session ID to retrieve user data

### Implementing Sessions

**requirements.txt**:
```
Flask
Flask-Session
```

**app.py**:
```python
from flask import Flask, render_template, request, session, redirect
from flask_session import Session

app = Flask(__name__)

# Configure session
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)

@app.route("/")
def index():
    # Check if user is logged in
    if "name" in session:
        return render_template("index.html", name=session["name"])
    return redirect("/login")

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        # Store name in session
        session["name"] = request.form.get("name")
        return redirect("/")
    return render_template("login.html")

@app.route("/logout")
def logout():
    # Clear session
    session.clear()
    return redirect("/")
```

### Session Dictionary

**Store data**:
```python
session["name"] = "Alice"
session["user_id"] = 123
session["cart"] = []
```

**Retrieve data**:
```python
name = session.get("name")  # Returns None if not found
```

**Check if exists**:
```python
if "name" in session:
    # User is logged in
```

**Clear session**:
```python
session.clear()
```

---

## 🛒 Shopping Cart Example

```python
@app.route("/cart", methods=["GET", "POST"])
def cart():
    # Ensure cart exists
    if "cart" not in session:
        session["cart"] = []
    
    # POST - Add to cart
    if request.method == "POST":
        book_id = request.form.get("id")
        if book_id:
            session["cart"].append(book_id)
        return redirect("/cart")
    
    # GET - View cart
    if session["cart"]:
        books = db.execute("SELECT * FROM books WHERE id IN (?)",
                          session["cart"])
    else:
        books = []
    
    return render_template("cart.html", books=books)
```

**Key concepts**:
- Session stores list of book IDs
- POST adds items
- GET displays items

---

## 🔍 Dynamic Search with AJAX

### Traditional Search (Page Reload)

**Problem**: Entire page reloads on every search

### AJAX Search (No Reload!)

**templates/index.html**:
```html
<input id="search" placeholder="Query" type="text">
<ul id="results"></ul>

<script>
    let input = document.querySelector('#search');
    
    input.addEventListener('input', async function() {
        // Fetch results from server
        let response = await fetch('/search?q=' + input.value);
        let shows = await response.text();
        
        // Update page
        document.querySelector('#results').innerHTML = shows;
    });
</script>
```

**app.py**:
```python
@app.route("/search")
def search():
    q = request.args.get("q")
    if q:
        shows = db.execute("SELECT * FROM shows WHERE title LIKE ? LIMIT 50",
                          "%" + q + "%")
    else:
        shows = []
    return render_template("search.html", shows=shows)
```

**templates/search.html**:
```html
{% for show in shows %}
    <li>{{ show.title }}</li>
{% endfor %}
```

**Flow**:
1. User types in search box
2. JavaScript detects input
3. Fetches results from server
4. Updates page without reload

---

## 📦 JSON APIs

### What is JSON?

**JSON**: JavaScript Object Notation
- Text format for data
- Human-readable
- Language-independent

**Example**:
```json
[
    {"id": 1, "title": "The Office", "year": 2005},
    {"id": 2, "title": "Friends", "year": 1994}
]
```

### Returning JSON from Flask

**app.py**:
```python
from flask import jsonify

@app.route("/search")
def search():
    q = request.args.get("q")
    if q:
        shows = db.execute("SELECT * FROM shows WHERE title LIKE ? LIMIT 50",
                          "%" + q + "%")
    else:
        shows = []
    
    # Return as JSON
    return jsonify(shows)
```

### Using JSON in JavaScript

**templates/index.html**:
```html
<input id="search" type="text">
<ul id="results"></ul>

<script>
    let input = document.querySelector('#search');
    
    input.addEventListener('input', async function() {
        // Fetch JSON
        let response = await fetch('/search?q=' + input.value);
        let shows = await response.json();  // Parse JSON
        
        // Build HTML
        let html = '';
        for (let show of shows) {
            html += '<li>' + show.title + '</li>';
        }
        
        // Update page
        document.querySelector('#results').innerHTML = html;
    });
</script>
```

**Key difference**: `response.json()` instead of `response.text()`

---

## 🏗️ MVC Architecture

### What is MVC?

**Model-View-Controller**: Design pattern for organizing code

**Model**: Data (SQL database)
**View**: Presentation (HTML templates)
**Controller**: Logic (Flask routes in app.py)

**Example**:
```
User clicks button
    ↓
Controller (app.py route)
    ↓
Model (SQL query)
    ↓
Controller (process data)
    ↓
View (render template)
    ↓
HTML sent to user
```

**Benefits**:
- ✅ Separation of concerns
- ✅ Easier to maintain
- ✅ Easier to test
- ✅ Multiple developers can work simultaneously

---

## 🔐 Security Best Practices

### 1. Validate All Input

```python
@app.route("/register", methods=["POST"])
def register():
    name = request.form.get("name")
    
    # Validate
    if not name:
        return render_template("error.html", message="Missing name")
    
    # Process...
```

### 2. Use POST for Sensitive Data

```html
<!-- WRONG - password visible in URL! -->
<form action="/login" method="get">
    <input name="password" type="password">
</form>

<!-- CORRECT - password hidden -->
<form action="/login" method="post">
    <input name="password" type="password">
</form>
```

### 3. Escape User Input

**Flask templates do this automatically!**

```html
<!-- User types: <script>alert('hack')</script> -->
<!-- Flask outputs: &lt;script&gt;alert('hack')&lt;/script&gt; -->
{{ user_input }}
```

### 4. Use Sessions Properly

```python
# Check if user is logged in
if "user_id" not in session:
    return redirect("/login")
```

### 5. SQL Injection Protection

```python
# WRONG - vulnerable to SQL injection!
name = request.form.get("name")
db.execute(f"SELECT * FROM users WHERE name = '{name}'")

# CORRECT - parameterized query
db.execute("SELECT * FROM users WHERE name = ?", name)
```

---

## ⚠️ Common Mistakes

### 1. Forgetting to Import

```python
# ERROR!
@app.route("/")
def index():
    return render_template("index.html")  # NameError!

# CORRECT
from flask import Flask, render_template

@app.route("/")
def index():
    return render_template("index.html")
```

### 2. Wrong Request Data Access

```python
# GET request
name = request.args.get("name")

# POST request
name = request.form.get("name")  # Not args!
```

### 3. Template in Wrong Folder

```
project/
├── app.py
└── index.html  # WRONG - won't be found!

CORRECT:
project/
├── app.py
└── templates/
    └── index.html
```

### 4. Not Redirecting After POST

```python
# WRONG - user can refresh and resubmit form
@app.route("/register", methods=["POST"])
def register():
    # Process registration...
    return render_template("success.html")

# CORRECT - redirect to prevent double submission
@app.route("/register", methods=["POST"])
def register():
    # Process registration...
    return redirect("/registrants")
```

---

## ✅ Self-Check Questions

1. Can I explain the difference between static and dynamic pages?
2. Do I understand what routes do in Flask?
3. Can I create and use templates with Jinja?
4. Do I know when to use GET vs POST?
5. Can I connect Flask to a SQL database?
6. Do I understand how sessions work?
7. Can I create an AJAX-powered search?
8. Do I know what JSON is and how to use it?

---

## 🚀 Practice Exercises

### Beginner
- [ ] Create a simple Flask app that greets users
- [ ] Add a form that accepts user input
- [ ] Create multiple routes
- [ ] Use template inheritance
- [ ] Display data from a list

### Intermediate
- [ ] Connect to a SQLite database
- [ ] Implement a registration form
- [ ] Create a login/logout system
- [ ] Display database data in a table
- [ ] Add form validation

### Advanced
- [ ] Build a shopping cart with sessions
- [ ] Implement AJAX search
- [ ] Create a JSON API
- [ ] Build a complete CRUD application
- [ ] Add user authentication

---

## 📌 Important Reminders

1. **Templates go in `templates/` folder**
2. **Static files go in `static/` folder**
3. **Use `render_template()`** not `return` for HTML
4. **`request.args`** for GET, **`request.form`** for POST
5. **Always validate user input**
6. **Use `?` placeholders** in SQL queries
7. **Redirect after POST** to prevent resubmission
8. **Sessions need configuration** before use
9. **`jsonify()`** for API responses
10. **Test in browser**, not just terminal!

---

*Flask brings together everything you've learned. You can now build real, dynamic web applications!*