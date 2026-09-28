# CredLite 💳

A Flutter-based credit card management app that brings together **bill tracking, transaction management, spending insights, and card rewards** in one clean interface.

CredLite was built as a portfolio project with an emphasis on **practical Flutter engineering, smooth interactions, performance, and maintainable code**. The project intentionally keeps the architecture straightforward instead of relying on heavy abstractions or generated code.

---

## ✨ What You Can Do

### 💳 Manage Credit Cards

* Browse multiple cards through a horizontally scrollable carousel.
* Flip cards with a 3D animation to view additional information.
* View available limits and upcoming payment dates.

### ⏳ Track Upcoming Bills

* Displays a live countdown until the next bill deadline.
* Uses `Timer.periodic` for real-time updates.
* Includes a haptic-feedback-powered payment action.

### 🧾 Browse Transactions Offline

* Transactions are cached locally using Hive.
* The application can display data immediately from local storage.
* Search transactions instantly.
* Filter transactions by spending category.
* Tested with a dataset of approximately **5,000 transactions**.

### 📊 Understand Your Spending

* Monthly spending visualizations using `fl_chart`.
* Category-wise spending distribution.
* Aggregation can be moved away from the UI thread for heavier datasets.

### 🎁 Unlock Rewards

* Interactive scratch-card style reward vouchers.
* Implemented using Flutter's `CustomPainter`.
* Uses `saveLayer` and `BlendMode.clear` for the scratch effect.
* Revealed rewards are persisted locally.

---

## 🛠️ Technical Highlights

| Area                  | Technology             | Implementation                                               |
| --------------------- | ---------------------- | ------------------------------------------------------------ |
| Networking            | Dio                    | REST requests, query parameters, timeouts and error handling |
| Local Storage         | hive_ce                | JSON-based local persistence without generated adapters      |
| State Management      | Provider               | `ChangeNotifier` with a cache-first application flow         |
| Background Processing | Dart Isolates          | `Isolate.run` for spend calculations                         |
| Charts                | fl_chart               | Monthly and category-based spending visualizations           |
| Custom UI             | CustomPainter          | Scratch-card rendering and interaction                       |
| Animations            | Flutter Animation APIs | `AnimationController` with 3D `Matrix4` transformations      |

---

## ⚡ Performance Considerations

The project includes several Flutter performance techniques rather than treating performance as an afterthought.

### Repaint Isolation

`RepaintBoundary` is used around independently changing areas such as:

* Credit card interactions
* Scratch-card canvases

This prevents unnecessary repaint propagation to surrounding widgets.

### Background Computation

Spend aggregation over thousands of transaction records can be executed using:

```dart
Isolate.run(...)
```

This keeps computational work away from the main UI isolate.

A `Stopwatch` is also included to compare the execution time of the main-thread implementation against the isolate-based implementation.

### Efficient Scrolling

Transaction lists use:

```dart
ListView.builder(...)
```

so widgets are created as required instead of constructing the complete list at once.

### Const Widgets

`const` constructors are used wherever possible to reduce unnecessary widget-tree work and improve element reuse.

---

## 🔍 Checking Performance with DevTools

The performance-related implementations can be inspected directly using Flutter DevTools.

### Profile the application

```bash
flutter run --profile
```

Then open Flutter DevTools and use the **Performance** section.

You can inspect:

1. **Repaint activity**
   Enable repaint visualization and interact with the cards and scratch rewards.

2. **CPU activity**
   Run the spend aggregation benchmark and inspect the CPU profiler to see the work performed by the isolate.

3. **Frame performance**
   Interact with animations and scrolling while observing frame timings.

---

## 📁 Project Setup

### Requirements

Make sure you have:

* Flutter SDK
* Dart SDK
* Node.js
* npm

### 1. Clone the project

```bash
git clone <repository-url>
cd CRED_LITE
```

### 2. Start the mock API

```bash
cd mock_server
npm install
node server.js
```

The server runs at:

```text
http://localhost:3000
```

For an Android emulator, the host machine can be accessed through:

```text
http://10.0.2.2:3000
```

### 3. Launch the Flutter application

From the Flutter project directory:

```bash
flutter pub get
flutter run
```

---

## 📱 Application Screens

| Credit Cards                       | Transactions                  | Analytics                     | Rewards                     |
| ---------------------------------- | ----------------------------- | ----------------------------- | --------------------------- |
| Card carousel and bill information | Search and category filtering | Spending charts and benchmark | Interactive scratch rewards |
| *Add screenshot*                   | *Add screenshot*              | *Add screenshot*              | *Add screenshot*            |

> Replace the placeholders above with screenshots from the application before using this repository as a portfolio project.

---

## 🧩 Architecture Approach

CredLite intentionally uses a relatively lightweight architecture.

The application follows a **cache-first data flow**:

```text
REST API
   ↓
Dio Service
   ↓
Local Hive Cache
   ↓
ChangeNotifier
   ↓
Provider
   ↓
Flutter UI
```

For computationally heavier analytics:

```text
Transactions
     ↓
Aggregation
     ↓
Isolate.run()
     ↓
Analytics Result
     ↓
UI
```

This keeps networking, persistence, state management, presentation, and background computation reasonably separated without introducing unnecessary architectural complexity.

---

## ⚠️ Current Trade-offs

A few implementation decisions were made deliberately to keep the project compact and easy to understand.

### Hive Serialization

Transaction and reward data are stored as JSON strings rather than custom Hive adapters.

This avoids generated code and keeps the persistence layer easier to inspect, although typed adapters could be preferable for a larger production application.

### Mock Backend

The included Node.js server is intended for development and demonstration.

Its state is held in memory, so server-side changes are lost when the process is restarted.

### Currency

The current UI and formatting are designed around **Indian Rupees (₹)**.

---

## 🎯 Project Focus

The main goal of CredLite is not to reproduce an entire production banking platform.

Instead, it demonstrates practical Flutter skills through a focused application:

* Flutter UI development
* REST API integration
* Offline persistence
* Reactive state management
* Custom rendering
* Animation
* Background computation
* Performance profiling
* Clean and readable project structure

---

## 👨‍💻 Built With

**Flutter · Dart · Provider · Dio · Hive CE · fl_chart · Node.js · Express**
