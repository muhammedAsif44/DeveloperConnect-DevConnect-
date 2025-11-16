## Chat Module - Comprehensive Testing Guide

### What Was Implemented

✅ Toast deduplication system
✅ Sidebar unread message indicators  
✅ Real-time online/offline status dots
✅ Mobile-responsive design
✅ Auto-sorting conversations (unread first)
✅ Auto-clear unread on open

---

## Testing Environment Setup

### Prerequisites
```bash
cd front-end
npm install
npm run dev
```

### Access Chat
```
URL: http://localhost:5173/developer-dashboard
Navigate to: Messages / Chat section
```

---

## Feature 1: Toast Deduplication

### Test Case 1.1: Error Toast Spam
**Steps**:
1. Open chat page
2. Click send without typing message (invalid input)
3. Repeat clicking send button rapidly (5+ times)
4. Check browser notifications

**Expected**:
- ❌ Should NOT see error toast 5 times
- ✅ Should see error toast only once per 2 seconds
- Console should show: `"Duplicate message detected"`

### Test Case 1.2: Network Error Simulation
**Steps**:
1. Open DevTools → Network tab
2. Set throttling to "Slow 3G"
3. Try sending a message
4. Observe retry behavior

**Expected**:
- ❌ Should NOT spam multiple error toasts
- ✅ Should show error once even if socket retries
- Messages auto-retry without toast spam

### Test Case 1.3: Multiple Different Errors
**Steps**:
1. Perform different error-causing actions
2. (e.g., invalid input → network error → permission error)

**Expected**:
- ✅ Each DIFFERENT error should show
- ❌ Same error should not repeat within 2 seconds
- Should see 3 different toasts, not duplicates

---

## Feature 2: Unread Message Indicators

### Test Case 2.1: New Message Notification
**Requires**: Two browser windows or devices with two user accounts

**Steps**:
1. User A: Keep chat window in background
2. User B: Send message to User A
3. User A: Look at sidebar

**Expected**:
- ✅ Unread badge appears next to User B's name
- ✅ Small blue pulsing dot shows on their avatar
- ✅ Left border highlights the conversation
- ❌ No visual indicator if User A is in that conversation

### Test Case 2.2: Auto-Highlight Clear
**Steps**:
1. Continue from Test 2.1 (conversation has unread)
2. User A: Click on User B's conversation to open it

**Expected**:
- ✅ Unread indicator disappears immediately
- ✅ Highlight border removes
- ✅ Blue dot stops pulsing
- ✅ Conversation stays selected (blue background)

### Test Case 2.3: Conversation Sorting
**Requires**: 3+ friends

**Steps**:
1. Have 3 conversations: Friends A, B, C (all offline)
2. B sends you a message
3. Look at sidebar order

**Expected**:
- ✅ Friend B moves to top (has unread)
- ✅ Order: B (unread), A, C, or A, C (depending on previous order)
- ❌ Friend B should NOT stay at original position

### Test Case 2.4: Multiple Unread
**Steps**:
1. Have conversations with Friends A, B, C
2. Friend A sends message (you don't read it)
3. Friend C sends message (you don't read it)
4. Check sidebar

**Expected**:
- ✅ Both A and C show unread indicators
- ✅ A and C are at top (sorted by unread)
- ✅ Each has independent unread badge
- ✅ Opening either one clears just that indicator

---

## Feature 3: Online Status Indicators

### Test Case 3.1: Online Dot Appears
**Requires**: Two accounts

**Steps**:
1. User A: Open chat and stay on page
2. User B: Open chat on same/different device
3. User A: Check User B's avatar in sidebar

**Expected**:
- ✅ Green dot appears bottom-right of User B's avatar
- ✅ Green dot appears in chat header when B is selected
- ✅ Dot is visible immediately (< 1 second)
- ❌ Dot should NOT be gray (offline color)

### Test Case 3.2: Offline Dot Appears
**Steps**:
1. User B: Close chat or go offline
2. User A: Wait 5-10 seconds
3. User A: Check User B's avatar

**Expected**:
- ✅ Green dot changes to gray
- ✅ Header shows "Offline" or no "Online" badge
- ✅ Transition happens in real-time
- ❌ Dot should NOT remain green

### Test Case 3.3: Online Status in Header
**Steps**:
1. User B: Send message to User A
2. User A: Click on User B's conversation
3. Look at chat header

**Expected**:
- ✅ User B's avatar shows green dot
- ✅ User B's role badge appears (Mentor/Developer)
- ✅ Header shows "• Online" text if online
- ✅ Status updates if User B goes offline

### Test Case 3.4: Online Count Updates
**Steps**:
1. Check header "X Users Online" counter
2. User B: Log in another account
3. Check counter again

**Expected**:
- ✅ Counter increases by 1
- ✅ Updates in real-time
- ❌ Counter should NOT be stale

---

## Feature 4: Mobile Responsiveness

### Test Case 4.1: Mobile Layout (320-425px)
**Device**: iPhone SE, iPhone 12 mini (320px width)

**Steps**:
1. Open chat page on mobile device
2. Look at layout

**Expected**:
- ✅ Sidebar NOT visible initially
- ✅ Chat area takes full width
- ✅ Header has menu icon (☰)
- ✅ All text readable without zoom
- ✅ No horizontal scrolling

### Test Case 4.2: Sidebar Toggle Mobile
**Steps**:
1. Click menu icon (☰) in header on mobile
2. Observe sidebar

**Expected**:
- ✅ Sidebar slides in from left
- ✅ Semi-transparent backdrop appears
- ✅ Can see both sidebar and chat (overlay)
- ✅ Click backdrop to close sidebar

### Test Case 4.3: Select Friend on Mobile
**Steps**:
1. Click menu to open sidebar
2. Click a friend name
3. Observe sidebar

**Expected**:
- ✅ Conversation opens
- ✅ Sidebar automatically closes
- ✅ Chat takes full width
- ✅ Can re-open sidebar with menu icon

### Test Case 4.4: Responsive Text Sizing
**Steps**:
1. View chat on: Mobile (320px), Tablet (768px), Desktop (1920px)
2. Check text sizes

**Expected**:
- ✅ Text scales smoothly at each breakpoint
- ✅ Mobile: All text readable (no zoom needed)
- ✅ Desktop: Proper spacing, not too small
- ✅ Tablet: Between mobile and desktop

### Test Case 4.5: Input Area Mobile
**Steps**:
1. On mobile, scroll down in chat
2. Type in input

**Expected**:
- ✅ Input stays at bottom (sticky)
- ✅ Input doesn't scroll up with messages
- ✅ Send button stays clickable
- ✅ Emoji and attachment buttons visible

### Test Case 4.6: Message Overflow Mobile
**Steps**:
1. Send a long message (100+ characters)
2. Receive a long message

**Expected**:
- ✅ Text wraps properly (no horizontal scroll)
- ✅ Message bubble doesn't overflow
- ✅ Time stamp visible
- ✅ Readable at all breakpoints

### Test Case 4.7: Tablet Layout (768px)
**Device**: iPad, Samsung Tab

**Steps**:
1. Open chat on tablet
2. Rotate landscape

**Expected**:
- ✅ Sidebar always visible (not hidden)
3. ✅ Side-by-side layout works
- ✅ No hamburger menu needed
- ✅ Full use of width
- ✅ Landscape and portrait work

### Test Case 4.8: Desktop Layout (1920px)
**Device**: Desktop monitor, laptop

**Steps**:
1. Open chat on desktop
2. Maximize window

**Expected**:
- ✅ Sidebar visible on left
- ✅ Chat on right with full messages
- ✅ No mobile menu icon visible
- ✅ Proper spacing, not cramped
- ✅ Uses full available width

---

## Feature 5: UI Consistency

### Test Case 5.1: Color Consistency
**Steps**:
1. Compare colors across:
   - Header (#032f60)
   - Selected conversation (same blue)
   - Buttons
   - Badges

**Expected**:
- ✅ Primary color (#032f60) consistent everywhere
- ✅ Online green (#22c55e) consistent
- ✅ Selected state uses primary color
- ✅ Badges use role colors consistently

### Test Case 5.2: Rounded Corners
**Steps**:
1. Check border radius on:
   - Main containers (rounded-3xl)
   - Buttons (rounded-2xl)
   - Avatar images (rounded-xl)
   - Input (rounded-2xl)

**Expected**:
- ✅ All elements have rounded corners
- ✅ Header rounded-b-3xl (bottom only)
- ✅ Messages rounded-3xl
- ✅ Consistent with design system

### Test Case 5.3: Spacing Consistency
**Steps**:
1. Measure spacing in:
   - Between sidebar items (p-3 md:p-4)
   - Around header (p-4 md:p-6)
   - Between messages (mb-3)

**Expected**:
- ✅ Consistent gap values (gap-2, gap-4)
- ✅ Consistent padding (p-2, p-4, p-6)
- ✅ Proper margins between elements
- ✅ No arbitrary spacing

### Test Case 5.4: Shadow Consistency
**Steps**:
1. Check shadows on:
   - Sidebar (shadow-lg)
   - Chat window (shadow-lg)
   - Cards (shadow-sm, shadow-md)

**Expected**:
- ✅ Shadows use consistent classes
- ✅ Elevated sections have shadow-lg
- ✅ Subtle elements use shadow-sm
- ✅ Hover states enhance shadows

---

## Feature 6: Performance Testing

### Test Case 6.1: Load Performance
**Steps**:
1. Open DevTools → Performance tab
2. Record page load
3. Check metrics

**Expected**:
- ✅ First Contentful Paint (FCP) < 3s
- ✅ Largest Contentful Paint (LCP) < 4s
- ✅ No layout shifts during load
- ✅ Smooth scrolling (60fps)

### Test Case 6.2: Message Scrolling
**Steps**:
1. Load chat with 50+ messages
2. Scroll up/down rapidly
3. Check DevTools Performance

**Expected**:
- ✅ Smooth 60fps scrolling
- ❌ No jank or stuttering
- ✅ GPU acceleration visible
- ✅ Memory usage stable

### Test Case 6.3: Toast Deduplication Memory
**Steps**:
1. Trigger 100+ errors in sequence
2. Check DevTools Memory tab
3. Force garbage collection

**Expected**:
- ✅ Memory doesn't grow excessively
- ✅ Old toast entries cleaned up
- ✅ No memory leaks
- ✅ Queue maintains max 50 entries

---

## Feature 7: Accessibility Testing

### Test Case 7.1: Keyboard Navigation
**Steps**:
1. Close mouse/trackpad
2. Use only Tab/Arrow keys
3. Navigate: Header → Sidebar → Chat → Input

**Expected**:
- ✅ All buttons accessible via Tab
- ✅ Enter key activates buttons
- ✅ Focus visible (blue outline)
- ✅ No focus traps
- ✅ Tab order logical

### Test Case 7.2: Screen Reader
**Steps**:
1. Enable screen reader (NVDA, JAWS, VoiceOver)
2. Navigate chat page
3. Try sending message

**Expected**:
- ✅ Header announced properly
- ✅ Friend names read out
- ✅ Online status described
- ✅ Button purposes clear
- ✅ Error messages announced

### Test Case 7.3: Color Contrast
**Steps**:
1. Use contrast checker tool
2. Check text on backgrounds:
   - White text on #032f60
   - Gray text on white
   - Green dot on light background

**Expected**:
- ✅ All ratios ≥ 4.5:1 (AAA standard)
- ✅ All text readable
- ✅ Sufficient contrast

---

## Integration Testing

### Test Case I.1: Full Chat Flow
**Steps**:
1. User A & B both online
2. User A sends message to User B
3. User B sees notification + unread
4. User B clicks conversation
5. Notification clears
6. User B sends reply
7. User A sees it in real-time

**Expected**:
- ✅ All steps work seamlessly
- ✅ Messages sync instantly
- ✅ Unread indicators update
- ✅ No errors in console

### Test Case I.2: Network Disruption
**Steps**:
1. Start sending message
2. Turn off network (DevTools → Offline)
3. Try sending
4. Turn network back on

**Expected**:
- ✅ Error toast shows (once)
- ❌ No spam
- ✅ Message queues/retries
- ✅ Sends when network back

### Test Case I.3: Multiple Users Online
**Steps**:
1. Have 3+ users online
2. Each sends messages
3. Check header counter
4. Each goes offline one by one

**Expected**:
- ✅ Counter accurate
- ✅ Dots update in real-time
- ✅ Conversations sync properly
- ✅ No duplicate messages

---

## Regression Testing

### Areas to Check After Changes

1. **Toast Manager**
   - Different error types still shown
   - Deduplication still works
   - Non-chat parts not affected

2. **Sidebar**
   - Search still works
   - Sorting by online/unread
   - Mobile toggle works

3. **Chat Window**
   - Messages render correctly
   - Scrolling smooth
   - Typing indicator appears

4. **Input**
   - Send button works
   - Placeholder visible
   - Emoji/attachment buttons respond

---

## Checklist Summary

### Must Pass
- [ ] No duplicate toasts
- [ ] Unread badges appear
- [ ] Online dots show
- [ ] Mobile layout responsive
- [ ] No console errors
- [ ] Messages sync properly

### Should Pass
- [ ] Smooth animations
- [ ] Good performance
- [ ] Proper spacing
- [ ] Color consistency
- [ ] Keyboard accessible

### Nice to Have
- [ ] Sorting works perfectly
- [ ] Screen reader friendly
- [ ] Mobile optimized
- [ ] Touch-friendly sizes
- [ ] No flicker on load

---

## Bug Report Template

If you find issues, report with:

```
Feature: [Feature Name]
Test Case: [Test Case Number]
Device: [Device Type/Size]
OS: [Windows/Mac/iOS/Android]
Browser: [Chrome/Firefox/Safari]

Steps to Reproduce:
1. ...
2. ...

Expected:
[What should happen]

Actual:
[What actually happened]

Screenshots/Video:
[Attach if possible]

Console Errors:
[Any errors from DevTools]
```

---

## Questions?

Refer to:
- `CHAT_IMPROVEMENTS_SUMMARY.md` - What was built
- `TOAST_MANAGER_GUIDE.md` - Toast API usage
- `RESPONSIVE_DESIGN_SPECS.md` - Layout specifications
