## BAC Services Registration Instructions

## Overview
This instruction file guides the registration of BAC delegate services in the `BAC-service.properties.xconf` file. All 8 delegate types must be registered for proper BAC object functionality.

### Directory Configuration
- `service_xconf_path` = `\\\\wsl.localhost\\WindchillVM\\opt\\wnc\\wcmod\\modules\\BAC\\src\\com\\ptc\\windchill\\bac\\BAC-service.properties.xconf`
- `delegate_folder` = `\\\\wsl.localhost\\WindchillVM\\opt\\wnc\\wcmod\\modules\\BAC\\src\\wt\\{domain}\\bac\\`
- `object.json` = `c:/FINAL/.github/object.json`

### Required Delegates for Registration
1. **Collection Delegate** → `BACCollectionDelegate`
2. **DeleteRecord Delegate** → `BACDeleteRecordAttributes`
3. **DeleteRecordAttr Delegate** → `BACDeleteRecordAttributes` (same service as DeleteRecord)
4. **Identity Delegate** → `BACIdentityDelegate`
5. **Processor Delegate** → `BACDeleteProcessor`
6. **Report Delegate** → `BACReportDelegate`
7. **SpecProcessor Delegate** → `BACSpecProcessor`
8. **Tracking Delegate** → `BACDeleteTrackingDelegate`

---

## Service Registration Patterns

### 1. Collection Delegate Registration
**Service Name**: `com.ptc.windchill.bac.delegates.BACCollectionDelegate`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}CollectionDelegate"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2CollectionDelegate"/>
```

**Location**: Add within the `<Service context="default" name="com.ptc.windchill.bac.delegates.BACCollectionDelegate">` block, before the closing `</Service>` tag.

---

### 2. DeleteRecord Delegate Registration
**Service Name**: `com.ptc.windchill.bac.BACDeleteRecordAttributes`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}DeleteRecordAttributes"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2DeleteRecordAttributes"/>
```

**Location**: Add within the `<Service context="default" name="com.ptc.windchill.bac.BACDeleteRecordAttributes">` block.

**Note**: This is the same service registration for both DeleteRecord and DeleteRecordAttr delegates.

---

### 3. DeleteRecordAttr Delegate Registration
**Service Name**: `com.ptc.windchill.bac.BACDeleteRecordAttributes`
**Cardinality**: `duplicate`

**Note**: DeleteRecordAttr uses the same service registration as DeleteRecord (see above). Both delegates register under `BACDeleteRecordAttributes` service. No separate entry needed.

---

### 4. Delete Tracking Delegate Registration
**Service Name**: `com.ptc.windchill.bac.delegates.BACDeleteTrackingDelegate`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}DeleteTrackingDelegate"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2DeleteTrackingDelegate"/>
```

**Location**: Add within the `<Service context="default" name="com.ptc.windchill.bac.delegates.BACDeleteTrackingDelegate">` block.

---

### 5. Delete Processor Delegate Registration
**Service Name**: `com.ptc.windchill.bac.delegate.BACDeleteProcessor`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}DeleteProcessor"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2DeleteProcessor"/>
```

**Location**: Add within the `<Service context="default" name="com.ptc.windchill.bac.delegate.BACDeleteProcessor">` block.

---

### 6. Report Delegate Registration
**Service Name**: `com.ptc.windchill.bac.delegates.BACReportDelegate`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}ReportDelegate"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2ReportDelegate"/>
```

**Location**: Add within the `<Service context="default" name="com.ptc.windchill.bac.delegates.BACReportDelegate">` block.

---

### 7. Identity Delegate Registration
**Service Name**: `wt.ixb.bac.BACIdentityDelegate`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}IdentityDelegate"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2IdentityDelegate"/>
```

**Location**: Add within the `<Service context="default" name="wt.ixb.bac.BACIdentityDelegate">` block.

---

### 8. SpecProcessor Delegate Registration
**Service Name**: `com.ptc.windchill.bac.delegates.BACSpecProcessor`
**Cardinality**: `duplicate`
**Pattern**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="{ObjectTemplate}" serviceClass="wt.{domain}.bac.BAC{ObjectTemplate}SpecProcessor"/>
```

**Example**:
```xml
<Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2SpecProcessor"/>
```

**Location**: Add within the `<Service context="default" name="com.ptc.windchill.bac.delegates.BACSpecProcessor">` block.

---

## Registration Workflow

### Step 1: Read Object Information
- Read `object.json` to extract:
  - `template` (ObjectTemplate name, e.g., "WTChangeOrder2")
  - `directory` (domain name, e.g., "change2")

### Step 2: Identify Delegate Files
- Navigate to `{delegate_folder}` = `wt/{domain}/bac/`
- List all Java files in the directory
- Identify the 8 delegate types by class name suffix:
  - `*CollectionDelegate.java`
  - `*DeleteRecordAttributes.java`
  - `*DeleteTrackingDelegate.java`
  - `*DeleteProcessor.java`
  - `*ReportDelegate.java`
  - `*IdentityDelegate.java`
  - `*SpecProcessor.java`

### Step 3: Read BAC-service.properties.xconf
- Open `BAC-service.properties.xconf` from `service_xconf_path`
- Understand the XML structure and existing service blocks
- Locate each of the 8 service sections:
  1. `<Service context="default" name="com.ptc.windchill.bac.delegates.BACCollectionDelegate">`
  2. `<Service context="default" name="com.ptc.windchill.bac.BACDeleteRecordAttributes">`
  3. `<Service context="default" name="com.ptc.windchill.bac.delegates.BACDeleteTrackingDelegate">`
  4. `<Service context="default" name="com.ptc.windchill.bac.delegate.BACDeleteProcessor">`
  5. `<Service context="default" name="com.ptc.windchill.bac.delegates.BACReportDelegate">`
  6. `<Service context="default" name="wt.ixb.bac.BACIdentityDelegate">`
  7. `<Service context="default" name="com.ptc.windchill.bac.delegates.BACSpecProcessor">`

### Step 4: Check Existing Registrations
- For each service section, check if the object template is already registered
- Search for `selector="{ObjectTemplate}"` within each service block
- If found, SKIP that registration (DO NOT duplicate)

### Step 5: Add New Registrations
- For each missing registration, add the appropriate `<Option>` entry
- Follow the exact pattern for each delegate type (see patterns above)
- **CRITICAL**: Add entries BEFORE the closing `</Service>` tag of each service block
- Maintain consistent indentation (4 spaces per level)
- Use the correct selector (ObjectTemplate name from object.json)
- Use the correct serviceClass path (wt.{domain}.bac.BAC{ObjectTemplate}{DelegateType})

### Step 6: Placeholder Replacement
Replace these placeholders in each pattern:
- `{ObjectTemplate}` → Template name from object.json (e.g., "WTChangeOrder2")
- `{domain}` → Directory name from object.json (e.g., "change2")
- `{DelegateType}` → Specific delegate type (e.g., "CollectionDelegate", "DeleteProcessor")

---

## Service Registration Summary Table

| Delegate Type | Service Name | Cardinality | Class Suffix |
|--------------|-------------|-------------|--------------|
| Collection | `com.ptc.windchill.bac.delegates.BACCollectionDelegate` | duplicate | CollectionDelegate |
| DeleteRecord | `com.ptc.windchill.bac.BACDeleteRecordAttributes` | duplicate | DeleteRecordAttributes |
| DeleteRecordAttr | `com.ptc.windchill.bac.BACDeleteRecordAttributes` | duplicate | *(same as DeleteRecord)* |
| DeleteTracking | `com.ptc.windchill.bac.delegates.BACDeleteTrackingDelegate` | duplicate | DeleteTrackingDelegate |
| Processor | `com.ptc.windchill.bac.delegate.BACDeleteProcessor` | duplicate | DeleteProcessor |
| Report | `com.ptc.windchill.bac.delegates.BACReportDelegate` | duplicate | ReportDelegate |
| Identity | `wt.ixb.bac.BACIdentityDelegate` | duplicate | IdentityDelegate |
| SpecProcessor | `com.ptc.windchill.bac.delegates.BACSpecProcessor` | duplicate | SpecProcessor |

---

## Validation Steps

### 1. XML Syntax Validation
- Verify the modified `BAC-service.properties.xconf` has valid XML syntax
- Ensure all tags are properly closed
- Check for proper nesting of elements
- Verify no duplicate entries for the same selector within a service

### 2. Pattern Validation
- Confirm `cardinality="duplicate"` for all entries
- Confirm `requestor="java.lang.Object"` for all entries
- Verify `selector` matches the template name exactly (CASE-SENSITIVE)
- Verify `serviceClass` follows the correct package and class naming pattern

### 3. Completeness Check
- Verify all 8 delegate types are registered (7 unique service entries, as DeleteRecord and DeleteRecordAttr share one)
- Confirm each delegate file in `{delegate_folder}` has a corresponding service entry
- Check that no entries are missing

### 4. Service Block Location
Ensure entries are added in the correct service blocks:
```
✓ CollectionDelegate      → com.ptc.windchill.bac.delegates.BACCollectionDelegate
✓ DeleteRecordAttributes  → com.ptc.windchill.bac.BACDeleteRecordAttributes (handles both DeleteRecord and DeleteRecordAttr)
✓ DeleteTrackingDelegate  → com.ptc.windchill.bac.delegates.BACDeleteTrackingDelegate
✓ DeleteProcessor         → com.ptc.windchill.bac.delegate.BACDeleteProcessor
✓ ReportDelegate          → com.ptc.windchill.bac.delegates.BACReportDelegate
✓ IdentityDelegate        → wt.ixb.bac.BACIdentityDelegate
✓ SpecProcessor           → com.ptc.windchill.bac.delegates.BACSpecProcessor
```

---

## Common Errors to Avoid

1. **Wrong Service Name**: Using incorrect service name for delegate type
2. **Wrong Cardinality**: Not using "duplicate" cardinality
3. **Selector Mismatch**: Selector doesn't match CollectionCategory name
4. **Wrong Package Path**: serviceClass path doesn't match actual delegate location
5. **Duplicate Entries**: Adding entry when it already exists
6. **Wrong Service Block**: Adding entry to wrong service section
7. **Missing Closing Tags**: Not properly closing XML tags
8. **Case Sensitivity**: Selector or class name case mismatch

---

## Example Complete Registration

For a `WTChangeOrder2` object in the `change2` domain:

```xml
<!-- 1. Collection Delegate -->
<Service context="default" name="com.ptc.windchill.bac.delegates.BACCollectionDelegate">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2CollectionDelegate"/>
</Service>

<!-- 2. DeleteRecord Delegate (handles both DeleteRecord and DeleteRecordAttr) -->
<Service context="default" name="com.ptc.windchill.bac.BACDeleteRecordAttributes">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2DeleteRecordAttributes"/>
</Service>

<!-- 3. DeleteTracking Delegate -->
<Service context="default" name="com.ptc.windchill.bac.delegates.BACDeleteTrackingDelegate">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2DeleteTrackingDelegate"/>
</Service>

<!-- 4. Processor Delegate -->
<Service context="default" name="com.ptc.windchill.bac.delegate.BACDeleteProcessor">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2DeleteProcessor"/>
</Service>

<!-- 5. Report Delegate -->
<Service context="default" name="com.ptc.windchill.bac.delegates.BACReportDelegate">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2ReportDelegate"/>
</Service>

<!-- 6. Identity Delegate -->
<Service context="default" name="wt.ixb.bac.BACIdentityDelegate">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2IdentityDelegate"/>
</Service>

<!-- 7. SpecProcessor Delegate -->
<Service context="default" name="com.ptc.windchill.bac.delegates.BACSpecProcessor">
    <!-- ...existing entries... -->
    <Option cardinality="duplicate" requestor="java.lang.Object" selector="WTChangeOrder2" serviceClass="wt.change2.bac.BACWTChangeOrder2SpecProcessor"/>
</Service>
```

---

## Post-Registration Verification

After completing all registrations:

1. ✓ Validate XML syntax using `xmllint` or XML validator
2. ✓ Confirm 7 service entries added (8 delegates, but DeleteRecord and DeleteRecordAttr share one entry)
3. ✓ Verify selector matches CollectionCategory enum value
4. ✓ Verify serviceClass paths are correct and files exist
5. ✓ Check for NO duplicate entries
6. ✓ Ensure proper indentation and formatting
7. ✓ Confirm all entries added before respective `</Service>` closing tags

---

## Notes

- **CRITICAL**: The selector value MUST exactly match the CollectionCategory enum value (CASE-SENSITIVE)
- **CRITICAL**: DeleteRecord and DeleteRecordAttr delegates share the same service registration
- All entries use `cardinality="duplicate"`
- All entries use `requestor="java.lang.Object"`
- Service class path format: `wt.{domain}.bac.BAC{ObjectTemplate}{DelegateType}`
- Always add entries BEFORE the closing `</Service>` tag
- Never modify existing service entries
- Each object requires 7 service registrations (not 8, because DeleteRecord and DeleteRecordAttr are combined)