# Saffron & Smoke - Restaurant Website

Customer-facing restaurant site with a 50-dish menu, QR table ordering, home delivery and online ordering. It uses external HTML, CSS and JavaScript files.

## Folder structure

    saffron-smoke/
    ├── index.html
    ├── css/style.css
    ├── js/
    │   ├── data.js              (restaurant info, payment numbers, dishes)
    │   ├── app.js               (menu, cart, orders, theme, QR)
    │   └── firebase-config.js   (your Firebase keys go here)
    └── images/payment/          (easypaisa-qr.png, jazzcash-qr.png)

## Features

- 50 dishes in 6 categories, with search and category filters
- Cart with quantity change, saved in the browser
- Order types: Dine-in (table number), Home Delivery (address) and Takeaway
- Order Now: saves the order to Firebase so staff see it instantly
- Send Order on WhatsApp: opens WhatsApp with the full order message
- Payment: Cash on Delivery (+ Rs 200, Home Delivery only), Pay at the restaurant (Dine-in and Takeaway), Easypaisa and JazzCash (QR, number and transaction ID)
- Home Delivery adds a delivery fee of Rs 150
- QR code generator for table menus (add a table number and download the QR)
- Contact form that sends a WhatsApp message
- Dark and light theme, mobile friendly

## Payment QR codes

Download your QR codes from the Easypaisa and JazzCash apps and save them as:

    images/payment/easypaisa-qr.png
    images/payment/jazzcash-qr.png

## Firebase setup

This site uses the same Firebase project as Hasan Mart (`hasan-mart`). Orders go to a separate collection called `restaurantOrders`.

1. Open `js/firebase-config.js` and replace everything with your config:

        window.FIREBASE_CONFIG = {
          apiKey: "...",
          authDomain: "...",
          projectId: "...",
          storageBucket: "...",
          messagingSenderId: "...",
          appId: "..."
        };

2. In Firebase go to Firestore Database > Rules, paste the rules below (replace STAFF_EMAIL with the staff email) and click Publish:

        rules_version = '2';
        service cloud.firestore {
          match /databases/{database}/documents {
            match /orders/{id} {
              allow create: if true;
              allow read, update: if request.auth != null
                && request.auth.token.email == 'STAFF_EMAIL';
            }
            match /restaurantOrders/{id} {
              allow create: if true;
              allow read, update: if request.auth != null
                && request.auth.token.email == 'STAFF_EMAIL';
            }
          }
        }

3. The GitHub Pages domain (`syedhasanshahqadri-ops.github.io`) is already authorized, so nothing else is needed.

Until Firebase is connected, Order Now runs in demo mode and the restaurant will not receive the order.

## Changing details

Open `js/data.js`:

- `CFG` holds the restaurant name, WhatsApp number, phone, email, delivery fee (`delivery`), COD fee (`codFee`) and the Easypaisa and JazzCash numbers.
- `D` holds the dishes: [category number, name, price, description].
- `CATS` holds the category names, and `IMG` holds the photo IDs for each category.

Write the WhatsApp number in international format (92300...), without a leading 0 or +.
The address and opening hours are in `index.html` (Contact section).

## QR codes for tables

1. Open the live site and scroll to **QR Menu**.
2. Type a table number and click **Download QR**.
3. Print it and place it on the table. Guests who scan it get the menu with the table number already filled in.

## Publishing on GitHub Pages

1. Create a repository named `saffron-smoke`.
2. Upload the contents of this folder to the root of the repository.
3. Go to Settings > Pages, choose the `main` branch and the root folder, then save.
4. Your link: `https://syedhasanshahqadri-ops.github.io/saffron-smoke/`
5. Create the QR codes only after the site is live, so they point to the correct link.# FOOD-WEBSITE
