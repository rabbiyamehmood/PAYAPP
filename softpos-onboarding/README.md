# PAYAPP - SoftPOS Onboarding Automation

This repo contains the PAYAPP MAESTRO AUTOMATION happy flows for SoftPOS onboarding.

## 🏗️ Architecture Pattern

This project implements a **Maestro Flow-Based Page Object Model (POM)**, which is a variation of the traditional POM pattern adapted for Maestro's YAML-based testing framework.

### Pattern Characteristics:

**Traditional POM (Selenium/Appium):**
```
Class-based → Methods represent actions → Object instantiation
```

**Maestro Flow-Based POM:**
```
YAML files → Commands represent actions → Flow composition with runFlow
```

### Key Components:

1. **Page Objects** (`pages/*.yaml`) - Reusable YAML files containing page-specific commands
2. **Main Flow** (`softpos-onboarding-pom.yaml`) - Orchestrates page objects and passes data
3. **Parameterization** - Uses `env` variables passed to each `runFlow` command
4. **Composition** - Combines multiple page flows into complete test scenarios

---

## 📂 Project Structure

```
softpos-onboarding/
│
├── softpos-onboarding-pom.yaml        # Main flow file using POM pattern
│                                       # - Contains all test data in 'env' section
│                                       # - Orchestrates the flow by calling page objects
│                                       # - Run with: maestro test softpos-onboarding-pom.yaml
│
├── README.md                           # This documentation file
│
├── FLOW_STRUCTURE.md                   # Detailed flow structure and technical specs
│
└── pages/                              # Page Object files directory
    │
    ├── shop-details.yaml               # Shop information page
    │                                   # - Shop name, legal entity, revenue
    │                                   # - Nature of business, location, address
    │
    ├── personal-details.yaml           # Personal information page
    │                                   # - Email, CNIC, mother/father names
    │                                   # - Place of birth, date of birth
    │                                   # - CNIC date of issue
    │
    ├── photo-capture.yaml              # Photo/document capture page
    │                                   # - Selfie capture
    │                                   # - CNIC front capture
    │                                   # - CNIC back capture
    │
    ├── attach-files.yaml               # Additional file attachments page
    │                                   # - Camera operations for extra documents
    │
    ├── bank-details.yaml               # Banking information page
    │                                   # - Bank name selection (with scroll)
    │                                   # - IBAN number entry
    │                                   # - Account title entry
    │
    └── final-submission.yaml           # Final submission and verification
                                        # - Transaction amount selection
                                        # - Terms & conditions acceptance
                                        # - OTP and completion verification
```

## 🎯 Benefits of Page Object Model (POM)

### 1. **Improved Readability & Abstraction**
   - Each page is isolated in its own file
   - Clear separation of concerns
   - Easy to understand what each page does

### 2. **Reduced Duplication**
   - Common actions are defined once per page
   - Reusable across multiple test flows
   - No need to copy-paste element locators

### 3. **Easy Maintenance**
   - Update an element in ONE place (page file)
   - Changes cascade to all flows using that page
   - Saves time when UI elements change
   - Easier debugging when issues occur

### 4. **Parameterization**
   - Test data stored centrally in `env` section
   - Variables like `${shopName}`, `${email}` used throughout
   - Easy to create multiple test scenarios with different data

### 5. **Scalability**
   - Add new pages without touching existing ones
   - Create multiple flows using same page objects
   - Team collaboration is easier

## 🚀 How to Use

### Run the automation:
```bash
maestro test softpos-onboarding-pom.yaml
```

### Modify test data:
Edit the `env` parameters in each `runFlow` call in `softpos-onboarding-pom.yaml`:
```yaml
# Example:
- runFlow:
    file: pages/shop-details.yaml
    env:
      shopName: Your Shop Name
      revenue: "75000"
      storeLocation: Lahore
      # ... etc
```

## 📝 Example: Updating an Element

If a button coordinate changes from `50%,88%` to `50%,90%`:

**Without POM:** Update in every test file where it appears ❌

**With POM:** Update once in the relevant page file ✅
```yaml
# pages/final-submission.yaml
- tapOn:
    point: 50%,90%  # Updated once, affects all flows
```

## 🔄 Creating New Test Flows

1. Create new main flow file (e.g., `negative-test.yaml`)
2. Define different test data in `env` section
3. Reuse existing page objects from `pages/` directory
4. No need to redefine page interactions!

## 📚 Best Practices

- Keep page objects focused on ONE page/screen
- Use meaningful variable names in `env` section
- Add comments to explain complex interactions
- Take screenshots for debugging (`takeScreenshot`)
- Use `waitForAnimationToEnd` for stability
- Group related actions in `runFlow` blocks

---

**Created:** December 2025  
**Pattern:** Page Object Model (POM)  
**Framework:** Maestro Mobile Testing  
**App:** SoftPOS Onboarding (com.tap_pay)
