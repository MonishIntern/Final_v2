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
    - Do not add any new attributes in object if user havent provided it

### Pattern for BACSpec.xsd Update
- ```xml
<xs:complexType name="Template">    </xs:complexType>

<xs:element name="Template" type="bac:Template" minOccurs="0" maxOccurs="1" />
```