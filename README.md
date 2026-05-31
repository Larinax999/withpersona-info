# Persona Detection Catalog

A structured catalog of the **282 verification checks ("detections")** used by
[Persona](https://withpersona.com) — the identity-verification platform — extracted
from the Persona dashboard frontend bundle.

Each detection is a single check that Persona runs against a piece of evidence
(an ID, a selfie, a document, a phone number, a business website, a government
database lookup, …) and that returns a pass/fail outcome together with a set of
machine-readable **reason codes** explaining *why* it failed.

This folder holds the cleaned, fully-parsed catalog. All 282 entries carry a
`name`, `description`, `type`, `category`, and `reasons` map.

## Files

| file | contents |
|------|----------|
| `detections.json` | The catalog. Maps each detection key → `{ name, category, description, type, reasons }`. 282 entries. |
| `detection_catalog_by_domain.json` | The same 282 keys grouped by domain prefix (16 domains), for navigation. |

## Detection schema

```jsonc
"selfie_account_comparison": {
  "name": "Account comparison",                  // human-readable label
  "category": "fraud",                           // see Categories below
  "description": "Compare if the selfie is inconsistent with the account portrait.",
  "type": "selfie",                              // detection family / data source
  "reasons": {                                   // reason code → user-facing explanation
    "face_mismatch": "The selfie is inconsistent with the portrait on the account.",
    "missing_face": "The selfie has no face.",
    "no_account_selfie_present": "The account has no selfie."
  }
}
```

The top-level key (e.g. `selfie_account_comparison`) is the stable detection ID.
The `reasons` map (837 reason strings in total, up to 13 per detection) is the
most useful field for anyone building dashboards, audit logs, or user-facing
messaging on top of Persona results.

## Categories

| category | count | meaning |
|----------|------:|---------|
| `fraud` | 172 | Signals that suggest a fraudulent or risky submission. |
| `user_action_required` | 93 | Something the user can fix and retry (wrong code, blurry capture, name mismatch, …). |
| `validity` | 7 | Structural validity of the evidence (format, checksums, required fields). |
| `biometrics` | 2 | Face / liveness biometric checks. |
| *(null)* | 8 | Eight legacy detections carry no category or type — see note below. |

## Domains

`detection_catalog_by_domain.json` groups detections by their domain prefix:

| domain | count | examples |
|--------|------:|----------|
| `database` | 52 | government & data-provider lookups (AAMVA, Aadhaar, eCBSV, SERPRO, TIN, VAT, phone carrier) |
| `id` | 52 | ID-document checks (MRZ, barcode, portrait, tampering, NFC chip) |
| `document` | 33 | generic document checks (color mode, VIN, classification) |
| `digital` | 30 | digital-ID / share-code providers (UK share-code, iDIN, itsme, MitID, BankID-style) |
| `selfie` | 24 | selfie & liveness checks |
| `phone` | 15 | phone-number ownership, OTP, silent network auth |
| `business` | 14 | business-website legitimacy checks |
| `aamva` | 8 | AAMVA driver-license field comparisons |
| `mdoc` | 7 | mobile driving licence (ISO mDL / mdoc) |
| `certificate` | 4 | Korea PKI certificate checks |
| `credit` | 4 | credit-card checks |
| `brand` | 3 | brand-asset / trademark checks |
| `email` | 3 | email-address checks |
| `verifiable` | 3 | verifiable-credential checks |
| `qes` | 2 | qualified electronic signature |
| `bank` | 1 | penny-drop bank-account verification |

## A note on the 8 untyped detections

These eight entries have a `name`, `description`, and `reasons` but a `null`
`category` and `null` `type`:

```
document_vin_checksum_detection   id_mrz_validation
id_required_property_detection    id_type_detection
phone_number_address_comparison   phone_number_birthdate_comparison
phone_number_name_comparison      phone_number_phone_number_comparison
```

They are still fully usable for their `name`/`description`/`reasons`; only the
classification metadata is missing in the source bundle.

## Related Notion docs

These are
Persona's internal enablement guides — they live in the private
`notion.so/withpersona` workspace, so the links likely require Persona access:

| signal(s) | Notion doc |
|-----------|------------|
| Age Delta | [Age Delta Note](https://www.notion.so/withpersona/Age-Delta-Note-2abef6bcb0fd80ad8cbfcb737429465a?v=2a2ef6bcb0fd80df8d2a000c2d6179ce) |
| Age Ratio (age-estimation signals) | [Age Estimation Based Signals — Enablement Guide](https://www.notion.so/withpersona/Age-Estimation-Based-Signals-Enablement-Guide-2f8ef6bcb0fd8052bc54dd966efd9de1) |
| Max Age Estimation Difference | [Max Age Estimation Difference Note](https://www.notion.so/withpersona/Max-Age-Estimation-Difference-Note-2e9ef6bcb0fd800f9886cf293e88e326?v=2a2ef6bcb0fd80df8d2a000c2d6179ce) |
| Apple App Attestation / Play Integrity | [iOS App Attest & Play Integrity — Enablement Notes](https://www.notion.so/withpersona/iOS-App-Attest-Play-Integrity-Enablement-Notes-280ef6bcb0fd809182c3fb7492742c46) |
| Sentinel signals (Bot Score, Connection Type, Country, Incognito Browsing, Network Threat Level, Proxy Detected, Region, Timezone, Tor Detected, VPN Detected, …) | [Sentinel Signals — Enablement](https://www.notion.so/withpersona/Sentinel-Signals-Enablement-306ef6bcb0fd80729121ebfcd04babdd?v=2a2ef6bcb0fd80df8d2a000c2d6179ce) |


## All detections (by type)

All 282 detections, grouped by `type`. Each row is one check; the
detection key is the stable ID returned by the Persona API.

### `aamva` (8)

| Detection key | Name | Description |
|---------------|------|-------------|
| `aamva_address_comparison` | Postal Code | Detect if the postal code matches AAMVA. |
| `aamva_birthdate_comparison` | Birthdate | Detect if the birthdate matches AAMVA. |
| `aamva_expiration_date_comparison` | Expiration Date | Detect if the expiration date matches AAMVA. |
| `aamva_identification_number_comparison` | Identification Number | Detect if the identification number matches AAMVA. |
| `aamva_identity_comparison` | Identity Comparison | Detect if various properties match AAMVA. |
| `aamva_issue_date_comparison` | Issue Date | Detect if the issue date matches AAMVA. |
| `aamva_name_comparison` | Name | Detect if the full name matches AAMVA. |
| `aamva_service_available_detection` | Service Available | Detect if the AAMVA service is available. |

### `bank_pennydrop` (5)

| Detection key | Name | Description |
|---------------|------|-------------|
| `bank_pennydrop_account_name_comparison` | Name Comparison | Compare user-provided name with the name associated with the Bank Account. |
| `bank_pennydrop_account_phone_number_comparison` | Phone Number Comparison | Compares user-provided phone number with the phone number associated with the Bank Account. |
| `bank_pennydrop_disallowed_country_detection` | Allowed Country | Disallowed country code detection |
| `bank_pennydrop_service_available_detection` | Service Available | Detect if results from Bank Pennydrop are available. |
| `bank_pennydrop_valid_account_detection` | Account Validation | Detect if the account is active. |

### `brand_asset` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `brand_asset_brand_association_detection` | Brand Association Detection | Detect if the brand asset  matches the business website branding. |
| `brand_asset_shaft_compliance_detection` | SHAFT Compliance Detection | Detect if the brand asset contains prohibited SHAFT content (Sex, Hate, Alcohol, Firearms, Tobacco). |
| `brand_asset_trademark_ownership_detection` | Trademark Ownership Detection | Detect if the brand asset logo has potential trademark conflicts. |

### `business_website` (14)

| Detection key | Name | Description |
|---------------|------|-------------|
| `business_website_backlink_detection` | Backlink Detection | Detect if a business website has sufficient backlinks. |
| `business_website_cookie_banner_detection` | Cookie Banner Detection Check | Validates that the website has a compliant cookie banner with proper reject options and equal prominence of buttons. |
| `business_website_domain_age_detection` | Domain Age Detection | Detect if the domain's age meets requirements. |
| `business_website_domain_expiration_detection` | Domain Expiration Detection | Detect if a domain's registration is expired or expiring soon. |
| `business_website_domain_redirect_detection` | Domain Redirect Detection | Detects if a URL redirects when visited |
| `business_website_domain_resolvable_detection` | Domain Resolvable Detection | Detect if the domain can resolve to an IP address. |
| `business_website_identity_comparison` | Identity Comparison | Whether the information extracted from the business's website matches the inputted information |
| `business_website_malicious_website_detection` | Malicious Website Detection | Detect if a business website is malicious. |
| `business_website_privacy_policy_legitimacy_detection` | Privacy Policy Legitimacy Detection | Detect if a business website has a legitimate Privacy Policy. |
| `business_website_sufficient_content_detection` | Sufficient Content Detection | Detect if the website has sufficient content to extract information from |
| `business_website_supported_domain_detection` | Supported Platform | Verifies that the website platform is supported |
| `business_website_terms_of_service_legitimacy_detection` | Terms of Service Legitimacy Detection | Detect if a business website has a legitimate Terms of Service. |
| `business_website_valid_url_format_detection` | Valid URL Format Detection | Validates that the website URL format is valid and processable. |
| `business_website_website_comparison` | Website Comparison | Whether the information extracted from the business's website matches the inputted information. |

### `certificate_korea` (4)

| Detection key | Name | Description |
|---------------|------|-------------|
| `certificate_korea_birthdate_comparison` | Birthdate comparison | Detect if the birthdate matches what is associated with certificate. |
| `certificate_korea_identification_number_comparison` | Identification number comparison | Detect if the ID number matches what is associated with certificate. |
| `certificate_korea_name_comparison` | Name comparison | Detect if the name matches what is associated with certificate. |
| `certificate_korea_pki_failure_detection` | PKI Validation | Detect if the digital certificate's Public Key Infrastructure is valid. |

### `credit_card` (4)

| Detection key | Name | Description |
|---------------|------|-------------|
| `credit_card_disallowed_country_detection` | Allowed country | Detect if the credit card's country of origin is allowed based on template requirements. |
| `credit_card_disallowed_type_detection` | Allowed card type | Detect if the credit card's funding type and brand are allowed for age verification. |
| `credit_card_name_comparison` | Name comparison | Compare the submitted name with the account holder's name for verification. |
| `credit_card_service_available_detection` | Service Available | Detect if the credit card service is available. |

### `database` (16)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_address_comparison` | Address Comparison | Detect if the address matches a record in the authoritative or issuing databases. |
| `database_address_deliverable_detection` | Deliverable address | Detect if the address is deliverable. |
| `database_address_residential_detection` | Residential address | Detect if the address is residential. When the `allowed_jurisdictions` config includes `university`, college campus addresses (dorms, on-campus annexes) also pass via an IPEDS / OpenStreetMap polygon lookup. |
| `database_attribute_comparison` | Attribute comparison | Verify that input values submitted for database verification match the claimed identity details provided during the verification flow. Flags cases where information may have changed mid-flow. |
| `database_birthdate_comparison` | Birthdate Comparison | Detect if the birthdate matches a record in the authoritative or issuing databases. |
| `database_deceased_detection` | Alive Detection | Detect if person is living. |
| `database_email_address_comparison` | Email Address Comparison | Detect if the email address matches a record in the authoritative or issuing databases. |
| `database_face_comparison` | Face comparison | Detect if the face in the user-provided selfie matches the database source. |
| `database_identity_comparison` | Identity Comparison | Detect if the identity matches a record in the authoritative or issuing databases. |
| `database_inquiry_comparison` | Inquiry comparison | Compare if attributes on the inquiry are inconsistent with what was submitted. |
| `database_name_comparison` | Name Comparison | Detect if the name matches a record in the authoritative or issuing databases. |
| `database_phone_number_comparison` | Phone Number Comparison | Detect if the phone number matches a record in the authoritative or issuing databases. |
| `database_po_box_detection` | P.O. Box Detection | Detect if the address is a PO box. |
| `database_service_available_detection` | Service Available | Detect if the service is available. |
| `database_social_security_number_comparison` | Identification Number Comparison | Detect if the identification number matches a record in the authoritative or issuing databases. |
| `database_two_plus_two_detection` | 2+2 Detection | Detect if claimed attributes passes 2+2 requirements. |

### `database_aadhaar` (4)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_aadhaar_birthdate_comparison` | Birthdate comparison | Detect if the birthdate matches the Aadhaar database. |
| `database_aadhaar_consent_check` | Consent check | Ensure the user gave consent to share Aadhaar data. |
| `database_aadhaar_face_comparison` | Face comparison | Detect if the face in the user-provided selfie matches the Aadhaar database. |
| `database_aadhaar_name_comparison` | Name comparison | Detect if the name matches the Aadhaar database. |

### `database_business` (9)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_business_active_business_detection` | Active Business Detection | Detect if business is legally active. |
| `database_business_ai_identity_comparison` | Business Identity Comparison using AI | Detect if the business identity matches a record in the authoritative or issuing databases using AI. |
| `database_business_associated_persons_comparison` | Associated Persons Comparison | Detect if the provided associated persons are found on a record in an authoritative source. |
| `database_business_enabled_country_detection` | Enabled Country Detection | Detect if the business's country is enabled based on template requirements. |
| `database_business_filing_status_type_detection` | Filing Status Type Detection | Detect if the business operating status maps to a blocked filing status category. |
| `database_business_identity_comparison` | Business Identity Comparison | Detect if the business identity matches a record in the authoritative or issuing databases. |
| `database_business_registered_agent_address_detection` | Registered Agent Address Detection | Detect if the submitted business address matches a registered agent address on the matched business record. |
| `database_business_service_available_detection` | Service Available Detection | Detects if BRV vendors are available. |
| `database_business_valid_registration_number_format` | Valid Registration Number Format | Validate if the business registration number matches an applicable format. |

### `database_business_footprint` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_business_footprint_enabled_country_detection` | Enabled Country Detection | Detect if the business's country is enabled based on template requirements. |
| `database_business_footprint_identity_comparison` | Business Footprint Identity Comparison | Detect if the business identity matches a record in the footprint databases. |

### `database_ecbsv` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_ecbsv_death_record` | Death Record | Detect if person found is deceased. |
| `database_ecbsv_record_match` | Record Matched | Detect if the record was matched in the database. |
| `database_ecbsv_service_available_detection` | Service Available Detection | Detect if the eCBSV database is unavailable. |

### `database_phone_carrier` (6)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_phone_carrier_address_comparison` | Address | Detect if the address matches with phone carrier. |
| `database_phone_carrier_age_threshold_comparison` | Age threshold | Detect if the user meets a specified age threshold. |
| `database_phone_carrier_birthdate_comparison` | Birthdate | Detect if the birthdate matches with phone carrier. |
| `database_phone_carrier_name_first_comparison` | First name | Detect if the first name matches with phone carrier. |
| `database_phone_carrier_name_last_comparison` | Last name | Detect if the last name matches with phone carrier. |
| `database_phone_carrier_service_available_detection` | Service Available Detection | Detect if the service is available |

### `database_serpro` (5)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_serpro_birthdate_comparison` | Birthdate comparison | Detect if the birthdate matches the Serpro database. |
| `database_serpro_cpf_comparison` | CPF comparison | Detect if the CPF matches the Serpro database. |
| `database_serpro_face_comparison` | Face comparison | Detect if the face matches the Serpro database. |
| `database_serpro_name_comparison` | Name comparison | Detect if the name matches the Serpro database. |
| `database_serpro_service_available_detection` | Service Available Detection | Detect if Serpro is unavailable. |

### `database_standard` (1)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_standard_identity_comparison` | Identity comparison | Detect if the identity matches a record. |

### `database_tin` (5)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_tin_deceased_detection` | Deceased Detection | Detect if the person is deceased. This check only applies when the TIN is successfully matched as an individual SSN. |
| `database_tin_disallowed_type_detection` | Disallowed Type Detection | Detect if the TIN type (e.g. SSN, EIN, ITIN) submitted is not allowed based on template requirements. |
| `database_tin_invalid_format_detection` | Invalid Format Detection | Detect if the TIN format is invalid. |
| `database_tin_name_comparison` | Name comparison | Detect if the name matches the TIN. |
| `database_tin_service_available_detection` | Service Available Detection | Detect if the IRS is unavailable. |

### `database_vat_number` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `database_vat_number_identity_comparison` | Business Identity Comparison | Detect if the business identity matches a record in the authoritative or issuing databases. |
| `database_vat_number_service_available_detection` | Service Available Detection | Detect if services for verifying VAT numbers are unavailable. |
| `database_vat_number_valid_detection` | Valid Detection | Detect if the VAT number is valid. |

### `digital_id_connect_id` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_connect_id_flow_completed_detection` | Flow completed | Detect if the user completed the ConnectID flow. |
| `digital_id_connect_id_service_available_detection` | Service Available | Detect if results from Connect ID are available. |

### `digital_id_e_do_app` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_e_do_app_flow_completed_detection` | Flow completed | Detect if the user completed the eDoApp flow. |
| `digital_id_e_do_app_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from E-Do App against the inquiry. |
| `digital_id_e_do_app_service_available_detection` | Service Available | Detect if results from eDO App are available. |

### `digital_id_finnish_trust_network` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_finnish_trust_network_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from Finnish Trust Network against the inquiry. |
| `digital_id_finnish_trust_network_service_available_detection` | Service Available | Detect if results from the Finnish Trust Network are available. |

### `digital_id_france_identite` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_france_identite_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from France Identité against the inquiry. |
| `digital_id_france_identite_service_available_detection` | Service Available | Detect if results from France Identite are available. |

### `digital_id_generic` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_generic_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from EID Easy against the inquiry. |
| `digital_id_generic_service_available_detection` | Service Available | Detect if results from the relevant Digital ID provider are available. |

### `digital_id_idin` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_idin_flow_completed_detection` | Flow completed | Detect if the user completed the iDIN flow. |
| `digital_id_idin_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from iDIN against the inquiry. |
| `digital_id_idin_service_available_detection` | Service Available | Detect if results from the iDIN service are available. |

### `digital_id_its_me` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_its_me_flow_completed_detection` | Flow completed | Detect if the user completed the ItsMe flow. |
| `digital_id_its_me_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from itsme against the inquiry. |
| `digital_id_its_me_service_available_detection` | Service Available | Detect if results from the itsme service are available. |

### `digital_id_kcb_credit_card` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_kcb_credit_card_flow_completed_detection` | Flow completed | Detect if the user completed the KCB credit card identity verification flow. |
| `digital_id_kcb_credit_card_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from KCB against the inquiry. |
| `digital_id_kcb_credit_card_service_available_detection` | Service Available | Detect if results from the KCB service are available. |

### `digital_id_mit_id` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_mit_id_flow_completed_detection` | Flow completed | Detect if the user completed the MitID flow. |
| `digital_id_mit_id_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from MitID against the inquiry. |
| `digital_id_mit_id_service_available_detection` | Service Available | Detect if results from the MitID service are available. |

### `digital_id_one_id` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_one_id_flow_completed_detection` | Flow completed | Detect if the user completed the OneID flow. |
| `digital_id_one_id_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from OneID against the inquiry. |
| `digital_id_one_id_service_available_detection` | Service Available | Detect if results from the OneID service are available. |

### `digital_id_philsys` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_philsys_identity_comparison` | Identity Comparison | Verify the user's identity using the PhilSys ID service. |
| `digital_id_philsys_service_available_detection` | Service Available | Detect if results from PhilSys ID are available. |

### `digital_id_serpro` (5)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_serpro_account_comparison` | Account Comparison | Compare identity attributes from the Serpro QR against the account. |
| `digital_id_serpro_identity_comparison` | Identity comparison | Verify the user's identity using the Serpro service. |
| `digital_id_serpro_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from the Serpro QR against the inquiry. |
| `digital_id_serpro_selfie_comparison` | Selfie comparison | Compare if the face portrait stored with Serpro and the selfie provided are different faces. |
| `digital_id_serpro_service_available_detection` | Service available | Detect if results from Serpro are available. |

### `digital_id_singpass` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_singpass_flow_completed_detection` | Flow completed | Detect if the user completed the Singpass flow. |
| `digital_id_singpass_service_available_detection` | Service Available | Detect if results from Singpass are available. |

### `digital_id_swedish_bank_id` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_swedish_bank_id_inquiry_comparison` | Inquiry Comparison | Compare identity attributes from Swedish Bank ID against the inquiry. |
| `digital_id_swedish_bank_id_service_available_detection` | Service Available | Detect if results from Swedish Bank ID are available. |

### `digital_id_uk_sharecode` (6)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_uk_sharecode_expired_detection` | Expired detection | Detect if user is able to work in the UK within the dates specified on the sharecode result. |
| `digital_id_uk_sharecode_identity_comparison` | Identity comparison | Compare the sharecode data from the user with the right to work data from the UK Sharecode service. |
| `digital_id_uk_sharecode_name_comparison` | Name comparison | Compare the inputted name with the name from the UK Sharecode service. |
| `digital_id_uk_sharecode_right_to_reside_detection` | Right to Reside | Detect if the user has the right to reside in the UK based on their immigration status. |
| `digital_id_uk_sharecode_right_to_work_detection` | Right to Work | Detect if the user is able to work in the UK. |
| `digital_id_uk_sharecode_service_available_detection` | Service Available | Detect if the UK share code service is available. |

### `digital_id_world_id` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `digital_id_world_id_flow_completed_detection` | Flow completed detection | Detects if the user has completed the World ID flow. |
| `digital_id_world_id_service_available_detection` | Service Available | Detect if results from World ID are available. |

### `document` (32)

| Detection key | Name | Description |
|---------------|------|-------------|
| `document_color_mode_detection` | Color mode detection | This check identifies the color mode of the document. |
| `document_compromised_detection` | Compromised submission | Detect if the submission can be found from a publicly available source (e.g. Google Images). |
| `document_custom_detection` | Custom detection | Customizable detection |
| `document_digital_text_modification_detection` | Digital text modification detection | This check identifies any digitally modified text in the document. |
| `document_enhanced_fraud_detection` | Enhanced fraud detection | This check identifies fraud signals in the document. |
| `document_expired_detection` | Expired detection | Ensure the document is not expired |
| `document_extracted_properties_detection` | Extracted properties detection | Ensure document contains required extractions. |
| `document_fabrication_detection` | Fabrication detection | Detects if the document has been fabricated. |
| `document_handwriting_detection` | Handwriting detection | Detect if handwriting is in the document. |
| `document_image_copy_move_detection` | Copy move detection | This check identifies any evidence of copy-move forgery in the submitted document image. |
| `document_image_inconsistent_timestamp_detection` | Image inconsistent timestamp detection | Detect if the image has inconsistent timestamps. |
| `document_image_suspicious_metadata_detection` | Image suspicious metadata detection | This check identifies any irregular or modification timestamps on an image. |
| `document_inconsistent_timestamp_detection` | Inconsistent timestamp detection | This check identifies any irregular or modification timestamps on a PDF. |
| `document_jpeg_modification_detection` | JPEG modification detection | Detect if the JPEG has been modified. |
| `document_jpeg_original_image_detection` | JPEG original image detection | Detect inconsistencies in JPEG image compression to detect if image has been modified; does not apply to screenshots or scanned images. |
| `document_pdf_abnormal_font_detection` | PDF abnormal font detection | Detects if the PDF contains font(s) that are considered an outlier relative to other fonts in the document. Font outliers in PDF metadata are common signs of an editor being used. |
| `document_pdf_annotation_detection` | PDF annotation detection | Detect if the PDF contains disallowed annotation subtypes. |
| `document_pdf_content_type_detection` | PDF content type detection | Detect whether or not the PDF contains strictly typical content pages, image based pages, or a mix of both. |
| `document_pdf_editor_detection` | PDF editor detection | Detect if the PDF has signs of a document editor. |
| `document_pdf_inconsistent_timestamp_detection` | PDF inconsistent timestamp detection | Detect if the PDF has inconsistent timestamps. |
| `document_pdf_modification_detection` | PDF modification detection | Detect if the PDF has been modified after creation. |
| `document_pdf_timestamp_inconsistency_detection` | PDF timestamp inconsistency detection | Detect if the PDF has inconsistent timestamps. |
| `document_portrait_detection` | Portrait detection | Detect if there is face in the submission. |
| `document_qr_code_detection` | QR Code Detection | Detects and validates QR codes in documents. Fails when no QR codes are detected or when detected QR codes are unreadable. |
| `document_recency_detection` | Recency / Effective detection | Detect if the document is recent. |
| `document_selfie_comparison` | Document selfie comparison | Compare if the face portrait on the document and the selfie are different faces. |
| `document_social_security_card_version` | Social Security Card Version | This check determines if the social security card is consistent with the version. |
| `document_synthetic_content_detection` | Synthetic content detection | Detects synthetic documents by identifying common placeholder entities (e.g. John Doe) and mock document details (e.g. 12345678). This check leverages OCR-extracted text to flag documents typically used for demonstrations or testing purposes. |
| `document_tamper_detection` | Document image tampering | Detect if the submission shows evidence of being edited by a photo editor. |
| `document_template_detection` | Template detection | Detect if the document was contructed from a fraudulent template available online. |
| `document_type_detection` | Type detection | Detect if the document one of the expected types. |
| `document_unprocessable_submission_detection` | Processable submission | Detect if the submission is unprocessable (e.g. malformed document). |

### `email_address` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `email_address_confirmation_code_comparison` | Code confirmation | Compare provided code with the one sent to email address. |
| `email_address_invalid_email_format_detection` | Invalid Email Format Detection | Detect formatting errors on the provided email address |
| `email_address_timeout_detection` | Confirmation code timeout | Confirmation code is submitted within the alloted time |

### `id` (43)

| Detection key | Name | Description |
|---------------|------|-------------|
| `id_aamva_database_lookup` | AAMVA database lookup | Detect if the ID details match an entry in the AAMVA database. |
| `id_account_comparison` | Account comparison | Compare if government ID details (e.g. name, birthdate) are consistent with account details. |
| `id_age_comparison` | Age comparison | Detect if the age listed on the ID meets the age restriction. |
| `id_age_inconsistency_detection` | Portrait age | Detect if the age listed on the ID is different than the age estimated from the face portrait. |
| `id_attribute_comparison` | Attribute comparison | Compare if government ID details (e.g. name, birthdate) are consistent with provided values. Generally used for verifications created outside an inquiry. |
| `id_barcode_detection` | Barcode | Detect if there is a barcode on the ID. |
| `id_barcode_inconsistency_detection` | Barcode inconsistency | Detect inconsistencies in the barcode on the ID. |
| `id_blur_detection` | Blur | Detect if the submission is blurry. |
| `id_color_inconsistency_detection` | Color | Detect if the colors on the ID are different than the expected colors. |
| `id_compromised_detection` | Compromised submission | Detect if the submission can be found from a publicly available source (e.g. Google Images). |
| `id_corner_detection` | Corner detection | Detect if the submitted ID is fully in frame with all four corners visible. |
| `id_custom_detection` | Custom detection | Customizable detection |
| `id_damaged_detection` | Damaged detection | Detect physical damages on the ID that render it invalid for verification. |
| `id_disallowed_country_detection` | Allowed country | Detect if the country of the ID is not allowed based on template requirements. |
| `id_disallowed_type_detection` | Allowed ID type | Detect if the ID type (e.g. driver license) submitted does not match the ID type the user selected. |
| `id_double_side_detection` | Double side | Detect if one side of the ID is submitted as both front and back. |
| `id_electronic_replica_detection` | Electronic replica | Detect if the submission is a replica of an image presented on another electronic device or screen. |
| `id_entity_detection` | Government ID | Detect if there is a government ID in the submission. |
| `id_experimental_model_detection` | Experimental | Experimental fraud checks. |
| `id_expired_detection` | Expiration | Detect if the ID is expired. |
| `id_extracted_properties_detection` | Extracted properties | Detect if template-required ID details are extracted. |
| `id_extraction_inconsistency_detection` | Extraction inconsistency | Detect if the details extracted from the front of the ID are different than the details extracted from the barcode. |
| `id_fabrication_detection` | Fabrication | Detect if the ID is likely to be fabricated, based on its format and issuing entity. |
| `id_glare_detection` | Glare | Detect if the submission has glare. |
| `id_handwriting_detection` | Handwriting | Detect if handwriting is found on the ID. |
| `id_inconsistent_repeat_detection` | Inconsistent repeat | Detect if either the ID details or face portrait match that of a previously submitted ID within the last year. |
| `id_inquiry_comparison` | Inquiry comparison | Compare if ID details like name or birthdate are inconsistent between different submission attempts. |
| `id_mrz_detection` | MRZ Detected | Detect if there is encoded, machine-readable text on the ID. |
| `id_mrz_inconsistency_detection` | MRZ Inconsistency | Detect if the encoded, machine-readable text on the ID is well-formed. |
| `id_number_format_inconsistency_detection` | ID number format inconsistency | Detect if the ID number format is well-formed based on issuing authority rules for certain countries. |
| `id_paper_detection` | Paper detection | Detect if the ID is a paper copy |
| `id_physical_tamper_detection` | ID physical tampering | Detect if the submission is physically tampered. |
| `id_po_box_detection` | PO box | Detect if the address is a PO box. Note that address parts may be extracted both visually and from the barcode / MRZ. |
| `id_portrait_clarity_detection` | Portrait clarity | Detect if the face portrait is clear and in focus. |
| `id_portrait_detection` | Portrait | Detect if there is a face portrait in the submission. |
| `id_public_figure_detection` | Public figure | Detect if the face portrait matches that of a known public figure. |
| `id_real_id_detection` | REAL ID | Detect if a U.S. REAL ID. |
| `id_repeat_detection` | Repeat | Detect if the ID details and face portrait match that of a previously submitted ID on a different account within the last year. |
| `id_selfie_comparison` | ID-to-Selfie comparison | Compare if the face portrait on the ID and the selfie are different faces. |
| `id_tamper_detection` | ID image tampering | Detect if the submission is tampered by a photo editor. |
| `id_unprocessable_submission_detection` | Processable submission | Detect if the submission is unprocessable. |
| `id_valid_dates_detection` | Valid dates | Detect if the dates on the ID are in violation of issuing authority rules. The check applies to US DLs and IDs, and CN IDs. |
| `id_video_quality_detection` | Video quality | Detect if the video is high enough quality. |

### `id_nfc` (6)

| Detection key | Name | Description |
|---------------|------|-------------|
| `id_nfc_attribute_comparison` | Attribute comparison | Compare if ID details like name or birthdate are inconsistent with claimed attributes. |
| `id_nfc_disallowed_country_detection` | Allowed Country | Detect if the ID's issuing country is not allowed based on template requirements. |
| `id_nfc_expired_detection` | Expiration | Detect if the ID is expired. |
| `id_nfc_pki_failure_detection` | PKI Validation | Detect if the ID's Public Key Infrastructure is valid. |
| `id_nfc_tamper_detection` | Data tampering | Detects if the NFC data scanned from an ICAO compliant Travel Document has been tampered with. |
| `id_nfc_unprocessable_submission_detection` | Processable Submission | Detect if the submission is unprocessable. |

### `jp_my_number_nfc_scan` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `jp_my_number_nfc_scan_expired_detection` | Expiration | Detects whether the scanned My Number card is expired. |
| `jp_my_number_nfc_scan_service_available_detection` | Service Available | Detects whether the NFC data verification service was reachable during processing. |
| `jp_my_number_nfc_scan_valid_scan_detection` | Valid Scan | Verifies that the NFC scan result is structurally valid and cryptographically genuine. |

### `mdoc` (7)

| Detection key | Name | Description |
|---------------|------|-------------|
| `mdoc_age_comparison` | Age comparison | Detect if the age listed on the ID meets the age restriction. |
| `mdoc_digest_match_detection` | Mdoc Digest Validation | Detect if the digests stored within the MSO data match the digests computed from the actual data. |
| `mdoc_parseable_detection` | Mdoc Parsed | Detect if the mdoc data could be successfully parsed and decrypted. |
| `mdoc_valid_cert_detection` | Mdoc Certificate Validation | Detect if the MSO certificate can be chained to a trusted root CA. |
| `mdoc_valid_device_authentication_detection` | Mdoc Device Authentication Validation | Detect if the mdoc device authentication (DeviceSignature or DeviceMac) was verified, preventing cloning attacks per ISO 18013-5. |
| `mdoc_valid_mso_data_detection` | Mdoc MSO Data Validation | Detect if both the mobile security object (MSO) document type matches the actual mdoc data, and that the mso digest algorithm is allowed. |
| `mdoc_valid_mso_signature_detection` | Mdoc MSO Signature Validation | Detect if the MSO data signature could be verified, indicating the data has not been tampered with. |

### `persona_fff_inquiry` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `persona_fff_inquiry_outcome_detection` | Outcome | Detect the outcome of the FFF inquiry that backs this verification. |
| `persona_fff_inquiry_service_available_detection` | Service Available | Detect whether the internal admin API returned a result for the FFF inquiry. |

### `persona_id` (1)

| Detection key | Name | Description |
|---------------|------|-------------|
| `persona_id_attribute_comparison` | Attribute comparison | Validates that matches were found. |

### `phone_number` (6)

| Detection key | Name | Description |
|---------------|------|-------------|
| `phone_number_confirmation_code_comparison` | Code confirmation | Compare provided code with the one sent to phone number. |
| `phone_number_deliverable_detection` | Deliverable Detection | Detect problems with sms deliverability for the provided phone number. |
| `phone_number_disallowed_country_detection` | Allowed country | Disallowed country code detection |
| `phone_number_invalid_number_format_detection` | Invalid Number Format Detection | Detect formatting errors on the provided phone number. |
| `phone_number_service_available_detection` | Service Available Detection | Detect if the service is unavailable. |
| `phone_number_timeout_detection` | Confirmation code timeout | Confirmation code is submitted within the allotted time |

### `phone_number_silent_network_authentication` (5)

| Detection key | Name | Description |
|---------------|------|-------------|
| `phone_number_silent_network_authentication_deliverable_detection` | Deliverable Detection | Detect if the phone number is deliverable through Silent Network Authentication (SNA). Some carriers (e.g., AT&T) may not be supported. |
| `phone_number_silent_network_authentication_disallowed_country_detection` | Allowed Country | Detect if the phone number's country is allowed. This check is restricted to the United States only. |
| `phone_number_silent_network_authentication_invalid_number_format_detection` | Invalid Number Format Detection | Detect formatting errors on the provided phone number. |
| `phone_number_silent_network_authentication_service_available_detection` | Service Available Detection | Detect if the Silent Network Authentication (SNA) vendor service is unavailable. |
| `phone_number_silent_network_authentication_sim_verified_detection` | SIM Verification | Detect whether the phone number's SIM is verified via Silent Network Authentication (SNA). |

### `qes_infocert` (2)

| Detection key | Name | Description |
|---------------|------|-------------|
| `qes_infocert_flow_completed_detection` | Flow Completed | Detect if the flow has been completed successfully. |
| `qes_infocert_service_available_detection` | Service Available | Detect if results from the QES Infocert service are available. |

### `selfie` (24)

| Detection key | Name | Description |
|---------------|------|-------------|
| `selfie_account_comparison` | Account comparison | Compare if the selfie is inconsistent with the account portrait. |
| `selfie_age_comparison` | Age comparison | Detect if the perceived age from the selfie is different from the reference age. |
| `selfie_age_inconsistency_detection` | Age inconsistency detection | Detect whether the perceived age from the selfie is consistent with the reference age. |
| `selfie_age_threshold_comparison` | Age threshold comparison | Detect if the perceived age from the selfie complies with min and/or max age limits. |
| `selfie_attribute_comparison` | Attribute comparison | Compare if the selfie is inconsistent with provided selfies. Generally used for verifications created outside an inquiry. |
| `selfie_custom_detection` | Custom detection | Customizable detection |
| `selfie_experimental_model_detection` | Experimental | Experimental fraud checks. |
| `selfie_face_covering_detection` | Face covering | Detect if face was covered in the selfie. |
| `selfie_glare_detection` | Glare | Detect if the submission has glare. |
| `selfie_glasses_detection` | Glasses | Detect if glasses are present. |
| `selfie_id_comparison` | Selfie-to-ID comparison | Compare if the selfie and the face portrait in the ID are different faces. |
| `selfie_image_quality_detection` | Image quality | Detect if the submission has low image quality. |
| `selfie_liveness_detection` | Selfie liveness | Detect if the selfie contains a live individual, not a recording, picture, or another spoof. |
| `selfie_multiple_faces_detection` | Multiple faces | Detect if the selfies are different faces across poses. |
| `selfie_persona_comparison` | Persona comparison | Compare if the selfie is inconsistent with the Reusable Persona. |
| `selfie_portrait_quality_detection` | Portrait quality | Detect if the face portrait is clear and in focus. |
| `selfie_pose_detection` | Pose position | Detect if the face in the selfies are correctly positioned across all poses. |
| `selfie_pose_repeat_detection` | Pose repeat | Detect if the selfies are exact repeats across poses. |
| `selfie_public_figure_detection` | Public figure | Detect if the selfie submission matches a known public figure. |
| `selfie_repeat_detection` | Selfie repeat | Detect if the selfie submission matches a selfie in another account. |
| `selfie_similar_background_detection` | Similar background detection | Detect if the selfie submission has a similar background to other submissions. |
| `selfie_suspicious_entity_detection` | Suspicious entity | Detect if the selfie submission is that of a government ID. |
| `selfie_video_quality_detection` | Video quality | Detect if the selfie video is high enough quality. |
| `selfie_voice_detection` | Voice detection | Detect if voice is present in the selfie video recording. |

### `verifiable_credential` (3)

| Detection key | Name | Description |
|---------------|------|-------------|
| `verifiable_credential_account_comparison` | Account Comparison | Compares claims from the verifiable credential against the account's fields. |
| `verifiable_credential_allowed_issuer_detection` | Allowed Issuer | Validates that the verifiable credential was issued by a trusted issuer and, for Persona-issued credentials, that administrative claims such as environment and organization binding match the allowlist. |
| `verifiable_credential_required_claims_detection` | Required Claims | Ensures specified claims are present in the verifiable credential payload. |

### `(untyped)` (8)

| Detection key | Name | Description |
|---------------|------|-------------|
| `document_vin_checksum_detection` |  |  |
| `id_mrz_validation` |  |  |
| `id_required_property_detection` |  |  |
| `id_type_detection` |  |  |
| `phone_number_address_comparison` |  |  |
| `phone_number_birthdate_comparison` |  |  |
| `phone_number_name_comparison` |  |  |
| `phone_number_phone_number_comparison` |  |  |