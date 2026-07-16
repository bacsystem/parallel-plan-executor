# Sample Implementation Plan

### Task 1: Widget core

**Files:**
- Create: `src/widget.js`
- Test: `tests/widget.test.js`

**Interfaces:**
- Consumes:
- Produces: `createWidget(name)`

- [ ] **Step 1: Write the failing test**

### Task 2: Gadget core

**Files:**
- Create: `src/gadget.js`
- Test: `tests/gadget.test.js`

**Interfaces:**
- Consumes:
- Produces: `createGadget(name)`

- [ ] **Step 1: Write the failing test**

### Task 3: Widget-Gadget bridge

**Files:**
- Create: `src/bridge.js`
- Modify: `src/widget.js`
- Test: `tests/bridge.test.js`

**Interfaces:**
- Consumes: `createWidget`, `createGadget`
- Produces: `bridge(widget, gadget)`

- [ ] **Step 1: Write the failing test**
