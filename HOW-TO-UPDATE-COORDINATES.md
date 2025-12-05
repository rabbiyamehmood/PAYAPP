# How to Update Coordinates - POM Guide

## 🎯 Single Source of Truth

All UI coordinates are defined in **ONE FILE**:
```
.maestro/coordinates.yaml
```

When you update coordinates here, ALL flows automatically use the new coordinates!

## 📝 Step-by-Step Guide to Update Coordinates

### Example: Login PIN Input Field Moved

**Before:** PIN input at `11%, 55%`
**After:** PIN input moved to `15%, 60%`

### Step 1: Open the Master Coordinates File
```bash
.maestro/coordinates.yaml
```

### Step 2: Find the Coordinate Section
```yaml
# ============================================
# LOGIN PAGE COORDINATES
# ============================================
env:
  # PIN Input Field
  LOGIN_PIN_INPUT_X: 11%    # ← OLD VALUE
  LOGIN_PIN_INPUT_Y: 55%    # ← OLD VALUE
```

### Step 3: Update the Values
```yaml
# ============================================
# LOGIN PAGE COORDINATES
# ============================================
env:
  # PIN Input Field
  LOGIN_PIN_INPUT_X: 15%    # ← NEW VALUE
  LOGIN_PIN_INPUT_Y: 60%    # ← NEW VALUE
```

### Step 4: Save the File
That's it! No need to update any flow files.

### Step 5: Test
```bash
maestro test .maestro/flows/first-login-flow.yaml
```

## 📋 All Available Coordinates

### Login Page
```yaml
LOGIN_PIN_INPUT_X: 11%
LOGIN_PIN_INPUT_Y: 55%
```

### Signup Page
```yaml
# Initial Navigation
SIGNUP_CHECKBOX_X: 8%
SIGNUP_CHECKBOX_Y: 81%
SIGNUP_CONTINUE_X: 50%
SIGNUP_CONTINUE_Y: 90%

# Name Input
NAME_INPUT_X: 51%
NAME_INPUT_Y: 43%
NAME_NEXT_X: 50%
NAME_NEXT_Y: 63%

# Mobile Number
MOBILE_NEXT_X: 50%
MOBILE_NEXT_Y: 63%
```

### PIN Entry Page
```yaml
# Enter PIN
ENTER_PIN_INPUT_X: 17%
ENTER_PIN_INPUT_Y: 41%

# Re-enter PIN
REENTER_PIN_INPUT_X: 17%
REENTER_PIN_INPUT_Y: 54%
```

### Terms & Conditions Page
```yaml
TERMS_CHECKBOX_X: 8%
TERMS_CHECKBOX_Y: 81%
TERMS_CONTINUE_X: 50%
TERMS_CONTINUE_Y: 90%
TERMS_SCROLL_COUNT: 38
```

### Fingerprint Page
```yaml
FINGERPRINT_BUTTON_X: 67%
FINGERPRINT_BUTTON_Y: 58%
```

### Completion Page
```yaml
COMPLETE_BACK_X: 50%
COMPLETE_BACK_Y: 72%
```

## 🔧 Common Update Scenarios

### Scenario 1: Button Position Changed
**Problem:** Signup Continue button moved from 50%,90% to 50%,85%

**Solution:**
```yaml
SIGNUP_CONTINUE_X: 50%
SIGNUP_CONTINUE_Y: 85%  # Changed from 90%
```

### Scenario 2: New Screen Resolution
**Problem:** Testing on tablet, all coordinates need adjustment

**Solution:** Update all coordinates in `coordinates.yaml` at once

### Scenario 3: PIN Entry Field Relocated
**Problem:** PIN input moved on both Entry and Re-entry screens

**Solution:**
```yaml
ENTER_PIN_INPUT_X: 20%     # New position
ENTER_PIN_INPUT_Y: 45%     # New position
REENTER_PIN_INPUT_X: 20%   # New position
REENTER_PIN_INPUT_Y: 58%   # New position
```

### Scenario 4: Less Scrolling Needed
**Problem:** Terms page now needs only 20 scrolls instead of 38

**Solution:**
```yaml
TERMS_SCROLL_COUNT: 20  # Changed from 38
```

## 🎨 How to Find New Coordinates

### Method 1: Maestro Studio
```bash
maestro studio
```
- Open the flow
- Click on elements
- Coordinates shown in inspector

### Method 2: Trial and Error
```bash
# Quick test with new coordinates
maestro test .maestro/flows/complete-signup-flow.yaml
```

### Method 3: Record New Flow
```bash
maestro record
```
- Perform actions
- Get coordinates from generated YAML

## ✅ Benefits of This Approach

| Before POM | After POM with Coordinates |
|------------|----------------------------|
| Update 10+ files | Update 1 file |
| Find/replace manually | Change once |
| Risk of missing files | Guaranteed consistency |
| Hard to maintain | Easy to maintain |

## 📊 Impact Analysis

When you update `coordinates.yaml`, these flows are automatically updated:

✅ `.maestro/flows/complete-signup-flow.yaml`
✅ `.maestro/flows/first-login-flow.yaml`
✅ Any new flows that import coordinates

## 🚨 Best Practices

### DO:
✅ Always update `coordinates.yaml` first
✅ Test after updating coordinates
✅ Add comments for complex coordinates
✅ Keep coordinate names descriptive
✅ Version control `coordinates.yaml`

### DON'T:
❌ Hardcode coordinates in flow files
❌ Create duplicate coordinate variables
❌ Forget to update related coordinates
❌ Delete coordinate definitions (comment them instead)

## 🔍 Quick Reference

**File to Update:**
```
.maestro/coordinates.yaml
```

**Files That Auto-Update:**
```
.maestro/flows/complete-signup-flow.yaml
.maestro/flows/first-login-flow.yaml
```

**Simple Files (NOT using coordinates):**
```
signup.yaml                    # Still uses hardcoded coordinates
first-login.yaml              # Still uses hardcoded coordinates
```

## 💡 Pro Tips

1. **Use Variables for Repeated Values**
   ```yaml
   CONTINUE_BUTTON_X: 50%
   CONTINUE_BUTTON_Y: 90%
   # Reuse these for multiple "Continue" buttons
   ```

2. **Add Descriptive Comments**
   ```yaml
   LOGIN_PIN_INPUT_X: 11%  # Left-aligned PIN pad on login screen
   ```

3. **Group Related Coordinates**
   ```yaml
   # PIN Entry Fields - All PIN-related coordinates together
   ENTER_PIN_INPUT_X: 17%
   ENTER_PIN_INPUT_Y: 41%
   ```

4. **Test After Major Updates**
   ```bash
   # Run all flows to verify
   maestro test .maestro/flows/
   ```

## 🎓 Example: Real-World Update

### Scenario: App Redesign
The app got redesigned. Multiple buttons moved.

### OLD coordinates.yaml:
```yaml
SIGNUP_CONTINUE_Y: 90%
TERMS_CONTINUE_Y: 90%
```

### NEW coordinates.yaml:
```yaml
SIGNUP_CONTINUE_Y: 85%  # Moved up
TERMS_CONTINUE_Y: 85%   # Moved up
```

### Result:
✅ 2 flows automatically fixed
✅ 0 manual edits to flow files
✅ 2 minutes to update
✅ Consistent across all tests

---

## 📞 Need Help?

Check `PROJECT-STRUCTURE.md` for more information about the POM structure.
