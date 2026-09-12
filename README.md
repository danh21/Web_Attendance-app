# Attendance Dashboard

## 📚 Table of Contents

- [Attendance Dashboard](#attendance-dashboard)
  - [📚 Table of Contents](#-table-of-contents)
  - [📝 About](#-about)
  - [📁 Source](#-source)
  - [🚀 Getting Started](#-getting-started)
    - [💻 Technology](#-technology)
    - [🛠️ Build / Verification](#️-build--verification)
  - [🔗 Reference](#-reference)

## 📝 About

Static web prototype for an attendance management dashboard. The interface includes a search field, navigation sidebar, attendance summary cards, month selectors, and an attendance list view.

- ✅ To write/read data from firestore, MUST login with correct authentication
- ✅ Responsive dashboard layout built with HTML and CSS
- ✅ Attendance summaries with monthly and yearly performance indicators
- ✅ Profile cards with local avatar assets
- ✅ Font Awesome icons and Google Fonts loaded from CDNs
- ℹ️ Firebase scripts are referenced in `src/index.html`, but this repository does not currently include a package or application build configuration

## 📁 Source

```
.
├── .github/          # git workflow
├── src/              # application source
│   ├── index.html    # main dashboard
│   ├── login.html    # login page
│   └── css/          # application styles
│   └── rsc/          # image
├── test/             # test scripts (unused)
├── doc/              # documentation and notes
├── .gitignore
└── README.md
```

## 🚀 Getting Started

### 💻 Technology

- HTML5
- CSS3
- Font Awesome 6.1.1 via CDN
- Google Fonts (Roboto and Anton) via CDN
- Firebase CDN references included in the page for planned data integration

### 🛠️ Build / Verification

No build step or package manager is required for the current static prototype.

1. Open [src/index.html](src/index.html) directly in a browser, or serve the project directory with any static HTTP server:

  ```bash
  python -m http.server 8000
  ```

2. Visit `http://localhost:8000/src/`.

The file [test/run_test.sh](test/run_test.sh) is currently empty, so automated tests are not configured yet. Verify the page manually in a modern browser and confirm that the external CDN resources are available.

## 🔗 Reference

- [Font Awesome](https://fontawesome.com/)
- [Google Fonts](https://fonts.google.com/)
- [Firebase Web Documentation](https://firebase.google.com/docs/web/setup)
