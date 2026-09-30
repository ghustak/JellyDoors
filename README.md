[Default Jellyfin With Date Check - Java Script.txt](https://github.com/user-attachments/files/32837503/Default.Jellyfin.With.Date.Check.-.Java.Script.txt)[Default Jellyfin With Date Check - Java Script.txt](https://github.com/user-attachments/files/32837430/Default.Jellyfin.With.Date.Check.-.Java.Script.txt)[Default Jellyfin With Date Check - Java Script.txt](https://github.com/user-attachments/files/32837327/Default.Jellyfin.With.Date.Check.-.Java.Script.txt)# 🎃 Halloween "Shuffle Doors" for Jellyfin (HBO Max Style)

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

https://github.com/ghustak/JellyDoors/blob/main/Default%20Jellyfin%20With%20Date%20Check%20-%20Java%20Script

---

## Elegantfin Without Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.

https://github.com/ghustak/JellyDoors/blob/main/Elegantfin%20Without%20Date%20Check%20-%20CSS

Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.

https://github.com/ghustak/JellyDoors/blob/main/Default%20Jellyfin%20With%20Date%20Check%20-%20Java%20Script

---

## Elegantfin With Date Check

Copy and past the following CSS into **Admin Dashboard -> Branding -> Custom CSS**.



Copy and past the following CSS into **Admin Dashboard -> Plugins -> JS InJector -> Add Script**.

[Elegantfin With Date Check - Java Script.txt](https://github.com/user-attachments/files/32836878/Elegantfin.With.Date.Check.-.Java.Script.txt)
