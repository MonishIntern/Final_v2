## Directory Configuration
- `ba_path` = `\\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\`

## Directory Access Commands
```bash
# Navigate to target directory
cd {ba_path}
# Show directory structure
ls -la
```
## collection category
- Ask User for Object Name to add Collection Category
- After getting Object Name from user, perform following steps
- Object_name = User Provided Object Name
- navigate to directory:
```bash
find . -name "*CollectionCategory*.java" -exec echo "=== FILE: {} ===" \; -exec cat {} \;
```
- understand existing Collection Category patterns
```java
public static final CollectionCategory TEMPLATE = toCollectionCategory("Template");
```
- Add Collection Category for the provided Object Name
- add this object as a new constant in the CollectionCategory enum or class