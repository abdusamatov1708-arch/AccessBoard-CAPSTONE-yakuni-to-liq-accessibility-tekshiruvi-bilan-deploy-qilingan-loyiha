# AccessBoard-CAPSTONE-yakuni-to-liq-accessibility-tekshiruvi-bilan-deploy-qilingan-loyiha
Plaintext
accessboard-capstone/
├── server.js               # Node.js / Express backend
├── package.json
├── public/
│   ├── index.html
│   ├── style.css
│   └── script.js
└── .github/
    └── workflows/
        └── lighthouse.yml  # CI avtomatik audit uchun
1. server.js (Node.js & Express Backend)
Backend serverni yaratish va uni Render/Railway hostinglariga moslab sozlash uchun:

JavaScript
const express = require('express');
const path = require('path');
const app = express();
const PORT = process.env.PORT || 3000;

// Frontend fayllarini ulash
app.use(express.static(path.join(__dirname, 'public')));
app.use(express.json());

// Oddiy API endpoint
app.get('/api/health', (req, res) => {
  res.json({ status: 'OK', message: 'AccessBoard API ishlayapti' });
});

app.listen(PORT, () => {
  console.log(`Server ${PORT}-portda ishga tushdi`);
});
2. package.json
Dependencies va start skriptlari:

JSON
{
  "name": "accessboard-capstone",
  "version": "1.0.0",
  "description": "AccessBoard Capstone Project",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.19.2"
  }
}
3. CI Avtomatik Auditi uchun: .github/workflows/lighthouse.yml
GitHub Actions orqali har bir push qilinganda Lighthouse CI yordamida avtomatik accessibility (qulaylik) tekshiruvini ishga tushirish uchun:

YAML
name: Lighthouse Accessibility CI

on:
  push:
    branches: [ main ]

jobs:
  accessibility:
    runs-on: ubuntu-latest
    steps:
      - name: Kodni yuklab olish (Checkout)
        uses: actions/checkout@v4

      - name: Node.js o'rnatish
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Paketlarni o'rnatish
        run: npm install

      - name: Serverni fon rejimida ishga tushirish
        run: node server.js &
        
      - name: Lighthouse CI tekshiruvini ishga tushirish (Accessibility 90+)
        uses: treosh/lighthouse-ci-action@v10
        with:
          urls: |
            http://localhost:3000
          uploadArtifacts: true
          temporaryPublicStorage: true
          minScoreAccessibility: 90
