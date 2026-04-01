# Live Selling Chatbot (Messenger + Facebook) — Project Specification

## Overview

This repo defines a Messenger chatbot project integrated with Facebook Page interactions that enables automated live-selling behavior.

When a user comments "mine" on a product post:

* The system detects the comment
* Verifies product availability
* Automatically adds the item to the user’s cart
* Sends a Messenger message confirming the action

This simulates a live-selling environment with an automated checkout flow.

---

## Goals

* Detect Facebook post comments in real-time
* Filter keyword triggers (e.g., "mine")
* Link posts to products
* Manage product stock
* Create and update user carts
* Send automated Messenger responses

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
* Each Facebook post should corresponds to a product
* Products are identified using a post_id

### 3. Stock Validation
* Checks if product stock is available
* Prevents overselling via atomic updates (to be implemented later)

### 4. Cart System (Chat-Based)
* Each user has an active cart
* On successful typing of "mine":
  * Add product (quantity = 1) to cart
  * Create cart if none exists

### 5. Messenger Notification
* Sends a message to the user confirming:
  * Product name
  * Price
  * Quantity added

* Provides next actions (checkout, view cart, etc.)

---

## Initial Flow

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
   * either notify user or ignore (to decide on)

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

---

## Specifics

* Each Facebook post will represent a single product (might change on per image)
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
- [ ] Create Facebook test page and sample product posts
- [ ] Configure Facebook webhook to n8n
- [ ] Implement comment trigger logic in n8n
- [ ] Implement cart creation and item addition
- [ ] Send Messenger confirmation message
- [ ] Test end-to-end flow

---

