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

