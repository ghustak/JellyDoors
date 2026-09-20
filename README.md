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

---

## Default Jellyfin With Date Check
