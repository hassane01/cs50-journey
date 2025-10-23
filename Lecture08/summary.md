# 🌐 CS50 Lecture 8: HTML, CSS, JavaScript

> **Course**: CS50 - Harvard University's Introduction to Computer Science  
> **Lecture**: Week 8 - Web Programming  
> **Focus**: Internet fundamentals, HTML markup, CSS styling, and JavaScript interactivity

---

## 🎯 Learning Objectives

By the end of this lecture, you will understand:
- How the internet works (TCP/IP, DNS, DHCP, HTTP/HTTPS)
- HTML structure and semantic tags
- CSS styling techniques and frameworks
- JavaScript for dynamic web interactivity
- DOM manipulation and event handling
- Form validation with regular expressions
- Responsive web design with Bootstrap

---

## 📑 Table of Contents

1. [The Internet](#-the-internet)
2. [TCP/IP Protocol](#-tcpip-protocol)
3. [DNS and DHCP](#-dns-and-dhcp)
4. [HTTP/HTTPS](#-httphttps)
5. [HTML Fundamentals](#-html-fundamentals)
6. [Regular Expressions](#-regular-expressions)
7. [CSS Styling](#-css-styling)
8. [Bootstrap Framework](#-bootstrap-framework)
9. [JavaScript Programming](#-javascript-programming)
10. [DOM Manipulation](#-dom-manipulation)

---

## 🌍 The Internet

### 🔌 What is the Internet?

The **Internet** is the underlying infrastructure that connects computers worldwide, enabling data transfer from point A to point B.

**Historical Context:**
- **1969**: ARPANET connected first few universities (West Coast USA)
- **Early expansion**: Harvard and others joined (East Coast)
- **Today**: Billions of interconnected devices worldwide

### 🛣️ Routers

**Routers** are specialized computers that direct data packets through the internet.

**Visualization:**
```
[Phyllis] → [Router 1] → [Router 2] → [Router 3] → [Brian]
```

**Key Concepts:**
- Routers live in **data centers** (large warehouses)
- Each router decides: **up, down, left, or right?**
- Multiple paths exist between any two points
- Dynamic routing based on network congestion

---

## 📦 TCP/IP Protocol

### 🔢 IP Addresses (Internet Protocol)

**Format**: `#.#.#.#` (e.g., `1.2.3.4`)

**Properties:**
- Each number ranges from **0-255**
- **32-bit addresses** (4 bytes)
- **4 billion possible addresses** (2³²)
- **IPv4** is current standard
- **IPv6** (128-bit) supports far more devices

### 📧 The Envelope Analogy

**Virtual envelope structure:**
```
┌────────────────────────────┐
│ From: 5.6.7.8             │ ← Source IP
│ To: 1.2.3.4               │ ← Destination IP
│ Port: 443                 │ ← Service type
│ Sequence: 1 of 4          │ ← Packet order
├────────────────────────────┤
│ [Data: Picture of cat]    │
└────────────────────────────┘
```

### 🔄 TCP (Transmission Control Protocol)

**TCP adds three critical features:**

#### 1️⃣ **Sequence Numbers**
Ensures packets are reassembled in correct order:
```
Packet 1 of 4 → [Quarter of cat image]
Packet 2 of 4 → [Another quarter]
Packet 3 of 4 → [Third quarter]
Packet 4 of 4 → [Final quarter]
```

#### 2️⃣ **Acknowledgments**
Receiver confirms receipt; sender resends if needed

#### 3️⃣ **Port Numbers**
Identifies which service should handle the request

| Port | Service | Protocol |
|------|---------|----------|
| `80` | Web (insecure) | HTTP |
| `443` | Web (secure) | HTTPS |
| `25` | Email | SMTP |
| `53` | Domain lookup | DNS |

### 📊 Packet Structure

**IP Packet Header:**
```
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| IHL |Type of Service |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Source Address          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Destination Address        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Complete addressing:**
```
1.2.3.4:443
└─┬──┘ └┬┘
  IP   Port
```

---

## 🗺️ DNS and DHCP

### 🔍 DNS (Domain Name System)

**Purpose**: Translates human-friendly domain names to IP addresses

**Example:**
```
google.com → 142.250.80.46
cs50.ai → [IP address]
```

**DNS Structure:**
```
┌──────────────────────┐
│   Domain Name   │ IP Address  │
├──────────────────────┤
│ google.com      │ 142.250.80.46│
│ harvard.edu     │ 23.185.0.4   │
│ cs50.ai         │ [IP address] │
└──────────────────────┘
```

**How it works:**
1. Your computer checks **local cache**
2. If not found, asks **ISP's DNS server**
3. ISP may ask **other DNS servers** recursively
4. **Root servers** know about `.com`, `.org`, `.edu`, etc.
5. Answer is **cached** for future use

**Domain Name Anatomy:**
```
www.example.com
│   │       │
│   │       └─ TLD (Top-Level Domain)
│   └───────── Domain Name
└───────────── Host Name (subdomain)
```

**TLDs (Top-Level Domains):**
- **.com** - Commercial (originally)
- **.org** - Organization
- **.edu** - Educational
- **.gov** - Government
- **.us, .uk, .jp** - Country codes (2 characters)
- **.ai, .tv, .io** - Repurposed country codes

### ⚙️ DHCP (Dynamic Host Configuration Protocol)

**Purpose**: Automatically assigns IP addresses to devices

**What DHCP provides:**
1. **Your IP address** (e.g., `192.168.1.100`)
2. **DNS server address** (where to look up domains)
3. **Default gateway** (router to send data through)

**Process:**
```
[Device boots up]
     ↓
"What's my IP address?"
     ↓
[DHCP Server responds]
     ↓
"Use 192.168.1.100"
```

---

## 🌐 HTTP/HTTPS

### 📨 HTTP (Hypertext Transfer Protocol)

**Protocol for requesting web pages**

#### GET Request

**Client sends:**
```http
GET / HTTP/2
Host: www.harvard.edu
```

**Server responds:**
```http
HTTP/2 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html>
...
</html>
```

#### POST Request

**Used for sending data (forms, uploads):**
```http
POST /login HTTP/2
Host: www.example.com
Content-Type: application/x-www-form-urlencoded

username=david&password=secret
```

### 🔒 HTTPS (HTTP Secure)

**Encrypted version of HTTP**
- Uses **port 443** instead of 80
- **Scrambles data** so only recipient can read
- **Preferred standard** for all websites

### 📊 HTTP Status Codes

| Code | Meaning | Example |
|------|---------|---------|
| `200` | OK | Page found successfully |
| `301` | Moved Permanently | Site changed address |
| `302` | Found (Temporary Redirect) | Temporary move |
| `304` | Not Modified | Use cached version |
| `401` | Unauthorized | Login required |
| `403` | Forbidden | No access permission |
| `404` | Not Found | Page doesn't exist |
| `418` | I'm a Teapot | Easter egg (joke) |
| `500` | Internal Server Error | **Your fault as developer!** |
| `503` | Service Unavailable | Server down |

### 🔗 URL Structure

**Complete URL breakdown:**
```
https://www.example.com:443/folder/file.html?key=value#section
│─┬─│  │─────┬──────│ │─┬┘ │───┬───│  │──┬───│ │───┬──│  │──┬──│
  │   │      │        │  │   │   │     │   │     │   │      │   │
  │   │      │        │  │   │   │     │   │     │   │      │   └─ Fragment
  │   │      │        │  │   │   │     │   │     │   └──────────── Query string
  │   │      │        │  │   │   │     │   └─────────────────────── File
  │   │      │        │  │   │   └─────────────────────────────────── Path
  │   │      │        │  │   └──────────────────────────────────────── Port
  │   │      │        │  └──────────────────────────────────────────── TLD
  │   │      └─────────────────────────────────────────────────────────── Domain
  │   └────────────────────────────────────────────────────────────────── Subdomain
  └────────────────────────────────────────────────────────────────────── Protocol
```

**Testing URLs:**
```bash
# Check headers
curl -I https://www.harvard.edu/

# Follow redirects
curl -I https://harvard.edu
```

---

## 📝 HTML Fundamentals

### 🏗️ Basic Structure

**Minimal HTML document:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>hello, title</title>
</head>
<body>
    hello, body
</body>
</html>
```

**Key elements:**
- `<!DOCTYPE html>` - Declares HTML5
- `<html lang="en">` - Root element with language
- `<head>` - Metadata (title, styles, scripts)
- `<body>` - Visible content

### 📊 Document Hierarchy

```
html
├── head
│   └── title
└── body
    ├── header
    ├── main
    │   ├── p
    │   ├── img
    │   └── ...
    └── footer
```

### 📄 Text Elements

#### Paragraphs
```html
<p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
</p>
```

#### Headings
```html
<h1>Main Title</h1>      <!-- Largest -->
<h2>Section Title</h2>
<h3>Subsection</h3>
<h4>Sub-subsection</h4>
<h5>Minor heading</h5>
<h6>Smallest heading</h6> <!-- Smallest -->
```

### 📋 Lists

#### Unordered List
```html
<ul>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>
```

**Output:**
- First item
- Second item
- Third item

#### Ordered List
```html
<ol>
    <li>First step</li>
    <li>Second step</li>
    <li>Third step</li>
</ol>
```

**Output:**
1. First step
2. Second step
3. Third step

### 📊 Tables

```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Number</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Alice</td>
            <td>+1-617-495-1000</td>
        </tr>
        <tr>
            <td>Bob</td>
            <td>+1-949-468-2750</td>
        </tr>
    </tbody>
</table>
```

**Elements:**
- `<table>` - Container
- `<thead>` - Header section
- `<tbody>` - Body section
- `<tr>` - Table row
- `<th>` - Header cell (bold)
- `<td>` - Data cell

### 🖼️ Media Elements

#### Images
```html
<img alt="Photo of bridge" src="bridge.png">
```

**Attributes:**
- `src` - Image file path
- `alt` - Alternative text (accessibility)

#### Videos
```html
<video controls muted>
    <source src="video.mp4" type="video/mp4">
</video>
```

**Attributes:**
- `controls` - Show play/pause buttons
- `muted` - Start without sound
- `type` - Video format

### 🔗 Links

```html
<a href="https://www.harvard.edu">Visit Harvard</a>
```

**Attributes:**
- `href` - Destination URL
- Creates clickable text

### 📝 Forms

#### Basic Form
```html
<form action="https://www.google.com/search" method="get">
    <input name="q" type="search">
    <input type="submit" value="Search">
</form>
```

#### Advanced Form
```html
<form action="https://www.google.com/search" method="get">
    <input 
        autocomplete="off" 
        autofocus 
        name="q" 
        placeholder="Query" 
        type="search">
    <button>Google Search</button>
</form>
```

**Input Attributes:**
- `autocomplete="off"` - Disable suggestions
- `autofocus` - Focus on page load
- `placeholder` - Hint text
- `type` - Input type (text, email, password, etc.)

### 🎨 Semantic Tags

**Modern HTML5 semantic elements:**
```html
<header>Site navigation</header>
<main>
    <article>
        <section>Content section</section>
    </article>
</main>
<footer>Copyright info</footer>
```

**Benefits:**
- Better **accessibility**
- Improved **SEO**
- Clearer **document structure**

### 💬 Comments

```html
<!-- This is a comment -->
<!-- 
    Multi-line
    comment
-->
```

---

## 🔍 Regular Expressions

**Regular expressions (regex)** validate input patterns.

### 📧 Email Validation

#### Basic Type
```html
<input type="email" name="email" placeholder="Email">
```

#### Pattern Matching
```html
<input 
    type="email" 
    name="email" 
    pattern=".+@.+\.edu" 
    placeholder="Email">
```

**Pattern breakdown:**
- `.+` - One or more characters
- `@` - Literal @ symbol
- `\.` - Literal period (escaped)
- `edu` - Must end with .edu

### 🔤 Common Regex Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| `.` | Any character | `a.c` matches "abc", "a9c" |
| `*` | Zero or more | `ab*c` matches "ac", "abc", "abbc" |
| `+` | One or more | `ab+c` matches "abc", "abbc" |
| `?` | Zero or one | `ab?c` matches "ac", "abc" |
| `\d` | Any digit | `\d\d\d` matches "123" |
| `\w` | Word character | `\w+` matches "hello" |
| `[abc]` | Any of a, b, c | `[aeiou]` matches vowels |
| `^` | Start of string | `^Hello` must start with "Hello" |
| `$` | End of string | `\.com$` must end with ".com" |

---

## 🎨 CSS Styling

### 🖌️ What is CSS?

**CSS (Cascading Style Sheets)** controls the visual presentation of HTML.

### 📍 Three Ways to Apply CSS

#### 1️⃣ Inline Styles
```html
<p style="font-size: large; text-align: center;">
    John Harvard
</p>
```

**❌ Problem**: Repetitive and hard to maintain

#### 2️⃣ Internal Stylesheet
```html
<head>
    <style>
        p {
            font-size: large;
            text-align: center;
        }
    </style>
</head>
<body>
    <p>John Harvard</p>
</body>
```

#### 3️⃣ External Stylesheet (Best Practice)
```html
<!-- index.html -->
<head>
    <link href="style.css" rel="stylesheet">
</head>
```

```css
/* style.css */
p {
    font-size: large;
    text-align: center;
}
```

### 🎯 CSS Selectors

#### Type Selector
```css
p {
    color: blue;
}
```

#### Class Selector
```html
<p class="centered">Text</p>
```

```css
.centered {
    text-align: center;
}
```

#### ID Selector
```html
<div id="header">Header</div>
```

```css
#header {
    background-color: navy;
}
```

### 📦 CSS Properties

#### Text Styling
```css
.text-style {
    font-size: 16px;
    font-family: Arial, sans-serif;
    font-weight: bold;
    text-align: center;
    color: #333;
}
```

#### Layout
```css
.box {
    width: 300px;
    height: 200px;
    margin: 20px;
    padding: 10px;
    border: 1px solid black;
}
```

#### Display Properties
```css
.hidden {
    display: none;          /* Completely hidden */
    visibility: hidden;     /* Hidden but space reserved */
}
```

### 🌈 Colors

```css
/* Named colors */
color: red;

/* Hexadecimal */
color: #FF0000;

/* RGB */
color: rgb(255, 0, 0);

/* RGBA (with transparency) */
color: rgba(255, 0, 0, 0.5);
```

### 🔄 Cascading Behavior

**CSS rules cascade from parent to children:**
```html
<body style="text-align: center">
    <div>This is centered</div>
    <div>This is also centered</div>
</body>
```

---

## 🚀 Bootstrap Framework

### 📦 What is Bootstrap?

**Bootstrap** is a CSS framework that provides pre-styled components for rapid web development.

### 🔗 Including Bootstrap

```html
<head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</head>
```

### 📊 Bootstrap Table

**Before Bootstrap:**
```html
<table>
    <tr>
        <th>Name</th>
        <th>Number</th>
    </tr>
    <tr>
        <td>Alice</td>
        <td>+1-617-495-1000</td>
    </tr>
</table>
```

**With Bootstrap:**
```html
<table class="table">
    <thead>
        <tr>
            <th scope="col">Name</th>
            <th scope="col">Number</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Alice</td>
            <td>+1-617-495-1000</td>
        </tr>
    </tbody>
</table>
```

### 🎨 Common Bootstrap Classes

| Class | Purpose |
|-------|---------|
| `.container` | Fixed-width container |
| `.container-fluid` | Full-width container |
| `.row` | Grid row |
| `.col` | Grid column |
| `.btn` | Button styling |
| `.btn-primary` | Primary button (blue) |
| `.btn-danger` | Danger button (red) |
| `.form-control` | Form input styling |
| `.table` | Table styling |
| `.nav` | Navigation bar |
| `.card` | Card component |

### 🎯 Bootstrap Grid System

```html
<div class="container">
    <div class="row">
        <div class="col">Column 1</div>
        <div class="col">Column 2</div>
        <div class="col">Column 3</div>
    </div>
</div>
```

### 🔘 Bootstrap Buttons

```html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary">Secondary</button>
<button class="btn btn-success">Success</button>
<button class="btn btn-danger">Danger</button>
```

---

## ⚡ JavaScript Programming

### 🎯 What is JavaScript?

**JavaScript** is a programming language that adds **interactivity** to web pages.

### 📝 Basic Syntax

#### Variables
```javascript
let name = "David";
const age = 30;
var oldStyle = "avoid";  // Avoid using var
```

#### Data Types
```javascript
let number = 42;              // Number
let text = "Hello";           // String
let flag = true;              // Boolean
let nothing = null;           // Null
let unknown;                  // Undefined
let list = [1, 2, 3];        // Array
let obj = {key: "value"};    // Object
```

#### Conditionals
```javascript
if (x > 0) {
    console.log("Positive");
} else if (x < 0) {
    console.log("Negative");
} else {
    console.log("Zero");
}
```

#### Loops
```javascript
// For loop
for (let i = 0; i < 10; i++) {
    console.log(i);
}

// While loop
while (condition) {
    // code
}

// For-of loop
for (let item of array) {
    console.log(item);
}
```

#### Functions
```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

// Arrow function
const greet = (name) => `Hello, ${name}!`;
```

### 🎯 Simple Alert Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script>
        function greet() {
            alert('Hello, ' + document.querySelector('#name').value);
        }
    </script>
    <title>Hello</title>
</head>
<body>
    <form onsubmit="greet(); return false;">
        <input id="name" placeholder="Name" type="text">
        <input type="submit">
    </form>
</body>
</html>
```

**❌ Problem**: Mixing HTML and JavaScript is poor design!

---

## 🔧 DOM Manipulation

### 🌳 The DOM (Document Object Model)

The **DOM** is a tree representation of HTML that JavaScript can manipulate.

```
document
  └── html
      ├── head
      │   └── title
      └── body
          ├── h1
          └── p
```

### 🎯 Selecting Elements

```javascript
// By ID
let element = document.querySelector('#myId');

// By class
let elements = document.querySelectorAll('.myClass');

// By tag
let paragraphs = document.querySelectorAll('p');
```

### ✏️ Modifying Content

```javascript
// Change text
element.innerHTML = 'New text';

// Change attribute
element.setAttribute('src', 'image.png');

// Change style
element.style.color = 'red';
element.style.backgroundColor = 'blue';
```

### 🎧 Event Listeners

#### DOMContentLoaded
```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Code runs after page loads
    console.log('Page is ready!');
});
```

#### Form Submit
```javascript
document.addEventListener('DOMContentLoaded', function() {
    document.querySelector('form').addEventListener('submit', function(e) {
        alert('Hello, ' + document.querySelector('#name').value);
        e.preventDefault();  // Prevent form submission
    });
});
```

#### Key Events
```javascript
let input = document.querySelector('input');
input.addEventListener('keyup', function(event) {
    console.log('User typed:', input.value);
});
```

**Common events:**
- `click` - Mouse click
- `submit` - Form submission
- `keyup` - Key released
- `keydown` - Key pressed
- `change` - Input value changed
- `load` - Page loaded
- `mouseover` - Mouse enters element

### 🎨 Live Greeting Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let input = document.querySelector('input');
            input.addEventListener('keyup', function() {
                let output = document.querySelector('p');
                if (input.value) {
                    output.innerHTML = `Hello, ${input.value}`;
                } else {
                    output.innerHTML = 'Hello, whoever you are';
                }
            });
        });
    </script>
    <title>Hello</title>
</head>
<body>
    <form>
        <input autocomplete="off" autofocus placeholder="Name" type="text">
    </form>
    <p></p>
</body>
</html>
```

**🎯 Key Concept**: Updates happen **in real-time** without page reload!

### 🎨 Background Color Changer

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Background</title>
</head>
<body>
    <button id="red">R</button>
    <button id="green">G</button>
    <button id="blue">B</button>
    
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

### 🔄 Blinking Text

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script>
        function blink() {
            let body = document.querySelector('body');
            if (body.style.visibility == 'hidden') {
                body.style.visibility = 'visible';
            } else {
                body.style.visibility = 'hidden';
            }
        }
        
        // Blink every 500ms
        window.setInterval(blink, 500);
    </script>
    <title>Blink</title>
</head>
<body>
    Hello, world
</body>
</html>
```

**Functions:**
- `setInterval(function, delay)` - Repeat function every delay ms
- `setTimeout(function, delay)` - Run function once after delay ms

### 🔍 Autocomplete Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Autocomplete</title>
</head>
<body>
    <input autocomplete="off" autofocus placeholder="Query" type="text">
    <ul></ul>
    
    <script src="large.js"></script>  <!-- Array of words -->
    <script>
        let input = document.querySelector('input');
        
        input.addEventListener('keyup', function() {
            let html = '';
            if (input.value) {
                for (word of WORDS) {
                    if (word.startsWith(input.value)) {
                        html += `<li>${word}</li>`;
                    }
                }
            }
            document.querySelector('ul').innerHTML = html;
        });
    </script>
</body>
</html>
```

**🎯 Demonstrates:**
- Real-time filtering
- Dynamic list generation
- Template literals (`${variable}`)

---

## 💡 Best Practices

### ✅ HTML Best Practices

1. **Use semantic tags** (`<header>`, `<main>`, `<footer>`)
2. **Always include `alt` attributes** for images
3. **Proper indentation** for readability
4. **Close all tags** properly
5. **Use lowercase** for tags and attributes

### ✅ CSS Best Practices

1. **External stylesheets** over inline styles
2. **Use classes** over IDs for styling
3. **Keep selectors simple** and maintainable
4. **Group related properties** together
5. **Comment complex styles**

### ✅ JavaScript Best Practices

1. **Wait for DOMContentLoaded** before manipulating DOM
2. **Use `let` and `const`** instead of `var`
3. **Prevent default** behavior when needed
4. **Separate JavaScript** from HTML
5. **Use event delegation** for dynamic content

---

## 🎯 Key Takeaways

### 1️⃣ **The Internet is Layered**

**Physical Layer**: Cables, routers, data centers  
**Network Layer**: IP addresses, TCP packets  
**Application Layer**: HTTP, HTTPS, web browsers

### 2️⃣ **HTML is Structure, CSS is Style, JavaScript is Behavior**

| Technology | Purpose | Example |
|------------|---------|---------|
| **HTML** | Content structure | `<p>Hello</p>` |
| **CSS** | Visual styling | `p { color: blue; }` |
| **JavaScript** | Interactivity | `alert('Hello');` |

### 3️⃣ **Separation of Concerns**

**Bad:**
```html
<p style="color: red;" onclick="alert('Hi')">Text</p>
```

**Good:**
```html
<!-- HTML -->
<p class="highlight">Text</p>

<!-- CSS -->
.highlight { color: red; }

<!-- JavaScript -->
document.querySelector('.highlight').addEventListener('click', ...);
```

### 4️⃣ **The DOM is Dynamic**

JavaScript can:
- ✅ Read element properties
- ✅ Modify content in real-time
- ✅ Respond to user actions
- ✅ Update without page reload

### 5️⃣ **Security Matters**

- ✅ Always use **HTTPS** (not HTTP)
- ✅ Validate user input (regex patterns)
- ✅ **Never trust** client-side validation alone
- ✅ Server-side validation is essential

---

## 🎬 Looking Ahead

### Week 9: Flask (Python Web Framework)

**Connecting the pieces:**
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')
```

**What you'll learn:**
- Server-side programming
- Dynamic content generation
- Database integration
- User authentication
- Session management

---

## 📚 Additional Resources

- **MDN Web Docs**: [developer.mozilla.org](https://developer.mozilla.org)
- **Bootstrap Documentation**: [getbootstrap.com](https://getbootstrap.com)
- **JavaScript Reference**: [javascript.info](https://javascript.info)
- **Regex Tester**: [regex101.com](https://regex101.com)
- **Can I Use**: [caniuse.com](https://caniuse.com) (Browser compatibility)

---

## 🎓 Practice Exercises

### 🔰 Beginner

1. **Create a personal homepage** with header, main content, and footer
2. **Style a form** using CSS classes
3. **Add a button** that changes text color

### 🔸 Intermediate

4. **Build a calculator** with JavaScript
5. **Create a to-do list** with add/remove functionality
6. **Implement form validation** with regex

### 🔥 Advanced

7. **Build an autocomplete search**
8. **Create a photo gallery** with modal popups
9. **Implement a single-page app** with dynamic content loading

---

## 🎬 Lecture Credits

**Instructor**: David J. Malan  
**University**: Harvard University  
**Course**: CS50x 2025  
**Video**: CS50x 2025 - Lecture 8 - HTML, CSS, JavaScript

---

## 📝 Quick Reference

### HTML Cheat Sheet

```html
<!-- Document structure -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Page Title</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- Content here -->
    <script src="script.js"></script>
</body>
</html>
```

### CSS Cheat Sheet

```css
/* Selector types */
element { }      /* Tag */
.class { }       /* Class */
#id { }          /* ID */
parent > child { } /* Direct child */
element:hover { }  /* Pseudo-class */
```

### JavaScript Cheat Sheet

```javascript
// DOM Selection
document.querySelector('#id');
document.querySelectorAll('.class');

// Event Listener
element.addEventListener('event', function(e) {
    // Handle event
    e.preventDefault();
});

// DOM Manipulation
element.innerHTML = 'text';
element.style.property = 'value';
element.classList.add('class');
```

---

*Made with 🌐 for CS50 students learning web development*