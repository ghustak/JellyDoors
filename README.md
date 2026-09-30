[Default Jellyfin With Date Check - Java Script.txt](https://github.com/user-attachments/files/32837327/Default.Jellyfin.With.Date.Check.-.Java.Script.txt)# 🎃 Halloween "Shuffle Doors" for Jellyfin (HBO Max Style)

Bring the HBO Max Halloween "Shuffle Doors" experience to your Jellyfin homepage! This script injects three customizable fear-tier doors (**Not Scary**, **Scary**, and **Very Scary**) onto your dashboard. Clicking any door queries your server for movies and TV episodes tagged with that fear level and instantly launches a random pick.

<img width="1792" height="633" alt="Screenshot 2026-09-29 220046" src="https://github.com/user-attachments/assets/ee468596-4289-4d8e-95d4-06aa76f908eb" />


---

## ⚡ Features

- **HBO Max Style Shuffle:** Instantly picks a random movie or TV episode tagged with your fear level across your whole library.
- **Tag-Based Searching:** Uses Jellyfin's `Tags` query API so you don't need to maintain manual collection folders.
- **Dual Theme Support:** Custom layout scanners for both standard **Jellyfin Web** and **Elegantfin**.
- **Optional Seasonal Auto-Toggle:** Includes versions with built-in date checks to automatically display doors only between October 1st and October 31st.

---

## Step 1: Download and Install Prerequistes
In order for the script to function and be implemented you will need to have download and installed both the:
  - File Transformation Plugin: https://github.com/n00bcodr/Jellyfin-JavaScript-Injector
  - Jellyfin Java Script Injector: https://github.com/n00bcodr/Jellyfin-JavaScript-Injector

---

## 🏷️ Step 2: Tag Your Media in Jellyfin

1. In Jellyfin, select the movies or TV episodes you want in each tier.
2. Click the **3 dots menu** on the movies or specific epsiodes -> **Edit Metadata**.
3. Under **Tags**, add your desired fear tag strings (e.g., `Not Scary Door`, `Scary Door`, `Very Scary Door`).
4. Ensure the `JALLOW_TAGS` object in the JavaScript code blocks below match your exact tag names.

---

## Step 3: Select your preferred format:
  1. Default Jellyfin Without Date Check
  2. Default Jellyfin With Date Check
  3. Elegantfin Without Date Check
  4. Elegantfin With Date Check

(Note: The CSS code will contain several long url strings. These strings link to the host site for the door images. Ensure you have copied the full code block or you may not have a fully functioning setup.)

  ---

  ## Default Jellyfin Without Date Check

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.
  
[Default Jellyfin Without Date Check - CSS.txt](https://github.com/user-attachments/files/32836223/Default.Jellyfin.Without.Date.Check.-.CSS.txt)

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.

[Default Jellyfin Without Date Check - Java Script.txt](https://github.com/user-attachments/files/32836224/Default.Jellyfin.Without.Date.Check.-.Java.Script.txt)

---

## Default Jellyfin With Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

[Default Jellyfin With Date Check - CSS.txt](https://github.com/user-attachments/files/32836696/Default.Jellyfin.With.Date.Check.-.CSS.txt)

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.

[Uploadin(function () {
    const JALLOW_TAGS = {
        "not-scary": "Not Scary Door",
        "scary": "Scary Door",
        "very-scary": "Very Scary Door"
    };

    function isHalloweenSeason() {
        if (localStorage.getItem('test_halloween_doors') === 'true') return true;
        const now = new Date();
        return now.getMonth() === 9 && now.getDate() >= 1 && now.getDate() <= 31;
    }

    function injectHalloweenDoors() {
        if (!isHalloweenSeason()) return;
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
})();g Default Jellyfin With Date Check - Java Script.txt…]()


---

## Elegantfin Without Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

[Uploading Elegantfin Without Date Check - CSS.txt…]()

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.

[Uploading Elegantfin Without Date Check - Java Script.txt…]()

---

## Elegantfin With Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

[Uploading Elegantfin With Date Check - CSS.txt…]()

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.

[Elegantfin With Date Check - Java Script.txt](https://github.com/user-attachments/files/32836878/Elegantfin.With.Date.Check.-.Java.Script.txt)
