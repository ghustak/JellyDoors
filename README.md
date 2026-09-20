# JellyDoors
# 🎃 Halloween "Shuffle Doors" for Jellyfin (HBO Max Style)

Bring the HBO Max Halloween "Shuffle Doors" experience to your Jellyfin homepage! This script injects three customizable fear-tier doors (**Not Scary**, **Scary**, and **Very Scary**) onto your dashboard. Clicking any door queries your server for movies and TV episodes tagged with that fear level and instantly launches a random pick.

![Halloween Doors Preview](https://via.placeholder.com/800x400.png?text=Halloween+Shuffle+Doors+Preview) *(Replace with a screenshot of your doors!)*

---

## ⚡ Features

- **HBO Max Style Shuffle:** Instantly picks a random movie or TV episode tagged with your fear level across your whole library.
- **Tag-Based Searching:** Uses Jellyfin's `Tags` query API so you don't need to maintain manual collection folders.
- **Dual Theme Support:** Custom layout scanners for both standard **Jellyfin Web** and **Elegantfin**.
- **Optional Seasonal Auto-Toggle:** Includes versions with built-in date checks to automatically display doors only between October 1st and October 31st.

---

## 🏷️ Step 1: Tag Your Media in Jellyfin

1. In Jellyfin, select the movies or TV episodes you want in each tier.
2. Click the **3 dots menu** on the items (or multi-select items) -> **Edit Metadata**.
3. Under **Tags**, add your desired fear tag strings (e.g., `NotScaryDoor`, `ScaryDoor`, `VeryScaryDoor`).
4. Ensure the `JALLOW_TAGS` object in the JavaScript code blocks below match your exact tag names.

---

## Step 2: Select your preferred format:
  -Default Jellyfin Without Date Check
  -Default Jellyfin With Date Check
  -Elegantfin Without Date Check
  -Elegantfin With Date Check

  ---

  ## Default Jellyfin Without Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

:root {
    --jallow-bg: rgba(20, 20, 20, 0.6);
    --jallow-purple: #8a2be2;
    --jallow-font: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

.jallow-section {
    background: var(--jallow-bg);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    padding: 30px 20px;
    margin: 1.5em 3.5%;
    display: flex;
    flex-direction: column;
    align-items: center;
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
}

.jallow-title {
    font-family: var(--jallow-font);
    font-size: 2rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: #ffffff;
    text-shadow: 0 0 15px var(--jallow-purple);
    margin: 0 0 5px 0;
    text-align: center;
}

.jallow-subtitle {
    font-family: var(--jallow-font);
    font-size: 0.95rem;
    color: #cccccc;
    margin: 0 0 30px 0;
    text-align: center;
}

.jallow-container {
    display: flex;
    gap: 25px;
    justify-content: center;
    flex-wrap: wrap;
    width: 100%;
    max-width: 950px;
}

.jallow-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    min-width: 200px;
    max-width: 260px;
}

.jallow-door {
    display: block !important;
    width: 100% !important;
    height: 360px !important;
    min-height: 360px !important;
    background-size: cover !important;
    background-position: center !important;
    background-color: #111111 !important;
    border: 2px solid rgba(255, 255, 255, 0.15);
    border-radius: 12px;
    cursor: pointer !important;
    transition: transform 0.25s cubic-bezier(0.4, 0, 0.2, 1), box-shadow 0.25s ease, border-color 0.25s ease;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.7);
    outline: none;
    position: relative !important;
    z-index: 100 !important;
}

.jallow-door:hover, .jallow-door:focus {
    transform: translateY(-8px) scale(1.03);
    box-shadow: 0 0 30px var(--jallow-purple);
    border-color: #a855f7 !important;
}

/* REPLACE 'YOUR_...' WITH YOUR IMAGE URLS OR BASE64 STRINGS */
.jallow-door[data-tier="not-scary"] { 
    background-image: url('YOUR_NOT_SCARY_IMAGE_URL');
    border-color: rgba(38, 115, 77, 0.5);
}
.jallow-door[data-tier="scary"] { 
    background-image: url('YOUR_SCARY_IMAGE_URL');
    border-color: rgba(217, 119, 6, 0.5);
}
.jallow-door[data-tier="very-scary"] { 
    background-image: url('YOUR_VERY_SCARY_IMAGE_URL');
    border-color: rgba(220, 38, 38, 0.5);
}

.jallow-label {
    font-family: var(--jallow-font);
    margin-top: 15px;
    font-size: 1.1rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1.5px;
}

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.**.

(function () {
    const JALLOW_TAGS = {
        "not-scary": "Halloween Kids",
        "scary": "Halloween",
        "very-scary": "Horror"
    };

    function injectHalloweenDoors() {
        if (document.getElementById('jellyfin-halloween-doors')) return;

        const activePage = document.querySelector('.page:not(.hide)');
        if (!activePage) return;

        const isHome = activePage.id === 'homePage' || activePage.classList.contains('home-page');
        if (!isHome) return;

        const targetView = activePage.querySelector('.scrollScroller') || activePage.querySelector('.sections') || activePage;
        if (!targetView) return;

        const section = document.createElement('div');
        section.id = 'jellyfin-halloween-doors';
        section.className = 'jallow-section';
        section.innerHTML = `
            <h2 class="jallow-title">Choose Your Scare</h2>
            <p class="jallow-subtitle">Select a door to instantly play a random movie or episode from that fear level.</p>
            <div class="jallow-container">
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="not-scary" tabindex="0" title="Not Scary"></div>
                    <div class="jallow-label" style="color: #4ade80;">Not Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="scary" tabindex="0" title="Scary"></div>
                    <div class="jallow-label" style="color: #fbbf24;">Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="very-scary" tabindex="0" title="Very Scary"></div>
                    <div class="jallow-label" style="color: #f87171;">Very Scary</div>
                </div>
            </div>
        `;

        targetView.insertBefore(section, targetView.firstChild);

        section.querySelectorAll('.jallow-door').forEach(door => {
            const triggerAction = () => playRandomMediaByTag(door.getAttribute('data-tier'));
            door.addEventListener('click', triggerAction);
            door.addEventListener('keydown', (e) => { if (e.key === 'Enter') triggerAction(); });
        });
    }

    async function playRandomMediaByTag(tier) {
        const tagName = JALLOW_TAGS[tier];
        if (!tagName) return;

        const apiClient = window.ApiClient;
        if (!apiClient) return;

        const serverUrl = apiClient.serverAddress();
        const apiKey = apiClient.accessToken();
        const userId = apiClient.getCurrentUserId();
        const fetchUrl = `${serverUrl}/Users/${userId}/Items?IncludeItemTypes=Movie,Episode&Recursive=true&Tags=${encodeURIComponent(tagName)}&api_key=${apiKey}`;

        try {
            const response = await fetch(fetchUrl);
            const data = await response.json();
            
            if (data.Items && data.Items.length > 0) {
                const selectedItem = data.Items[Math.floor(Math.random() * data.Items.length)];
                window.location.href = `${window.location.origin}/web/index.html#!/details?id=${selectedItem.Id}`;
            } else {
                alert(`No movies or episodes found with the tag "${tagName}"!`);
            }
        } catch (error) {
            console.error("Jellyfin Tag Doors API Failure:", error);
        }
    }

    const observer = new MutationObserver(() => injectHalloweenDoors());
    observer.observe(document.body, { childList: true, subtree: true });

    document.addEventListener('viewshow', injectHalloweenDoors);
    setTimeout(injectHalloweenDoors, 500);
    setTimeout(injectHalloweenDoors, 1500);
})();

---

## Default Jellyfin With Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

:root {
    --jallow-bg: rgba(20, 20, 20, 0.6);
    --jallow-purple: #8a2be2;
    --jallow-font: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

.jallow-section {
    background: var(--jallow-bg);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    padding: 30px 20px;
    margin: 1.5em 3.5%;
    display: flex;
    flex-direction: column;
    align-items: center;
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
}

.jallow-title {
    font-family: var(--jallow-font);
    font-size: 2rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: #ffffff;
    text-shadow: 0 0 15px var(--jallow-purple);
    margin: 0 0 5px 0;
    text-align: center;
}

.jallow-subtitle {
    font-family: var(--jallow-font);
    font-size: 0.95rem;
    color: #cccccc;
    margin: 0 0 30px 0;
    text-align: center;
}

.jallow-container {
    display: flex;
    gap: 25px;
    justify-content: center;
    flex-wrap: wrap;
    width: 100%;
    max-width: 950px;
}

.jallow-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    min-width: 200px;
    max-width: 260px;
}

.jallow-door {
    display: block !important;
    width: 100% !important;
    height: 360px !important;
    min-height: 360px !important;
    background-size: cover !important;
    background-position: center !important;
    background-color: #111111 !important;
    border: 2px solid rgba(255, 255, 255, 0.15);
    border-radius: 12px;
    cursor: pointer !important;
    transition: transform 0.25s cubic-bezier(0.4, 0, 0.2, 1), box-shadow 0.25s ease, border-color 0.25s ease;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.7);
    outline: none;
    position: relative !important;
    z-index: 100 !important;
}

.jallow-door:hover, .jallow-door:focus {
    transform: translateY(-8px) scale(1.03);
    box-shadow: 0 0 30px var(--jallow-purple);
    border-color: #a855f7 !important;
}

/* REPLACE 'YOUR_...' WITH YOUR IMAGE URLS OR BASE64 STRINGS */
.jallow-door[data-tier="not-scary"] { 
    background-image: url('YOUR_NOT_SCARY_IMAGE_URL');
    border-color: rgba(38, 115, 77, 0.5);
}
.jallow-door[data-tier="scary"] { 
    background-image: url('YOUR_SCARY_IMAGE_URL');
    border-color: rgba(217, 119, 6, 0.5);
}
.jallow-door[data-tier="very-scary"] { 
    background-image: url('YOUR_VERY_SCARY_IMAGE_URL');
    border-color: rgba(220, 38, 38, 0.5);
}

.jallow-label {
    font-family: var(--jallow-font);
    margin-top: 15px;
    font-size: 1.1rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1.5px;
}

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.**.

(function () {
    const JALLOW_TAGS = {
        "not-scary": "Halloween Kids",
        "scary": "Halloween",
        "very-scary": "Horror"
    };

    function isHalloweenSeason() {
        if (localStorage.getItem('test_halloween_doors') === 'true') return true;
        const now = new Date();
        return now.getMonth() === 9 && now.getDate() >= 1 && now.getDate() <= 31;
    }

    function injectHalloweenDoors() {
        if (!isHalloweenSeason()) return;
        if (document.getElementById('jellyfin-halloween-doors')) return;

        const targetView = document.querySelector('.mainAnimatedPages .page:not(.hide) .scrollScroller') || 
                           document.querySelector('#homePage .scrollScroller') ||
                           document.querySelector('.page:not(.hide) [data-role="page"] .scrollScroller') ||
                           document.querySelector('.scrollScroller') ||
                           document.querySelector('.sections');
        
        if (!targetView) return;

        const section = document.createElement('div');
        section.id = 'jellyfin-halloween-doors';
        section.className = 'jallow-section';
        section.innerHTML = `
            <h2 class="jallow-title">Choose Your Scare</h2>
            <p class="jallow-subtitle">Select a door to instantly play a random movie or episode from that fear level.</p>
            <div class="jallow-container">
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="not-scary" tabindex="0" title="Not Scary"></div>
                    <div class="jallow-label" style="color: #4ade80;">Not Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="scary" tabindex="0" title="Scary"></div>
                    <div class="jallow-label" style="color: #fbbf24;">Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="very-scary" tabindex="0" title="Very Scary"></div>
                    <div class="jallow-label" style="color: #f87171;">Very Scary</div>
                </div>
            </div>
        `;

        targetView.insertBefore(section, targetView.firstChild);

        section.querySelectorAll('.jallow-door').forEach(door => {
            const triggerAction = () => playRandomMediaByTag(door.getAttribute('data-tier'));
            door.addEventListener('click', triggerAction);
            door.addEventListener('keydown', (e) => { if (e.key === 'Enter') triggerAction(); });
        });
    }

    async function playRandomMediaByTag(tier) {
        const tagName = JALLOW_TAGS[tier];
        if (!tagName) return;

        const apiClient = window.ApiClient;
        if (!apiClient) return;

        const serverUrl = apiClient.serverAddress();
        const apiKey = apiClient.accessToken();
        const userId = apiClient.getCurrentUserId();
        const fetchUrl = `${serverUrl}/Users/${userId}/Items?IncludeItemTypes=Movie,Episode&Recursive=true&Tags=${encodeURIComponent(tagName)}&api_key=${apiKey}`;

        try {
            const response = await fetch(fetchUrl);
            const data = await response.json();
            
            if (data.Items && data.Items.length > 0) {
                const selectedItem = data.Items[Math.floor(Math.random() * data.Items.length)];
                window.location.href = `${window.location.origin}/web/index.html#!/details?id=${selectedItem.Id}`;
            } else {
                alert(`No movies or episodes found with the tag "${tagName}"!`);
            }
        } catch (error) {
            console.error("Jellyfin Tag Doors API Failure:", error);
        }
    }

    const observer = new MutationObserver(() => {
        const isHomePage = document.querySelector('#homePage') || document.querySelector('.home-page');
        if (isHomePage) injectHalloweenDoors();
    });

    observer.observe(document.body, { childList: true, subtree: true });

    document.addEventListener('viewshow', injectHalloweenDoors);
    setTimeout(injectHalloweenDoors, 500);
    setTimeout(injectHalloweenDoors, 2000);
})();

## Elegantfin Without Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

@import url("[https://jsdelivr.net](https://jsdelivr.net)");

:root {
    --jallow-bg: rgba(18, 11, 30, 0.4);
    --jallow-purple: #8a2be2;
    --jallow-font: 'Quicksand', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

.jallow-section {
    background: var(--jallow-bg);
    border: 1px solid rgba(255, 255, 255, 0.05);
    border-radius: 12px;
    padding: 30px 20px;
    margin: 2em 3.5%;
    display: flex;
    flex-direction: column;
    align-items: center;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
}

.jallow-title {
    font-family: var(--jallow-font);
    font-size: 2rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 3px;
    color: #fff;
    text-shadow: 0 0 15px var(--jallow-purple);
    margin: 0 0 5px 0;
    text-align: center;
}

.jallow-subtitle {
    font-family: var(--jallow-font);
    font-size: 0.95rem;
    color: #b0a2c7;
    margin: 0 0 35px 0;
    text-align: center;
}

.jallow-container {
    display: flex;
    gap: 25px;
    justify-content: center;
    flex-wrap: wrap;
    width: 100%;
    max-width: 950px;
}

.jallow-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    min-width: 200px;
    max-width: 260px;
}

.jallow-door {
    display: block !important;
    width: 100% !important;
    height: 360px !important;
    min-height: 360px !important;
    background-size: cover !important;
    background-position: center !important;
    background-color: #140d21 !important;
    border: 2px solid rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    cursor: pointer !important;
    transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 10px 25px rgba(0,0,0,0.6);
    outline: none;
    position: relative !important;
    z-index: 100 !important;
}

.jallow-door:hover, .jallow-door:focus {
    transform: translateY(-8px) scale(1.03);
    box-shadow: 0 0 30px var(--jallow-purple);
    border-color: #a855f7 !important;
}

/* REPLACE 'YOUR_...' WITH YOUR IMAGE URLS OR BASE64 STRINGS */
.jallow-door[data-tier="not-scary"] { 
    background-image: url('YOUR_NOT_SCARY_IMAGE_URL'); 
    border-color: rgba(38, 115, 77, 0.4);
}
.jallow-door[data-tier="scary"] { 
    background-image: url('YOUR_SCARY_IMAGE_URL'); 
    border-color: rgba(217, 119, 6, 0.4);
}
.jallow-door[data-tier="very-scary"] { 
    background-image: url('YOUR_VERY_SCARY_IMAGE_URL'); 
    border-color: rgba(220, 38, 38, 0.4);
}

.jallow-label {
    font-family: var(--jallow-font);
    margin-top: 15px;
    font-size: 1.1rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1.5px;
}

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.**.

(function () {
    const JALLOW_TAGS = {
        "not-scary": "Halloween Kids",
        "scary": "Halloween",
        "very-scary": "Horror"
    };

    function injectHalloweenDoors() {
        if (document.getElementById('jellyfin-halloween-doors')) return;

        const activePage = document.querySelector('.page:not(.hide)');
        if (!activePage) return;

        const isHome = activePage.id === 'homePage' || activePage.classList.contains('home-page');
        if (!isHome) return;

        const targetView = activePage.querySelector('.scrollScroller') || activePage.querySelector('.sections') || activePage;
        if (!targetView) return;

        const section = document.createElement('div');
        section.id = 'jellyfin-halloween-doors';
        section.className = 'jallow-section';
        section.innerHTML = `
            <h2 class="jallow-title">Choose Your Scare</h2>
            <p class="jallow-subtitle">Select a door to instantly play a random movie or episode from that fear level.</p>
            <div class="jallow-container">
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="not-scary" tabindex="0" title="Not Scary"></div>
                    <div class="jallow-label" style="color: #4ade80;">Not Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="scary" tabindex="0" title="Scary"></div>
                    <div class="jallow-label" style="color: #fbbf24;">Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="very-scary" tabindex="0" title="Very Scary"></div>
                    <div class="jallow-label" style="color: #f87171;">Very Scary</div>
                </div>
            </div>
        `;

        targetView.insertBefore(section, targetView.firstChild);

        section.querySelectorAll('.jallow-door').forEach(door => {
            const triggerAction = () => playRandomMediaByTag(door.getAttribute('data-tier'));
            door.addEventListener('click', triggerAction);
            door.addEventListener('keydown', (e) => { if (e.key === 'Enter') triggerAction(); });
        });
    }

    async function playRandomMediaByTag(tier) {
        const tagName = JALLOW_TAGS[tier];
        if (!tagName) return;

        const apiClient = window.ApiClient;
        if (!apiClient) return;

        const serverUrl = apiClient.serverAddress();
        const apiKey = apiClient.accessToken();
        const userId = apiClient.getCurrentUserId();
        const fetchUrl = `${serverUrl}/Users/${userId}/Items?IncludeItemTypes=Movie,Episode&Recursive=true&Tags=${encodeURIComponent(tagName)}&api_key=${apiKey}`;

        try {
            const response = await fetch(fetchUrl);
            const data = await response.json();
            
            if (data.Items && data.Items.length > 0) {
                const selectedItem = data.Items[Math.floor(Math.random() * data.Items.length)];
                window.location.href = `${window.location.origin}/web/index.html#!/details?id=${selectedItem.Id}`;
            } else {
                alert(`No movies or episodes found with the tag "${tagName}"!`);
            }
        } catch (error) {
            console.error("Jellyfin Tag Doors API Failure:", error);
        }
    }

    const observer = new MutationObserver(() => injectHalloweenDoors());
    observer.observe(document.body, { childList: true, subtree: true });

    document.addEventListener('viewshow', injectHalloweenDoors);
    setTimeout(injectHalloweenDoors, 500);
    setTimeout(injectHalloweenDoors, 1500);
})();

## Elegantfin With Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

@import url("[https://jsdelivr.net](https://jsdelivr.net)");

:root {
    --jallow-bg: rgba(18, 11, 30, 0.4);
    --jallow-purple: #8a2be2;
    --jallow-font: 'Quicksand', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

.jallow-section {
    background: var(--jallow-bg);
    border: 1px solid rgba(255, 255, 255, 0.05);
    border-radius: 12px;
    padding: 30px 20px;
    margin: 2em 3.5%;
    display: flex;
    flex-direction: column;
    align-items: center;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
}

.jallow-title {
    font-family: var(--jallow-font);
    font-size: 2rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 3px;
    color: #fff;
    text-shadow: 0 0 15px var(--jallow-purple);
    margin: 0 0 5px 0;
    text-align: center;
}

.jallow-subtitle {
    font-family: var(--jallow-font);
    font-size: 0.95rem;
    color: #b0a2c7;
    margin: 0 0 35px 0;
    text-align: center;
}

.jallow-container {
    display: flex;
    gap: 25px;
    justify-content: center;
    flex-wrap: wrap;
    width: 100%;
    max-width: 950px;
}

.jallow-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    min-width: 200px;
    max-width: 260px;
}

.jallow-door {
    display: block !important;
    width: 100% !important;
    height: 360px !important;
    min-height: 360px !important;
    background-size: cover !important;
    background-position: center !important;
    background-color: #140d21 !important;
    border: 2px solid rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    cursor: pointer !important;
    transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 10px 25px rgba(0,0,0,0.6);
    outline: none;
    position: relative !important;
    z-index: 100 !important;
}

.jallow-door:hover, .jallow-door:focus {
    transform: translateY(-8px) scale(1.03);
    box-shadow: 0 0 30px var(--jallow-purple);
    border-color: #a855f7 !important;
}

/* REPLACE 'YOUR_...' WITH YOUR IMAGE URLS OR BASE64 STRINGS */
.jallow-door[data-tier="not-scary"] { 
    background-image: url('YOUR_NOT_SCARY_IMAGE_URL'); 
    border-color: rgba(38, 115, 77, 0.4);
}
.jallow-door[data-tier="scary"] { 
    background-image: url('YOUR_SCARY_IMAGE_URL'); 
    border-color: rgba(217, 119, 6, 0.4);
}
.jallow-door[data-tier="very-scary"] { 
    background-image: url('YOUR_VERY_SCARY_IMAGE_URL'); 
    border-color: rgba(220, 38, 38, 0.4);
}

.jallow-label {
    font-family: var(--jallow-font);
    margin-top: 15px;
    font-size: 1.1rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1.5px;
}

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.**.

(function () {
    // 1. Map door tiers to your exact Jellyfin Tag names
    const JALLOW_TAGS = {
        "not-scary": "Not Scary Door",
        "scary": "Scary Door",
        "very-scary": "Very Scary Door"
    };

    // Helper: Verify if date is within Oct 1 - Oct 31 (or if test mode is enabled)
    function isHalloweenSeason() {
        // Optional override: run `localStorage.setItem('test_halloween_doors', 'true')` in browser console to force-enable
        if (localStorage.getItem('test_halloween_doors') === 'true') {
            return true;
        }

        const now = new Date();
        const month = now.getMonth(); // 0 = Jan, 9 = Oct
        const day = now.getDate();

        // Check if current month is October (9) and day is between 1 and 31
        return month === 9 && day >= 1 && day <= 31;
    }

    function injectHalloweenDoors() {
        // 1. Date Check: Exit immediately outside of October
        if (!isHalloweenSeason()) return;

        // 2. Prevent duplicate rendering
        if (document.getElementById('jellyfin-halloween-doors')) return;

        // Elegantfin layout container scanner
        const targetView = document.querySelector('.mainAnimatedPages .page:not(.hide) .scrollScroller') || 
                           document.querySelector('#homePage .scrollScroller') ||
                           document.querySelector('.page:not(.hide) [data-role="page"] .scrollScroller') ||
                           document.querySelector('.scrollScroller') ||
                           document.querySelector('.sections');
        
        if (!targetView) return;

        const section = document.createElement('div');
        section.id = 'jellyfin-halloween-doors';
        section.className = 'jallow-section';
        section.innerHTML = `
            <h2 class="jallow-title">Choose Your Scare</h2>
            <p class="jallow-subtitle">Select a door to instantly play a random movie or episode from that fear level.</p>
            <div class="jallow-container">
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="not-scary" tabindex="0" title="Not Scary"></div>
                    <div class="jallow-label" style="color: #4ade80;">Not Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="scary" tabindex="0" title="Scary"></div>
                    <div class="jallow-label" style="color: #fbbf24;">Scary</div>
                </div>
                <div class="jallow-wrapper">
                    <div class="jallow-door" data-tier="very-scary" tabindex="0" title="Very Scary"></div>
                    <div class="jallow-label" style="color: #f87171;">Very Scary</div>
                </div>
            </div>
        `;

        targetView.insertBefore(section, targetView.firstChild);

        section.querySelectorAll('.jallow-door').forEach(door => {
            const triggerAction = () => {
                const tier = door.getAttribute('data-tier');
                playRandomMediaByTag(tier);
            };
            door.addEventListener('click', triggerAction);
            door.addEventListener('keydown', (e) => { if (e.key === 'Enter') triggerAction(); });
        });
    }

    async function playRandomMediaByTag(tier) {
        const tagName = JALLOW_TAGS[tier];
        if (!tagName) return;

        const apiClient = window.ApiClient;
        if (!apiClient) return;

        const serverUrl = apiClient.serverAddress();
        const apiKey = apiClient.accessToken();
        const userId = apiClient.getCurrentUserId();

        // Query both Movies and TV Episodes recursively across your library matching the tag
        const fetchUrl = `${serverUrl}/Users/${userId}/Items?IncludeItemTypes=Movie,Episode&Recursive=true&Tags=${encodeURIComponent(tagName)}&api_key=${apiKey}`;

        try {
            const response = await fetch(fetchUrl);
            const data = await response.json();
            
            if (data.Items && data.Items.length > 0) {
                const randomIndex = Math.floor(Math.random() * data.Items.length);
                const selectedItem = data.Items[randomIndex];
                
                // Redirect to details page
                window.location.href = `${window.location.origin}/web/index.html#!/details?id=${selectedItem.Id}`;
            } else {
                alert(`No movies or episodes found with the tag "${tagName}"!`);
            }
        } catch (error) {
            console.error("Jellyfin Tag Doors API Failure:", error);
        }
    }

    // Observer configured specifically for Elegantfin navigation events
    const observer = new MutationObserver(() => {
        const isHomePage = document.querySelector('#homePage') || document.querySelector('.home-page');
        if (isHomePage) {
            injectHalloweenDoors();
        }
    });

    observer.observe(document.body, { childList: true, subtree: true });

    document.addEventListener('viewshow', function () { injectHalloweenDoors(); });
    setTimeout(injectHalloweenDoors, 500);
    setTimeout(injectHalloweenDoors, 2000);
})();
