## BAC Generic Delete Record Schema Update Instructions

## Overview
This instruction file guides the update of `BACGenericDeleteRecordObjInfos.xsd` to register delete record object information schemas. This involves creating a complete XSD complexType with a sequence containing all object attributes.

## Directory Configuration
- `generic_xsd_path` = `\\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\schema\BACGenericDeleteRecordObjInfos.xsd`
- `object.json` = `c:/FINAL_V2/.github/object.json`

## Purpose
When objects are deleted in the BAC system, their delete records need to be tracked with all relevant attributes. This schema defines the structure for storing deleted object information.

---

## Schema Update Requirements

### Two Required Additions:
1. **complexType Definition**: Defines the structure with all attributes in a sequence
2. **Element Reference**: Declares the element that uses the complexType

---

## XSD Structure Pattern

### 1. ComplexType Definition
**Pattern**:
```xml
<xs:complexType name="{ObjectTemplate}">
    <xs:sequence>
        <xs:element name="{attribute1}" type="xs:string" minOccurs="0"/>
        <xs:element name="{attribute2}" type="xs:string" minOccurs="0"/>
        <xs:element name="{attribute3}" type="xs:string" minOccurs="0"/>
        <!-- Add all attributes from object.json -->
    </xs:sequence>
</xs:complexType>
```

**Example** (WTChangeOrder2 with standard attributes):
```xml
<xs:complexType name="WTChangeOrder2">
    <xs:sequence>
        <xs:element name="name" type="xs:string" minOccurs="0"/>
        <xs:element name="number" type="xs:string" minOccurs="0"/>
    </xs:sequence>
</xs:complexType>
```

### 2. Element Declaration
**Pattern**:
```xml
<xs:element name="{ObjectTemplate}" type="bac:{ObjectTemplate}"/>
```

**Example**:
```xml
<xs:element name="WTChangeOrder2" type="bac:WTChangeOrder2"/>
```

---

## Workflow Steps

### Step 1: Read Object Configuration
- Read `object.json` to extract:
  - `template` (e.g., "WTChangeOrder2")
  - `key_attributes` (list of attributes to include)
  - Any custom attributes defined for the object

**Safety check (required):** If `object.json` is missing any of `template` or `key_attributes`, STOP and ask the user — do not proceed with default or guessed values.

### Step 2: Read Schema File
- Open `BACGenericDeleteRecordObjInfos.xsd` from `generic_xsd_path`
- Understand the existing schema structure:
  - XML namespace declarations
  - Existing complexType definitions
  - Existing element declarations
  - Location for new additions

### Step 3: Check for Existing Entries
- Search for `<xs:complexType name="{ObjectTemplate}">`
- Search for `<xs:element name="{ObjectTemplate}"`
- If either exists, STOP and report (DO NOT duplicate)

### Step 4: Prepare Attribute List
From `object.json`, extract all attributes to include in the sequence:

**Standard Attributes** (commonly included):
- `name` - Object name
- `number` - Object number/identifier


**Custom Attributes** (from object.json):
- Extract from `key_attributes` array
- Include any domain-specific attributes

### Step 5: Build ComplexType Definition
Create the complexType with all attributes:

```xml
<xs:complexType name="{ObjectTemplate}">
    <xs:sequence>
        <!-- Standard attributes -->
        <xs:element name="name" type="xs:string" minOccurs="0"/>
        <xs:element name="number" type="xs:string" minOccurs="0"/>
        
        <!-- Custom attributes from object.json -->
        <xs:element name="{customAttribute1}" type="xs:string" minOccurs="0"/>
        <xs:element name="{customAttribute2}" type="xs:string" minOccurs="0"/>
    </xs:sequence>
</xs:complexType>
```

**Rules**:
- All elements use `type="xs:string"`
- All elements use `minOccurs="0"` (optional)
- Maintain proper indentation (4 spaces per level)
- Keep alphabetical order within categories (standard, then custom)

### Step 6: Build Element Declaration
Create the element reference:

```xml
<xs:element name="{ObjectTemplate}" type="bac:{ObjectTemplate}"/>
```

**Rules**:
- Element name MUST match complexType name exactly (CASE-SENSITIVE)
- Type MUST use `bac:` namespace prefix
- Single line, no closing tag content

### Step 7: Locate Insertion Points

#### For ComplexType:
- Find the section with existing `<xs:complexType>` definitions
- Add new complexType **BEFORE** the closing `</xs:schema>` tag
- Maintain alphabetical order if possible
- Add blank line before and after for readability

**Preferred precise insertion point:** Add the new complexType in the section where other `<xs:complexType>` definitions appear (above the `<xs:element name="BACGenericDeleteRecordObjInfos">` element). Do NOT append new complexTypes after the `BACGenericDeleteRecordObjInfos` element — that element contains the choice that references named types and should reference types declared earlier.

#### For Element:
- Find the section with existing `<xs:element>` declarations
- Add new element **BEFORE** the closing `</xs:schema>` tag
- Maintain alphabetical order if possible
- Usually elements come after all complexTypes

### Step 8: Insert Schema Updates
Insert both the complexType and element declarations at their appropriate locations.

---

## Complete Example

### Input from object.json:
```json
{
  "template": "WTChangeOrder2",
  "directory": "change2",
  "key_attributes": ["priority", "severity", "changeType"]
}
```

### Generated Schema Additions:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" 
           xmlns:bac="http://www.ptc.com/windchill/bac"
           targetNamespace="http://www.ptc.com/windchill/bac"
           elementFormDefault="qualified">

    <!-- ...existing complexTypes... -->

    <!-- ComplexType for WTChangeOrder2 -->
    <xs:complexType name="WTChangeOrder2">
        <xs:sequence>
            <!-- Standard attributes -->
            <xs:element name="name" type="xs:string" minOccurs="0"/>
            <xs:element name="number" type="xs:string" minOccurs="0"/>
            
            <!-- Custom attributes -->
            <xs:element name="priority" type="xs:string" minOccurs="0"/>
            <xs:element name="severity" type="xs:string" minOccurs="0"/>
            <xs:element name="changeType" type="xs:string" minOccurs="0"/>
        </xs:sequence>
    </xs:complexType>

    <!-- ...other complexTypes... -->

    <!-- Element declarations -->
    <!-- ...existing elements... -->
    
    <xs:element name="WTChangeOrder2" type="bac:WTChangeOrder2"/>
    
    <!-- ...other elements... -->

</xs:schema>
```

---

## Attribute Data Type Guidelines

### Default Type: xs:string
Use for most attributes (text, identifiers, names)

### Other Possible Types (if needed):
- `xs:int` - Integer numbers (counts, IDs)
- `xs:boolean` - True/false values
- `xs:dateTime` - Timestamp values
- `xs:decimal` - Decimal numbers

**Note**: In most BAC schemas, all attributes use `xs:string` for flexibility. Use other types only if explicitly required.

---

## Validation Steps

### 1. XML Syntax Validation
- Verify schema file has valid XML syntax
- Ensure all tags are properly closed
- Check proper nesting of elements
- Validate namespace declarations

### 2. ComplexType Validation
- ✓ ComplexType name matches ObjectTemplate (CASE-SENSITIVE)
- ✓ Contains `<xs:sequence>` wrapper
- ✓ All attributes have `minOccurs="0"`
- ✓ All attributes use appropriate data type (typically `xs:string`)
- ✓ Proper indentation maintained

### 3. Element Validation
- ✓ Element name matches complexType name exactly
- ✓ Uses `bac:` namespace prefix for type
- ✓ Single-line format with self-closing tag

### 4. Completeness Check
- ✓ All standard attributes included
- ✓ All custom attributes from object.json included
- ✓ No duplicate entries in schema
- ✓ Both complexType and element added

### 5. Schema Integration
- ✓ Entries added in correct locations
- ✓ Alphabetical order maintained (if applicable)
- ✓ Proper spacing and formatting
- ✓ No conflicts with existing definitions

---

## Standard Attributes Reference

Common attributes to include for most Windchill objects:

| Attribute | Description | Type |
|-----------|-------------|------|
| name | Object name | xs:string |
| number | Object identifier | xs:string |

**Note**: Not all objects need all attributes. Use object.json to determine which attributes are relevant.

---

## Common Errors to Avoid

1. **Case Sensitivity**: Element and type names must match exactly
2. **Namespace Missing**: Type must use `bac:` prefix for element declarations
3. **Duplicate Entries**: Check for existing entries before adding
4. **Wrong Location**: Add complexType and element in correct sections
5. **Missing minOccurs**: All sequence elements need `minOccurs="0"`
6. **Unclosed Tags**: Ensure proper XML structure
7. **Missing Attributes**: Include all attributes from object.json

---

## Special Considerations

### Master/Version Objects
If the object has a master-version relationship:
- Include both object and master attributes
- Add `masterReference` attribute
- Add `versionInfo` attribute

### Container Objects
If the object is a container:
- Add `containerType` attribute
- Add `contextId` attribute

### Lifecycle-Managed Objects
If the object uses lifecycle:
- Ensure `lifeCycleState` is included
- Add `lifeCycleTemplateName` if needed

---

## Post-Update Verification

After updating the schema:

1. ✓ Validate XML syntax using xmllint or XML validator:
   ```bash
   xmllint --noout --schema BACGenericDeleteRecordObjInfos.xsd
   ```

2. ✓ Verify both complexType and element were added

3. ✓ Confirm attribute count matches object.json specifications

4. ✓ Test schema by creating a sample XML instance

5. ✓ Rebuild BAC module to compile schema changes

---

## Example Attribute Mapping

### From object.json:
```json
{
  "template": "WTChangeOrder2",
  "key_attributes": [
    "name",
    "number"
  ]
}
```

### To XSD Sequence:
```xml
<xs:sequence>
    <xs:element name="name" type="xs:string" minOccurs="0"/>
    <xs:element name="number" type="xs:string" minOccurs="0"/>
</xs:sequence>
```

---

## Notes

- **CRITICAL**: All attribute elements MUST have `minOccurs="0"` (optional)
- **CRITICAL**: ComplexType name and element name must match exactly
- **CRITICAL**: Element type must use `bac:` namespace prefix
- Schema changes require BAC module rebuild to take effect
- Delete record schema is for tracking deleted objects, not live objects
- Attributes capture the state of the object at deletion time
- More attributes = more detailed delete tracking (recommended)
- Standard attributes provide baseline tracking for all objects
