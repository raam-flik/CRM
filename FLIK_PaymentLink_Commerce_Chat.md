# FLIK Payment Link Product - Commerce Chat
## With Flexible Automation Configuration

**Status:** Ready for Implementation
**Version:** 3.0 (Automation Integrated)
**Date:** October 2026

## Document Purpose

This document describes how **Payment Link** transforms in the Commerce Chat model. Payment Links transition from **standalone shareable URLs** to **embedded chat actions** that appear naturally within customer conversations.

---

## 1. PARADIGM SHIFT

### OLD MODEL (Dashboard-First)
```
WORKFLOW:
1. Merchant creates payment link (standalone)
2. Merchant generates QR code or copies link
3. Merchant shares via WhatsApp/Email separately
4. Customer receives link outside of context
5. Customer clicks → goes to payment page
6. Payment completed

Payment link is separate artifact from conversation.
```

### NEW MODEL (Commerce Chat-First)
```
WORKFLOW:
1. Merchant and customer agree on items in chat
2. Merchant creates order [Create Order button]
3. Order generates invoice + payment link (automatic)
4. [Send Payment Link] button appears in chat
5. Payment link appears in WhatsApp as action button
6. Customer clicks in same chat thread
7. Pays on payment page (can return to chat)
8. Payment status updates in chat: "✓ Received!"

Payment link is natural part of order conversation.
```


---

## 2. AUTOMATION IMPACT (NEW in v3.0)

### How Automation Changes Payment Link Workflow

Merchants now have **flexible control** over payment link generation and sending:

#### Manual Mode (All OFF)
```
Merchant: Creates invoice manually
         ↓
Merchant: Clicks [Generate Payment Link]
         ↓
System: Generates link via payment provider
         ↓
Merchant: Clicks [Send Payment Link]
         ↓
Link sent to customer via WhatsApp
```

#### Balanced Mode (Mixed)
```
Merchant Settings:
├─ Auto-generate link: ON
└─ Auto-send link: OFF

Invoice: Created
         ↓
System: Auto-generates payment link
         ↓
Merchant: Sees [Send Payment Link] button
         ↓
Merchant: [Send Payment Link]
         ↓
Link sent to customer
```

#### Full AI Automation (All ON)
```
Merchant Settings: All automation enabled

Invoice: Created
         ↓
System: Auto-generates payment link
         ↓
System: Auto-sends to customer via WhatsApp
         ↓
Merchant: Sees notification "Payment link sent ✓"
         ↓
Merchant: Monitors payment status
         ↓
When paid: Auto-notification "Payment received ✓"
```

### Key Automation Features for Payment Link

| Feature | Toggle | Default | Effect |
|---------|--------|---------|--------|
| Auto-generate with invoice | `auto_generate_link` | OFF | Link created automatically when invoice created |
| Auto-send to customer | `auto_send_link` | OFF | Link sent to customer without merchant approval |
| Auto-update on payment | `auto_update_on_payment` | OFF | Conversation auto-updated when payment received |

---

## 2. CURRENT STATE vs. NEW STATE

### CURRENT STATE ❌

**Current Payment Link Workflow:**
```
Dashboard > Products > Payment Link > [Create Payment Link]
├─ Select products
├─ Configure payment methods
├─ Custom fields (optional)
├─ Promotion codes (optional)
└─ Share via link/QR

Features:
├─ Standalone URL
├─ No customer association
├─ No tracking of who clicked
├─ Multiple times shareable
├─ No conversation context
└─ No status updates
```

**Data Structure:**
```json
{
  "payment_link": {
    "id": "string",
    "merchant_id": "string",
    "url": "string",
    "items": [{
      "product_id": "string",
      "quantity": "number",
      "price": "number"
    }],
    "total_amount": "number",
    "payment_methods": ["string"],
    "created_at": "datetime"
  }
}
```

**Problems:**
- Not linked to customer
- No context about why customer is paying
- Can't track individual customer's payment journey
- Creates multiple links for same merchant
- Not integrated with order workflow
- No automatic status updates

---

### NEW STATE ✅

**New Payment Link Workflow (Embedded in Chat):**

```
CONVERSATION FLOW:

[Order created in chat]
│
Order Summary appears:
┌─────────────────────────────────┐
│ PESANAN DIKONFIRMASI ✓          │
│ #INV-00124                       │
│ 2x Nasi Goreng: Rp 100K          │
│ TOTAL: Rp 100K                   │
│                                 │
│ [Bayar QRIS] [Bayar Transfer]   │
│ [Bayar Card]                    │
└─────────────────────────────────┘

Customer clicks [Bayar QRIS]
│
Payment page opens (customized with customer name)
│
Payment successful
│
Chat updates instantly: "✓ Pembayaran diterima!"
│
Customer returns to chat: Order status visible
```

**New Data Structure:**
```json
{
  "payment_link": {
    "id": "string",
    "merchant_id": "string",
    "customer_id": "string", // ✅ NOW LINKED
    "invoice_id": "string", // ✅ NEW - linked to order
    "conversation_id": "string", // ✅ NEW - linked to chat
    "url": "string",
    "items": [{
      "product_id": "string",
      "quantity": "number",
      "price": "number"
    }],
    "total_amount": "number",
    "payment_methods": ["qris", "va", "card", "ewallet"],
    "custom_fields": ["field_1"], // optional customer data
    "promotion_codes": ["CODE1"], // optional discounts
    "chat_format": {
      "display_type": "button | qr_code | both", // ✅ NEW
      "button_text": "Bayar Sekarang", // ✅ NEW
      "short_message": "Pesanan Rp 100K", // ✅ NEW
      "customer_name": "Satria" // ✅ NEW - personalized
    },
    "tracking": {
      "link_sent_at": "datetime", // ✅ NEW
      "link_clicked_at": "datetime", // ✅ NEW
      "payment_started_at": "datetime", // ✅ NEW
      "payment_completed_at": "datetime", // ✅ NEW
      "payment_method_used": "string", // ✅ NEW
      "click_count": "number", // ✅ NEW
      "conversion_status": "not_started | in_progress | completed" // ✅ NEW
    },
    "status": "active | completed | expired | cancelled",
    "expires_at": "datetime",
    "created_at": "datetime"
  }
}
```

---

## 3. KEY CHANGES FOR COMMERCE CHAT

### CHANGE 1: Payment Links are Auto-Generated from Orders

**Before:**
```
Payment link created manually, shared separately
```

**After:**
```
Order created → Payment link auto-generated → Appears in chat
```

**Why:**
- No extra manual step
- Every order has payment link automatically
- Linked to correct order data
- Less chance of merchant sharing wrong link

---

### CHANGE 2: Payment Link Shows in Chat as Button

**Before:**
```
Merchant shares: "paymentlink.flik.com/pl-12345"
Customer sees: Text link, must click
Experience: Formal, impersonal
```

**After:**
```
CHAT SHOWS:
┌──────────────────────────┐
│ [Bayar QRIS] [Bayar VA]  │
│ [Bayar Card] [E-Wallet]  │
└──────────────────────────┘

Customer sees: Action buttons, not text
Experience: Modern, familiar (like chat commerce)
```

**Why:**
- Matches chat UX expectations
- Higher click-through rate (buttons vs links)
- Multiple payment options visible at once
- One-tap experience

---

### CHANGE 3: Payment Status Updates Appear in Chat

**Before:**
```
Merchant sends link via WhatsApp
Customer clicks → goes to payment page
Customer pays → returns to chat manually
Merchant doesn't know status without checking backend
```

**After:**
```
Merchant sends payment link in chat
Customer clicks [Bayar QRIS]
Customer pays on payment page (takes 30 seconds)
Chat updates automatically: "✓ Pembayaran diterima!"
Merchant sees update in same conversation
No need to check backend
```

**Why:**
- Real-time confirmation
- Merchant knows immediately
- Customer gets confirmation
- No confusion or follow-up messages needed

---

### CHANGE 4: Payment Link Linked to Customer & Conversation

**Before:**
```
Payment link exists independently
No way to know which customer it's for
Multiple links for same merchant to same customer
Hard to track who clicked what
```

**After:**
```
Payment link linked to:
├─ Customer record
├─ Conversation thread
├─ Invoice/Order
└─ Specific product combination

Merchant can see:
- Which customers clicked payment link
- How long they take to pay
- Which payment method they prefer
- All payment history in one customer view
```

**Why:**
- Better CRM insights
- Know customer payment behavior
- Can follow up intelligently
- Build better payment reminders

---

### CHANGE 5: Tracking & Analytics Embedded

**Before:**
```
Payment link created
No way to know if link was clicked
No tracking of payment journey
Analytics requires manual checking
```

**After:**
```
Every payment link tracked:
├─ When sent to customer
├─ When customer clicked
├─ Payment method used
├─ Payment completion time
├─ Failed payment attempts
└─ All data in customer view + conversation

Merchant sees automatically in:
├─ Conversation thread ("Link clicked at 3:05 PM")
├─ Customer profile ("Payment history")
├─ Order view ("Status: Paid")
└─ Analytics dashboard
```

**Why:**
- Understand payment behavior
- Identify payment friction
- Optimize checkout experience
- Reduce payment failures

---

### CHANGE 6: Personalized Payment Experience

**Before:**
```
Generic payment page: "Your total is Rp 100,000"
No customer context
Professional but cold
```

**After:**
```
Payment page shows:
├─ Customer name: "Hi Satria!"
├─ Merchant: "From Warung Nasi"
├─ Order summary: "2x Nasi Goreng"
├─ Total: "Rp 100,000"
├─ Delivery time: "Today 5-6 PM"
└─ [Pay Now]

Experience: Personal, contextual, warm
```

**Why:**
- Higher payment completion
- Fewer payment cancellations
- Better customer experience
- Increases trust

---

## 4. NEW WORKFLOWS

### Workflow 1: Payment Link Created Automatically with Order

```
CONVERSATION:

Customer: "OK saya ambil 2"

Merchant: [Create Order] in FLIK
│
Order created #INV-00124
│
Payment link auto-generated
│
Chat shows order summary:
┌─────────────────────────────────┐
│ ✓ PESANAN DIKONFIRMASI          │
│ #INV-00124                       │
│ 2x Nasi Goreng: Rp 100K          │
│ Pengiriman: Gratis               │
│ TOTAL: Rp 100K                   │
│                                 │
│ [Bayar QRIS] [Bayar Transfer]   │
│ [Bayar Card] [E-Wallet]         │
└─────────────────────────────────┘

(No separate payment link step needed - it's automatic)

Customer: [Clicks Bayar QRIS]
│
Payment page opens with:
├─ "Hi Satria!"
├─ From: Warung Nasi
├─ Order: 2x Nasi Goreng
├─ Total: Rp 100,000
└─ Delivery: Today 5-6 PM

Customer pays QRIS (30 seconds)
│
Chat updates: "✓ Pembayaran diterima!"
              "Pesanan dikonfirmasi. Akan dikirim jam 5 sore"
```

---

### Workflow 2: Send Payment Link to Existing Order

```
CONVERSATION WITH UNPAID ORDER:

Merchant: "Satria, pembayaran sudah kami siapkan"

Merchant clicks: [Send Payment Link] (in conversation)
│
Payment link resent to customer
│
Chat shows:
"Silakan bayar: Rp 100,000"
[Bayar QRIS] [Bayar Transfer]

Customer: [Clicks button]
│
Pays quickly
│
Status updates in chat
```

---

### Workflow 3: Resend Payment Link if Not Paid

```
24 HOURS AFTER ORDER:

Order still unpaid
FLIK suggests: "Send payment reminder?"

Merchant: [Send Reminder]
│
Chat message: "Satria, pesanannya masih belum dibayar nih"
              "Pembayaran: Rp 100,000"
              [Bayar Sekarang]

Customer sees reminder in chat
Clicks and pays
```

---

### Workflow 4: Payment Method Options

```
CUSTOMER SEES IN CHAT:

Pesanan: Rp 100,000

Metode Pembayaran:
[Bayar QRIS]  - Instant transfer
[Bayar VA]    - Bank transfer
[Bayar Card]  - Credit/Debit card
[E-Wallet]    - OVO, GoPay, Dana

Customer chooses their preferred method
Faster checkout experience
```

---

### Workflow 5: Track Payment Behavior

```
CUSTOMER PROFILE > PAYMENT HISTORY

Satria Aryawan
├─ Total Orders: 5
├─ Total Paid: 5 ✓ (100% payment rate)
├─ Avg Payment Time: 12 min
├─ Preferred Method: QRIS (60% of payments)
├─ Failed Attempts: 0
└─ Last Payment: Oct 8, 3:05 PM via QRIS

INSIGHT:
"Satria always pays fast via QRIS.
Suggest QRIS as default next time."
```

---

## 5. NEW PAYMENT LINK INTERFACE

### Payment Link Details (in Order Context)

```
ORDER #INV-00124
═════════════════════════════════════

PAYMENT LINK
├─ Status: Active
├─ Total Amount: Rp 100,000
├─ Payment Methods: QRIS, VA, Card, E-Wallet
├─ Created: Oct 8, 2:50 PM
├─ Sent to customer: Oct 8, 2:52 PM
├─ Customer clicked: Oct 8, 3:00 PM
├─ Payment completed: Oct 8, 3:05 PM ✓
│
├─ Tracking:
│  ├─ Time from creation to payment: 15 min
│  ├─ Payment method used: QRIS
│  ├─ Payment success: ✓
│  └─ Customer returned to chat: Oct 8, 3:10 PM
│
└─ [Resend Payment Link] [Generate QR] [View Payment Page]
```

---

### Payment Link List/Analytics

```
PAYMENT LINKS (Analytics)
════════════════════════════════════════════════

Filter: [All] [Completed] [Pending] [Expired]
Sort: [Recent ▼]

┌──────────┬──────────┬────────┬──────────┬────────┐
│ Customer │ Amount   │ Status │ Sent     │ Paid   │
├──────────┼──────────┼────────┼──────────┼────────┤
│ Satria   │ Rp 100K  │ ✓      │ 2:52 PM  │ 3:05 PM│
│          │          │ Paid   │ Oct 8    │ Oct 8  │
├──────────┼──────────┼────────┼──────────┼────────┤
│ Brisa    │ Rp 250K  │ ⏳      │ 4:00 PM  │ -      │
│          │          │ Pending│ Oct 8    │        │
├──────────┼──────────┼────────┼──────────┼────────┤
│ Hani     │ Rp 75K   │ ✓      │ 5:15 PM  │ 5:20 PM│
│          │          │ Paid   │ Oct 8    │ Oct 8  │
└──────────┴──────────┴────────┴──────────┴────────┘

ANALYTICS:
├─ Total Links Sent: 23
├─ Completed Payments: 20 (87%)
├─ Pending Payments: 3 (13%)
├─ Avg Time to Payment: 8 min
├─ Most Used Method: QRIS (65%)
└─ Failure Rate: 2%
```

---

## 6. PAYMENT LINK IN CHAT

### How Payment Link Appears to Customer

```
IN WHATSAPP:

Warung Nasi
─────────────────────

Pesanan Anda:
2x Nasi Goreng - Rp 100K
Pengiriman Gratis
TOTAL: Rp 100K

Silakan bayar dengan:

[Bayar QRIS]
Scan QR atau transfer
instan

[Bayar Transfer]
Nomor rekening:
BNI 1234567890

[Bayar Card]
Kartu Kredit/Debit

[E-Wallet]
OVO, GoPay, Dana

Pesanan dikirim setelah pembayaran ✓
```

---

### QR Code Alternative

```
MERCHANT CAN CHOOSE:

Display Option 1: Action Buttons (Default)
[Bayar QRIS] [Bayar Transfer] [Bayar Card]

Display Option 2: QR Code Only
[QR Code Image]
Scan untuk bayar

Display Option 3: Both
[QR Code Image]
[Bayar QRIS] [Bayar Transfer] [Bayar Card]

Merchant chooses based on use case:
- In-person: QR code better
- Chat: Buttons better
- Combined: Both options
```

---

## 7. REQUIRED CHANGES

### MUST HAVE (Blocking):
- ✅ **Auto-generate with order** - Payment link created when order created
- ✅ **Customer linking** - Link associated with customer_id
- ✅ **Conversation linking** - Link associated with conversation_id
- ✅ **Chat button format** - Appears as action buttons in WhatsApp
- ✅ **Payment tracking** - Track when clicked, when paid
- ✅ **Status in chat** - Payment confirmation appears in conversation

### SHOULD HAVE (High Priority):
- ✅ **Personalized payment page** - Show customer name, order context
- ✅ **Multiple payment methods** - QRIS, VA, Card, E-Wallet
- ✅ **Payment method tracking** - Know which method customer prefers
- ✅ **Resend capability** - Can resend if not paid
- ✅ **QR code option** - For in-person or printed sharing

### NICE TO HAVE (Medium Priority):
- ✅ **Discount codes** - Apply promo codes at payment
- ✅ **Custom fields** - Collect additional customer data
- ✅ **Payment reminders** - Auto-remind if unpaid after 24h
- ✅ **Retry on failure** - Auto-retry if payment fails
- ✅ **Webhook integration** - Real-time payment confirmations

---

## 8. IMPLEMENTATION PRIORITIES

### PHASE 1: Foundation (Week 1-2) - BLOCKING

```
□ Update payment link data model
  ├─ Add customer_id (link to customer)
  ├─ Add invoice_id (link to order)
  ├─ Add conversation_id (link to chat)
  ├─ Add tracking fields (clicked_at, paid_at, etc)
  └─ Add chat_format fields (button_text, display_type)

□ Build auto-generate payment link with order
  ├─ When order created, auto-generate link
  ├─ Link included in order data
  └─ No manual link creation step

□ Build chat button display
  ├─ Format payment link as action buttons
  ├─ Multiple payment methods shown
  ├─ Insert into chat conversation
  └─ Track when customer clicks

□ Build payment completion webhook
  ├─ Receive payment confirmation from payment provider
  ├─ Update order status
  ├─ Send status update to chat
  └─ Notify merchant
```

**Deliverable:** Payment links work in chat flow

---

### PHASE 2: Smart Features (Week 3-4) - HIGH

```
□ Build personalized payment page
  ├─ Show customer name
  ├─ Show order summary with items
  ├─ Show merchant name
  ├─ Show delivery info
  ├─ Dynamic content based on customer
  └─ A/B test variations

□ Build payment method tracking
  ├─ Track which method customer used
  ├─ Store as preference in customer profile
  ├─ Suggest preferred method next time
  └─ Analytics by payment method

□ Build payment journey tracking
  ├─ Track link sent → clicked → payment started → completed
  ├─ Measure time at each step
  ├─ Identify friction points (e.g., "payment page takes too long")
  └─ Insights for optimization

□ Build resend capability
  ├─ [Resend Payment Link] button in chat
  ├─ Same link or new link (configurable)
  ├─ Track resends
  └─ Update status in chat
```

**Deliverable:** Payment experience personalized and optimized

---

### PHASE 3: Automation (Week 5) - MEDIUM

```
□ Build payment reminders
  ├─ If unpaid after X hours, send reminder
  ├─ Suggestion: "Satria, pesanannya belum dibayar..."
  ├─ Include payment link again
  └─ Customizable reminder timing

□ Build failed payment recovery
  ├─ If payment fails, notify merchant
  ├─ Suggest resend or follow-up
  ├─ Track failure reasons (insufficient funds, etc)
  └─ Analytics on failure patterns

□ Build QR code option
  ├─ Generate QR from payment link
  ├─ Display in chat or print
  ├─ Useful for in-person sales
  └─ Track QR scans

□ Build discount code support
  ├─ Apply promo codes at payment
  ├─ Show discount in payment page
  ├─ Update total amount
  └─ Track coupon usage
```

**Deliverable:** Automate follow-ups, reduce manual work

---

### PHASE 4: Advanced (Week 6+) - MEDIUM

```
□ Build payment analytics dashboard
  ├─ Payment success rate over time
  ├─ Most used payment methods
  ├─ Avg payment time
  ├─ Customer payment patterns
  └─ Revenue trends

□ Build recurring payment support
  ├─ Auto-generate links for recurring orders
  ├─ Auto-retry on failure
  ├─ Subscription tracking
  └─ Cancellation handling

□ Build payment split support
  ├─ Allow partial payments
  ├─ Installment option (if available from payment provider)
  ├─ Payment plan tracking
  └─ Reminder for installment due dates

□ Build payment reconciliation
  ├─ Match payments to orders
  ├─ Identify mismatches
  ├─ Accounting export
  └─ Tax reporting support
```

**Deliverable:** Payments integrate seamlessly with business operations

---

## 9. MIGRATION FROM OLD TO NEW

**For existing FLIK payment links:**

```
1. Data migration:
   ├─ All existing links keep data
   ├─ Add customer_id: Match by context if possible
   ├─ Add conversation_id: Blank (create on next interaction)
   ├─ Add tracking fields: Populate with available data
   └─ Preserve all payment history

2. Workflow migration:
   ├─ OLD: Payment link created manually in dashboard
   ├─ NEW: Auto-generated with order in chat
   ├─ OLD: Shared as text link or QR
   ├─ NEW: Appears as action buttons in chat
   └─ OLD: Limited tracking
   └─ NEW: Full journey tracking in conversation

3. User education:
   ├─ Show merchants: "Payment links now auto-generated"
   ├─ Explain: "Send payment link directly in chat"
   ├─ Demo: How payment appears to customer
   ├─ Show: Tracking data in order details
   └─ Teach: How to resend if needed
```

---

## 10. SUCCESS METRICS

**How to measure if Payment Link redesign works:**

```
ADOPTION METRICS
├─ % of payment links generated with orders (target: 95%+)
├─ % of payment links sent via chat buttons (target: 80%+)
├─ % of manual payment link creation (target: reduce to <5%)
└─ Merchant satisfaction with payment flow (target: 4.5+/5)

PAYMENT COMPLETION METRICS
├─ Payment success rate (target: 98%+, up from baseline)
├─ Avg time from link sent to payment (target: <10 min)
├─ Payment completion rate same day (target: 90%+)
├─ Failed payment recovery rate (target: 80%+)
└─ Payment cancellation rate (target: <3%)

CUSTOMER EXPERIENCE METRICS
├─ Payment link click-through rate (target: 75%+)
├─ Time spent on payment page (target: <2 min)
├─ Payment method preference stability (target: 70%+)
├─ Customer satisfaction with payment (target: 4.5+/5)
└─ Support tickets about payment (target: reduce by 70%)

BUSINESS METRICS
├─ Revenue per customer (should increase with faster payment)
├─ Repeat purchase rate (should increase)
├─ Churn rate (should decrease)
└─ Working capital efficiency (faster cash collection)
```

---

## 11. TECHNICAL CONSIDERATIONS

**Infrastructure needed:**
```
Payment Link Management
├─ Create/update/delete links
├─ Generate unique URLs
├─ Link tracking and analytics
└─ Expiration management

Payment Processing Integration
├─ Connect to multiple payment providers
├─ Handle payment confirmations
├─ Webhook management
├─ Error handling and retries
└─ Reconciliation

Chat Integration
├─ Format payment links for WhatsApp
├─ Track link clicks from chat
├─ Display payment status updates
└─ Webhook for payment → chat update

Analytics & Reporting
├─ Track payment journey
├─ Generate payment reports
├─ Customer payment patterns
└─ Revenue analytics
```

---

## SUMMARY

**From Standalone URLs to Embedded Chat Actions:**

| Aspect | OLD | NEW |
|--------|-----|-----|
| **Creation** | Manual in dashboard | Auto-generated with order |
| **Sharing** | Text link or QR | Action buttons in chat |
| **Context** | No customer/order link | Linked to customer + order + conversation |
| **Personalization** | Generic | Customer name, order summary shown |
| **Status** | No updates | Updates appear in chat |
| **Tracking** | Limited | Full journey from sent → clicked → paid |
| **Experience** | Formal, separate | Integrated, natural flow |
| **Payment confirmation** | Manual check | Automatic notification |

**Result:** Payment becomes seamless part of chat conversation, not friction point.

---

*Document Version: 2.0 (Commerce Chat Edition)*  
*Last Updated: October 2026*  
*Status: Ready for Implementation*
