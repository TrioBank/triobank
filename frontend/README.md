# Frontend

Bu dizin tüm frontend uygulamalarını içerir.

## Dizin Yapısı

```
frontend/
├── web-portal/         # Müşteri web portalı
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── README.md
│
├── admin-portal/       # Admin yönetim paneli
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── README.md
│
└── mobile-api/         # Mobile için BFF (Backend for Frontend)
    ├── src/
    ├── package.json
    └── README.md
```

## Teknoloji Stack

### Web Portal (Müşteri)
- **Framework**: React 18+ / Next.js veya Vue 3
- **State Management**: Redux Toolkit / Zustand / Pinia
- **UI Library**: Material-UI / Ant Design / Tailwind CSS
- **API Client**: Axios / React Query
- **Testing**: Jest, React Testing Library

### Admin Portal
- **Framework**: React 18+ / Vue 3
- **Admin Framework**: React Admin / Ant Design Pro
- **Charts**: Chart.js / Recharts / ECharts
- **Testing**: Jest, Cypress

### Mobile API (BFF)
- **Framework**: Node.js + Express / NestJS
- **Purpose**: Mobile cihazlar için optimize edilmiş API aggregation

## Geliştirme Standartları

- TypeScript kullanımı zorunlu
- ESLint ve Prettier ile code formatting
- Husky ile pre-commit hooks
- Responsive design (mobile-first)
- PWA desteği
- Accessibility (WCAG 2.1 AA)

## Build ve Deploy

```bash
# Development
npm install
npm run dev

# Production build
npm run build

# Testing
npm run test
npm run test:e2e
```

## Environment Değişkenleri

Her uygulama için `.env.example` dosyası oluşturulmalıdır:

```
REACT_APP_API_URL=https://api.triobank.com
REACT_APP_AUTH_URL=https://auth.triobank.com
REACT_APP_ENVIRONMENT=production
```
