# SingleInterface reference data (used in the opening showcase)

This is the data that powers the **credibility showcase** screen the interviewer shows the
shop owner. It all lives inside `index.html` — this file is a readable copy so you can review
and tell me what to change (or edit `index.html` directly).

## Where it is in `index.html`
- **Platform stats + trust badges** → the `SI_PITCH` object (search for `const SI_PITCH`).
- **Client brands per industry** → the `CLIENTS` object (search for `const CLIENTS`).
- **Brand logos** → loaded live from Google's favicon service
  (`https://www.google.com/s2/favicons?domain=<domain>`), with the brand name always shown.
  To change a brand's logo/icon, change its `d:` (domain) value.

> Clearbit's logo API (the "obvious" logo source) was shut down after HubSpot acquired it,
> so we use Google favicons, which are reliable. If you later get a folder of official logo
> image files, tell me and I'll switch the wall to use those instead.

---

## Platform stats (the navy hero band)
| Stat | Label |
|---|---|
| 400K+ | Locations powered |
| 300+ | Brands onboard |
| 9.5 | G2 ease of business |
| 6 of 10 | Top auto brands |
| Global | SG · India · APAC · ME · Africa |
| 300+ | Team members |

**Headline:** Asia's Largest Full-Stack AI Retail Tech Platform
**Sub:** Powering Digital Marketing to Commerce for Storefronts
**Trust badges:** ISO 27001 · SOC 2 · Deloitte Fast 50 · Great Place To Work · Asia Partners & PayPal Ventures backed

---

## Client brands shown per industry
These are the hero names shown when the interviewer taps an industry. (Your full export has
far more; this is a curated, recognisable set per industry — tell me to add/remove any.)

- **Automobiles:** Tata Motors, Mahindra, Bajaj Auto, TVS Motor, Honda Cars, Kia, Audi, Skoda, Ather, Yamaha, Apollo Tyres, CEAT, MRF, JK Tyre, Exide, Michelin
- **BFSI:** SBI Life, HDFC Life, Bajaj Finserv, Bajaj Allianz, Angel One, Motilal Oswal, Tata Capital, Aditya Birla Capital, Axis Mutual Fund, ICICI Prudential, PNB MetLife, HDFC ERGO, Shriram Finance, AU Small Finance Bank, HSBC, Policybazaar
- **Consumer Durables:** Samsung, Panasonic, Xiaomi, OnePlus, HP, Canon, Epson, Croma, Reliance Digital, Vijay Sales, Logitech, Siemens, Bosch, Atomberg
- **Consumer Goods:** Puma, Levi's, Nykaa, Sephora, MAC Cosmetics, The Body Shop, Manyavar, Shoppers Stop, Lifestyle, Max Fashion, U.S. Polo Assn., Pepe Jeans, Asian Paints, Godrej Interio, Hamleys, Wildcraft
- **Education:** Aakash Institute, Mount Litera Zee School, Leverage Edu
- **Health & Fitness:** Tata 1mg, HealthKart, Dr Lal PathLabs, Agilus Diagnostics, Lakmé Salon, Naturals, Amplifon, Jiva Ayurveda, Tattva Wellness Spa
- **Energy:** BPCL, HPCL, IndianOil, Reliance Jio-bp
- **Food & Beverages:** Starbucks, Burger King, KFC, Domino's, Haldiram's, 7-Eleven, Blue Tokai, Wow! Momo, Pret A Manger, Monginis
- **Infrastructure:** JSW Steel, Shree Cement, Orient Cement, Greenlam, Orientbell Tiles, Sintex
- **Telecom:** Vi (Vodafone Idea), Jio, GTPL Hathway, Excitel

---

## To change anything
Tell me (e.g. "add Ola Electric to Automobiles", "remove HSBC", "change the 300+ team stat to
500+") and I'll update `index.html`. Or edit the `CLIENTS` / `SI_PITCH` objects directly — each
brand is `{n:"Display Name", d:"domain.com"}`.
