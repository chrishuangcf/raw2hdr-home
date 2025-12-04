
# ⭐ Turning RAW Photos Into HDR Magic

![hdr](https://cdn.shopify.com/s/files/1/0570/9280/0675/files/SDR-vs-HDR-Comparison.webp?v=1727663813)
![hdr2](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj_Lk_rc8Vna74saHrd6Y5QmV3uLRgBBknwP_PzcQN3AwlNvATKrF9uJ9qoLwxeXkbZHgGSxpT-SChxipwxtquudli2rzztkkWcfFrWutYYh6TyPYkCyzEY1jKrGaq1gQH-kilRHAo6vXWUu5TiT4TK6iwB1LQuRKQHUi-CbIIUC-p5gL39hKmGnPgd4bzi/s1280/brigh.jpg)
![hdr3](https://static1.makeuseofimages.com/wordpress/wp-content/uploads/2022/08/Color-Gamut-1.jpg)

Imagine you take a picture using a fancy camera. Inside the camera is a tiny “light bucket” that catches everything it sees — super bright parts, super dark parts, and all the colors in-between.
This bucket of light is called **RAW data**.

Now we want this RAW file to shine like magic on a bright screen — like an iPhone or an XDR display — where the screen can get **very, very bright** (500, 1600, sometimes 2000 nits!).
To do that, we turn the RAW file into **HDR**.

And guess what?
**HDR is not a mysterious creature.**
It’s *basically your RAW file turned into a 10-bit HEIC file after converting the colors from “linear RGB” to special HDR color spaces like PQ + Rec.2020.*

Let’s walk through the magic step-by-step.

---

# 🌈 What Is RAW?

**RAW is like uncolored coloring-book pages.**
Everything is there — shadows, highlights, information — but it looks flat, gray, and boring.

Why? Because RAW uses **linear light**, meaning:

* If something is twice as bright, the number in the file is literally “2× bigger.”
* There is **no fancy color curve**, **no contrast**, and **no camera “look”** added yet.

This is why filmmakers have “flat” profiles like Sony S-Log or Canon C-Log — same idea!

---

# 💡 What Is HDR (High Dynamic Range)?

HDR means the picture can store **VERY bright highlights** and **VERY dark shadows** at the same time — more range than your eyes see in SDR.

A screen showing HDR can “boost” its brightness to **1600 nits** (iPhone 16 Pro) or more.

To make HDR work, we convert the RAW’s linear numbers into:

* **PQ curve** (Perceptual Quantizer) for brightness
* **Rec.2020** for wide color
* **HEIC container**, because Apple devices understand it beautifully

---

# 🛠️ The RAW → HDR Pipeline

**RAW Linear Sensor Data**
⬇ (demosaic + white balance)
**Linear Camera RGB**
⬇ (convert using camera → XYZ → Rec.2020 matrix)
**Linear Rec.2020 RGB**
⬇ (apply PQ encoding)
**PQ Rec.2020 10-bit Image**
⬇ (store)
**HEIC HDR File**

Your app basically reproduces the core of what large software like Hasselblad Phocus 2 does — but focuses on *simplicity and direct conversion* so users get HDR results instantly.

---

# 🎨 Why Does the Preview Look “Flat”?

Because:

* The app uses the **raw linear color curve** (just like S-Log footage).
* **Camera manufacturer color profiles are NOT applied**.
* The macOS **Metal canvas doesn’t do HDR tone-mapping** or manufacturer “looks.”

So the preview looks flat and quiet…

…but the final **HDR HEIC file on an iPhone** lights up beautifully, showing the *real* HDR brightness and contrast.

---

# 📱 Why the Image Starts Dim Then Brightens On iPhone

When you open an HDR HEIC file:

1. iPhone first shows an SDR placeholder → looks dim.
2. Then it animates into **HDR mode** and the screen boosts to higher nits.

This lets you *see* the image brighten into the HDR version.

---

# 🖼️ SDR vs HDR Histogram

### SDR Histogram

```
|█████████--|   (only small range of brightness)
```

### HDR Histogram

```
|█████████████████████████████████|   (much wider!)
```

HDR simply has **way more room**, so the bright parts don’t get squished or clipped.

---

# 🔦 Understanding Nits (Brightness)

* **1 nit** = the brightness of 1 candle
* **1600 nits** = *1,600 candles!*
* **2000 nits** = super-duper bright
* **500 nits** = older monitors/laptops

The HEIC HDR file tells the device:

> “Hey, this photo can get up to ___ nits!”

The device tries to show it correctly.

---

# 🎚️ Why Higher Nits = Darker Image (Important!)

This is the part that confuses many users — even professionals.

Let’s break it down:

* If you export HDR for **1600 nits**, the system expects very bright highlights available.
* When viewed on a **500-nit** screen, the viewer **can’t reach 1600**, so it tries to “fit” the entire range → **image looks darker**.
* If you export at **500 nits**, a 500-nit display can show it perfectly → **it looks brighter**.
* If you export at **2000 nits**, even a 1600-nit iPhone will show it slightly darker because it’s trying to preserve “headroom.”

### Summary Table

| Export Nit Level | Display Max Nit                                        | Result           |
| ---------------- | ------------------------------------------------------ | ---------------- |
| 1600 → 1600      | Match                                                  | Perfect exposure |
| 500 → 1600       | Display brighter than encoded → image looks **bright** |                  |
| 2000 → 1600      | Image looks **darker** with more preserved highlights  |                  |
| 1600 → 500       | Display too dim → image looks **dark**                 |                  |

---

# 🧪 Why Create an 8-bit JPEG Side-by-Side?

Because it helps the user see:

* SDR (8-bit JPEG) vs
* HDR (10-bit HEIC)

This comparison makes the “HDR magic” obvious.

---

# 🧱 Full Architecture

### **1. RAW Sensor Stage**

* Demosaic
* Linearization
* Black level subtraction
* White balance
* Apply camera → XYZ matrix

### **2. Wide-Gamut Stage**

* Convert XYZ → Rec.2020
* Still linear

### **3. HDR Encoding Stage**

* Convert Linear Rec.2020 → PQ
* Normalize brightness for export nit value
* 10-bit quantization

### **4. Packaging Stage**

* Wrap PQ Rec.2020 data into HEIC container
* Add Apple-specific HDR metadata
* Set mastering display info (max nits)

### **5. Display Stage**

* iPhone/macOS detects HDR metadata
* Switches display from SDR mode to HDR mode
* Tone maps based on display’s nit capability

---

# 🏁 Final Notes for Users

* Your HDR HEIC output is **true HDR**, as raw as possible.
* The preview is intentionally simple: it helps users see SDR vs HDR without fake contrast.
* Export nit level matters:

  * **Match your target device for the most accurate brightness.**
  * Higher nit = more headroom = darker appearance on dim displays.

Just tell me!
