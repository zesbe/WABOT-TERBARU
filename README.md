Buatkan sistem WhatsApp bot sender multi-user untuk **bisnis marketing dan promosi** dengan web dashboard management yang lengkap. Fokus pada pengiriman pesan massal, broadcast, dan automation marketing.

**🇺🇸 English:**
Create a multi-user WhatsApp bot sender system for **marketing and promotion businesses** with comprehensive web dashboard management. Focus on mass messaging, broadcasting, and marketing automation.

### Stack Teknologi / Technology Stack
- **Frontend Dashboard**: Vue 3 + Composition API + TypeScript + Tailwind CSS + Vite
- **Backend**: RAILWAY + Supabase (Database, Auth.)
- **WhatsApp Bot**: Browserless + Playwright (Stealth Mode)
- **Multi-language**: Vue i18n (Indonesian & English)
- **Queue System**: Bull Queue / Agenda.js
### **Correct Deployment Architecture**

#### **Frontend (Vercel/Netlify) ✅**
```javascript
// Vue 3 Dashboard - Static Site ONLY
Domain: app.whatsappsender.com

Responsibilities:
✅ User authentication (Supabase Auth)
✅ Dashboard UI (Vue 3 + Tailwind)
✅ Campaign management interface
✅ Analytics & reporting
✅ Billing management
✅ Real-time updates (Supabase Realtime)

#### **Backend Server (VPS/Cloud Required) ⚠️**
```javascript
// MANDATORY: Dedicated Server for Bot Operations
Domain: api.whatsappsender.com

Server Requirements:
- VPS: DigitalOcean/Vultr/AWS EC2
- Memory: 4GB+ RAM (untuk multiple browser instances)
- CPU: 2+ cores
- Storage: 50GB+ SSD
- OS: Ubuntu 22.04 LTS

Services Running:
✅ Node.js API server
✅ Browserless containers (Docker)
✅ Playwright bot instances
✅ Redis queue system
✅ Nginx reverse proxy
✅ PM2 process manager
```

#### **Database (Supabase) ✅**
```javascript
// Supabase Cloud - Fully Managed
Services Used:
✅ PostgreSQL database
✅ Real-time subscriptions
✅ Authentication
✅ Storage (untuk media files)
✅ Edge functions (optional)
```

#### **Complete Infrastructure Cost:**
```javascript
Monthly Costs:
- Frontend (Vercel): $0 - $20/month
- Backend VPS: $20 - $100/month (depending on scale)
- Database (Supabase): $0 - $25/month
- Proxy-Seller: $35.40/month (20 proxies)
- Total: $55.40 - $145.40/month

Revenue Potential:
- 50 customers x $99/month = $4,950/month
- Profit: $4,804.60 - $4,894.60/month
- ROI: 3,400%+ 🚀
```

## Spesifikasi Sistem / System Specifications

### 1. WhatsApp Bot Multi-Instance Sender
**🇮🇩 Fitur Utama:**
- **Multi-user bot instances** dengan Browserless + Playwright
- **Stealth mode sender** yang sulit dideteksi WhatsApp
- **Real browser simulation** untuk bypass anti-bot detection
- **Bulk message sender** dengan rotating IP & user agents
- **Contact import** dari CSV/Excel/Google Contacts
- **Message templates** dengan personalisasi
- **Scheduled broadcasting** dengan proxy rotation
- **Media sender** (gambar, video, dokumen, audio)
- **Session persistence** dengan browser state management
- **Auto-delay** dan human-like behavior simulation

**🇺🇸 Main Features:**
- **Multi-user bot instances** with Browserless + Playwright
- **Stealth mode sender** that's hard to detect by WhatsApp
- **Real browser simulation** to bypass anti-bot detection
- **Bulk message sender** with rotating IP & user agents
- **Contact import** from CSV/Excel/Google Contacts
- **Message templates** with personalization
- **Scheduled broadcasting** with proxy rotation
- **Media sender** (images, videos, documents, audio)
- **Session persistence** with browser state management
- **Auto-delay** and human-like behavior simulation

### 2. Web Dashboard Management
**🇮🇩 Dashboard Fitur:**
- **Multi-bahasa** (Indonesia & English)
- **Bot management**: Start/stop, status monitoring, QR scan
- **Contact management** dengan segmentasi
- **Campaign builder** untuk broadcast
- **Template message** designer
- **Analytics dashboard** dengan reporting
- **Billing system** dengan Rupiah & Dollar

**🇺🇸 Dashboard Features:**
- **Multi-language** (Indonesia & English)
- **Bot management**: Start/stop, status monitoring, QR scan
- **Contact management** with segmentation
- **Campaign builder** for broadcasting
- **Template message** designer
- **Analytics dashboard** with reporting
- **Billing system** with Rupiah & Dollar

## Database Schema (Supabase)

```sql
-- Users/Pengguna table
CREATE TABLE users (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  full_name TEXT NOT NULL,
  phone TEXT,
  business_name TEXT,
  country TEXT DEFAULT 'ID', -- ID, US, etc
  preferred_language TEXT DEFAULT 'id', -- id, en
  preferred_currency TEXT DEFAULT 'IDR', -- IDR, USD
  subscription_plan TEXT DEFAULT 'free',
  billing_cycle TEXT DEFAULT 'monthly', -- monthly, 6-month, annual
  subscription_started_at TIMESTAMP,
  subscription_expires_at TIMESTAMP,
  subscription_price DECIMAL(12,2),
  subscription_currency TEXT DEFAULT 'IDR',
  discount_applied DECIMAL(5,2) DEFAULT 0,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- WhatsApp Bot Sender Instances (Browserless + Playwright)
CREATE TABLE whatsapp_instances (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  instance_name TEXT NOT NULL,
  phone_number TEXT,
  browser_session_id TEXT UNIQUE,
  browserless_endpoint TEXT,
  proxy_config JSONB, -- proxy rotation settings
  user_agent TEXT,
  browser_profile JSONB, -- fingerprint, viewport, etc
  status TEXT DEFAULT 'disconnected', -- connected, disconnected, qr_needed, banned, suspended
  session_data JSONB, -- cookies, localStorage, sessionStorage
  last_activity TIMESTAMP,
  sender_settings JSONB, -- delay settings, safety configs, stealth options
  daily_limit INTEGER DEFAULT 1000,
  sent_today INTEGER DEFAULT 0,
  last_reset_date DATE DEFAULT CURRENT_DATE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Contact Lists & Management
CREATE TABLE contact_lists (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  list_name TEXT NOT NULL,
  description TEXT,
  total_contacts INTEGER DEFAULT 0,
  tags TEXT[], -- marketing, customer, prospect, vip
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE contacts (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  list_id UUID REFERENCES contact_lists(id) ON DELETE CASCADE,
  whatsapp_number TEXT NOT NULL,
  contact_name TEXT,
  first_name TEXT,
  last_name TEXT,
  email TEXT,
  company TEXT,
  tags TEXT[],
  custom_fields JSONB,
  is_active BOOLEAN DEFAULT true,
  last_contacted TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Message Templates
CREATE TABLE message_templates (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  template_name TEXT NOT NULL,
  template_category TEXT NOT NULL, -- marketing, greeting, promotion, announcement
  message_content TEXT NOT NULL,
  message_content_en TEXT, -- English version
  variables JSONB, -- {name}, {company}, {product} etc
  media_urls TEXT[],
  is_active BOOLEAN DEFAULT true,
  usage_count INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Broadcast Campaigns
CREATE TABLE broadcast_campaigns (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  campaign_name TEXT NOT NULL,
  campaign_description TEXT,
  template_id UUID REFERENCES message_templates(id),
  target_lists UUID[], -- array of contact_list ids
  target_contacts UUID[], -- specific contacts
  schedule_type TEXT DEFAULT 'immediate', -- immediate, scheduled, recurring
  scheduled_at TIMESTAMP,
  timezone TEXT DEFAULT 'Asia/Jakarta',
  status TEXT DEFAULT 'draft', -- draft, scheduled, running, paused, completed, cancelled
  total_recipients INTEGER DEFAULT 0,
  sent_count INTEGER DEFAULT 0,
  delivered_count INTEGER DEFAULT 0,
  failed_count INTEGER DEFAULT 0,
  sender_settings JSONB, -- delay, randomization settings
  created_at TIMESTAMP DEFAULT NOW(),
  started_at TIMESTAMP,
  completed_at TIMESTAMP
);

-- Campaign Messages Log
CREATE TABLE campaign_messages (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  campaign_id UUID REFERENCES broadcast_campaigns(id) ON DELETE CASCADE,
  contact_id UUID REFERENCES contacts(id),
  instance_id UUID REFERENCES whatsapp_instances(id),
  message_content TEXT,
  media_urls TEXT[],
  status TEXT DEFAULT 'pending', -- pending, sent, delivered, failed, blocked
  error_message TEXT,
  sent_at TIMESTAMP,
  delivered_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Usage Tracking & Analytics
CREATE TABLE usage_logs (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  instance_id UUID REFERENCES whatsapp_instances(id),
  action_type TEXT NOT NULL, -- message_sent, broadcast_sent, contact_imported
  quantity INTEGER DEFAULT 1,
  campaign_id UUID REFERENCES broadcast_campaigns(id),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Subscription Plans & Pricing
CREATE TABLE subscription_plans (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  plan_code TEXT UNIQUE NOT NULL, -- free, starter, business, enterprise
  plan_name_id TEXT NOT NULL, -- Indonesian name
  plan_name_en TEXT NOT NULL, -- English name
  price_idr DECIMAL(12,2) NOT NULL,
  price_usd DECIMAL(10,2) NOT NULL,
  billing_cycle TEXT NOT NULL, -- monthly, 6-month, annual
  features JSONB NOT NULL, -- max_instances, max_messages, etc
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Payment Transactions
CREATE TABLE payment_transactions (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  plan_id UUID REFERENCES subscription_plans(id),
  transaction_id TEXT UNIQUE,
  amount DECIMAL(12,2) NOT NULL,
  currency TEXT NOT NULL, -- IDR, USD
  payment_method TEXT, -- credit_card, bank_transfer, e_wallet, paypal
  payment_gateway TEXT, -- midtrans, xendit, stripe, paypal
  status TEXT DEFAULT 'pending', -- pending, paid, failed, refunded
  gateway_response JSONB,
  invoice_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  paid_at TIMESTAMP
);

-- Multi-language Content
CREATE TABLE translations (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  translation_key TEXT NOT NULL,
  language_code TEXT NOT NULL, -- id, en
  translation_value TEXT NOT NULL,
  context TEXT, -- ui, email, sms, etc
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(translation_key, language_code)
);
```

## Pricing Plans (Multi-Currency)

### 1. **FREE PLAN / PAKET GRATIS**
**🇮🇩 Indonesia:**
- **Harga**: Gratis
- 1 WhatsApp instance
- 50 pesan/hari
- 100 kontak
- 3 template pesan
- Analytics dasar

**🇺🇸 International:**
- **Price**: Free
- 1 WhatsApp instance  
- 50 messages/day
- 100 contacts
- 3 message templates
- Basic analytics

### 2. **STARTER PLAN / PAKET STARTER**
**🇮🇩 Indonesia:**
```
Bulanan: Rp 299,000/bulan
6 Bulan: Rp 1,495,000 (Rp 249,167/bulan) - Hemat 17%
Tahunan: Rp 2,691,000 (Rp 224,250/bulan) - Hemat 25%

Fitur:
- 3 WhatsApp instances
- 1,000 pesan/hari
- 5,000 kontak
- Template pesan unlimited
- Penjadwalan broadcast
- Import kontak CSV/Excel
- Analytics lanjutan
- Support email
```

**🇺🇸 International:**
```
Monthly: $19/month
6-Month: $95 ($15.83/month) - Save 17%
Annual: $171 ($14.25/month) - Save 25%

Features:
- 3 WhatsApp instances
- 1,000 messages/day
- 5,000 contacts
- Unlimited message templates
- Broadcast scheduling
- CSV/Excel contact import
- Advanced analytics
- Email support
```

### 3. **BUSINESS PLAN / PAKET BISNIS** (Most Popular)
**🇮🇩 Indonesia:**
```
Bulanan: Rp 799,000/bulan
6 Bulan: Rp 3,995,000 (Rp 665,833/bulan) - Hemat 17%
Tahunan: Rp 7,191,000 (Rp 599,250/bulan) - Hemat 25%

Fitur:
- 10 WhatsApp instances
- 10,000 pesan/hari
- 50,000 kontak
- Campaign automation
- Personalisasi pesan advanced
- Multi-media sender
- Analytics & reporting detail
- API access
- Priority support
- White-label option
```

**🇺🇸 International:**
```
Monthly: $49/month
6-Month: $245 ($40.83/month) - Save 17%
Annual: $441 ($36.75/month) - Save 25%

Features:
- 10 WhatsApp instances
- 10,000 messages/day
- 50,000 contacts
- Campaign automation
- Advanced message personalization
- Multi-media sender
- Detailed analytics & reporting
- API access
- Priority support
- White-label option
```

### 4. **ENTERPRISE PLAN / PAKET ENTERPRISE**
**🇮🇩 Indonesia:**
```
Bulanan: Rp 1,599,000/bulan
6 Bulan: Rp 7,995,000 (Rp 1,332,500/bulan) - Hemat 17%
Tahunan: Rp 14,391,000 (Rp 1,199,250/bulan) - Hemat 25%

Fitur:
- Unlimited instances
- Unlimited pesan/hari
- Unlimited kontak
- Custom integrations
- Dedicated account manager
- On-premise deployment
- SLA guarantee
- Training & onboarding
- Custom development
- Multi-user dashboard
```

**🇺🇸 International:**
```
Monthly: $99/month
6-Month: $495 ($82.50/month) - Save 17%
Annual: $891 ($74.25/month) - Save 25%

Features:
- Unlimited instances
- Unlimited messages/day
- Unlimited contacts
- Custom integrations
- Dedicated account manager
- On-premise deployment
- SLA guarantee
- Training & onboarding
- Custom development
- Multi-user dashboard
```

## Bot Sender Features Yang Harus Diimplementasikan

### 1. **Core Sending Features dengan Playwright**
**🇮🇩 Fitur Inti:**
```javascript
// Browserless + Playwright Sender
- Real browser automation dengan stealth mode
- Multiple browser instances per user
- Proxy rotation untuk IP diversity
- User agent randomization
- Browser fingerprint management
- Session persistence dengan browser state
- Human-like behavior simulation (mouse movement, typing delays)
- Anti-detection mechanisms
- Captcha handling automation

// Bulk Message Sender
- Import kontak dari CSV/Excel/Google Contacts
- Personalisasi pesan dengan variable {nama}, {perusahaan}
- Queue management dengan Playwright browser pool
- Retry mechanism dengan different browser sessions
- Real-time sending progress dengan browser screenshots
- Error handling dengan browser console logs
```

**🇺🇸 Core Features:**
```javascript
// Browserless + Playwright Sender
- Real browser automation with stealth mode
- Multiple browser instances per user
- Proxy rotation for IP diversity
- User agent randomization
- Browser fingerprint management
- Session persistence with browser state
- Human-like behavior simulation (mouse movement, typing delays)
- Anti-detection mechanisms
- Captcha handling automation

// Bulk Message Sender
- Import contacts from CSV/Excel/Google Contacts
- Message personalization with variables {name}, {company}
- Queue management with Playwright browser pool
- Retry mechanism with different browser sessions
- Real-time sending progress with browser screenshots
- Error handling with browser console logs
```

### 2. **Advanced Playwright Features**
```javascript
// Stealth & Anti-Detection
- Browser fingerprint randomization
- Viewport size randomization
- Timezone spoofing
- WebRTC leak protection
- Canvas fingerprint blocking
- Audio context fingerprint blocking
- Screen resolution spoofing
- Language header randomization

// Session Management
- Browser context isolation per user
- Session storage persistence
- Cookie management
- Local storage backup/restore
- Browser profile switching
- Automated session refresh

// Performance & Scaling
- Browser pool management
- Resource optimization (disable images/css for speed)
- Memory management
- Concurrent browser instances
- Load balancing across Browserless instances
- Health checking for browser instances
```

### 3. **Browserless Integration**
```javascript
// Browserless Setup
- Docker containerization
- Horizontal scaling
- Resource limits per container
- Browser session monitoring
- Automatic browser cleanup
- Performance metrics collection

// Multi-Instance Architecture
- Dedicated browser per WhatsApp account
- Isolated browser contexts
- Proxy rotation per instance
- Failover mechanism
- Load distribution
```

### 3. **Multi-Language Dashboard**

#### A. **Indonesian Interface**
```vue
// Main Navigation
- Dasbor
- Kelola Bot
- Daftar Kontak
- Template Pesan
- Kampanye Broadcast
- Analitik
- Pengaturan
- Berlangganan

// Campaign Builder
- Buat Kampanye Baru
- Pilih Template
- Pilih Penerima
- Jadwalkan Pengiriman
- Pratinjau & Kirim
```

#### B. **English Interface**
```vue
// Main Navigation
- Dashboard
- Manage Bots
- Contact Lists
- Message Templates
- Broadcast Campaigns
- Analytics
- Settings
- Subscription

// Campaign Builder
- Create New Campaign
- Select Template
- Choose Recipients
- Schedule Delivery
- Preview & Send
```

## Frontend Structure (Vue 3)

```
src/
├── components/
│   ├── sender/           # Bot sender components
│   ├── contacts/         # Contact management
│   ├── templates/        # Message templates
│   ├── campaigns/        # Campaign management
│   ├── analytics/        # Analytics dashboard
│   ├── billing/          # Subscription & billing
│   └── ui/              # Reusable UI components
├── composables/
│   ├── useWhatsAppSender.js  # Bot sender operations
│   ├── useContacts.js        # Contact management
│   ├── useCampaigns.js       # Campaign operations
│   ├── useAnalytics.js       # Analytics data
│   └── useI18n.js           # Multi-language
├── stores/
│   ├── auth.js          # Authentication
│   ├── sender.js        # Bot instances & sending
│   ├── contacts.js      # Contact management
│   ├── campaigns.js     # Campaign data
│   ├── billing.js       # Subscription & payments
│   └── ui.js           # UI state & language
├── locales/
│   ├── id.json         # Indonesian translations
│   └── en.json         # English translations
└── views/
    ├── Dashboard.vue    # Main dashboard (bilingual)
    ├── BotSender.vue    # Bot management
    ├── ContactManager.vue
    ├── TemplateBuilder.vue
    ├── CampaignCenter.vue
    ├── Analytics.vue
    ├── Billing.vue
    └── Settings.vue
```

## Backend Services

```
api/
├── whatsapp/
│   ├── instances.js     # Browser instance management
│   ├── playwright.js    # Playwright automation scripts
│   ├── sender.js        # Message sending with browser automation
│   ├── queue.js         # Browser pool & queue management
│   ├── stealth.js       # Anti-detection & fingerprinting
│   └── session.js       # Browser session persistence
├── browserless/
│   ├── pool.js          # Browser pool management
│   ├── proxy.js         # Proxy rotation management
│   ├── monitoring.js    # Instance health monitoring
│   └── scaling.js       # Auto-scaling logic
├── contacts/
│   ├── import.js        # CSV/Excel import
│   ├── management.js    # CRUD operations
│   ├── segmentation.js  # Contact segmentation
│   └── validation.js    # Phone validation
├── campaigns/
│   ├── builder.js       # Campaign creation
│   ├── scheduler.js     # Scheduled sending with browser automation
│   ├── analytics.js     # Campaign tracking
│   └── templates.js     # Template management
├── billing/
│   ├── plans.js         # Subscription plans
│   ├── payments.js      # Payment processing
│   ├── currency.js      # Multi-currency handling
│   └── invoices.js      # Invoice generation
└── i18n/
    ├── translations.js  # Translation management
    └── localization.js  # Content localization
```

## Payment Gateway Integration

### **Indonesia Payment Methods**
```javascript
// Midtrans Integration
- Credit/Debit Card
- Bank Transfer (BCA, BNI, BRI, Mandiri)
- E-wallet (OVO, GoPay, Dana, LinkAja)
- Convenience Store (Alfamart, Indomaret)
- QRIS

// Xendit Integration  
- Virtual Account
- Retail Outlets
- Credit Card
- E-wallet
```

### **International Payment Methods**
```javascript
// Stripe Integration
- Credit/Debit Card
- PayPal
- Apple Pay
- Google Pay
- Bank Transfer

// PayPal Direct
- PayPal Balance
- Credit Card via PayPal
- Bank Account
```

## Prompt untuk Implementation

**🇮🇩 Tolong generate kode lengkap untuk:**

### **Recommended VPS Setup:**

#### **Budget Option ($20/month):**
```javascript
// DigitalOcean Basic Droplet
- 2 CPU cores
- 4GB RAM  
- 80GB SSD
- Ubuntu 22.04
- Suitable for: 5-10 concurrent bots

// Setup included:
- Docker & Docker Compose
- Nginx reverse proxy  
- SSL certificate (Let's Encrypt)
- PM2 process manager
- Monitoring tools
```

#### **Production Option ($50/month):**
```javascript
// DigitalOcean Premium Droplet  
- 4 CPU cores
- 8GB RAM
- 160GB SSD
- Ubuntu 22.04
- Suitable for: 20-50 concurrent bots

// Additional features:
- Load balancing
- Auto-scaling
- Backup system
- Monitoring & alerts
```

#### **Enterprise Option ($100+/month):**
```javascript
// Multiple VPS or Cloud Infrastructure
- Auto-scaling groups
- Load balancers
- Multiple regions
- Disaster recovery
- Suitable for: 100+ concurrent bots
```

## Proxy Integration System

**🇺🇸 Please generate complete code for:**

### Technical Requirements:
- **Multi-tenancy**: Data isolation per user
- **Scalability**: Handle 10,000+ concurrent campaigns
- **Reliability**: Auto-retry, error handling, failover
- **Performance**: Queue optimization, database indexing
- **Security**: Input validation, rate limiting, encryption
- **Compliance**: GDPR ready, opt-out mechanisms

### **Why VPS is MANDATORY for WhatsApp Bot:**

#### **A. Browser Session Requirements:**
```javascript
// WhatsApp Web Login Process:
1. Open browser with specific fingerprint
2. Navigate to https://web.whatsapp.com  
3. Scan QR code (or restore session)
4. Maintain persistent connection
5. Keep session alive 24/7
6. Handle reconnections automatically

// This CANNOT work on Vercel because:
❌ Browser closes after 60 seconds max
❌ No persistent memory for session data
❌ Cold starts restart everything
❌ No access to browser DevTools Protocol
❌ Cannot maintain WebSocket connections
```

#### **B. Browserless Requirements:**
```javascript
// Browserless Docker Container needs:
- Persistent browser instances
- Memory allocation (1GB+ per browser)
- CPU resources for rendering
- File system for downloads/uploads
- Network access for proxy connections
- Process management for scaling

// VPS provides:
✅ 24/7 uptime
✅ Persistent storage
✅ Full system access
✅ Docker support
✅ Resource allocation
✅ Process management
```

#### **C. Queue System Requirements:**
```javascript
// Message Queue for Bulk Sending:
- Redis/Bull Queue for job processing
- Background workers
- Retry mechanisms
- Rate limiting
- Job scheduling
- Progress tracking

// Needs persistent processes that Vercel cannot provide
```

### **A. Proxy-Seller.com Integration**
```javascript
// proxySeller.js - Auto-integration with Proxy-Seller
class ProxySellerManager {
  constructor(apiKey, username, password) {
    this.apiKey = apiKey;
    this.auth = { username, password };
    this.baseUrl = 'https://proxy-seller.com/api';
    this.proxies = [];
  }

  async fetchProxyList() {
    // Get proxy list from Proxy-Seller API
    const response = await fetch(`${this.baseUrl}/proxies`, {
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json'
      }
    });
    
    this.proxies = await response.json();
    return this.proxies;
  }

  async getRandomProxy(location = 'any') {
    if (this.proxies.length === 0) {
      await this.fetchProxyList();
    }

    let availableProxies = this.proxies.filter(proxy => 
      proxy.status === 'active' && 
      (location === 'any' || proxy.location.includes(location))
    );

    const randomProxy = availableProxies[
      Math.floor(Math.random() * availableProxies.length)
    ];

    return {
      server: `http://${randomProxy.ip}:${randomProxy.port}`,
      username: randomProxy.username || this.auth.username,
      password: randomProxy.password || this.auth.password,
      location: randomProxy.location,
      type: 'datacenter'
    };
  }

  async testProxyHealth(proxy) {
    try {
      const testResponse = await fetch('https://httpbin.org/ip', {
        method: 'GET',
        proxy: proxy.server,
        timeout: 10000
      });
      
      return {
        working: testResponse.ok,
        ip: await testResponse.json(),
        latency: Date.now() - startTime
      };
    } catch (error) {
      return { working: false, error: error.message };
    }
  }
}

// Usage dalam WhatsApp sender:
const proxyManager = new ProxySellerManager(
  process.env.PROXY_SELLER_API_KEY,
  process.env.PROXY_USERNAME, 
  process.env.PROXY_PASSWORD
);

// Auto-rotate proxy setiap session
async function createSessionWithProxy() {
  const proxy = await proxyManager.getRandomProxy('Jakarta');
  
  const context = await browser.newContext({
    proxy: {
      server: proxy.server,
      username: proxy.username,
      password: proxy.password
    },
    userAgent: getRandomUserAgent(),
    viewport: getRandomViewport()
  });

  return context;
}
```

### **B. Dashboard Proxy Management**
```vue
<!-- ProxyManagement.vue -->
<template>
  <div class="proxy-management">
    <!-- Proxy Provider Setup -->
    <div class="proxy-provider-setup">
      <h3>{{ $t('proxy.setup_title') }}</h3>
      
      <div class="provider-selection">
        <label>
          <input type="radio" v-model="selectedProvider" value="proxy-seller">
          Proxy-Seller.com
        </label>
        <label>
          <input type="radio" v-model="selectedProvider" value="bright-data"> 
          Bright Data
        </label>
        <label>
          <input type="radio" v-model="selectedProvider" value="custom">
          Custom VPS
        </label>
      </div>

      <!-- Proxy-Seller Configuration -->
      <div v-if="selectedProvider === 'proxy-seller'" class="proxy-config">
        <input 
          v-model="proxyConfig.apiKey" 
          placeholder="Proxy-Seller API Key"
          type="password"
        >
        <input 
          v-model="proxyConfig.username" 
          placeholder="Proxy Username"
        >
        <input 
          v-model="proxyConfig.password" 
          placeholder="Proxy Password" 
          type="password"
        >
        <button @click="testProxyConnection">
          {{ $t('proxy.test_connection') }}
        </button>
      </div>
    </div>

    <!-- Proxy Pool Status -->
    <div class="proxy-pool-status">
      <h3>{{ $t('proxy.pool_status') }}</h3>
      
      <div class="proxy-grid">
        <div 
          v-for="proxy in proxyPool" 
          :key="proxy.id"
          class="proxy-card"
          :class="{ 'healthy': proxy.status === 'healthy', 'error': proxy.status === 'error' }"
        >
          <div class="proxy-info">
            <span class="proxy-ip">{{ proxy.ip }}</span>
            <span class="proxy-location">{{ proxy.location }}</span>
            <span class="proxy-latency">{{ proxy.latency }}ms</span>
          </div>
          <div class="proxy-stats">
            <span>{{ $t('proxy.usage') }}: {{ proxy.usage_today }}</span>
            <span>{{ $t('proxy.success_rate') }}: {{ proxy.success_rate }}%</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Cost Analytics -->
    <div class="proxy-cost-analytics">
      <h3>{{ $t('proxy.cost_analytics') }}</h3>
      
      <div class="cost-breakdown">
        <div class="cost-item">
          <span>{{ $t('proxy.monthly_cost') }}</span>
          <span>${{ totalProxyCost.toFixed(2) }}</span>
        </div>
        <div class="cost-item">
          <span>{{ $t('proxy.cost_per_message') }}</span>
          <span>${{ (totalProxyCost / totalMessagesSent).toFixed(4) }}</span>
        </div>
        <div class="cost-item">
          <span>{{ $t('proxy.roi') }}</span>
          <span>{{ ((totalRevenue - totalProxyCost) / totalProxyCost * 100).toFixed(1) }}%</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useProxyStore } from '@/stores/proxy';

const proxyStore = useProxyStore();
const selectedProvider = ref('proxy-seller');
const proxyConfig = ref({
  apiKey: '',
  username: '',
  password: ''
});

const proxyPool = ref([]);
const totalProxyCost = ref(0);
const totalMessagesSent = ref(0);
const totalRevenue = ref(0);

async function testProxyConnection() {
  try {
    const result = await proxyStore.testConnection(proxyConfig.value);
    if (result.success) {
      alert('Proxy connection successful!');
      await loadProxyPool();
    } else {
      alert('Proxy connection failed: ' + result.error);
    }
  } catch (error) {
    alert('Error testing proxy: ' + error.message);
  }
}

async function loadProxyPool() {
  proxyPool.value = await proxyStore.getProxyPool();
  totalProxyCost.value = proxyStore.getTotalCost();
  totalMessagesSent.value = proxyStore.getTotalMessages();
  totalRevenue.value = proxyStore.getTotalRevenue();
}

onMounted(() => {
  loadProxyPool();
});
</script>
```

### **C. Smart Proxy Routing System**
```javascript
// smartProxyRouter.js
class SmartProxyRouter {
  constructor(proxyManager) {
    this.proxyManager = proxyManager;
    this.routingRules = {
      // Route berdasarkan customer tier
      premium: { 
        provider: 'residential', 
        rotation_interval: 50, // messages
        cost_limit: 10 // USD per day
      },
      business: { 
        provider: 'datacenter', 
        rotation_interval: 100,
        cost_limit: 5
      },
      free: { 
        provider: 'datacenter', 
        rotation_interval: 200,
        cost_limit: 1
      }
    };
  }

  async routeProxy(customer, messageCount) {
    const tier = customer.subscription_plan;
    const rule = this.routingRules[tier] || this.routingRules.free;
    
    // Check if need rotation
    if (messageCount % rule.rotation_interval === 0) {
      console.log(`🔄 Rotating proxy for ${customer.business_name} (${tier} tier)`);
      
      // Get appropriate proxy based on tier
      if (tier === 'premium' && rule.provider === 'residential') {
        return await this.getResidentialProxy();
      } else {
        return await this.proxyManager.getRandomProxy();
      }
    }
    
    return null; // Keep current proxy
  }

  async getResidentialProxy() {
    // Fallback to residential proxy service if available
    return await this.proxyManager.getRandomProxy('residential');
  }

  calculateCostPerMessage(proxyType, location) {
    const costs = {
      datacenter: 0.001, // $0.001 per message
      residential: 0.01   // $0.01 per message
    };
    
    return costs[proxyType] || costs.datacenter;
  }
}
```

**Target**: Production-ready WhatsApp bulk sender platform menggunakan **Browserless + Playwright** untuk maksimum stealth, reliability, dan scalability untuk marketing agencies, UMKM, dan enterprise dengan support multi-bahasa dan multi-mata uang.
