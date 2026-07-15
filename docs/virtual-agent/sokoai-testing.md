# Step 6: Testing the Workflow

Now that the SokoAI agent is deployed, use the built-in **Preview pane** to verify that all agents and routing work correctly before going live.

---

## How to Open the Preview Pane

In Agent Builder, click the **Preview** button on the right side of the screen. A chat interface will appear where you can simulate real user conversations.

---

## Test Scenarios

Work through each scenario below to verify the full system. Each test checks a different agent's routing and functionality.

---

### Test 1: Session Initialisation (Root Agent)

**Goal:** Confirm the root agent greets the user and calls `getCustomerInfo`.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | `Nothing` | Root agent greets you. If a session exists, it uses your name. If not, it gives a generic welcome. |

---

### Test 2: Grocery Routing (Marketplace Agent)

**Goal:** Confirm the root agent silently transfers grocery requests to the Marketplace Agent, which returns a product carousel.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | `I need some flour` | Root agent does **not** respond. Marketplace Agent immediately takes over and shows a product carousel. |
| 2 | Select a product from the carousel | Marketplace Agent calls `addToCart` and confirms the item was added. |
| 3 | `What's in my cart?` | Marketplace Agent calls `getCartSummary` and returns a conversational summary with subtotal. |
| 4 | `Continue to checkout` | Marketplace Agent calls `initiateCheckout` and returns a payment link. |
| 5 | `Check order status` | Marketplace Agent calls `getOrderStatus` and returns current payment/delivery status. |

---

### Test 3: Automotive Parts Routing (Carparts Agent)

**Goal:** Confirm vehicle-specific parts searches route to the Carparts Agent and return correct fitment results.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | `I need a battery for my 2016 Mazda Demio` | Carparts Agent takes over and shows a product carousel with relevant batteries. |
| 2 | Select a battery from the carousel | Carparts Agent calls `addToCart` and confirms. |
| 3 | `Total` | Carparts Agent calls `getCartSummary` and returns the subtotal. |

---

### Test 4: Financial Query Routing (Finance Agent)

**Goal:** Confirm financial queries route to the Finance Agent, which uses tools and mirrors the user's language.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | `What are the best MMFs right now?` | Finance Agent calls `finance_table` or `lookup_finance` and responds with results. Response ends with the compliance note. |
| 2 | `idk mahn what do you recommend?` | Finance Agent responds in a matching casual tone, within the 40-word limit. |

---

### Test 5: Order Tracking (Accounts Agent)

**Goal:** Confirm order status and delivery tracking work through the Accounts Agent.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | `Where is my order?` | Accounts Agent calls `getOrderStatus` and returns delivery location and status. |
| 2 | `What are your store hours?` | Accounts Agent responds using internal knowledge (no tool call needed). |

---

### Test 6: Cross-Agent Routing

**Goal:** Confirm that agents correctly hand off out-of-scope requests back to the Root Agent for re-routing.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | While in the Marketplace Agent: `I want to invest in bonds` | Marketplace Agent silently transfers to Root Agent, which then routes to Finance Agent. |
| 2 | While in the Carparts Agent: `I need tomatoes` | Carparts Agent silently transfers to Root Agent, which routes to Marketplace Agent. |

---

### Test 7: Policy & Human Escalation (Root Agent)

**Goal:** Confirm the root agent handles general queries and escalation requests correctly.

| Step | You type | Expected behaviour |
|---|---|---|
| 1 | `Do you give discounts?` | Root Agent responds with the discount message in English or Swahili depending on the question language. |
| 2 | `I want to speak to a human` | Root Agent responds explaining no live agent is currently available. |
| 3 | `Goodbye` | Root Agent calls `end_session` and closes the conversation with a farewell message. |

---

## Expected Final Result

- ✅ All routing is **silent** — the root agent never responds to product/cart/financial queries directly
- ✅ Product searches always return a **carousel widget**, never plain text lists
- ✅ Cart operations work end-to-end (add → view → checkout → confirm)
- ✅ Finance responses are **within 40 words** and always end with the compliance disclaimer
- ✅ The agent correctly **mirrors user language** (English / Swahili / Sheng)
- ✅ Session ends cleanly when the user says goodbye

!!! tip "Debugging Tips"
    - If routing fails, check that the sub-agent **descriptions** match the intent triggers in the root agent's XML instructions.
    - If tools return errors, verify the **Cloud Run service URLs** in each OpenAPI YAML are correct and the services are deployed.
    - If the carousel does not appear, confirm the `product_carousel` widget is connected to both `marketplace_agent` and `carparts_agent`.

---

!!! success "Lab Complete 🎉"
    Congratulations — you have successfully built, deployed, and tested the SokoAI multi-agent system! You now have a production-ready AI marketplace assistant running on Vertex AI Agent Builder.

---

<div class="page-nav">
  <div class="nav-item">
    <a href="../sokoai-deployment/" class="btn-secondary">← Previous: Deployment</a>
  </div>
  <div class="nav-item">
    <span><strong>SokoAI: Testing</strong></span>
  </div>
  <div class="nav-item">
    <a href="/end-of-training/" class="btn-primary">Next: End of Training →</a>
  </div>
</div>
