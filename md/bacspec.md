## BAC Specification Instructions

### Directory Configuration
`bacspec_path` = `\\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\schema\`

### BAC Specification Update Process
1. **Navigate to BAC Specification Directory**:
   ```bash
   cd {bacspec_path}
   ```
2. **BACSpec.xsd**:
   - Locate the `BACSpec.xsd` file in the `bacspec_path` directory.
   - This file defines the XML schema for BAC specifications.
3. **Update BACSpec.xsd**:
    - Based on the object information from `object.json`, identify the necessary updates to the BAC specifications.
    - Make the required changes to the `BACSpec.xsd` file, ensuring that all new elements and attributes are correctly defined according to XML schema standards.
    - Do not add any new attributes in object if user hasn't provided it
    - **CRITICAL**: Always use CASE-SENSITIVE template names from `object.json`

### Schema Structure Overview
The `BACSpec.xsd` file has TWO main sections:
1. **ComplexType Definitions** (near the top of the file)
2. **BACSpec Element** (at the bottom of the file)

### Pattern for BACSpec.xsd Update

#### Step 1: Add ComplexType Definition
Add the complexType definition **BEFORE** the `<xs:element name="BACSpec">` element.

**Basic Pattern (minimal attributes)**:
```xml
<xs:complexType name="TemplateName">
</xs:complexType>
```

**Pattern with Attributes** (use if object has key_attributes):
```xml
<xs:complexType name="TemplateName">
    <xs:sequence>
        <xs:element name="attributeName1" type="xs:string" minOccurs="0" maxOccurs="1"/>
        <xs:element name="attributeName2" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
</xs:complexType>
```

**Example (from existing WTChangeOrder2)**:
```xml
<xs:complexType name="WTChangeOrder2">
    <xs:sequence>
        <xs:element name="name" type="xs:string" minOccurs="0" maxOccurs="1"/>
        <xs:element name="number" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
</xs:complexType>
```

#### Step 2: Add Element Reference in BACSpec
Add the element reference **INSIDE** the `<xs:element name="BACSpec">` section, within the `<xs:sequence>` tag, **BEFORE** the closing `</xs:sequence>` tag.

**Pattern**:
```xml
<xs:element name="TemplateName" type="bac:TemplateName" minOccurs="0" maxOccurs="1"/>
```

**Location Example**:
```xml
<xs:element name="BACSpec">
    <xs:complexType>
        <xs:sequence>
            <!-- Existing elements like GlobalContainerCriteria, Profile, etc. -->
            <xs:element name="UIComponentConfiguration" type="bac:UIComponentConfiguration" minOccurs="0" maxOccurs="1"/>

            <!-- ADD NEW ELEMENT HERE -->
            <xs:element name="TemplateName" type="bac:TemplateName" minOccurs="0" maxOccurs="1"/>
        </xs:sequence>
    </xs:complexType>
</xs:element>
```

### Complete Example (Full Registration)

For an object with template name `WTPart`:

**1. Add ComplexType Definition (before BACSpec element)**:
```xml
<xs:complexType name="WTPart">
    <xs:sequence>
        <xs:element name="number" type="xs:string" minOccurs="0" maxOccurs="1"/>
        <xs:element name="name" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
</xs:complexType>
```

**2. Add Element Reference (inside BACSpec sequence)**:
```xml
<xs:element name="WTPart" type="bac:WTPart" minOccurs="0" maxOccurs="1"/>
```

### Important Notes
1. **Namespace**: Always use `bac:` prefix for type references (e.g., `type="bac:WTPart"`)
2. **Naming**: Use the exact CASE-SENSITIVE template name from `object.json`
3. **Ordering**: 
   - ComplexType definitions go BEFORE the BACSpec element
   - Element references go INSIDE the BACSpec sequence
4. **Attributes**: 
   - Only add attributes if they exist in `object.json` under `key_attributes`
   - Use `minOccurs="0" maxOccurs="1"` for optional single-value attributes
   - Use appropriate XML schema types: `xs:string`, `xs:boolean`, `xs:int`, etc.
5. **Validation**: Ensure XML syntax is valid (matching tags, proper nesting)
6. **DO NOT**: Modify existing entries when adding new ones

### Validation Checklist
- [ ] ComplexType definition added before BACSpec element
- [ ] Element reference added inside BACSpec sequence
- [ ] Template name is CASE-SENSITIVE and matches `object.json`
- [ ] Namespace prefix `bac:` is used correctly
- [ ] XML syntax is valid (no unclosed tags)
- [ ] Attributes match those defined in `object.json` (if any)
- [ ] No existing entries were modified