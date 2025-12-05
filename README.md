# Page Object Model Structure for PAY App Automation
# This folder contains reusable page objects and flows

## Directory Structure:
```
.maestro/
├── config.yaml              # Global configuration
├── pages/                   # Page objects (reusable UI elements)
│   ├── login-page.yaml
│   ├── signup-page.yaml
│   ├── pin-page.yaml
│   └── terms-page.yaml
└── flows/                   # Test flows (using page objects)
    ├── complete-signup-flow.yaml
    └── first-login-flow.yaml
```

## Benefits of POM:
- Reusable page elements
- Easier maintenance
- Cleaner test flows
- Single source of truth for UI elements
