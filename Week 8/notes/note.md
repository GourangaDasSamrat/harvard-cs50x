# CS50 Lecture 8 - HTML, CSS & JavaScript
## Personal Learning Notes

---

## 🎯 Key Takeaways

- **The Internet connects computers** - Through routers and protocols
- **HTML structures content** - The skeleton of web pages
- **CSS styles content** - The appearance and design
- **JavaScript adds interactivity** - Dynamic behavior in the browser
- **Frameworks speed development** - Bootstrap and others provide pre-built components
- **Web development builds on programming fundamentals** - Same logic, different languages

---

## 🌐 How the Internet Works

### Internet Basics

**Internet**: Interconnected network of computers worldwide

**ARPANET**: First version of the internet

### IP Addresses

**IP (Internet Protocol)**: Way computers identify each other

**IPv4 Format**: `#.#.#.#`
- Each number: 0-255
- Example: `192.168.1.1`
- 32-bit addresses = ~4 billion possible addresses

**IPv6**: 
- 128-bit addresses
- Many more possible addresses
- Designed for future growth

### Routers

**Router**: Device that directs data between computers

**How it works**:
```
Computer A → Router 1 → Router 2 → Router 3 → Computer B
```

**Multiple paths**: If one route is congested, data can take another path

### Packets

**Packet**: Small chunk of data sent over the internet

**Contains**:
- Source IP address
- Destination IP address
- Portion of the actual data
- Sequence number

**Large files** split into multiple packets

### TCP/IP Protocols

**TCP (Transmission Control Protocol)**:
- Keeps track of packet sequence
- Requests missing packets
- Acknowledges complete transmission

**Port Numbers**:
- Distinguish different services
- HTTP: Port 80
- HTTPS: Port 443

---

## 🔍 Internet Services

### DNS (Domain Name System)

**Problem**: Remembering IP addresses is hard!

**Solution**: DNS translates domain names to IP addresses

```
harvard.edu → 123.45.67.89
```

**DNS is a giant lookup table** (database) mapping names to IPs

### DHCP (Dynamic Host Configuration Protocol)

**What it does**:
- Assigns IP address to your device
- Defines default gateway
- Defines nameservers

### HTTP/HTTPS

**HTTP**: HyperText Transfer Protocol
**HTTPS**: HTTP Secure (encrypted)

**URL Structure**:
```
https://www.example.com/folder/file.html
  │      │            │      │
protocol domain      path   file
```

**Implicit slash**: `example.com` = `example.com/`

**Top-level domain**: `.com`, `.edu`, `.org`, etc.

---

## 📡 HTTP Requests & Responses

### HTTP Methods

**GET**: Request data from server
```
GET / HTTP/2
Host: www.harvard.edu
```

**POST**: Send data to server (forms, uploads)

### HTTP Response Codes

**2xx - Success**:
- `200 OK` - Request succeeded

**3xx - Redirection**:
- `301 Moved Permanently` - Resource has new location
- `302 Found` - Temporary redirect
- `304 Not Modified` - Use cached version

**4xx - Client Errors**:
- `401 Unauthorized` - Authentication required
- `403 Forbidden` - Access denied
- `404 Not Found` - Resource doesn't exist
- `418 I'm a Teapot` - Easter egg!

**5xx - Server Errors**:
- `500 Internal Server Error` - Server-side problem
- `503 Service Unavailable` - Server overloaded

**Your fault**: Usually 500 errors in your own apps!

### Using curl

```bash
# Get response headers
curl -I https://www.harvard.edu/

# Check for redirects
curl -I https://harvard.edu
# Shows: 301 redirect to www.harvard.edu

# Check HTTP vs HTTPS
curl -I http://www.harvard.edu/
# Shows: 301 redirect to https version
```

---

## 📝 HTML Basics

### What is HTML?

**HTML**: HyperText Markup Language

**Markup language**: Uses tags to structure content

**Not a programming language** - describes structure, not logic

### Basic HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Page</title>
</head>
<body>
    Content goes here
</body>
</html>
```

**Key parts**:
- `<!DOCTYPE html>` - Declares HTML5
- `<html>` - Root element
- `<head>` - Metadata (not visible)
- `<body>` - Visible content

### HTML Tag Structure

**Opening tag**: `<tag>`
**Closing tag**: `</tag>`
**Self-closing**: `<img src="photo.jpg">`

**Attributes**: Modify tag behavior
```html
<html lang="en">
      ↑     ↑
   attribute value
```

### Comments

```html
<!-- This is a comment -->
<!-- Comments are not displayed in the browser -->
```

---

## 📄 Common HTML Elements

### Headings

```html
<h1>Biggest Heading</h1>
<h2>Second Level</h2>
<h3>Third Level</h3>
<h4>Fourth Level</h4>
<h5>Fifth Level</h5>
<h6>Smallest Heading</h6>
```

**6 levels** of headings, `<h1>` being most important

### Paragraphs

```html
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>
```

**Whitespace and newlines** in HTML are ignored - use `<p>` tags!

### Lists

**Unordered (bullets)**:
```html
<ul>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>
```

**Ordered (numbered)**:
```html
<ol>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ol>
```

### Links

```html
<a href="https://www.harvard.edu">Visit Harvard</a>
<a href="page2.html">Another page on my site</a>
```

**`href`**: Hypertext reference (where to go)

### Images

```html
<img src="photo.jpg" alt="Description of photo">
```

**Attributes**:
- `src` - Source (file path)
- `alt` - Alternative text (accessibility & if image fails)

**Self-closing tag** - no `</img>`!

### Videos

```html
<video controls muted>
    <source src="video.mp4" type="video/mp4">
</video>
```

**Attributes**:
- `controls` - Show play/pause buttons
- `muted` - Start muted

### Tables

```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Alice</td>
            <td>25</td>
        </tr>
        <tr>
            <td>Bob</td>
            <td>30</td>
        </tr>
    </tbody>
</table>
```

**Structure**:
- `<table>` - Container
- `<thead>` - Header rows
- `<tbody>` - Body rows
- `<tr>` - Table row
- `<th>` - Header cell
- `<td>` - Data cell

### Divs (Generic Containers)

```html
<div>
    <p>Content grouped together</p>
    <p>More content</p>
</div>
```

**`<div>`**: Generic container for grouping content

### Semantic HTML5 Tags

```html
<header>Site header</header>
<nav>Navigation menu</nav>
<main>Main content</main>
<article>Article content</article>
<section>Section of content</section>
<footer>Site footer</footer>
```

**Better than `<div>`** - conveys meaning!

---

## 📋 HTML Forms

### Basic Form

```html
<form action="https://www.google.com/search" method="get">
    <input name="q" type="text">
    <input type="submit" value="Search">
</form>
```

**Attributes**:
- `action` - Where to send data
- `method` - How to send (GET or POST)

### Input Types

```html
<!-- Text input -->
<input type="text" name="username">

<!-- Password (hidden text) -->
<input type="password" name="pass">

<!-- Email (validated) -->
<input type="email" name="email">

<!-- Number -->
<input type="number" name="age">

<!-- Search box -->
<input type="search" name="q">

<!-- Submit button -->
<input type="submit" value="Submit">

<!-- Regular button -->
<button>Click Me</button>
```

### Input Attributes

```html
<input 
    type="text" 
    name="username"
    placeholder="Enter your name"
    autocomplete="off"
    autofocus
    required
>
```

**Common attributes**:
- `placeholder` - Hint text
- `autocomplete` - Enable/disable autocomplete
- `autofocus` - Focus on page load
- `required` - Must be filled
- `name` - Identifies the field

---

## 🎨 Regular Expressions (Regex)

### What Are Regular Expressions?

**Regex**: Pattern for matching text

**Used for**: Validating user input

### Basic Patterns

```html
<!-- Email must contain @ and .edu -->
<input 
    type="email" 
    pattern=".+@.+\.edu"
    placeholder="Email"
>
```

**Pattern breakdown**:
- `.` - Any character
- `+` - One or more
- `@` - Literal @ symbol
- `\.` - Literal . (escaped)

### Common Regex Symbols

| Symbol | Meaning |
|--------|---------|
| `.` | Any character |
| `+` | One or more |
| `*` | Zero or more |
| `?` | Zero or one |
| `^` | Start of string |
| `$` | End of string |
| `\d` | Any digit |
| `\w` | Any word character |

**Example**: Phone number pattern
```html
<input pattern="\d{3}-\d{3}-\d{4}">
<!-- Matches: 123-456-7890 -->
```

---

## 🎨 CSS (Cascading Style Sheets)

### What is CSS?

**CSS**: Styling language for HTML

**Key-value pairs** of properties:
```css
property: value;
```

### Three Ways to Apply CSS

**1. Inline (not recommended)**:
```html
<p style="color: red; font-size: 20px;">Text</p>
```

**2. Internal (in `<head>`)**:
```html
<head>
    <style>
        p {
            color: red;
            font-size: 20px;
        }
    </style>
</head>
```

**3. External (separate file) ✅ Best**:
```html
<!-- In HTML file -->
<head>
    <link href="style.css" rel="stylesheet">
</head>
```

```css
/* In style.css file */
p {
    color: red;
    font-size: 20px;
}
```

### CSS Selectors

**Element selector**:
```css
p {
    color: blue;
}
/* Applies to all <p> tags */
```

**Class selector** (with `.`):
```css
.large {
    font-size: 24px;
}
```
```html
<p class="large">Big text</p>
```

**ID selector** (with `#`):
```css
#header {
    background-color: gray;
}
```
```html
<div id="header">Header</div>
```

**Multiple selectors**:
```css
h1, h2, h3 {
    color: navy;
}
```

### Common CSS Properties

**Text styling**:
```css
.text {
    color: blue;
    font-size: 16px;
    font-family: Arial, sans-serif;
    font-weight: bold;
    text-align: center;
    text-decoration: underline;
}
```

**Box properties**:
```css
.box {
    width: 300px;
    height: 200px;
    margin: 10px;        /* Outside spacing */
    padding: 15px;       /* Inside spacing */
    border: 2px solid black;
    background-color: lightgray;
}
```

**Display**:
```css
.hidden {
    display: none;
    visibility: hidden;
}
```

### Cascading

**Why "Cascading"?** Styles flow down to children!

```html
<body style="text-align: center">
    <div>This is centered</div>
    <p>This is also centered</p>
</body>
```

Children inherit parent styles (unless overridden)

---

## 🎭 CSS Classes Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        .centered {
            text-align: center;
        }
        .large {
            font-size: 24px;
        }
        .medium {
            font-size: 16px;
        }
        .small {
            font-size: 12px;
        }
    </style>
    <title>CSS Demo</title>
</head>
<body class="centered">
    <header class="large">
        John Harvard
    </header>
    <main class="medium">
        Welcome to my home page!
    </main>
    <footer class="small">
        Copyright © John Harvard
    </footer>
</body>
</html>
```

---

## 🎨 Bootstrap Framework

### What is Bootstrap?

**Bootstrap**: CSS framework for beautiful, responsive websites

**Benefits**:
- Pre-designed components
- Responsive (mobile-friendly)
- Saves time
- Professional look

### Including Bootstrap

```html
<head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js"></script>
    <title>My Page</title>
</head>
```

### Bootstrap Classes

**Table**:
```html
<table class="table">
    <thead>
        <tr>
            <th scope="col">Name</th>
            <th scope="col">Age</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Alice</td>
            <td>25</td>
        </tr>
    </tbody>
</table>
```

**Buttons**:
```html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-success">Success</button>
<button class="btn btn-danger">Danger</button>
```

**Grid system**:
```html
<div class="container">
    <div class="row">
        <div class="col">Column 1</div>
        <div class="col">Column 2</div>
        <div class="col">Column 3</div>
    </div>
</div>
```

**Learn more**: [Bootstrap Documentation](https://getbootstrap.com/docs/)

---

## ⚡ JavaScript Basics

### What is JavaScript?

**JavaScript**: Programming language that runs in the browser

**Adds interactivity**:
- Responding to clicks
- Validating forms
- Updating page content
- Animations

**Not Java!** Completely different language.

### Including JavaScript

**Internal** (in HTML):
```html
<script>
    // JavaScript code here
    alert('Hello!');
</script>
```

**External** (separate file):
```html
<script src="script.js"></script>
```

### Basic Syntax

```javascript
// Variables
let name = "Alice";
const age = 25;

// Functions
function greet() {
    alert('Hello!');
}

// Conditionals
if (age >= 18) {
    console.log('Adult');
} else {
    console.log('Minor');
}

// Loops
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

**Similar to C and Python** - same concepts!

---

## 🎯 DOM Manipulation

### What is the DOM?

**DOM**: Document Object Model
- Tree representation of HTML
- JavaScript can read and modify it

**Tree structure**:
```
html
 ├── head
 │    └── title
 └── body
      ├── h1
      └── p
```

### Selecting Elements

```javascript
// By ID
let element = document.querySelector('#myId');

// By class
let elements = document.querySelectorAll('.myClass');

// By tag
let paragraphs = document.querySelectorAll('p');
```

### Modifying Content

```javascript
// Change text
element.innerHTML = 'New text';

// Change attribute
element.setAttribute('class', 'highlight');

// Change style
element.style.color = 'red';
element.style.fontSize = '20px';
```

### Getting Input Values

```javascript
let input = document.querySelector('#name');
let value = input.value;  // Get what user typed
```

---

## 🎬 Event Listeners

### What Are Events?

**Events**: Things that happen in the browser
- Click
- Submit
- Key press
- Mouse movement

### Adding Event Listeners

```javascript
// Wait for page to load
document.addEventListener('DOMContentLoaded', function() {
    
    // Listen for button click
    document.querySelector('#myButton').addEventListener('click', function() {
        alert('Button clicked!');
    });
    
});
```

**Why `DOMContentLoaded`?** Ensures HTML is loaded before JavaScript runs!

### Form Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            document.querySelector('form').addEventListener('submit', function(e) {
                let name = document.querySelector('#name').value;
                alert('Hello, ' + name);
                e.preventDefault();  // Don't actually submit
            });
        });
    </script>
    <title>Greeting</title>
</head>
<body>
    <form>
        <input id="name" placeholder="Name" type="text">
        <input type="submit">
    </form>
</body>
</html>
```

### Common Events

```javascript
// Click
element.addEventListener('click', function() { });

// Submit
form.addEventListener('submit', function(e) { });

// Keyup (typing)
input.addEventListener('keyup', function(e) { });

// Change
select.addEventListener('change', function() { });
```

---

## 🔄 Dynamic Updates

### Live Greeting Example

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
    let input = document.querySelector('#name');
    
    input.addEventListener('keyup', function() {
        let greeting = document.querySelector('#greeting');
        
        if (input.value) {
            greeting.innerHTML = `Hello, ${input.value}`;
        } else {
            greeting.innerHTML = 'Hello, whoever you are';
        }
    });
});
</script>

<input id="name" type="text">
<p id="greeting">Hello, whoever you are</p>
```

**Updates in real-time** as user types!

### Template Literals

**Old way**:
```javascript
let msg = 'Hello, ' + name + '!';
```

**New way** (template literals):
```javascript
let msg = `Hello, ${name}!`;
```

Use backticks ``` ` ``` and `${variable}` for interpolation!

---

## 🎨 Changing Styles with JavaScript

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Background Color</title>
</head>
<body>
    <button id="red">Red</button>
    <button id="green">Green</button>
    <button id="blue">Blue</button>
    
    <script>
        let body = document.querySelector('body');
        
        document.querySelector('#red').addEventListener('click', function() {
            body.style.backgroundColor = 'red';
        });
        
        document.querySelector('#green').addEventListener('click', function() {
            body.style.backgroundColor = 'green';
        });
        
        document.querySelector('#blue').addEventListener('click', function() {
            body.style.backgroundColor = 'blue';
        });
    </script>
</body>
</html>
```

**Click buttons** to change background color!

---

## ⏰ Timers and Intervals

### setInterval

**Repeat function** at regular intervals:

```javascript
function blink() {
    let body = document.querySelector('body');
    
    if (body.style.visibility == 'hidden') {
        body.style.visibility = 'visible';
    } else {
        body.style.visibility = 'hidden';
    }
}

// Call blink() every 500 milliseconds
window.setInterval(blink, 500);
```

### setTimeout

**Run function once** after delay:

```javascript
setTimeout(function() {
    alert('5 seconds passed!');
}, 5000);  // 5000 milliseconds = 5 seconds
```

---

## 🔍 Autocomplete Example

```html
<input id="search" type="text">
<ul id="results"></ul>

<script>
const WORDS = ['apple', 'application', 'apply', 'banana', 'berry'];

let input = document.querySelector('#search');

input.addEventListener('keyup', function() {
    let html = '';
    
    if (input.value) {
        for (let word of WORDS) {
            if (word.startsWith(input.value)) {
                html += `<li>${word}</li>`;
            }
        }
    }
    
    document.querySelector('#results').innerHTML = html;
});
</script>
```

**Shows matching words** as user types!

---

## 📚 HTML Document Hierarchy

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
        <link rel="stylesheet">
        <script></script>
    </head>
    <body>
        <header>
            <nav></nav>
        </header>
        <main>
            <section>
                <article></article>
            </section>
        </main>
        <footer></footer>
    </body>
</html>
```

**Parent-child relationships** important for CSS and JavaScript!

---

## ⚠️ Common Mistakes

### 1. Forgetting Closing Tags

```html
<!-- WRONG ❌ -->
<p>Paragraph
<p>Another paragraph

<!-- CORRECT ✅ -->
<p>Paragraph</p>
<p>Another paragraph</p>
```

### 2. Running JavaScript Before DOM Loads

```javascript
// WRONG ❌
document.querySelector('#button').addEventListener('click', ...);
// Error: element doesn't exist yet!

// CORRECT ✅
document.addEventListener('DOMContentLoaded', function() {
    document.querySelector('#button').addEventListener('click', ...);
});
```

### 3. Using = Instead of ==

```javascript
// WRONG ❌ (assignment, not comparison)
if (x = 5) {
    // Always true!
}

// CORRECT ✅
if (x == 5) {
    // Compares
}
```

### 4. Inline Styles Everywhere

```html
<!-- WRONG ❌ - Hard to maintain -->
<p style="color: red; font-size: 20px;">Text</p>
<p style="color: red; font-size: 20px;">More text</p>

<!-- CORRECT ✅ - Use CSS classes -->
<p class="highlight">Text</p>
<p class="highlight">More text</p>
```

---

## ✅ Self-Check Questions

1. Can I explain how the internet routes data?
2. Do I know the basic structure of an HTML document?
3. Can I create forms with different input types?
4. Do I understand the difference between inline, internal, and external CSS?
5. Can I select elements with JavaScript?
6. Do I know how to add event listeners?
7. Can I modify the DOM dynamically?
8. Do I understand when to use Bootstrap?

---

## 🚀 Practice Exercises

### Beginner
- [ ] Create a simple HTML page with headings and paragraphs
- [ ] Add a list and a table
- [ ] Style elements with inline CSS
- [ ] Create a form with validation
- [ ] Include an image and video

### Intermediate
- [ ] Create external CSS file and use classes
- [ ] Build a responsive page with Bootstrap
- [ ] Add JavaScript alert on button click
- [ ] Create a form that greets user on submit
- [ ] Change background color with buttons

### Advanced
- [ ] Build autocomplete search box
- [ ] Create dynamic to-do list
- [ ] Implement form validation with regex
- [ ] Build interactive color picker
- [ ] Create animated elements with JavaScript

---

## 📌 Important Reminders

1. **HTML structures**, CSS styles, JavaScript adds behavior
2. **Close all tags** (except self-closing ones)
3. **Use semantic HTML5 tags** when possible
4. **External CSS is best** for maintainability
5. **Always wait for `DOMContentLoaded`** before running JavaScript
6. **`querySelector`** returns first match
7. **`querySelectorAll`** returns all matches
8. **Prevent form submission** with `e.preventDefault()`
9. **Bootstrap classes** must match documentation exactly
10. **Test in multiple browsers** for compatibility

---

*Web development combines all your programming skills with visual design. Start simple, then add complexity!*