<!-- SHOWCASE_START --><div align="center">[![Typing](https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=22&duration=2800&pause=900&color=58A6FF&center=true&vCenter=true&width=900&lines=customer%20data%20synchronization%20platform;AI%20%7C%20Automation%20%7C%20Engineering;Explore%20the%20project%20%F0%9F%9A%80)](https://github.com/shaikshahid777/customer-data-synchronization-platform)<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,50:161B22,100:58A6FF&height=110&section=header&text=customer-data-synchronization-platform&fontSize=26&fontColor=FFFFFF&animation=twinkling&fontAlignY=65" width="100%" alt="Animated project banner"/>

[![Repository](https://img.shields.io/badge/Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/shaikshahid777/customer-data-synchronization-platform) [![Issues](https://img.shields.io/badge/Report-Issue-red?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/customer-data-synchronization-platform/issues/new) [![Stars](https://img.shields.io/github/stars/shaikshahid777/customer-data-synchronization-platform?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/customer-data-synchronization-platform/stargazers) [![Fork](https://img.shields.io/github/forks/shaikshahid777/customer-data-synchronization-platform?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/customer-data-synchronization-platform/fork) [![Profile](https://img.shields.io/badge/Profile-Visit-0A66C2?style=for-the-badge&logo=github)](https://github.com/shaikshahid777)</div>

> ✨ **Project Showcase Mode:** animated banner • interactive navigation • live repository actions

[🚀 Repository](https://github.com/shaikshahid777/customer-data-synchronization-platform) · [🐞 Report Issue](https://github.com/shaikshahid777/customer-data-synchronization-platform/issues/new) · [⭐ Star](https://github.com/shaikshahid777/customer-data-synchronization-platform/stargazers) · [🔱 Fork](https://github.com/shaikshahid777/customer-data-synchronization-platform/fork) · [👤 Profile](https://github.com/shaikshahid777)

<!-- SHOWCASE_END -->

# Customer Data Synchronization Platform — Lesson 5 Assessment

Production-style n8n workflow that synchronizes customer data from three mock sources using email as the common matching key.

## Workflow Architecture

```text
Manual Trigger
   ├── API Session Data → Normalize API Email ──┐
   ├── DB Customer Profiles → Normalize DB Email ─┼→ MERGE_API+Database
   └── Sheet Marketing Prefs → Normalize Sheet Email ───────→ MERGE_MarketingSheet
                                                                  ↓
                                                         CLEAN_CoalesceFields
                                                                  ↓
                                                            IF_HasFullName
                                                             ↙           ↘
                                          OUTPUT_UnifiedCustomerProfiles   Quarantine_UnmatchedProfiles
```

## Source Datasets

The workflow uses three independent mock customer datasets. Each source contains five records.

### API session data

Fields include `email`, `deviceType`, and `lastActive`.

### Database customer profiles

Fields include `email`, `firstName`, `lastName`, and `preferredLanguage`.

### Spreadsheet marketing preferences

Fields include `email` and `marketingOptIn`.

## Email Normalization

All three source branches normalize the shared email key using trimming and lowercasing:

```text
trim().toLowerCase()
```

This allows values with inconsistent casing and whitespace to match reliably.

## Merge Strategy

1. `MERGE_APIDatabase` performs a field-based combine using `email`.
2. `MERGE_MarketingSheet` performs the second field-based combine using `email`.
3. The merged customer record is passed to the schema-cleaning stage.

## Standard Unified Profile

The `CLEAN_CoalesceFields` stage produces the standardized customer profile:

- `email`
- `fullName`
- `deviceType`
- `lastActive`
- `marketingOptIn`
- `preferredLanguage`

Fallback values are applied for missing device, activity, language, and marketing fields. `fullName` is generated from `firstName` and `lastName`.

## Validation and Quarantine

`IF_HasFullName` is the post-merge quality gate.

- Complete profiles → `OUTPUT_UnifiedCustomerProfiles`
- Incomplete profiles → `Quarantine_UnmatchedProfiles`

The quarantine branch preserves the record and adds a quarantine status and reason.

## Test Scenarios

The assessment requires testing:

- Inconsistent email casing
- Leading/trailing whitespace in email values
- Missing values
- Successful unified customer output
- Quarantine behavior for incomplete records

## Repository Structure

```text
customer-data-synchronization-platform/
├── README.md
├── workflow/
│   └── customer-data-synchronization-platform.json
├── documentation/
│   └── Customer_Data_Synchronization_Documentation.pdf
├── datasets/
│   ├── api-session-data.json
│   ├── database-customer-profiles.json
│   └── sheet-marketing-preferences.json
└── screenshots/
    ├── workflow-overview.png
    ├── normalized-email.png
    ├── merge-output.png
    ├── unified-output.png
    └── quarantine-output.png
```

## Setup Instructions

1. Import the exported n8n workflow JSON into an n8n workspace.
2. Confirm the three mock source branches are present.
3. Verify email normalization uses trim and lowercase.
4. Verify both merge stages use `email` as the matching field.
5. Run the workflow with the Manual Trigger.
6. Inspect unified profiles and quarantine output.
7. Capture screenshots for the assessment evidence.
8. Export the latest workflow JSON after final validation.

> Do not commit credentials, API keys, passwords, or other private secrets to this repository.
