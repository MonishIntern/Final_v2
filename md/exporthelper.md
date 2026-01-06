# BACExportHelper: Add Object Instructions

## Purpose
This file documents the exact steps and the XML fragment to add a new object type into `BACExportHelper.java`'s `typesXmlSpec` method. Follow this template to ensure the generated `BACSpec` includes the new object.

## When to use
- You created new delegate classes and corresponding `CollectionCategory` entry (e.g., `WTMyObject`)
- You want the `BACExportHelper` to include the object in export BAC specs

## Overview of change
Open `BACExportHelper.java` and update the `typesXmlSpec(List<String> types)` method. The method builds an XML `BACSpec` by checking `CollectionCategory.toCollectionCategory(type)` and appending element tags for each supported category.

## Example change
For a new object template `WTMyObject` in domain `mydomain` the code addition should look like:

```java
spec.append(typeSet.contains(CollectionCategory.WTMyObject) ? "<WTMyObject />" : StringUtils.EMPTY);
```

Add that line near the other `spec.append(typeSet.contains(...` lines. Keep the same ordering style used by existing entries.

## Steps
1. Open file: `\\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\client\helpers\BACExportHelper.java`
2. Locate `private static BACSpec typesXmlSpec(List<String> types)` method
3. Inside the loop/after the `typeSet` construction, add a single `spec.append(...)` line for your new object as shown above
4. Use `CollectionCategory.<ENUM_NAME>` where `<ENUM_NAME>` is the enum constant added to `CollectionCategory` for your object. This constant is usually the template name (CASE-SENSITIVE)
5. Save the file

## Validation
- Ensure Java compiles locally (inside Windchill build environment). Example commands:

```bash
# inside WindchillVM WSL
wsl -d WindchillVM -- bash -c "cd /opt/wnc/wcmod/modules/BAC/src && ant clean clobber all"
```

- Run unit tests if available
- Optionally, call `BACSpec.newBACSpec(spec.toString())` in a small unit test to validate the generated XML string parses correctly

## Example block with surrounding lines
Use the following context to add your new append line (example insertion for `WTMyObject`):

```java
spec.append(typeSet.contains(CollectionCategory.UI_COMPONENT_CONFIGURATION) ? "<UIComponentConfiguration />" : StringUtils.EMPTY);
spec.append(typeSet.contains(CollectionCategory.WTMyObject) ? "<WTMyObject />" : StringUtils.EMPTY);
spec.append(typeSet.contains(CollectionCategory.WTChangeOrder2) ? "<WTChangeOrder2 />" : StringUtils.EMPTY);
```

## Notes
- The `CollectionCategory` enum must contain the new entry before adding the `spec.append` line
- Keep `CARDINALITY`/naming conventions consistent with existing code
- Use `StringUtils.EMPTY` for the false case to match existing style

## Quick checklist
- [ ] `CollectionCategory` enum includes `WTMyObject`
- [ ] `BACExportHelper.java` updated with `spec.append(...)` line
- [ ] Code compiles in Windchill VM and unit tests pass
- [ ] Optional: `BACSpec.newBACSpec(...)` validates the XML

---

If you want, I can add the `spec.append` line directly to `BACExportHelper.java` for a specific object name — tell me the template (`WTMyObject`) and domain (`mydomain`) and I'll patch the file (dry-run by default).