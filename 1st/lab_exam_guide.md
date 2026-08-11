# Web Technology Lab Exam Study Guide 🚀
This guide breaks down the structure, styling, layouts, and scripting patterns used in the current directory's frontend files (`homec.html`, `style.css`, `form.css`, and the content pages). Use this to study for your lab exam!

---

## 1. Application Architecture & Overview

The codebase is structured as a **Single-Page Shell with an Iframe Container**. 

```
+-------------------------------------------------------------+
| homec.html (Shell Page)                                     |
|  +-------------------------------------------------------+  |
|  | header (Logo & Title)                                 |  |
|  +-------------------------------------------------------+  |
|  | .container (Flexbox)                                  |  |
|  |  +------------------+ +-----------------------------+  |  |
|  |  | nav (Sidebar)    | | .content (Main Area)       |  |  |
|  |  | - Register (Link)| | +------------------------+ |  |  |
|  |  | - Login (Link)   | | | iframe name="cf"       | |  |  |
|  |  | - Contact (Link) | | |                        | |  |  |
|  |  |                  | | | Loads regc.html, etc.  | |  |  |
|  |  |                  | | +------------------------+ |  |  |
|  |  +------------------+ +-----------------------------+  |  |
|  +-------------------------------------------------------+  |
+-------------------------------------------------------------+
```

### Key Concept: How the `iframe` Navigation Works
Instead of loading completely new pages that reload the entire browser tab, the main shell ([homec.html](file:///a:/web-t/1st/homec.html)) holds a sidebar navigation panel and a central content area housing an `<iframe>`.

* **The Shell Definition:**
  ```html
  <iframe name="cf" src="regc.html"></iframe>
  ```
* **The Navigation Link:**
  ```html
  <a href="login.html" target="cf">Login</a>
  ```
* **How they connect:** The `target` attribute of the anchor tag (`<a>`) matches the `name` attribute of the `<iframe>`. When the user clicks the link, the browser loads the new document *inside* the iframe, preserving the header and navigation menu without a full page refresh.

---

## 2. HTML Structure & Elements

### Semantic HTML5 Layout Tags
Using semantic tags instead of generic `<div>`s improves SEO, accessibility, and code readability:
* `<header>`: Represents the introductory header portion of the site (holds logo & title).
* `<nav>`: Specifically designates a section of navigation links.
* `<section>`: Defines a thematic group of content.
* `<ul>` & `<li>`: Used inside the nav to structure the links as an unordered list, which is standard web practice.

### Cross-Frame DOM Interaction (Synchronizing Active States)
In [login.html](file:///a:/web-t/1st/login.html) and [regc.html](file:///a:/web-t/1st/regc.html), there are links to switch between pages. Since these pages load inside the `<iframe>`, clicking them normally wouldn't update the active styling of the navigation menu in the parent window.

To fix this, a small JavaScript snippet is embedded in the link:
```html
<a href="login.html" onclick="parent.document.querySelector('a[href=\'login.html\']').click();">Login</a>
```
* **`parent`**: Accesses the global `window` object of the parent document ([homec.html](file:///a:/web-t/1st/homec.html)).
* **`document.querySelector(...)`**: Locates the specific navigation link in the parent window.
* **`.click()`**: Simulates a click event on that parent link, triggering the parent's navigation handlers and styling updates.

---

## 3. HTML5 Forms & Input Types

The file [regc.html](file:///a:/web-t/1st/regc.html) showcases a wide range of HTML5 input elements. In your exam, you may be asked to demonstrate these inputs. Here is a handy reference:

| Input Type | Syntax | Purpose / Features |
| :--- | :--- | :--- |
| **Text** | `<input type="text">` | Single-line plain text. |
| **Email** | `<input type="email">` | Validates that input matches a standard email format (`user@domain.com`). |
| **Password** | `<input type="password">` | Obscures characters as bullets/asterisks for security. |
| **Number** | `<input type="number">` | Restricts input to numerical values; provides up/down arrows. |
| **Telephone** | `<input type="tel">` | Specifically meant for telephone numbers (often opens numeric keypad on mobile). |
| **Date** | `<input type="date">` | Provides a calendar dropdown date picker. |
| **DateTime Local** | `<input type="datetime-local">` | Calendar picker combined with time selection (hour/minute/AM-PM). |
| **Month** | `<input type="month">` | Restricts picker to year and month selection (e.g., "August 2026"). |
| **Color** | `<input type="color">` | Opens the operating system's native color selection tool. |
| **File** | `<input type="file">` | Opens a file explorer to select and upload local files. |
| **Range (Slider)** | `<input type="range" min="0" max="10">` | Visual slider control. Specify bounds with `min` and `max`. |
| **Radio** | `<input type="radio" name="gender" value="male">` | Grouped selection where **only one** can be selected. Grouping is established by matching the `name` attribute. |
| **Checkbox** | `<input type="checkbox">` | Independent on/off toggle (e.g., "Agree to Terms"). |

### Form Validation Attributes
* `required`: Prevents form submission if the field is empty.
* `onsubmit="event.preventDefault(); ..."`: Prevents the browser's default behavior of reloading the page on form submission, allowing JavaScript to handle the data or show alerts.

---

## 4. CSS Layouts: Flexbox Deep Dive

Both [style.css](file:///a:/web-t/1st/style.css) and [form.css](file:///a:/web-t/1st/form.css) rely heavily on CSS Flexbox for layout control.

### Concept 1: Vertical Layout Stacking (The Main Shell)
```css
body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}
```
* `display: flex`: Activates Flexbox container context.
* `flex-direction: column`: Aligns child elements (`header` and `.container`) vertically instead of horizontally.
* `min-height: 100vh`: Sets the body height to at least 100% of the viewport (screen) height, ensuring full-page coverage.

### Concept 2: Distributing Remaining Space (`flex: 1`)
```css
.container {
    display: flex;
    flex: 1;
}
```
* In the vertical shell, the body contains two items: `header` (which takes its natural height) and `.container`.
* `flex: 1`: Instructs the container to expand and occupy **all remaining vertical space** on the screen.
* Inside `.container`, the direction defaults to `row`, laying out `<nav>` and `.content` side-by-side.
* In `.content`, `flex: 1` ensures it takes up all remaining **horizontal** space not occupied by the 200px wide sidebar navigation.

### Concept 3: Perfect Centering (Form Cards)
```css
.iframe-body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}
```
Used in [form.css](file:///a:/web-t/1st/form.css) to center the login and registration cards perfectly on screen:
* `justify-content: center`: Centers elements along the **main axis** (horizontally, since default direction is `row`).
* `align-items: center`: Centers elements along the **cross axis** (vertically).

### Concept 4: Flex Gap
```css
form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}
```
* `gap: 10px`: Automatically inserts a 10px space between child items inside the flex container without needing manual `margin-bottom` on each element.

---

## 5. Responsive Design & Media Queries

Media queries allow us to modify CSS properties based on characteristics of the user's device, most commonly screen width.

```css
@media (max-width: 600px) {
    .container { 
        flex-direction: column; 
    }
    nav { 
        width: 100%; 
        border-right: none; 
        border-bottom: 1px solid #000; 
    }
    nav ul { 
        flex-direction: row; 
        justify-content: center; 
    }
}
```

### Responsive Flow Table:
| Element | Desktop Style (> 600px) | Mobile Style (<= 600px) |
| :--- | :--- | :--- |
| **`.container` Flow** | `row` (Sidebar and Content side-by-side) | `column` (Sidebar stacked on top of Content) |
| **`nav` Width** | `200px` (fixed column width) | `100%` (spans full width of screen) |
| **`nav` Borders** | Border on the right side (`border-right`) | Right border removed; border added at bottom (`border-bottom`) |
| **`nav ul` Link Alignment**| Vertical stack (`flex-direction: column`) | Horizontal row (`flex-direction: row`), centered (`justify-content: center`) |

---

## 6. JavaScript DOM Manipulation

In [homec.html](file:///a:/web-t/1st/homec.html), vanilla JavaScript handles tab selection visual states:

```javascript
// 1. Select all navigation link elements
const links = document.querySelectorAll('.nav-link');

// 2. Add an event listener to each link
links.forEach(link => link.addEventListener('click', function() {
    // 3. Loop through all links and remove the 'active' class
    links.forEach(l => l.classList.remove('active'));
    
    // 4. Add the 'active' class to the clicked link ('this')
    this.classList.add('active');
}));
```

### Explaining the DOM APIs:
* **`document.querySelectorAll('.nav-link')`**: Queries the DOM and returns a static `NodeList` of all elements matching the class `.nav-link`.
* **`.forEach()`**: A method used to iterate over elements in the NodeList.
* **`addEventListener('click', ...)`**: Registers a callback function to run when the user clicks the element.
* **`classList.remove('active')` / `classList.add('active')`**: Modifies the CSS class list of an element. This swaps the background and text color of the buttons dynamically.
* **`this`**: Within the callback function, `this` refers to the element that triggered the event (the clicked link).

---

## 🔬 High-Yield Quick Cheat Sheets for Your Exam

### 1. Minimal Flexbox Centering Shell
```html
<div style="display: flex; justify-content: center; align-items: center; height: 100vh;">
    <div>Centered Element</div>
</div>
```

### 2. Basic Form with Validation
```html
<form action="/submit" method="POST">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>
    
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
    
    <button type="submit">Submit</button>
</form>
```

### 3. Iframe Target Binding
```html
<!-- Link loads contact.html inside iframe 'my-frame' -->
<a href="contact.html" target="my-frame">Contact Us</a>
<iframe name="my-frame" src="about.html"></iframe>
```
