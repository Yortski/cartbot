# Live Selling Chatbot (Messenger + Facebook) — Project Specification

## Overview

This project aims to build a Messenger chatbot integrated with Facebook Page interactions that enables automated live-selling behavior.

When a user comments "mine" on a product post:

* The system detects the comment
* Verifies product availability
* Automatically adds the item to the user’s cart
* Sends a Messenger message confirming the action

This simulates a live-selling environment with an automated checkout flow.

---

## Goals

### Primary Goals

* Detect Facebook post comments in real-time
* Filter keyword triggers (e.g., "mine")
* Link posts to products
* Manage product stock
* Create and update user carts
* Send automated Messenger responses

### Secondary Goals (Future Enhancements)

* Full checkout flow
* Payment integration (GCash, bank, etc.)
* Order tracking
* Seller dashboard
* Multi-product live selling

---

## Core Features

### 1. Comment Trigger System

* Listens to new comments on Facebook posts
* Filters for keyword: "mine"
* Extracts:

  * User ID (PSID)
  * Post ID
  * Comment timestamp

### 2. Product Mapping

* Each Facebook post corresponds to a product
* Product is identified via post_id

### 3. Stock Validation

* Checks if product stock is available
* Prevents overselling via atomic updates (to be implemented later)

### 4. Cart System (Chat-Based)

* Each user has an active cart
* On successful claim:

  * Add product (quantity = 1) to cart
  * Create cart if none exists

### 5. Messenger Notification

* Sends a message to the user confirming:

  * Product name
  * Price
  * Quantity added
* Provides next actions (checkout, view cart, etc.)

---

## System Architecture

### Flow Overview

1. User comments "mine" on a Facebook post
2. Facebook sends webhook event
3. n8n processes the event
4. System identifies the related product
5. System checks stock availability
6. If available:

   * Decrement stock
   * Add item to cart
   * Send Messenger message
7. If not available:

   * Optionally notify user or ignore

---

## Tech Stack

### Backend / Automation

* n8n (workflow automation and orchestration)
* Facebook Graph API (webhooks and messaging)

### Database

* Supabase (PostgreSQL)

### Messaging

* Facebook Messenger Platform

### Optional (Future)

* Vercel (API endpoints / serverless functions)
* React (admin dashboard)

---

## Data Models (High-Level)

### Product

* id
* name
* description
* price
* stock
* image_url
* post_id

### Cart

* id
* user_id
* status (active, checked_out)
* created_at

### Cart Item

* id
* cart_id
* product_id
* quantity

### Order (Future)

* id
* user_id
* total
* status
* created_at

---

## Assumptions

* Each Facebook post represents a single product
* Users interact using a consistent Facebook account (PSID)
* Messenger messaging is allowed within Facebook’s policy window

---

## Constraints

* Facebook API permissions and webhook setup required
* Messaging restrictions (24-hour window)
* Must handle race conditions for stock updates (to be implemented later)

---

## Future Enhancements

* Multi-item cart management via chat
* Natural language interaction (AI-powered chatbot)
* Payment confirmation flow
* Inventory dashboard for sellers
* Analytics and reporting

---

## Development Approach

1. Set up Supabase database with sample products
2. Create Facebook test page and sample product posts
3. Configure Facebook webhook to n8n
4. Implement comment trigger logic in n8n
5. Implement cart creation and item addition
6. Send Messenger confirmation message
7. Test end-to-end flow

---

## Success Criteria

* System correctly detects "mine" comments
* Only valid claims update stock
* Users receive Messenger confirmation
* Cart is correctly updated per user

---

## Notes

* Focus on MVP simplicity first
* Avoid over-engineering early
* Ensure da
