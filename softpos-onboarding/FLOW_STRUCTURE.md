# SoftPOS Onboarding Flow Structure

## Overview
This is an end-to-end automated flow for merchant onboarding in the SoftPOS (Tap Pay) application. The flow follows a multi-step form submission process with document capture and verification.

---

## Flow Architecture

### **Project Type**: Page Object Model (POM)
- **Main Orchestrator**: `softpos-onboarding-pom.yaml`
- **Page Objects**: Located in `pages/` directory
- **Monolithic Backup**: `new-softpos.yaml` (standalone version)

---

## Flow Sequence (7 Major Stages)

### **Stage 1: Initial Setup**
**Location**: `softpos-onboarding-pom.yaml` (lines 6-9)

**Actions**:
1. Tap on start button at coordinates (17%, 69%)
2. Tap "OK" to proceed

**Purpose**: Launch the onboarding process

---

### **Stage 2: Shop/Business Details**
**Page Object**: `pages/shop-details.yaml`

**Data Required**:
- Shop Name (text input)
- Legal Entity (dropdown selection)
- Expected Revenue per month (numeric input)
- Nature of Business (dropdown - "Fast food restaurants-5814")
- Store Location (dropdown - city selection)
- Store Address (text input)

**Flow Steps**:
1. Enter shop name → Input field at (51%, 30%)
2. Select legal entity → Tap sequence: (50%, 41%) → (89%, 41%) → (50%, 46%)
3. Enter revenue → Input at (51%, 52%) + Hide keyboard
4. Select business nature → Tap (89%, 63%) → Select category
5. Select location → Tap (89%, 74%) → Select city
6. Enter address → Input at (50%, 85%) + Hide keyboard
7. Scroll and submit → Tap (50%, 89%) → Scroll → Tap (50%, 89%)

**Technical Details**:
- Uses keyboard "Back" button to dismiss keyboard
- Requires scroll to reveal submit button
- Parameters passed via `env` variables

---

### **Stage 3: Personal/KYC Details**
**Page Object**: `pages/personal-details.yaml`

**Data Required**:
- Email Address
- CNIC (National ID - 13 digits)
- Mother's Maiden Name
- Father's Maiden Name
- Place of Birth (city)
- Date of Birth (date picker - year, day)
- CNIC Date of Issue (date picker - year, month, day)

**Flow Steps**:
1. Enter email → (50%, 29%)
2. Enter CNIC → (50%, 40%)
3. Enter mother's name → (50%, 51%)
4. Enter father's name → (51%, 62%) + Hide keyboard
5. Select place of birth → (89%, 72%) → Select city
6. Select DOB → (89%, 82%) → Year picker with `scrollUntilVisible` → Select day → OK
7. Select CNIC issue date → (89%, 91%) → Select year → Previous month → Select day → OK
8. Wait for date picker animation (2000ms)
9. Swipe down to reveal submit button (50%, 70% → 50%, 30%, 500ms duration)
10. Submit form → (50%, 89%)

**Technical Details**:
- **Date Picker Logic**: Uses Android system date picker (`android:id/text1`)
- **scrollUntilVisible**: For year selection (direction: UP)
- Requires manual navigation to previous month for CNIC issue date
- Animation waits between date selections

---

### **Stage 4: Photo Capture (Manual Process)**
**Page Object**: `pages/photo-capture.yaml`

**Required Photos** (3 captures):
1. **Selfie** (Face detection auto-capture)
2. **CNIC Front** (National ID front side)
3. **CNIC Back** (National ID back side)

**Flow Steps**:
Each photo capture follows:
1. Wait for camera screen to load (3000ms)
2. Wait for user to manually capture photo (15000ms)
3. Auto-advance to next screen

**Technical Details**:
- **Manual Intervention Required**: User must position face/document
- **Auto-capture**: Face detection automatically triggers selfie
- **Total Wait Time**: ~48 seconds (3 screens × 16 seconds each)
- No tap actions needed - fully automated waiting

---

### **Stage 5: Document Attachments**
**Page Object**: `pages/attach-files.yaml`

**Required Documents** (2 additional files):
- Additional document 1
- Additional document 2

**Flow Steps**:
1. Assert "Attach Files" screen visible
2. Tap attachment area → (50%, 76%)
3. **First Document**:
   - Select camera option → (20%, 49%)
   - Tap shutter button → `id: com.android.camera:id/shutter_button`
   - Confirm/Done → (76%, 86%)
4. **Second Document**:
   - Select camera option → (20%, 81%)
   - Tap shutter button → `id: com.android.camera:id/shutter_button`
   - Confirm/Done → (76%, 86%)
5. Scroll down
6. Tap "Next" button (text selector)

**Technical Details**:
- Uses camera app ID selector for shutter button
- Requires scroll before "Next" button becomes visible
- Manual user action needed for document capture

---

### **Stage 6: Bank Details**
**Page Object**: `pages/bank-details.yaml`

**Data Required**:
- Bank Name (dropdown selection)
- IBAN (International Bank Account Number)
- Account Title (account holder name)

**Flow Steps**:
1. Assert "Select Bank Name" visible
2. Tap bank dropdown → (89%, 31%)
3. Wait for dropdown animation (2000ms)
4. **Scroll to Bank** (3 controlled swipes):
   - Swipe 1: (50%, 65%) → (50%, 45%), duration 400ms, wait 800ms
   - Swipe 2: (50%, 65%) → (50%, 45%), duration 400ms, wait 800ms
   - Swipe 3: (50%, 65%) → (50%, 45%), duration 400ms, wait 1000ms
5. Select bank by text → "Faysal Bank Limited"
6. Wait for selection (2500ms)
7. Enter IBAN → (50%, 42%) + Hide keyboard
8. Enter account title → (50%, 53%) + Hide keyboard

**Technical Details**:
- **Bank Position**: Faysal Bank at index 16 (requires scrolling)
- **Scroll Strategy**: 3 gentle swipes with precise timing
- Uses text selector for bank name (dynamic selection)
- Longer wait times for dropdown animations

---

### **Stage 7: Final Submission**
**Page Object**: `pages/final-submission.yaml`

**Data Required**:
- Transaction Amount Range (dropdown)

**Flow Steps**:
1. Tap amount dropdown → (50%, 88%)
2. Select amount range → "Rs 25 - 50k" (text selector)
3. Tap "Request a SoftPOS" button
4. Assert Terms & Conditions visible
5. Accept T&C checkbox → (8%, 79%)
6. Submit form → (50%, 91%)
7. **Verification**:
   - Assert "Enter your OTP" visible
   - Assert "Congratulations" visible

**Technical Details**:
- Uses text selectors for amount and button
- Final assertions confirm successful submission
- OTP screen indicates completion

---

## Data Flow Architecture

```
softpos-onboarding-pom.yaml (Main Orchestrator)
    ↓
    ├─→ pages/shop-details.yaml (env: shopName, revenue, storeLocation, storeAddress)
    ├─→ pages/personal-details.yaml (env: email, cnic, motherName, fatherName, placeOfBirth, birthYear, birthDay, issueYear, issueDay)
    ├─→ pages/photo-capture.yaml (no params - pure wait logic)
    ├─→ pages/attach-files.yaml (no params - manual capture)
    ├─→ pages/bank-details.yaml (env: bankName, iban, accountTitle)
    └─→ pages/final-submission.yaml (env: transactionAmount)
```

---

## Technical Specifications

### **Test Data Structure** (in main POM file)
```yaml
# Example current values
shopName: "burger boss shop"
revenue: "50000"
storeLocation: "Karachi"
storeAddress: "Shop #123 bhadurabad street"
email: "bossburger@gmail.com"
cnic: "4550051452576"
birthYear: "2000"
birthDay: "31"
issueYear: "2019"
issueDay: "19"
bankName: "Faysal Bank Limited"
iban: "PK08FAYS0110101001298755"
accountTitle: "Burger Boss Shop"
transactionAmount: "Rs 25 - 50k"
```

### **Timing & Performance**
- **Total Duration**: ~8 minutes (without speed optimization)
- **Manual Interaction Time**: ~48 seconds (photo captures)
- **Animation Waits**: ~10 seconds total
- **Form Filling Time**: ~5-6 minutes

### **Element Selection Strategies**
1. **Coordinates** (70%): Used for input fields, buttons, checkboxes
2. **Text Selectors** (20%): Used for dropdown options, bank names, buttons
3. **ID Selectors** (10%): Used for Android system elements (date picker, camera shutter)

### **Error Handling**
- `assertVisible` checks before major actions
- `waitForAnimationToEnd` for UI transitions
- Controlled swipe durations to prevent overshooting

---

## File Structure

```
softpos-onboarding/
├── softpos-onboarding-pom.yaml       # Main POM orchestrator
├── new-softpos.yaml                   # Monolithic backup (251 lines)
├── maestro-config.yaml                # Configuration (env variables reference)
├── run-with-speed-optimization.ps1   # Speed optimization script
├── README.md                          # Documentation
└── pages/                             # Page Object files
    ├── shop-details.yaml              # Stage 2: Business info
    ├── personal-details.yaml          # Stage 3: KYC info
    ├── photo-capture.yaml             # Stage 4: Photo captures
    ├── attach-files.yaml              # Stage 5: Document attachments
    ├── bank-details.yaml              # Stage 6: Banking info
    └── final-submission.yaml          # Stage 7: Submit & verify
```

---

## Execution Commands

### **Standard Execution**
```powershell
maestro test softpos-onboarding-pom.yaml
```

### **With Configuration**
```powershell
maestro test softpos-onboarding-pom.yaml --config maestro-config.yaml
```

### **With Speed Optimization** (using script)
```powershell
.\run-with-speed-optimization.ps1
```

### **Monolithic Version**
```powershell
maestro test new-softpos.yaml
```

---

## Best Practices Implemented

1. ✅ **Modular Design**: Each stage in separate page object
2. ✅ **Parameterization**: All test data passed via environment variables
3. ✅ **Reusability**: Page objects can be called with different data
4. ✅ **Maintainability**: Changes to one page don't affect others
5. ✅ **Clear Comments**: Each section documented inline
6. ✅ **Assertion Strategy**: Validates screen state before actions
7. ✅ **Wait Strategy**: Appropriate timeouts for animations
8. ✅ **Backup Flow**: Monolithic file as fallback

---

## Known Limitations & Considerations

1. **Manual Intervention**: Photo/document captures require user action
2. **Hardcoded Waits**: 15-second waits for photo captures (may need adjustment)
3. **Bank Position Dependency**: Assumes Faysal Bank at position 16
4. **Camera ID Selector**: Must use `com.android.camera:id/shutter_button` (device-specific)
5. **Date Picker Logic**: Requires navigation to previous month for CNIC issue date
6. **Fixed Coordinates**: Screen size changes may require coordinate updates

---

## Success Criteria

**Test Passes When**:
- ✅ All form fields populated correctly
- ✅ All 3 photos captured
- ✅ 2 documents attached
- ✅ Bank details submitted
- ✅ "Enter your OTP" screen visible
- ✅ "Congratulations" message displayed

---

## Maintenance Notes

- **Camera Shutter Button**: Keep using ID selector, not text
- **Date Picker**: Use `scrollUntilVisible` with `android:id/text1`
- **Bank Selection**: Maintain 3-swipe approach for Faysal Bank
- **Screenshot Commands**: Removed for speed (can re-enable for debugging)
- **Animation Scales**: Can be reduced for speed (currently at 1.0x)
