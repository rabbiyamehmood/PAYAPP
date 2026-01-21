# MPOS Transaction Automation

Mobile Payment Terminal (MPOS) automation test suite using Maestro framework with Page Object Model (POM) design pattern.

## 🎯 Overview

This project automates the complete MPOS payment flow including:
- User authentication (PIN entry)
- MPOS device connection
- Transaction amount entry
- Card payment processing
- Digital receipt handling

## 🛠️ Technology Stack

- **Framework**: Maestro (Mobile UI Testing)
- **Pattern**: Page Object Model (POM)
- **App**: TapPay (com.tap_pay)
- **Platform**: Android/iOS

## 📁 Project Structure

```
MPOS/
├── flows/                          # Reusable flow components (POM)
│   ├── login.yaml                  # Authentication flow
│   ├── mpos_device_connection.yaml # Device connection flow
│   ├── enter_amount.yaml           # Amount entry flow
│   └── payment_processing.yaml     # Payment & receipt flow
├── config/
│   └── test_data.yaml              # Test data configuration
├── mpos_transaction_pom.yaml       # Main test orchestrator (POM)
├── mpos_transaction.yaml           # Simple linear test
├── initial_mpos.yaml               # Initial test version
└── README.md                       # This file
```

## 🎨 Design Pattern: Page Object Model (POM)

### Benefits
- **Reusability**: Each flow is a separate, reusable component
- **Maintainability**: Changes in one flow don't affect others
- **Scalability**: Easy to add new flows or test scenarios
- **Clean Code**: Main test file is simple and readable

### Flow Components

#### 1. Login Flow (`flows/login.yaml`)
- Handles user authentication
- PIN entry with configurable credentials
- Optional fingerprint skip
- Fast execution with optimized waits

#### 2. MPOS Device Connection (`flows/mpos_device_connection.yaml`)
- Waits for MPOS device connection
- Verifies amount entry screen
- Optimized 8s timeout

#### 3. Enter Amount (`flows/enter_amount.yaml`)
- Digit-by-digit amount entry
- Parameterized for different amounts
- Ultra-fast taps (200ms waits)

#### 4. Payment Processing (`flows/payment_processing.yaml`)
- Card insertion/tap prompt
- Transaction success validation
- Digital receipt confirmation
- Back navigation

## 🚀 Performance Optimizations

- **Fast Button Clicks**: 200-1000ms waits instead of default 3s
- **Reduced Timeouts**: Smart timeout configurations (8-25s)
- **Minimal Assertions**: Only critical validations
- **Result**: 40-50% faster execution compared to standard flows

## 📝 Usage

### Run Main Test (POM)
```bash
maestro test mpos_transaction_pom.yaml
```

### Run Individual Flows
```bash
maestro test flows/login.yaml
maestro test flows/enter_amount.yaml
```

### Configuration
Edit `config/test_data.yaml` to modify:
- PIN credentials
- Transaction amounts
- Test scenarios

### Environment Variables (mpos_transaction_pom.yaml)
```yaml
env:
  PIN: "1122"
  DIGIT_1: "4"
  DIGIT_2: "0"
  DIGIT_3: "0"
  DIGIT_4: "0"
```

## 📊 Test Scenarios

### Scenario 1: Standard MPOS Transaction
File: `mpos_transaction_pom.yaml`
- Uses POM structure
- Amount: 4000 (configurable via env)

### Scenario 2: Simple Linear Test
File: `mpos_transaction.yaml`
- Linear execution
- Amount: 2000 (hardcoded)

### Scenario 3: Initial Setup
File: `initial_mpos.yaml`
- Includes configuration loading
- Amount: 3500 (hardcoded)

## ⚙️ Test Flow Sequence

```
1. Launch App → Tap "PAY"
2. Enter PIN (1122)
3. Skip Fingerprint
4. Wait for MPOS Connection (8s max)
5. Enter Amount (4 digits)
6. Confirm Amount
7. Wait for Card Tap/Insert
8. Verify Transaction Success (25s max)
9. Save Digital Receipt
10. Navigate Back
```

## 🔧 Customization

### Change Transaction Amount
Update in `mpos_transaction_pom.yaml`:
```yaml
env:
  DIGIT_1: "5"  # Change digits
  DIGIT_2: "0"
  DIGIT_3: "0"
  DIGIT_4: "0"
```

### Add New Flow
1. Create new file in `flows/` directory
2. Add flow logic
3. Import in main test using `runFlow`

### Adjust Timeouts
Edit individual flow files to modify:
- `waitToSettleTimeoutMs`: For tap delays
- `timeout`: For wait conditions

## 📋 Prerequisites

- Maestro CLI installed
- Android device/emulator or iOS simulator
- TapPay app (com.tap_pay) installed
- Device configured with MPOS connection

## 🐛 Troubleshooting

**Issue**: Slow execution
- **Solution**: Waits are optimized to 200-1000ms

**Issue**: MPOS connection timeout
- **Solution**: Timeout is 8s, check device connection

**Issue**: Transaction fails
- **Solution**: Verify card reader is connected and active

## 📈 Future Enhancements

- [ ] Multiple payment methods (card, NFC, QR)
- [ ] Error scenario testing
- [ ] Receipt validation
- [ ] Performance metrics collection
- [ ] CI/CD integration

## 👤 Author

**Rabbiya Mehmood**
- GitHub: [@rabbiyamehmood](https://github.com/rabbiyamehmood)
- Repository: [PAYAPP](https://github.com/rabbiyamehmood/PAYAPP)

## 📄 License

This project is part of TapPay automation testing suite.

---

**Last Updated**: January 2026
