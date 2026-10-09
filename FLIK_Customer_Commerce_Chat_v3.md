# FLIK Customer Product - Commerce Chat v3
## With Flexible Automation Configuration

**Status:** Ready for Implementation  
**Version:** 3.0 (Granular Automation + Flexible Configuration)  
**Date:** October 2026

---

## DOCUMENT PURPOSE

This document describes how the **Customer** product transforms in the Commerce Chat model with **flexible automation configuration**. Customer records are no longer passive lists to manage, but **active conversation partners** with rich interaction history.

Key difference from v2: Merchants can **independently configure automation for each AI feature**, choosing their comfort level from full manual to full AI automation.

---

## 1. PARADIGM SHIFT

### OLD MODEL (Dashboard-First)
```
Customer Page
├─ Search/Filter customer
├─ View transactions
├─ Create customer manually
└─ Export list

Limited engagement with customer record.
```

### NEW MODEL (Commerce Chat-First)
```
CONVERSATIONS INBOX (Primary)
├─ Incoming WhatsApp messages from customers
├─ Chat thread with full conversation history
├─ Customer profile context (purchases, preferences)
├─ Quick actions (Create Order, Share Product, Send Payment Link)
├─ AI insights (configurable suggestions)
└─ ⚙️ Automation Settings (merchant controls every feature)

Customer record is PRIMARY workspace for merchant.
Merchant chooses: Full Manual? Balanced? Full AI? Mix & Match?
```

---

## 2. CURRENT STATE vs. NEW STATE

### CURRENT STATE ❌

**Customer List Page:**
- Search by name or phone
- Filter by Last Transaction and City
- Columns: Name, Company, Phone, Shopper Group, Last Transaction, Total Spend, City
- Metrics: Total Customers (72), Returning (49), Avg Order Value

**Create Customer Form (BROKEN):**
- Name (required)
- Company (optional)
- Email (duplicate - BUG)
- Email (duplicate - BUG)
- NO phone number field (critical gap)

**Data Structure:**
```json
{
  "customer": {
    "id": "string",
    "name": "string",
    "email": "string",
    "company": "string (optional)",
    "total_spend": "number",
    "total_orders": "number"
  }
}
```

---

### NEW STATE ✅

**Conversations Inbox (Primary Interface):**
```
FLIK Dashboard > Conversations

┌─────────────────────────────────────┐
│ INBOX (Unread: 7)                   │
├─────────────────────────────────────┤
│ [Search conversations...]           │
│ Filters: [All ▼] [Unread ▼] [Today ▼] │
│ ⚙️ Settings                         │
│                                     │
│ 🟢 Satria                           │
│    "OK saya ambil 2..."             │
│    Now | Unread                     │
│                                     │
│ Brisa Parisian                      │
│    "Berapa ongkir ke Jakarta?"      │
│    2 hours ago | Read               │
│                                     │
│ 🟡 Hani                             │
│    "Masih ada stok?"                │
│    5 hours ago | Read               │
│                                     │
│ Berta Glover                        │
│    "Terima kasih pesanannya..."     │
│    Yesterday | Read                 │
│                                     │
│ [Load More]                         │
└─────────────────────────────────────┘
```

**Conversation Thread (Click on customer):**
```
CONVERSATION WITH SATRIA
═════════════════════════════════════

Today, 2:45 PM
────────────────────────────────────

Satria: "Halo, apakah masih buka?"

2:46 PM - Merchant: "Halo Satria! Kami buka sampai jam 9 malam. Ada yang bisa kami bantu?"

Satria: "Ada nasi goreng?"

2:47 PM - Merchant: "Ya, kami ada! Mau lihat?"
[+ Share Nasi Goreng Product]

Satria: "Berapa harganya?"

2:48 PM - Merchant: "Rp 50,000 per porsi. Stok 50 unit tersedia hari ini"

Satria: "OK saya ambil 2"

2:50 PM - Merchant:
[Workflow depends on automation settings...]
```

**New Customer Data Model:**
```json
{
  "customer": {
    "id": "string",
    "name": "string",
    "phone": "string (required)", // ✅ NOW REQUIRED
    "email": "string (required)", // ✅ NOT DUPLICATED
    "company": "string (optional)",
    "address": {
      "street": "string (optional)",
      "province": "string (optional)",
      "postal_code": "string (optional)"
    },
    "status": "active | inactive | blocked", // ✅ NEW
    "preferences": {
      "whatsapp_optin": "yes | no | pending", // ✅ NEW
      "email_optin": "yes | no | pending", // ✅ NEW
      "preferred_channel": "whatsapp | email | both", // ✅ NEW
      "do_not_contact_times": ["19:00-07:00"] // ✅ NEW
    },
    "conversation_data": {
      "total_messages": "number", // ✅ NEW
      "last_message_at": "datetime", // ✅ NEW
      "response_time_avg": "minutes", // ✅ NEW
      "sentiment": "positive | neutral | negative", // ✅ NEW
      "preferred_topics": ["nasi goreng", "drinks"] // ✅ NEW
    },
    "transaction_data": {
      "total_orders": "number",
      "total_spend": "number",
      "average_order_value": "number",
      "last_purchase_date": "datetime",
      "purchase_frequency": "every_X_days",
      "repeat_customer": "boolean"
    },
    "interaction_patterns": {
      "peak_activity_time": "19:00-21:00", // ✅ NEW
      "response_rate": "percentage", // ✅ NEW
      "bargaining_tendency": "high | medium | low", // ✅ NEW
      "order_confirmation_rate": "percentage" // ✅ NEW
    },
    "intelligence": {
      "customer_lifetime_value": "number", // ✅ NEW
      "predicted_ltv": "number", // ✅ NEW
      "churn_risk": "high | medium | low", // ✅ NEW
      "segment": "repeat | high_value | at_risk | new", // ✅ NEW
      "recommended_products": ["product_id"] // ✅ NEW
    },
    "notes": "string (merchant internal notes)", // ✅ NEW
    "tags": ["VIP", "bulk_buyer", "negotiator"], // ✅ NEW
    "created_at": "datetime",
    "last_interaction_at": "datetime"
  }
}
```

---

## 3. KEY CHANGES FOR COMMERCE CHAT v3

### CHANGE 1: Conversations Become Primary Interface

**Before:**
- Customer page = list of names to search
- Limited engagement

**After:**
- Conversations inbox = active messaging interface
- Customers organized by recency, unread status
- Each customer is a conversation thread
- ⚙️ Settings button to configure automation

**Why:**
- Mirrors how merchants naturally work (in WhatsApp)
- Higher engagement with customer data
- Natural place to take action (reply, create order)
- Merchant has full control over automation level

---

### CHANGE 2: Automation Configuration (NEW in v3)

**Merchants configure each feature independently:**

```
⚙️ AUTOMATION SETTINGS (In Conversations Page)

INVOICE CREATION
├─ Auto-detect purchase intent from messages: [OFF/ON]
├─ Auto-fill invoice from chat context: [OFF/ON]
└─ Auto-create invoice when purchase detected: [OFF/ON]

PAYMENT LINK GENERATION
├─ Auto-generate payment link with invoice: [OFF/ON]
├─ Auto-send payment link to customer: [OFF/ON]
└─ Auto-update conversation when paid: [OFF/ON]

AI ASSISTANCE
├─ Suggest products to offer: [OFF/ON]
├─ Suggest discount offers: [OFF/ON]
├─ Suggest response templates: [OFF/ON]
└─ Prioritize urgent messages: [OFF/ON]

DEFAULT: All OFF (Safe mode for MVP)
MERCHANT: Enables features as confidence grows
```

**Why this matters:**
- Merchants have different risk tolerances
- Different merchants need different features
- AI automation adoption is gradual (not all-at-once)
- Merchant maintains control and confidence

---

### CHANGE 3: Customer Data Enriched by Conversation

**Before:**
- Customer has: Name, Email, Company, Last Transaction Date

**After:**
- Customer has all above PLUS:
  - Phone number (required)
  - Address fields
  - Status (Active/Inactive/Blocked)
  - Communication preferences
  - **Conversation history** (messages, sentiment, topics)
  - **Interaction patterns** (response time, peak activity, bargaining)
  - **Intelligence scores** (LTV, churn risk, segment)
  - **Recommendations** (products they might like)
  - **Tags** (VIP, bulk buyer, etc.)

**Why:**
- Conversation history is CRM gold (context for personalization)
- Interaction patterns help predict best times to reach out
- Intelligence scores guide targeting and strategy
- Data supports all automation features

---

### CHANGE 4: Manual Customer Creation (Always Available)

**Flexibility options:**

```
Option 1: In Conversations (New)
└─ Right-click conversation > [Create as Customer]
   └─ Auto-fills from WhatsApp contact

Option 2: In Customers Section (Traditional)
└─ Customers > [+ Create Customer]
   └─ Manual form entry

Option 3: Bulk Import (CSV)
└─ Conversations > Import Customers
   └─ Bulk create from file

MERCHANT CHOOSES based on:
├─ Workflow preference
├─ Whether customers message first
└─ Import from external platform
```

**Why both options:**
- Some merchants prefer upfront customer management (manual creation)
- Some prefer conversation-first approach (auto-create)
- Both workflows are supported
- Merchant flexibility = higher adoption

---

### CHANGE 5: Conversation Profile Sidebar (Updated)

When merchant opens a conversation, they see:

```
CONVERSATION THREAD        │ CUSTOMER PROFILE (Sidebar)
                           │ ────────────────────────
[Message history]          │ Satria Aryawan
                           │ Phone: 0812345678
                           │ Status: 🟢 Active
                           │ 
                           │ TRANSACTION HISTORY
                           │ Total Orders: 5
                           │ Total Spend: Rp 5M
                           │ Avg Order Value: Rp 1M
                           │ Last Purchase: Oct 8
                           │ 
                           │ INTERACTION PATTERN
                           │ Response Time: 8 min avg
                           │ Peak Activity: 7-9 PM
                           │ Bargaining: Medium
                           │ 
                           │ SEGMENT
                           │ Repeat Customer ⭐⭐⭐
                           │ High-Value: Yes
                           │ At-Risk: No
                           │ 
                           │ RECOMMENDATIONS
                           │ (shows only if automation enabled)
                           │ • Nasi Goreng (bought 5x)
                           │ • Drinks (usually bundled)
                           │ • Offer: 10% bulk discount
                           │ 
                           │ QUICK ACTIONS
                           │ [Create Invoice]
                           │ [Share Product]
                           │ [Add Note]
                           │ [View Full Profile]
```

**Sidebar behavior depends on automation settings:**
- If "Auto-suggest products" is OFF: Don't show recommendations
- If "Auto-suggest discount" is OFF: Don't show offers
- If "Suggest response templates" is OFF: Hide draft responses

---

### CHANGE 6: Smart Inbox Filtering & Organization

**Filters available:**
```
[All Conversations ▼] 
├─ Unread messages (7)
├─ Awaiting payment (3)
├─ Awaiting shipment (2)
├─ Needs response (5)
├─ VIP customers (12)
├─ Active today (24)
├─ Inactive 30+ days (156)
└─ Archive

[Time range ▼]
├─ Today
├─ This week
├─ This month
└─ Custom

[Sentiment ▼]
├─ Positive
├─ Neutral
├─ Negative
```

**Why:**
- Merchant can focus on urgent conversations first
- Can run segment campaigns easily (e.g., "message all inactive customers")
- Reduces cognitive load
- Automation settings affect filter availability

---

## 4. THREE AUTOMATION SCENARIOS

### SCENARIO 1: Full Manual (All automation OFF)

**Merchant Settings:**
```
All automation disabled (default for MVP)
```

**Workflow:**
```
Customer: "OK saya ambil 2 nasi goreng"
         ↓
Merchant: Sees message in Conversations
         ↓
Merchant: [Create Invoice] (manual click)
         ↓
Dialog: Shows form with merchant-entered data
       └─ Merchant can use context from message for hints
         ↓
Merchant: Fills in:
       ├─ Customer: Satria (auto-selected)
       ├─ Products: Nasi Goreng
       ├─ Quantity: 2
       ├─ Price: Rp 100,000
       └─ [Confirm]
         ↓
Invoice created
         ↓
Merchant: [Send Payment Link] (manual click)
         ↓
Dialog: Generate payment link
       └─ Merchant can customize
         ↓
Merchant: [Confirm & Send]
         ↓
Payment link sent to customer in chat
         ↓
Merchant: Manually monitors payment status
```

**UI Appearance:**
- Simple buttons: [Create Invoice] [Share Product] [Add Note]
- No AI suggestions in sidebar
- No auto-completion
- Merchant in full control

---

### SCENARIO 2: Balanced (Mixed Automation)

**Merchant Settings:**
```
Auto-detect purchase intent: ON
Auto-fill invoice: ON
Auto-create invoice: OFF
Auto-generate link: ON
Auto-send: OFF
Suggest products: ON
```

**Workflow:**
```
Customer: "OK saya ambil 2 nasi goreng"
         ↓
AI: Detects purchase intent + extracts:
   ├─ Product: Nasi Goreng
   ├─ Quantity: 2
   └─ Estimated price: Rp 100K
         ↓
System: Shows inline suggestion in conversation:
        "Purchase detected for Satria
         └─ 2x Nasi Goreng, Rp 100,000
         ├─ [Approve & Create Invoice] 
         ├─ [Edit]
         └─ [Cancel]"
         ↓
Merchant: [Approve & Create Invoice]
         ↓
System: Auto-creates invoice
        Auto-generates payment link
        Shows: "Ready to send?
               ├─ [Send Payment Link]
               ├─ [Edit]
               └─ [Cancel]"
         ↓
Merchant: [Send Payment Link]
         ↓
System: Auto-sends payment link to customer
        Updates conversation: "Payment link sent ✓"
         ↓
Merchant: Monitors payment status
         ↓
When paid: Auto-notification "Payment received ✓"
```

**UI Appearance:**
- Inline suggestions in conversation thread
- Sidebar shows AI recommendations
- One-click approve/send buttons
- Merchant can override/edit before any action

---

### SCENARIO 3: Full AI Automation (All automation ON)

**Merchant Settings:**
```
All automation features enabled
```

**Workflow:**
```
Customer: "OK saya ambil 2 nasi goreng"
         ↓
AI: Detects intent + extracts data
         ↓
System: Auto-creates invoice
        Auto-generates payment link
        Auto-sends to customer
         ↓
Merchant: Sees notification in Conversations
          "Order created ✓ Payment link sent to Satria"
         ↓
Merchant: Only intervenes if needed
         ├─ [Edit & Resend] (if customer questions)
         ├─ [Cancel Order] (if out of stock)
         └─ [View Details] (for verification)
         ↓
System: Monitors payment status automatically
         ↓
When paid: Auto-notification + auto-update in chat
           "Payment received ✓"
           + Invoice marked as PAID
           + Conversation updated
           + Customer record updated
```

**UI Appearance:**
- Minimal merchant interaction
- Automatic inline notifications
- Override buttons for exceptions only
- Dashboard shows "Orders Created Today: 12"

---

## 5. AUTOMATION CONFIGURATION SPEC

### Configuration Page Location

```
FLIK Dashboard
└─ Conversations (primary)
   ├─ Inbox
   ├─ Chat threads (when clicked)
   └─ ⚙️ Settings (configuration page)
```

### Settings Page UI

```
⚙️ AUTOMATION SETTINGS
═════════════════════════════════════

┌─ INVOICE CREATION
│  Auto-detect purchase intent from messages
│  └─ "When customer says 'ambil 2', system recognizes order"
│  └─ Toggle: [OFF / ON]
│
│  Auto-fill invoice from chat context
│  └─ "Use customer, product, quantity from conversation"
│  └─ Toggle: [OFF / ON]
│
│  Auto-create invoice
│  └─ "Automatically create invoice when purchase detected"
│  └─ Toggle: [OFF / ON]
│
├─ PAYMENT LINK
│  Auto-generate with invoice
│  └─ "Payment link auto-generated when invoice created"
│  └─ Toggle: [OFF / ON]
│
│  Auto-send to customer
│  └─ "Send payment link to customer automatically"
│  └─ Toggle: [OFF / ON]
│
│  Auto-update conversation when paid
│  └─ "Post payment confirmation in chat automatically"
│  └─ Toggle: [OFF / ON]
│
└─ AI SUGGESTIONS
   Suggest products to offer
   └─ "AI recommends products customer might like"
   └─ Toggle: [OFF / ON]
   
   Suggest discount offers
   └─ "AI suggests discounts based on purchase pattern"
   └─ Toggle: [OFF / ON]
   
   Suggest response templates
   └─ "AI drafts responses to customer questions"
   └─ Toggle: [OFF / ON]
   
   Prioritize urgent messages
   └─ "Flag unpaid orders, customer complaints first"
   └─ Toggle: [OFF / ON]

[Recommended Settings ▼]
[Save Changes] [Reset to Default]
```

### Configuration Data Model

```json
{
  "merchant_automation_config": {
    "merchant_id": "string",
    "automation_settings": {
      "invoice_creation": {
        "auto_detect_intent": "boolean (default: false)",
        "auto_fill_context": "boolean (default: false)",
        "auto_create": "boolean (default: false)"
      },
      "payment_link": {
        "auto_generate": "boolean (default: false)",
        "auto_send": "boolean (default: false)",
        "auto_update_on_payment": "boolean (default: false)"
      },
      "ai_suggestions": {
        "suggest_products": "boolean (default: false)",
        "suggest_discounts": "boolean (default: false)",
        "suggest_responses": "boolean (default: false)",
        "prioritize_urgent": "boolean (default: false)"
      }
    },
    "created_at": "datetime",
    "updated_at": "datetime"
  }
}
```

### Default Configuration (MVP Safe Mode)

```
All features: OFF

Rationale:
├─ Merchants need to build confidence in AI
├─ Full manual mode lets them verify accuracy
├─ Gradual enablement = higher adoption
└─ Safety first (no surprises)
```

### Recommended Presets (Optional)

```
[Recommended Settings ▼]

1. Safe & Manual (Recommended for beginners)
   └─ All OFF
   └─ Full merchant control
   
2. Balanced (Recommended for established merchants)
   └─ Auto-detect: ON
   └─ Auto-fill: ON
   └─ Auto-create: OFF
   └─ Auto-send: OFF
   └─ Suggestions: ON
   
3. Fast & Automated (Recommended for volume)
   └─ All ON
   └─ Maximum efficiency
   └─ Merchant can override anytime
   
4. Custom (Create your own)
   └─ Pick and choose which features
```

---

## 6. MERCHANT WORKFLOWS

### Workflow 1: Customer Messages For First Time

```
1. Customer sends WhatsApp to merchant
2. Message arrives in FLIK Conversations Inbox
3. FLIK auto-detects new customer
4. Customer record created with:
   ├─ Phone (from WhatsApp)
   ├─ Name (from WhatsApp contact, if available)
   ├─ Status: Active (default)
   └─ Preferences: WhatsApp opt-in (default: Yes)
5. Merchant clicks conversation
6. Merchant can enrich profile:
   ├─ Confirm/correct name
   ├─ Add company (optional)
   ├─ Add address (optional)
   ├─ Set preferences
   └─ [Save]
7. Conversation continues
```

---

### Workflow 2: Create Order (Automation Impact)

```
SCENARIO A: Automation OFF (Manual Mode)
   Customer: "OK saya ambil 2 nasi goreng"
   └─ Merchant: Clicks [Create Invoice]
      └─ Fills form manually
      └─ [Confirm]
      └─ Merchant: [Send Payment Link]

SCENARIO B: Automation Balanced (Suggested Mode)
   Customer: "OK saya ambil 2 nasi goreng"
   └─ AI: Detects + shows suggestion
   └─ Merchant: [Approve & Create]
   └─ System: Auto-creates invoice
   └─ System: Shows "Send?" dialog
   └─ Merchant: [Send]

SCENARIO C: Automation Full (Automatic Mode)
   Customer: "OK saya ambil 2 nasi goreng"
   └─ System: Auto-creates invoice
   └─ System: Auto-sends payment link
   └─ Merchant: Only sees notification
   └─ Merchant: Can override if needed
```

---

### Workflow 3: Re-Engage Inactive Customer

```
1. Merchant opens Conversations
2. Filters: [Inactive 30+ days] (156 customers)
3. Sees list of customers not contacted in 30+ days
4. Selects group or individuals
5. Merchant can:
   ├─ Send: "Satria, sudah lama! Ada menu baru nih 👇"
   ├─ Or select campaign template (if enabled)
   └─ Send to group automatically
6. Responses come back to same conversation thread
7. Each conversation tracked separately

Note: Automation config doesn't affect this workflow
      (Merchant-initiated outreach, not inbound)
```

---

### Workflow 4: Customer Manual Creation

```
OPTION A: Create in Conversations
1. Right-click on conversation
2. [Create as Customer]
3. Dialog: Auto-fills name/phone from WhatsApp
4. Merchant adds additional fields (optional)
5. [Create]

OPTION B: Create in Customers Section
1. Customers > [+ Create Customer]
2. Form:
   ├─ Name *
   ├─ Phone *
   ├─ Email *
   ├─ Company (optional)
   ├─ Address (optional)
   ├─ Status (default: Active)
   ├─ Preferences
   └─ [Create]

OPTION C: Bulk Import (CSV)
1. Conversations > Import Customers
2. Upload CSV with: Name, Phone, Email, Company
3. FLIK creates customer records
4. Merchant can send broadcast
5. Customers reply to start conversations
```

---

## 7. CUSTOMER PROFILE PAGE (NEW)

When merchant clicks on customer name in sidebar or "[View Full Profile]":

```
CUSTOMER DETAIL VIEW
════════════════════════════════════════

SATRIA ARYAWAN
Phone: 0812-3456-7890
Email: satria@email.com
Company: PT ABC
Status: 🟢 Active
Tags: [VIP] [Repeat] [Bulk Buyer]

COMMUNICATION PREFERENCES
└─ WhatsApp Opt-in: ✅ Yes
└─ Email Opt-in: ⚪ Pending
└─ Preferred Channel: WhatsApp
└─ Do Not Contact: None set

ADDRESS (from profile or conversation)
├─ Street: Jl Sudirman No. 1
├─ Province: Jakarta
├─ Postal Code: 12345

TRANSACTION HISTORY
├─ Total Orders: 5
├─ Total Spend: Rp 5,000,000
├─ Avg Order Value: Rp 1,000,000
├─ Purchase Frequency: Every 7 days
├─ Last Order: Oct 8, 2024 (2 days ago)
├─ Next Predicted Order: Oct 15, 2024

CONVERSATION INSIGHTS
├─ Total Messages: 47
├─ Avg Response Time: 8 minutes
├─ Peak Activity: 7-9 PM
├─ Sentiment: Positive ✅
├─ Bargaining Tendency: Medium
└─ Favorite Topics: [Nasi Goreng] [Bulk] [Discount]

AI INTELLIGENCE
(Shows only if related automation is enabled)
├─ Customer Lifetime Value: Rp 5M (actual)
├─ Predicted LTV: Rp 8M (next 12 months)
├─ Churn Risk: LOW ✅
├─ Segment: Repeat + High-Value
├─ Recommended Actions:
│  • Offer loyalty discount (retains high-value customers)
│  • Bundle promotion (they buy together)
│  • VIP early access to new products
└─ Recommended Products: [Nasi Goreng] [Drinks] [Bundle XYZ]

MERCHANT NOTES
├─ "VIP customer - always negotiates for bulk discount"
├─ "Prefers delivery after 7 PM"
├─ "Has large order capacity (~Rp 1-2M per order)"

[Edit Profile] [Send Message] [Create Campaign for Segment] [Export]
```

---

## 8. CUSTOMER LIST VIEW (SECONDARY)

For merchants who still need a traditional list view (for reporting, analysis):

```
CUSTOMER LIST
═══════════════════════════════════════════════════

Filter: [All (72)] [Active (72)] [Inactive (156)] [Blocked (0)]
Sort: [Last Activity ▼] Search: [________]

┌──────────┬──────────┬──────────┬────────┬────────┬──────────┐
│ Name     │ Status   │ Last Msg │ Orders │ Spend  │ Segment  │
├──────────┼──────────┼──────────┼────────┼────────┼──────────┤
│ Satria   │ 🟢 Act   │ 2h ago   │ 5      │ Rp 5M  │ Repeat   │
│ Brisa    │ 🟢 Act   │ 1 day    │ 12     │ Rp 15M │ High-Val │
│ Hani     │ 🟢 Act   │ 3 days   │ 3      │ Rp 2M  │ Repeat   │
│ Berta    │ 🟡 Inac  │ 15 days  │ 20     │ Rp 100M│ At-Risk  │
│ Satria A │ 🔴 Blk   │ 30 days  │ 2      │ Rp 200M│ Blocked  │
└──────────┴──────────┴──────────┴────────┴────────┴──────────┘

[Click to open conversation] [Bulk Actions ▼]
```

---

## 9. REQUIRED FIELDS (Critical for Commerce Chat)

### MUST HAVE (Blocking):
- ✅ **Phone Number** - For WhatsApp messaging
- ✅ **Status** (Active/Inactive/Blocked) - For compliance
- ✅ **Communication Preferences** (Opt-in) - For legal/targeting

### SHOULD HAVE (High Priority):
- ✅ **Email** - For multi-channel campaigns
- ✅ **Address** - For location-based targeting
- ✅ **Conversation History** - For context
- ✅ **Interaction Patterns** - For smart suggestions

### NICE TO HAVE (Medium Priority):
- ✅ **Merchant Notes** - For team communication
- ✅ **Tags** - For segment organization
- ✅ **Predicted Metrics** - For decision-making

---

## 10. IMPLEMENTATION PHASES

### PHASE 1: Foundation (Week 1-2) - BLOCKING

```
□ Fix customer data model
  ├─ Add phone number field (required)
  ├─ Remove duplicate email
  ├─ Add status field
  ├─ Add preferences fields
  └─ Add address fields

□ Build Conversations Inbox UI
  ├─ Message list with customer names
  ├─ Last message preview
  ├─ Unread badges
  ├─ Search/filter
  └─ Click to open thread

□ Build Conversation Thread UI
  ├─ Full message history
  ├─ Message timestamps
  ├─ Display incoming/outgoing clearly
  └─ Merchant can type reply

□ Auto-create customer on first message
  ├─ Extract name from WhatsApp
  ├─ Extract phone from WhatsApp
  └─ Create record automatically

□ Manual customer creation (ALWAYS AVAILABLE)
  ├─ In Customers section: [+ Create Customer]
  ├─ In Conversations: Right-click > [Create as Customer]
  └─ Both workflows supported
```

**Deliverable:** Merchants can see customer conversations; automation OFF by default

---

### PHASE 2: Automation Configuration (Week 3-4) - HIGH

```
□ Build automation settings page
  ├─ Located in Conversations > Settings
  ├─ 9 toggleable features
  ├─ Descriptions for each feature
  ├─ Default: All OFF
  └─ [Save Changes]

□ Implement conditional UI
  ├─ Show/hide features based on automation settings
  ├─ Auto-detection checkbox toggles suggestions
  ├─ Auto-send checkbox affects workflow
  └─ Real-time UI updates when settings change

□ Build default configuration
  ├─ Safe defaults: All automation OFF
  ├─ Per-merchant storage
  └─ Persist across sessions

□ Recommended presets (optional)
  ├─ Safe & Manual
  ├─ Balanced
  ├─ Fast & Automated
  └─ Custom
```

**Deliverable:** Merchants can configure automation level for each feature

---

### PHASE 3: Customer Intelligence (Week 5-6) - HIGH

```
□ Build customer profile sidebar
  ├─ Transaction history
  ├─ Interaction metrics
  ├─ Segment classification
  ├─ Quick actions
  └─ Notes field

□ Calculate conversation metrics
  ├─ Message count
  ├─ Response time average
  ├─ Sentiment analysis (basic)
  ├─ Topic extraction
  └─ Activity timeline

□ Build customer detail page
  ├─ Full profile view
  ├─ All conversation history
  ├─ Transaction timeline
  ├─ Edit profile fields
  └─ Add tags/notes
```

**Deliverable:** Merchants see customer data in conversation context

---

### PHASE 4: AI Automation Pipeline (Week 7-10) - MEDIUM

```
□ Auto-detect purchase intent
  ├─ NLP to identify "ambil 2", "pesan", "checkout"
  ├─ Extract product + quantity
  └─ Confidence scoring

□ Auto-fill invoice from chat
  ├─ Use extracted data
  ├─ Look up product prices
  └─ Pre-populate form

□ Auto-create invoice
  ├─ If automation enabled, create without merchant click
  ├─ Link to customer + conversation
  └─ Set default values

□ Auto-generate payment link
  ├─ Generate with invoice
  ├─ Based on merchant's payment link settings
  └─ Customize based on invoice data

□ Auto-send payment link
  ├─ Send to customer in WhatsApp
  ├─ Post in conversation thread
  └─ Track send timestamp

□ Auto-update conversation
  ├─ Post: "Invoice created ✓"
  ├─ Post: "Payment link sent ✓"
  ├─ Post: "Payment received ✓"
  └─ Keep merchant in loop
```

**Deliverable:** Full automation pipeline working; merchants can enable features gradually

---

### PHASE 5: Advanced Filtering & Analytics (Week 11-12) - MEDIUM

```
□ Build advanced inbox filters
  ├─ By status (active/inactive/blocked)
  ├─ By segment (repeat/high-value/at-risk)
  ├─ By unread status
  ├─ By time range
  └─ By keyword search

□ Build list view (secondary)
  ├─ Traditional table for reporting
  ├─ Export functionality
  └─ Bulk actions

□ Build analytics dashboard
  ├─ Merchant automation adoption %
  ├─ Avg response time
  ├─ Order conversion rate
  └─ ROI of automation features
```

**Deliverable:** Merchants can work efficiently with any configuration level

---

## 11. SUCCESS METRICS

### Adoption Metrics
- % of merchants using Conversations (target: 80%+)
- % of merchants enabling automation (target: 60%+ by month 3)
- Avg features enabled per merchant (target: 3+ by month 6)
- Avg time in Conversations per day (target: 30+ min)
- Messages processed per merchant (target: 50+/day)

### Engagement Metrics
- Avg response time to customer (target: <10 min)
- % of conversations with order created (target: 20%+)
- Customer satisfaction (sentiment positive: 75%+)
- Repeat message rate (customer messages back: 60%+)

### Automation Metrics
- % of merchants confident in automation (target: 70%+ by month 6)
- Time saved per order (target: 80% reduction in manual work)
- Error rate (target: <2% incorrect invoices auto-created)
- Override rate (target: <5% need merchant correction)

### Data Quality Metrics
- % customers with phone number (target: 100%)
- % customers with preferences set (target: 80%+)
- % customers with conversation history (target: 100%)
- % segments accurately classified (target: 90%+)

### Business Metrics
- Avg order value (should increase with personalization)
- Repeat purchase rate (should increase)
- Customer lifetime value (should increase)
- Churn rate (should decrease)

---

## 12. CRITICAL SUCCESS FACTORS

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

3. **Automation Configuration System**
   - Settings persist per merchant
   - Each feature independently toggleable
   - Safe defaults (all OFF for MVP)
   - Easy on/off in UI

4. **Invoice-Payment-Customer Linking**
   - Every invoice linked to customer_id
   - Every payment linked to invoice_id
   - Every conversation linked to customer_id
   - Unified customer view across all channels

---

## 13. MIGRATION FROM OLD TO NEW

**For existing FLIK users with 72 customers:**

```
1. Data migration:
   ├─ Phone numbers (already in list) → move to phone field
   ├─ Remove duplicate email fields
   ├─ Set all status to "Active" (default)
   ├─ Set all to "WhatsApp opt-in: Yes" (existing users)
   └─ Address fields stay empty (fill on next interaction)

2. Workflow migration:
   ├─ OLD: "Customer List" page
   ├─ NEW: "Conversations" inbox
   ├─ OLD: "Create Customer manually" (still available)
   ├─ NEW: "Auto-created on first message" (also available)
   └─ OLD: "Search customers"
   └─ NEW: "Search conversations" (by name/message content)

3. Automation defaults:
   ├─ Set all merchants to: Automation OFF
   ├─ Show them settings location
   ├─ Provide guidance on enabling features
   └─ Track adoption metrics

4. Communication:
   ├─ Tell merchants: "Customer page is now Conversations"
   ├─ Show: "Your existing 72 customers are here"
   ├─ Guidance: "Click on customer to see full conversation"
   ├─ Teach: New quick-action buttons
   └─ Show: "Automation settings" to enable later
```

---

## 14. EDGE CASES & CONSIDERATIONS

### Edge Case 1: Customer Messages Before Account Created

```
Scenario: Customer messages via WhatsApp but merchant hasn't created FLIK account yet

Solution:
├─ Messages queue in system
├─ When merchant signs up, messages available
├─ Customer auto-created from WhatsApp contact
└─ Conversation continues seamlessly
```

---

### Edge Case 2: Merchant Disables Automation Mid-Conversation

```
Scenario: Merchant had auto-send ON, now turns it OFF

Solution:
├─ New conversations: Use OFF setting
├─ In-progress conversations: Merchant approves before send
├─ Already-sent orders: No change (already processed)
└─ Smooth transition, no data loss
```

---

### Edge Case 3: Duplicate Customer Phone Numbers

```
Scenario: Same phone number appears twice in system (merge needed)

Solution:
├─ System detects duplicates during auto-create
├─ Asks merchant: "Merge with existing customer?"
├─ Preserve both conversation histories
├─ Merge transaction data
└─ Keep single customer record
```

---

### Edge Case 4: Automation Confidence Scoring

```
Scenario: AI detects "ambil 2" but confidence is 60%

Solution:
├─ If confidence < 75%: Show suggestion only (not auto-create)
├─ If confidence 75-90%: Show suggestion + [Approve]
├─ If confidence > 90%: Auto-create (if automation ON)
└─ Merchant can always override
```

---

## 15. TECHNICAL ARCHITECTURE

### Data Flow

```
WhatsApp API
    ↓
Message Webhook Handler
    ↓
├─ Extract customer (phone → lookup)
├─ Auto-create customer (if new)
├─ Store message in database
├─ Run NLP (purchase intent, sentiment)
├─ Apply automation rules (based on merchant settings)
├─ Update customer metrics
└─ Display in Conversations UI

When automation enabled:
├─ Auto-create invoice (if intent detected + confidence high)
├─ Auto-generate payment link (if automation ON)
├─ Auto-send to customer (if automation ON)
└─ All actions logged for merchant audit trail
```

### Database Schema Addition

```
CREATE TABLE merchant_automation_config (
  id UUID PRIMARY KEY,
  merchant_id UUID NOT NULL,
  auto_detect_intent BOOLEAN DEFAULT FALSE,
  auto_fill_context BOOLEAN DEFAULT FALSE,
  auto_create_invoice BOOLEAN DEFAULT FALSE,
  auto_generate_link BOOLEAN DEFAULT FALSE,
  auto_send_link BOOLEAN DEFAULT FALSE,
  auto_update_on_payment BOOLEAN DEFAULT FALSE,
  suggest_products BOOLEAN DEFAULT FALSE,
  suggest_discounts BOOLEAN DEFAULT FALSE,
  suggest_responses BOOLEAN DEFAULT FALSE,
  prioritize_urgent BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

ALTER TABLE customers ADD COLUMN (
  status ENUM('active', 'inactive', 'blocked') DEFAULT 'active',
  phone VARCHAR(20) UNIQUE NOT NULL,
  whatsapp_optin ENUM('yes', 'no', 'pending') DEFAULT 'pending',
  email_optin ENUM('yes', 'no', 'pending') DEFAULT 'pending',
  preferred_channel ENUM('whatsapp', 'email', 'both') DEFAULT 'whatsapp',
  do_not_contact_times TEXT, -- JSON array
  conversation_sentiment ENUM('positive', 'neutral', 'negative'),
  customer_lifetime_value DECIMAL(15, 2),
  predicted_ltv DECIMAL(15, 2),
  churn_risk ENUM('high', 'medium', 'low'),
  segment VARCHAR(50)
);
```

---

## SUMMARY

**From Transaction Records to Conversation Partners with Flexible Automation:**

| Aspect | OLD | NEW (v3) |
|--------|-----|----------|
| **View** | Customer list | Conversations inbox |
| **Data** | Name, email, spend | + Phone, preferences, conversation history |
| **Interface** | Search/filter table | Chat thread with sidebar |
| **Creation** | Manual via form | Auto on first message OR manual (both supported) |
| **Context** | Transaction-only | Conversation + transaction |
| **Action** | View history | Chat, create order, share product |
| **Intelligence** | None | AI suggestions (configurable per feature) |
| **Automation** | N/A | FLEXIBLE: Merchant chooses each feature |

**Key Innovation:** Merchants control their own automation level - from full manual to full AI, with independent toggles for each feature.

**Result:** Customer becomes active partner, not passive record. Merchant confidence drives adoption of automation features.

---

*Document Version: 3.0 (Commerce Chat Edition - Granular Automation)*  
*Last Updated: October 2026*  
*Status: Ready for Implementation*
