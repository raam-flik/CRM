# FLIK Chat Commerce MVP - Complete Implementation Plan

**Status:** Ready for Implementation  
**Version:** 3.0 (Granular Automation + Flexible Configuration)  
**Date:** October 2026  
**Scope:** 5 Products Only (Chat Commerce, Customer, Products, Invoice, Payment Link)

---

## EXECUTIVE SUMMARY

FLIK is building a **Chat Commerce platform** enabling merchants to:
1. Chat with customers on WhatsApp
2. Create orders directly in conversations
3. Generate and send payments in-chat
4. Manage all business data from one unified system

**Key Innovation:** Merchants choose their **automation level per feature** - from full manual control to AI-driven automation.

---

## 5-PRODUCT SCOPE

### 1. **Commerce Chat (Conversations)**
- WhatsApp message inbox
- Chat threads with customer history
- AI suggestions panel (configurable)
- Quick actions: reply, share product, create invoice

### 2. **Customer Management**
- Customer list view
- Conversation-linked customer profiles
- Manual customer creation (always available)
- Full customer journey tracking

### 3. **Product Catalog**
- Product library with images, prices, stock
- Quick-share to conversations
- Performance tracking per product
- Recommendation engine

### 4. **Invoice (Redesigned)**
- Separate from checkout (dedicated page)
- Real invoice look + PDF + Email
- Auto-fill from chat context (configurable)
- Optional shipping (merchant choice)

### 5. **Payment Link**
- Auto-generates with invoice (configurable)
- Chat-friendly action buttons
- Personalized payment page
- Full tracking and status updates

---

## AUTOMATION CONFIGURATION (Granular)

Merchants configure **each feature independently** inside Chat Commerce settings:

```
⚙️ AUTOMATION SETTINGS (In Chat Commerce Page)

INVOICE CREATION
├─ Auto-detect purchase intent: [OFF/ON]
├─ Auto-fill from chat context: [OFF/ON]
└─ Auto-create invoice: [OFF/ON]

PAYMENT LINK
├─ Auto-generate with invoice: [OFF/ON]
├─ Auto-send to customer: [OFF/ON]
└─ Auto-update conversation: [OFF/ON]

AI ASSISTANCE
├─ Suggest products: [OFF/ON]
├─ Suggest discount offers: [OFF/ON]
├─ Suggest response templates: [OFF/ON]
└─ Prioritize urgent messages: [OFF/ON]

DEFAULT: All OFF (Safe mode for MVP)
MERCHANT: Enables features as confidence grows
```

---

## NAVIGATION STRUCTURE

**5 Separate First-Level Sections:**

```
FLIK Dashboard
├─ 💬 CONVERSATIONS (Chat Commerce)
│  ├─ Inbox (message list)
│  ├─ Chat thread (when clicked)
│  ├─ ⚙️ Automation Settings (in this section)
│  └─ Context panel (customer + AI suggestions)
│
├─ 👥 CUSTOMERS (Management)
│  ├─ Customer list
│  ├─ [+ Create Customer] (manual, always available)
│  └─ Customer detail page
│
├─ 🛍️ PRODUCTS (Catalog)
│  ├─ Product list
│  ├─ Create/edit products
│  └─ Performance analytics
│
├─ 📄 INVOICE (Redesigned)
│  ├─ Create invoice
│  ├─ Invoice list
│  └─ Invoice detail
│
└─ 💳 PAYMENT LINK (Management)
   ├─ Create payment link
   ├─ Payment link list
   └─ Link analytics
```

---

## MERCHANT WORKFLOWS (3 Scenarios)

### SCENARIO 1: Full Manual (All automation OFF)

```
Customer: "OK saya ambil 2 nasi goreng"
         ↓
Merchant: Sees message in Conversations
         ↓
Merchant: [Create Invoice] (manual button click)
         ↓
Dialog: Auto-fills from chat context (name, products, qty)
       └─ AI assists: fills data from message
         ↓
Merchant: Reviews/edits data
         ↓
Merchant: [Confirm Invoice]
         ↓
Invoice created
         ↓
Merchant: [Send Payment Link] (manual click)
         ↓
Dialog: Generates payment link
       └─ AI assists: customizes based on invoice
         ↓
Merchant: [Confirm & Send]
         ↓
Payment link sent to customer in chat
         ↓
Merchant: Monitors payment status manually
```

---

### SCENARIO 2: Balanced (Mixed automation)

```
Merchant Settings: 
├─ Auto-detect purchase: ON
├─ Auto-fill invoice: ON
├─ Auto-create invoice: OFF
├─ Auto-generate link: ON
├─ Auto-send: OFF

Customer: "OK saya ambil 2 nasi goreng"
         ↓
AI: Detects purchase intent + extracts: 2x Nasi Goreng
         ↓
System: Shows notification in Conversations
        "Order detected: 2x Nasi Goreng, Rp 100K
         [Approve & Create] [Edit] [Cancel]"
         ↓
Merchant: [Approve & Create]
         ↓
System: Auto-creates invoice (AI-filled)
        Auto-generates payment link
        Shows: "Ready to send payment link?
               [Send Now] [Edit] [Cancel]"
         ↓
Merchant: [Send Now]
         ↓
Payment link sent to customer
```

---

### SCENARIO 3: Full AI Automation (All automation ON)

```
Merchant Settings: All automation enabled

Customer: "OK saya ambil 2 nasi goreng"
         ↓
AI: Detects intent + extracts data
         ↓
System: Auto-creates invoice
        Auto-generates payment link
        Auto-sends to customer
         ↓
Merchant: Sees notification "Order created ✓ Payment link sent"
         ↓
Merchant: Only intervenes if needed
         ├─ [Edit & resend] (if customer questions price)
         ├─ [Cancel order] (if out of stock)
         └─ [View details] (for record-keeping)
         ↓
Merchant: Monitors payment status
         ↓
When paid: Auto-notification "Payment received ✓"
```

---

## KEY FEATURES

### CONVERSATIONS PAGE (Separate)

```
Left Sidebar: Inbox
├─ Message list (sorted by recency)
├─ Unread badges
├─ Search/filter
└─ Customer names + preview

Main: Chat Thread
├─ Full conversation history
├─ Message timestamps
├─ Input box to reply
└─ Visual distinction: merchant vs customer messages

Right Sidebar: Context Panel
├─ 👤 Customer Profile
│  ├─ Name, phone, email
│  ├─ Total orders, total spend
│  ├─ Last transaction date
│  └─ [View Full Profile]
│
├─ 💡 AI Suggestions
│  ├─ Response template (1-click send)
│  ├─ Product recommendation
│  ├─ Discount suggestion
│  └─ Best time to follow up
│
└─ ⚡ Quick Actions
   ├─ [Create Invoice]
   ├─ [Share Product]
   ├─ [View Customer] 
   └─ [Add Note]
```

---

### AUTOMATION SETTINGS (In Chat Commerce)

```
⚙️ Location: Conversations page > Settings icon

Dashboard shows:
├─ Current automation status
├─ All toggleable features
├─ Descriptions of what each does
├─ Recommended settings
└─ [Save Changes]

Example toggles:
├─ Auto-detect purchase intent from messages
│  └─ "When customer says 'ambil 2', system recognizes order"
│
├─ Auto-fill invoice from chat context
│  └─ "Use customer, product, quantity from conversation"
│
├─ Auto-create invoice
│  └─ "When purchase detected, auto-create invoice immediately"
│
├─ Auto-generate payment link
│  └─ "Payment link auto-generated when invoice created"
│
├─ Auto-send payment link to customer
│  └─ "Send link automatically, don't wait for merchant approval"
│
├─ Auto-update conversation when paid
│  └─ "Post payment confirmation in chat automatically"
│
├─ Suggest products to offer
│  └─ "AI recommends products customer might like"
│
├─ Suggest discount offers
│  └─ "AI suggests discounts based on purchase pattern"
│
├─ Suggest response templates
│  └─ "AI drafts responses to customer questions"
│
└─ Prioritize urgent messages
   └─ "Flag unpaid orders, customer complaints first"
```

---

## PAYMENT LINK AUTO-GENERATION

**Merchant configures:**

```
⚙️ PAYMENT LINK SETTINGS

Auto-generate with invoice:
├─ ○ Always auto-generate
├─ ○ Ask me each time
└─ ○ Never auto-generate (manual only)

DEFAULT: Always auto-generate
MERCHANT: Can change per-transaction if needed
```

---

## CUSTOMER MANUAL CREATION

**Always Available:**

```
In CUSTOMERS section:
├─ Customer list view
├─ [+ Create Customer] button (always visible)
└─ Opens modal dialog:
   ├─ Name *
   ├─ Phone *
   ├─ Email *
   ├─ Company (optional)
   ├─ Address (optional)
   ├─ Preferences (optional)
   └─ [Create]

Also accessible from Conversations:
├─ When new customer messages
├─ Option to create customer quickly
└─ Auto-fills name/phone from WhatsApp contact
```

---

## DATA FLOW & INTEGRATION

```
MERCHANT'S WORKDAY:

Morning:
└─ Open Conversations
   ├─ See new customer messages (priority: unread first)
   ├─ Get AI suggestions (based on settings)
   └─ [Choose action: reply, suggest product, create order]

Customer Transaction:
└─ Customer: "OK saya ambil 2 nasi goreng"
   ├─ [Action depends on merchant's automation settings]
   ├─ Manual: Merchant [Create Invoice] → [Send Payment Link]
   ├─ Balanced: System suggests → Merchant approves
   └─ Full AI: System auto-creates & sends

System Updates:
├─ Invoice created + linked to customer
├─ Payment link generated + linked to invoice
├─ Customer record updated with order
├─ Payment tracked in conversation
└─ Status updates visible in chat

Analytics:
└─ All data flows to dashboard
   ├─ Customer lifetime value
   ├─ Order history
   ├─ Payment patterns
   ├─ Conversation sentiment
   └─ Product performance
```

---

## IMPLEMENTATION TIMELINE

### PHASE 1: Foundation (Weeks 1-4)
- WhatsApp API integration
- Conversations inbox UI
- Customer linking
- Manual invoice creation in chat
- Manual payment link sending

**Deliverable:** Basic chat-to-order workflow (all manual)

---

### PHASE 2: Smart Features (Weeks 5-8)
- AI suggestion engine
- Auto-fill from chat context
- Product recommendations
- Discount suggestions
- Configuration settings UI

**Deliverable:** Merchants can enable automation features

---

### PHASE 3: Advanced Automation (Weeks 9-12)
- Auto-detect purchase intent
- Auto-create invoice
- Auto-generate payment link
- Auto-send to customer
- Auto-update conversation on payment

**Deliverable:** Full automation pipeline available

---

### PHASE 4: Intelligence & Analytics (Weeks 13-16)
- Customer journey analytics
- Product performance tracking
- Automation effectiveness metrics
- Recommendation optimization

**Deliverable:** Merchants see ROI of automation

---

### PHASE 5: Optimization & Scale (Weeks 17+)
- A/B testing framework
- Advanced segments
- Recurring orders
- Team collaboration
- Mobile app

**Deliverable:** Enterprise-ready platform

---

## CRITICAL SUCCESS FACTORS

### Must Complete (Blocking)

1. **WhatsApp Business API Integration**
   - Cost: ~$0.005-0.01/message
   - Timeline: 2-4 weeks for Meta approval
   - Action: Submit application immediately

2. **Customer Data Model**
   - Phone number (required)
   - Status field (for compliance)
   - Preferences (opt-in tracking)
   - Address fields (location targeting)

3. **Invoice-Payment-Customer Linking**
   - Every invoice linked to customer_id
   - Every payment linked to invoice_id
   - Every conversation linked to customer_id
   - Unified customer view across all channels

4. **Automation Configuration System**
   - Settings persist per merchant
   - Each feature independently toggleable
   - Safe defaults (all OFF for MVP)
   - Easy on/off in UI

---

## SUCCESS METRICS

### Adoption
- % of merchants enabling automation (target: 60%+ by month 3)
- Avg features enabled per merchant (target: 3+ by month 6)
- Active Conversations users (target: 80%+ of merchants)

### Performance
- Avg time to create order (target: reduce 80% vs manual)
- Avg response time to customer (target: <5 min)
- Order completion rate (target: 90%+)

### Business
- Revenue per merchant (should increase with efficiency)
- Customer satisfaction (target: 4.5+/5)
- Repeat purchase rate (target: 60%+)

---

## NEXT DOCUMENTS

This plan is supported by:

1. **FLIK_Customer_Commerce_Chat_v3.md**
   - Revised customer product spec
   - Automation configuration details
   - Data model with automation fields

2. **FLIK_Commerce_Chat_Workflows.md**
   - Detailed workflows for all 3 automation levels
   - Step-by-step user journeys
   - Decision trees

3. **FLIK_Implementation_Roadmap.md**
   - Phase breakdown
   - Sprint structure
   - Dependencies
   - Resource allocation

4. **FLIK_Automation_Configuration.md**
   - Settings UI specifications
   - Configuration data model
   - Default values
   - Edge cases

5. **FLIK_Page_Layouts.md**
   - Conversations page layout
   - Customer page layout
   - Invoice page layout
   - Payment link page layout
   - Navigation structure

---

## READY TO BUILD

This plan includes everything needed to start:
- ✅ Clear scope (5 products)
- ✅ User workflows (3 automation levels)
- ✅ Automation configuration (granular control)
- ✅ Data models (supporting automation)
- ✅ Implementation roadmap (5 phases)
- ✅ Success metrics (measurable outcomes)

**Next step:** Detailed implementation plan with specifications for each product.

---

*FLIK Chat Commerce MVP - Version 3.0*  
*Granular Automation + Flexible Configuration*  
*Ready for Development*
