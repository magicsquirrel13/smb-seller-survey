# SingleInterface reference data (archived — no longer used by the app)

The app used to open with a **credibility showcase** screen: a SingleInterface hero band with
platform stats and trust badges, an industry picker, and a wall of client brands for that
industry. **That screen was removed from `index.html` in November 2026.** The survey now goes
straight from the interviewee card into Section A, and the `SI_PITCH` / `CLIENTS` objects no
longer exist in the code.

This file is kept as a readable copy of that data so it can be reused later — for a leave-behind
card, a pitch deck, or if the showcase is ever brought back. Nothing here affects the survey.

---

## Platform stats (what the hero band showed)
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

## Client brands by industry
A curated, recognisable set per industry (the full client list is much longer).

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

## If the showcase is ever brought back
Brand logos were loaded live from Google's favicon service
(`https://www.google.com/s2/favicons?domain=<domain>&sz=64`), with the brand name always shown
as a fallback. Clearbit's logo API was shut down after HubSpot acquired it, so avoid that
source. Each brand was stored as `{n:"Display Name", d:"domain.com"}`.
