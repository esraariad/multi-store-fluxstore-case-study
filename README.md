# 🏢 Multi-Tenant E-Commerce Platform | 60+ White-Label Apps from a Single Codebase

> **Case Study:** Architecting and scaling a Flutter-based white-label e-commerce platform that powers 60+ branded restaurant apps from a single source of truth.

---

## ⚡ Key Highlights

| Metric | Value |
|--------|-------|
| 📱 Apps Generated | **60+** |
| 🏗️ Codebases | **1** (single source) |
| 🚀 Platforms | iOS + Android |
| 💳 Payment Gateways | **8 integrated** — Stripe, PayPal, Razorpay, Tap, Paytm, Mercado Pago, Authorize.Net, Converge |
| 🌍 Regional Reach | US, Middle East, India, LATAM |
| 🔧 Architecture | Flutter Flavors + Modular Theming + Pluggable Payments |

---

## 🎯 The Problem

The client operates a network of restaurants across multiple regions, each requiring its **own branded mobile app** with:
- Unique app name, icon, splash, theme colors
- Region-specific payment gateways (Stripe in the US, Tap in the Middle East, Razorpay in India, etc.)
- Separate payment gateway credentials per restaurant
- Independent App Store / Play Store listings
- Distinct push notification channels and analytics

The naive approach (one codebase per restaurant) would have meant:
- ❌ 60+ Git repositories to maintain
- ❌ Every bug fix multiplied by 60
- ❌ Feature rollout nightmare
- ❌ Unsustainable engineering cost

**The challenge:** Deliver 60+ fully branded, store-published apps without 60x the engineering effort.

---

## 🏗️ The Solution: Flutter Flavors + Configuration-Driven Architecture

I architected a **multi-flavor build pipeline** where the entire app behavior is driven by per-client configuration files. A new branded app is added by:

1. Creating a new flavor config (JSON + asset folder)
2. Running a single build command
3. Submitting to stores

No code changes. No new repository. No regression risk to existing apps.

### Flavor Configuration Example

```dart
// Per-client config drives everything at build time:
// branding, backend, payment gateways, regional defaults.
Map<String, dynamic> environmentClientA = {
  "version": "2.2.0",
  "appConfig": "lib/config/clientA_config.json",
  "serverConfig": {
    "url": "https://clientA.com",
    "type": "woo",
    "consumerKey": "ck_...",
    "consumerSecret": "cs_...",
  },
  "androidPackageId": "com.clientA.www",
  "iosPackageId": "com.clientA.www",
  "advanceConfig": {
    "appName": "Client A",
    "DefaultCurrency": {"currency": "USD", "symbol": "\$"},
    // ...
  },
  "paymentConfig": {
    "payments": {
      "stripe": "assets/icons/payment/stripe.png",
      "paypal": "assets/icons/payment/paypal.png",
      "tap":    "assets/icons/payment/tap.png",
      // ... other gateways enabled for this client
    },
    "stripeConfig":   { "paymentMethodId": "stripe",    "publishableKey": "..." },
    "paypalConfig":   { "paymentMethodId": "paypal",    "clientId": "..." },
    "razorpayConfig": { "paymentMethodId": "razorpay",  "keyId": "..." },
    "tapConfig":      { "paymentMethodId": "tap",       "publicKey": "..." },
    // Pluggable: each client enables only what they need
  },
};

// main_<client>.dart entry points stay minimal
void main() => runApp(MyApp(env: environmentClientA));
```

### Build Pipeline

```bash
flutter build apk --flavor clientA -t lib/main_clientA.dart
flutter build ios --flavor clientA -t lib/main_clientA.dart
```

---

## 👩‍💻 My Role

- **Architected** the multi-flavor pipeline from scratch — config schema, build automation, and asset management
- **Integrated 8 payment gateways** (Stripe, PayPal, Razorpay, Tap, Paytm, Mercado Pago, Authorize.Net, Converge) behind a unified payment abstraction so each client enables only the providers they need
- **Built loyalty points & subscription systems** that work consistently across all 60+ branded apps
- **Managed store publishing lifecycle** — handled Apple/Google review processes, policy escalations, and store-specific compliance issues
- **Established maintenance workflow** that scales — a bug fix in core code propagates to all 60 apps with one release cycle

---

## 🚧 Real Challenges & How I Solved Them

### 1. Asset Management at Scale
**Problem:** 60+ sets of logos, splash screens, and brand assets risked turning the repo into chaos.
**Solution:** Structured asset folders per flavor with a validation script that checks every required asset exists before build — caught missing assets before they hit production.

### 2. iOS App Store Review Rejections
**Problem:** Apple flagged multiple apps as "duplicate apps" due to similar structure.
**Solution:** Worked with each restaurant's content team to emphasize unique branding, distinct value propositions, and clear differentiation in app descriptions. Eventually established a submission template that passed reviews consistently.

### 3. Multi-Gateway Payment Abstraction

**Problem:** Different restaurants operate in different regions and demand different payment providers — Stripe in the US, Tap in the Middle East, Razorpay in India, Mercado Pago in LATAM. Hard-coding any single gateway would have made the platform unusable in entire markets.

**Solution:** Built a **pluggable payment layer** with a unified `PaymentMethodType` interface that abstracts gateway-specific logic. Each flavor's config declares which gateways are enabled and supplies the credentials, while the UI auto-renders only the relevant payment options. Adding a new gateway means implementing one adapter — every client that opts in gets it instantly. Also integrated WooCommerce-side gateways (Authorize.Net, Converge) for saved-card and subscription flows.

### 4. Performance Across Low-End Devices
**Problem:** Restaurant customers use a wide range of devices, including older Android phones.
**Solution:** Lazy-loaded heavy components, optimized image caching with `cached_network_image`, and used `const` constructors aggressively. Reduced cold-start time on low-end Android devices significantly.

---

## 🛠️ Tech Stack

| Category | Technologies |
|----------|--------------|
| **Framework** | Flutter, Dart |
| **Build Management** | Flutter Flavors, 70+ entry points, per-flavor env configs |
| **Base Template** | FluxStore (extensively customized) |
| **Payments** | Stripe, PayPal, Razorpay, Tap, Paytm, Mercado Pago, Authorize.Net, Converge |
| **Backend** | WooCommerce REST API, Firebase Auth, Firestore, Firebase Messaging |
| **State Management** | Provider, BLoC |
| **CI/CD** | Codemagic, Flutter Flavors automation |

---

## 📸 Screenshots

### Onboarding & Authentication
![Image](https://github.com/user-attachments/assets/864b2705-aa56-4a03-9ada-6d5a4340b80d)
![Image](https://github.com/user-attachments/assets/b9e6dc88-6cf6-41fd-ab78-f4a935c1ac79)

### Browsing & Menu
![Image](https://github.com/user-attachments/assets/ae200dc4-a6d5-4df0-b8f1-5a9338fba55c)
![Image](https://github.com/user-attachments/assets/c7dee07d-c2d8-47fa-8dce-9ad0c563a0f7)
![Image](https://github.com/user-attachments/assets/95eec095-6c44-4d42-b83b-c125f8054122)
![Image](https://github.com/user-attachments/assets/78302655-cb6d-4a1a-bd11-6a1a4bd6b321)

### Cart & Checkout
![Image](https://github.com/user-attachments/assets/07f7f855-adba-49c4-af4c-799e50937907)
![Image](https://github.com/user-attachments/assets/cb7b291c-8ed8-4458-8d82-d21db0c7e2c3)
![Image](https://github.com/user-attachments/assets/ec9f66c8-f110-418d-9c58-8a54254a048b)

### Loyalty & Subscriptions
![Image](https://github.com/user-attachments/assets/2ee557cd-dc6f-4e87-bfbc-23f4a5d0b6f0)
![Image](https://github.com/user-attachments/assets/ab8d55ef-b0b0-4efe-8a02-56a78924093e)
![Image](https://github.com/user-attachments/assets/b25f263a-d61f-44a8-a060-1ef3080f161f)

### Branded Variations Across Clients
![Image](https://github.com/user-attachments/assets/870b0c6f-12b3-4f50-ba9b-5c46207f5682)
![Image](https://github.com/user-attachments/assets/deaf8681-d2ae-4b9f-834c-6ee9d558f7ad)
![Image](https://github.com/user-attachments/assets/5849144c-6d74-40db-9e4c-0037be5cb4f6)
![Image](https://github.com/user-attachments/assets/b93866f6-7a91-4cc2-8356-c5ae822989c6)
![Image](https://github.com/user-attachments/assets/f9562070-2f82-4b54-9a1b-f4695802e553)

---

## 📈 Outcomes

- ✅ **60+ branded apps** published across App Store and Play Store
- ✅ **45% reduction** in maintenance overhead compared to multi-codebase approach
- ✅ **Single release cycle** rolls out features and fixes to all clients simultaneously
- ✅ **Scalable onboarding** — adding a new branded app takes hours, not weeks
- ✅ **8 payment gateways** unified behind one abstraction, unlocking US, Middle East, India, and LATAM markets from a single codebase
- ✅ **PCI-compliant payment** flows established as reusable patterns across the company

---

## 🎓 Key Takeaways

- **Architecture decisions compound.** The upfront investment in a proper Flavors pipeline paid back 60x.
- **Configuration-driven design** beats code-driven branching for multi-tenant systems.
- **Store review processes** are part of the engineering work — not an afterthought.

---

## 🔗 Live Apps

📲 [View All Published Apps on App Store](https://apps.apple.com/us/developer/combustion-innovation/id767284592?see-all=i-phonei-pad-apps)

📲 [View All Published Apps on Google Play Store](https://play.google.com/store/apps/developer?id=Combustion)


---

## 💼 Let's Connect

- **LinkedIn:** [esraa-riad](https://www.linkedin.com/in/esraa-riad-1b9592b4/)
- **GitHub:** [@esraariad](https://github.com/esraariad)
- **Email:** engesraariad@gmail.com
- **Phone:** +201016363566 - +201144320304

> 💡 *Full source code is private (proprietary to client). I'd be happy to walk through the architecture in a call or share code samples under NDA.*
