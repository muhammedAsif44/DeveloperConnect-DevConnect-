## Quick Reference: Toast Manager Implementation

### Global Toast Manager Location
`src/utils/toastManager.js`

### Available Functions

```javascript
import { showError, showSuccess, showLoading, showToast, clearAllToasts } from "../../utils/toastManager";

// Error toast (5 second duration, auto-deduplicates)
showError("Failed to send message!");

// Success toast (3 second duration)
showSuccess("Message sent successfully!");

// Loading toast (no auto-dismiss)
showLoading("Processing your request...");

// Generic toast with custom type and duration
showToast("Custom message", "success", 3000);

// Clear all toasts immediately
clearAllToasts();
```

---

## Key Features

✅ **Automatic Deduplication**
- Identical messages shown only once per 2-second window
- Prevents toast spam from socket retries and API errors

✅ **Memory Efficient**
- Tracks up to 50 unique messages
- Automatically cleans up old entries
- No memory leaks

✅ **Easy Integration**
- Drop-in replacement for `toast.error()` and `toast.success()`
- Same styling as existing toasts
- Works with react-hot-toast

---

## Replace Existing Toast Calls

### In ChatPage.jsx
```javascript
// ❌ Before
import toast from "react-hot-toast";
toast.error("User ID or Friend ID missing!");
toast.error("Could not create or obtain a conversation!");
toast.error("Error creating or fetching conversation!");

// ✅ After
import { showError } from "../../utils/toastManager";
showError("User ID or Friend ID missing!");
showError("Could not create or obtain a conversation!");
showError("Error creating or fetching conversation!");
```

### In ChatSidebar.jsx
```javascript
// ❌ Before
import toast from "react-hot-toast";
toast.error("User ID or Friend ID missing!");
toast.error("Failed to start conversation");

// ✅ After
import { showError } from "../../../utils/toastManager";
showError("User ID or Friend ID missing!");
showError("Failed to start conversation");
```

### In Other Components
Use the same pattern:
1. Import `showError`, `showSuccess`, or `showLoading` from `src/utils/toastManager`
2. Replace `toast.error()` with `showError()`
3. Replace `toast.success()` with `showSuccess()`
4. Replace `toast.loading()` with `showLoading()`

---

## Example Usage Across App

### API Error Handling
```javascript
try {
  const response = await api.post('/endpoint', data);
  showSuccess("Operation successful!");
} catch (error) {
  showError(error.response?.data?.message || "Something went wrong!");
}
```

### Form Validation
```javascript
const handleSubmit = (data) => {
  if (!data.email) {
    showError("Email is required");
    return;
  }
  // Process form...
  showSuccess("Form submitted!");
};
```

### Loading States
```javascript
const handleDelete = async (id) => {
  showLoading("Deleting...");
  try {
    await api.delete(`/item/${id}`);
    showSuccess("Deleted successfully!");
  } catch (error) {
    showError("Failed to delete");
  }
};
```

---

## Testing Deduplication

To verify deduplication is working:

1. Open browser console
2. Trigger the same error multiple times (e.g., send invalid data)
3. You should see the error toast only appear once per 2 seconds
4. Check console logs for deduplication messages

Example console output:
```
Duplicate message detected: "Failed to send message!"
Toast queue size: 5
```

---

## Debounce Window

The 2-second debounce window means:
- Show error at time 0s
- Same error at 0.5s → blocked (hidden)
- Same error at 2.1s → shown (new window)

This prevents:
- ❌ Socket retry spam (common with unreliable networks)
- ❌ Multiple API error responses
- ❌ User accidentally clicking multiple times
- ❌ Form validation firing repeatedly

---

## Customization

To adjust debounce window, edit `src/utils/toastManager.js`:

```javascript
const DEBOUNCE_WINDOW = 2000; // Change this value (in milliseconds)
```

**Common values:**
- 1000 = 1 second (aggressive deduplication)
- 2000 = 2 seconds (default, recommended)
- 3000 = 3 seconds (lenient)

---

## Integration Timeline

Gradually replace all toasts in the app:

1. ✅ **Phase 1**: Chat module (already done)
2. Next: Dashboard components
3. Then: Profile & settings
4. Finally: All other pages

This way, critical functionality is covered first, and you can test gradually.

---

## Support

If toasts aren't deduplicating:
1. Check import path is correct
2. Verify exact message text matches
3. Check that messages are within 2-second window
4. Look at browser console for errors

If you need to force show duplicate immediately:
```javascript
clearAllToasts(); // Clear history
showError("Same message"); // Show even if just shown
```
