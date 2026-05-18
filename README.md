#  macOS Big Sur Portfolio OS

A highly interactive, visually stunning personal portfolio designed as a fully functional macOS Big Sur desktop environment. This web application replicates key desktop experiences including dynamic window controls, Finder file browser, multi-instance support, dynamic local photos integration, interactive terminal CLI, custom locations overview, and native booting sequences.

---

## ✨ Features

###  Original macOS Boot Sequence
* Features a large high-fidelity Apple logo syncronized cleanly with an interactive loading bar to mirror the original macOS startup sequence.
* Seamless transition from hardware boot loading directly onto the fluid glassmorphic desktop environment.

### 📁 Dynamic Finder App
* Replicates the classic Finder app layout with a clear favorites, recents, and locations sidebar.
* **Favorites — Profile:** Renders a gorgeous personal bio view featuring profile avatars.
* **Favorites — Core Tech:** Displays engineering skills as interactive badges.
* **Favorites — Education:** Chronologically tracks academic milestones.
* **Recents — Photos:** A dynamically updating photo explorer that loads folder assets (`photos/`) using specific prefix matching (e.g. `profile*`).
* **Locations — Gorkha & Madhyapur Thimi:** Full location-specific profiles detailed in beautiful orange-red and amber color palettes complete with historical overviews, geographical elevation, and key cultural highlights.

### 💻 Multi-Instance Window Manager
* Fully working macOS title bar buttons (**Close**, **Minimize**, **Maximize**).
* Support for opening **multiple concurrent instances** of any application just like a native OS.
* **Minimize All** and **Close All** control commands fully enabled across active windows.

### 🐚 Interactive Terminal App
* Fully functional command-line prompt.
* Custom commands tailored to explore developer history, credentials, system statistics, or trigger desktop easter eggs.

### 🎨 Fluid Big Sur Aesthetics
* Smooth responsive Dock bar with hover magnifying effects.
* Custom status menu bar updating battery, Wi-Fi, control center, and system local time in real-time.
* Interactive wallpaper switcher supporting macOS dynamic Dark / Light wallpapers.

---

## 🛠️ Technology Stack

* **Logic & Engine:** Vanilla Modern JavaScript (ES6+)
* **Structure & UI:** HTML5 Semantic Markup
* **Aesthetics & Layout:** Responsive Vanilla CSS3 Grid, Flexbox, & Glassmorphic variables
* **Icons:** Feather Icons / Heroicons customized SVG paths

---

## 🚀 Local Setup & Run

No compilation or build step required! Since this is a pure, optimized static web application, you can run it locally in a matter of seconds.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/sujalkunwar22/MacPortfolio.git
   cd MacPortfolio
   ```

2. **Run locally using any static web server:**
   * Using **Python 3:**
     ```bash
     python -m http.server 8000
     ```
   * Using **Node.js (http-server):**
     ```bash
     npx http-server -p 8000
     ```
   * Or simply double-click the `index.html` file to open directly in your web browser!

3. Open **`http://localhost:8000`** in your browser and enjoy the experience!

---

## 📍 Featured Locations

### 🗻 Gorkha
* **Haramtari, Gorkha (Gandaki Province, Nepal)**
* **Overview:** The ancestral birthplace of unified modern Nepal and home of the legendary brave Gurkhas. Detailed with custom views of Gorkha Durbar and Gorakhnath Cave.

### 🏺 Madhyapur Thimi
* **Thimi (Bagmati Province, Nepal)**
* **Overview:** The legendary "Town of Pottery" in Kathmandu Valley, celebrated for vibrant cultural heritage, living clay arts, and the traditional Sindoor Jatra.

---

## 👤 Author

* **Sujal Kunwar**
* GitHub: [@sujalkunwar22](https://github.com/sujalkunwar22)
* Email: [sujalkunwar22@gmail.com](mailto:sujalkunwar22@gmail.com)
