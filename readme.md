# Live Selling Chatbot (Messenger + Facebook) — Project Specification

## Overview

This repo defines a Messenger chatbot project integrated with Facebook Page interactions that enables automated live-selling behavior.

The system is designed for Facebook Live selling, where a seller can dynamically control which product is currently being sold.

When a user comments "mine" during a live session:

* The system detects the comment
* Identifies the currently active product
* Verifies product availability
* Automatically adds the item to the user’s cart
* Sends a Messenger message confirming the action

This simulates a live-selling environment with an automated checkout flow.

---

## Goals

* Detect Facebook Live comments in real-time
* Filter keyword triggers (e.g., "mine")
* Track live sessions and active products
* Manage product stock
* Create and update user carts
* Send automated Messenger responses

---

## Core Features

### 1. Comment Trigger System
* Listens to new comments on Facebook Live videos
* Filters for keyword: "mine"
* Extracts:
  * User ID (PSID)
  * Live Video ID
  * Comment timestamp

### 2. Live Session Management
* Tracks active Facebook Live sessions
* Associates incoming comments with a live session
* Maintains session state (active, ended)

### 3. Active Product Control
* Seller can set a currently active product during a live session
* Only one product is active at a time
* All valid "mine" comments are mapped to the active product

### 4. Stock Validation
* Checks if product stock is available
* Prevents overselling via atomic updates (to be implemented later)

### 5. Cart System (Chat-Based)
* Each user has an active cart
* On successful typing of "mine":
  * Add active product (quantity = 1) to cart
  * Create cart if none exists

### 6. Messenger Notification
* Sends a message to the user confirming:
  * Product name
  * Price
  * Quantity added

* Provides next actions (checkout, view cart, etc.)

---

## Initial Flow

1. Seller starts Facebook Live
2. System creates or identifies an active live session
3. Seller sets an active product via control interface
4. User comments "mine" on the live video
5. Facebook sends webhook event
6. n8n processes the event
7. System identifies the corresponding live session
8. System retrieves current active product
9. System checks stock availability
10. If available:
    * Decrement stock
    * Add item to cart
    * Send Messenger message
11. If not available:
    * either notify user or ignore (to decide on)
12. If no active product:
    * ignore or notify user (to decide on)

---

## Requirements

### Automation

* n8n (workflow automation | can skip, will see if graph api can directly communicate with a system backend )
* Facebook Graph API (facebook webhooks and messaging)

  !!! I may need to wait for a day or two. Meta for developers has a policy that an account must be on a device in a reasonable amount of time to keep an account safe. Here in my computer, my account has been logged in for a long time yet i was still able to trigger the block, forced to wait.

### Database

* Supabase (for now just to simulate product movements, might change to follow actual sysreqs in the future)

### Messaging

* Facebook Messenger 

---

## Data Models (to be build in supabase for now)

### Product

* id
* name
* description
* price
* stock
* image_url

### Live Session

* id
* page_id
* live_video_id
* status (active, ended)
* active_product_id
* started_at
* ended_at

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

---

## Specifics

* Products are dynamically assigned during live sessions
* Only one product is active at a time per live session
* Users interact using a consistent Facebook account
* Messenger events (sending and updating) is to follow Facebook’s policy window on chat interactions by bots

---

## Constraints

* Facebook Graph API permissions and webhook setup required
* Messaging restrictions (24-hour window)

---

## Development Approach

- [ ] Set up Supabase database with sample products
- [ ] Create Developer account
- [ ] Create Facebook test page and live setup
- [ ] Configure Facebook webhook to n8n
- [ ] Implement live session tracking
- [ ] Implement active product control
- [ ] Implement comment trigger logic in n8n
- [ ] Implement cart creation and item addition
- [ ] Send Messenger confirmation message
- [ ] Test end-to-end flow

---
