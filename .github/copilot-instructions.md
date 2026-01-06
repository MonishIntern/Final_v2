# BAC Object Registration Automation Instructions

## CRITICAL EXECUTION RULES
1. **NO HALLUCINATIONS**: Only use information from `object.json` and MD template files
2. **NO ASSUMPTIONS**: If information is missing, ask the user explicitly
3. **NO SKIPPING**: Read complete files without line limits
4. **NO BACKUPS**: Do not create backup files unless explicitly requested
5. **STRICT VALIDATION**: Verify each step completion before proceeding to next step
6. **EXACT PATHS**: Use the configured paths exactly as defined below
7. **NAME CONVENTIONS**: Follow naming conventions strictly as per templates and it should be consistent across all files and CollectionCategory naming should be just CASE-SENSITIVE object template name
8. **DRY RUN DEFAULT**: All MCP tool invocations should default to `dry_run=true` unless specified otherwise
9. **USE `wsl -d WindchillVM` after step 12**: to run commands in WindchillVM WSL before that do not use this
---

## PATH CONFIGURATION (DO NOT MODIFY)
```
wt_path              = \\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\wt\
test_path            = \\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src_test\wt\
object_json_path     = c:\final_v2\.github\object.json
bac_src_path         = \\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\
ixl_handler_path     = \\wsl.localhost\WindchillVM\opt\wnc\Windchill\DevModules\IXLoad\src\wt\ixb\handlers\forclasses\
obj_registry_path    = \\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\objreg\
service_xconf_path   = \\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\BAC-service.properties.xconf
md_templates_path    = C:\final_v2\md\
test_templates_path   = C:\final_v2\test\
preview = `//wsl.localhost/WindchillVM/opt/wnc/wcmod/modules/BAC/src/com/ptc/windchill/bac/client/delegates/preview/impl/`
preview_delegate_path = `//wsl.localhost/WindchillVM/opt/wnc/wcmod/modules/BAC/src/com/ptc/windchill/bac/client/`

```

---

## TO BE NOTED
- CollectionCategory naming should be just CASE-SENSITIVE object template name
- Use `wsl -d WindchillVM -- <command>` after 12 steps to run commands in WindchillVM WSL
## WORKFLOW EXECUTION SEQUENCE

### STEP 1: Object Selection & Initialization
**ACTION**: 
- Read `object.json` from `object_json_path` completely (no line limits)
- Display all available objects to user
- Ask user: "Which object would you like to work with?"
- Store user's selection in variable `selected_object`
- Extract all properties for `selected_object`:
  - `template`
  - `directory` 
  - `key_attributes`
  - `dependencies`

**VALIDATION**: Confirm all object properties are extracted before proceeding

---

### STEP 2: Directory Structure Creation
**ACTION**:
- Determine directory name from `selected_object.directory` or use object name if directory not specified
- Create folder structure: `{wt_path}{directory_name}\bac\`
 - Ensure directories: create `{wt_path}{directory_name}\bac\` and `{test_path}{directory_name}\bac\`

**VALIDATION**: Verify directory creation successful before proceeding

---

### STEP 3: Register Admin Object

**INSTRUCTION FILE**: `{md_templates_path}register.md`

**ACTION**:
- Read complete `register.md` file (no line limits) for guidance on admin object registration
- Follow template instructions for proper registration patterns

## Admin object registration guidance
- Register the admin object by following the `register.md` template instructions. Ensure XML syntax is valid and do not modify existing entries.

**VALIDATION**: 
- Confirm XML syntax is valid
- Confirm new entry added
- DO NOT modify existing entries

---

### STEP 4: Register Collection Category

**INSTRUCTION FILE**: `{md_templates_path}category.md`

**ACTION**:
- Read complete `category.md` file (no line limits) for guidance on collection category registration
- Follow template instructions for proper category naming and registration patterns

## Collection category registration guidance
- Update `CollectionCategory.java` by adding `public static final_v2 CollectionCategory <Template> = toCollectionCategory("<Template>");` following the existing constant anchors. Use the CASE-SENSITIVE template name.

**VALIDATION**: Confirm category registration complete

---

### STEP 5: Generate Delegate Files

- Collection Category naming should be just CASE-SENSITIVE object template name 

**INSTRUCTION FILES**: 
- Collection Delegate --> `collection.md`
- DeleteRecord Delegate --> `deleterecord.md`
- DeleteRecordAttr Delegate --> `deleterecordattr.md`
- Identity Delegate --> `identity.md`
- Processor Delegate --> `processor.md`
- Report Delegate --> `report.md`
- SpecProcessor Delegate --> `spec.md`
- Tracking Delegate --> `tracking.md`

**ACTION FOR EACH FILE**:
1. Read complete MD file from `{md_templates_path}` (no line limits)
2. Generate Java delegate file following template instructions
3. Replace all placeholders with actual values from `selected_object`
4. Save generated file to: `{wt_path}{directory_name}\bac\{DelegateName}.java`
5. Ensure proper Java syntax and package declarations

**VALIDATION**: 
- Verify 8 delegate files created
- Confirm each file has valid Java syntax
- Confirm proper package naming: `wt.{directory_name}.bac`

---

### STEP 6: Generate ExpImp Handler
**INSTRUCTION FILE**: `{md_templates_path}expimp.md`

**ACTION**:
- Read complete `expimp.md` file (no line limits)
- Generate handler Java file following template
- Save to: `{ixl_handler_path}{selected_object.template}.java`
- Ensure proper imports and class structure

**VALIDATION**: Confirm handler file created with valid Java syntax

---

### STEP 7: Register Services

**INSTRUCTION FILE**: `{md_templates_path}services.md`

**ACTION**:
- Read complete `services.md` file (no line limits) for guidance on service registration
- Follow template instructions for proper service configuration patterns

- `service_path` = `\\\\wsl.localhost\\WindchillVM\\opt\\wnc\\wcmod\\modules\\BAC\\src\\com\\ptc\\windchill\\bac\\BAC-service.properties.xconf`
- `delegate_folder` = `\\\\wsl.localhost\\WindchillVM\\opt\\wnc\\wcmod\\modules\\BAC\\src\\wt\\{object_directory}\\bac\\`

## Service registration guidance
- Register services in `BAC-service.properties.xconf` by adding `<Option>` entries that match the delegates you created. Follow the pattern used by existing entries and validate XML syntax.

## Validation Steps
1. **First**: After adding the new service registration, validate the `BAC-service.properties.xconf` file for correct XML syntax
2. **Second**: Ensure that the new service registration follows the existing pattern and conventions
3. **Third**: Each delegate from `delegate_folder` should have a corresponding service registration entry in `BAC-service.properties.xconf`

**VALIDATION**:
- Confirm all 8 delegates registered
- Validate XML syntax
- DO NOT modify existing service entries

---

### STEP 8: Update BAC Specifications

**INSTRUCTION FILE**: `{md_templates_path}bacspec.md`

**ACTION**:
- Read complete `bacspec.md` file (no line limits) for guidance on BAC specification updates
- Follow template instructions for proper XSD schema patterns and element definitions

## BAC specification update guidance
- Update `BACSpec.xsd` by adding the appropriate `<xs:complexType>` and `<xs:element>` entries for the new template. Follow the existing schema conventions and validate XSD syntax.

**VALIDATION**: Confirm spec entry added correctly

---

### STEP 9: Update Delete Records

**INSTRUCTION FILE**: `{md_templates_path}generic.md`

**ACTION**:
- Read complete `generic.md` file (no line limits) for guidance on delete record configuration
- Follow template instructions for proper delete record schema patterns

## Delete record update guidance
- Update `BACGenericDeleteRecordObjInfos.xsd` by adding the necessary `<xs:element>` entries for the template, following existing patterns and validating XSD syntax.

**VALIDATION**: Confirm delete record entry added

---

### STEP 10: Update RB Info

**INSTRUCTION FILE**: `{md_templates_path}rb.info.md`

**ACTION**:
- Read complete `rb.info.md` file (no line limits) for guidance on RB info configuration
- Follow template instructions for proper resource bundle entry patterns

## Directory Configuration
- `category_path` = `\\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\`

## Category Registration Process
- Get `CollectionCategoryRB.rbInfo` file from `category_path`
- Understand the structure of `CollectionCategoryRB.rbInfo` file
- Based on the object information from `object.json`, identify the categories to be registered
- Register the same Template name which is used above in chat
- For each category to be registered, create a new entry in `CollectionCategoryRB.rbInfo` file
- Follow the existing pattern for category registration

---

### STEP 11: Batch Preview

**INSTRUCTION FILE**: `{md_templates_path}preview.md`

**ACTION**:
- Read `preview.md` which contains instructions to create a `BatchPreviewDelegate` implementation.
- Use `preview.md` to generate a `Batch[ObjectName]PreviewDelegate.java` (BatchPreviewDelegate) following the guidance in that file.
- Suggested sections in `preview.md` for the human-readable batch preview summary:
  - Summary: object name and template
  - Files created: bullet list with full paths
  - Registry entries added: brief descriptions and locations
  - Warnings/Errors: any issues encountered during generation
- then read a `BAC-delegates.xconf` file from `preview_delegate_path` to register the generated delegate.
- understand the structure of `BAC-delegates.xconf` file
- Add a new entry for the generated `BatchPreviewDelegate` in `BAC-delegates.xconf` file following the existing pattern.
- and ensure proper package declaration and imports in the generated Java file. 

**OUTPUT LOCATION**:
- The generated `BatchPreviewDelegate` (if created) should be saved to the delegate output folder noted in `preview.md` (default: `.github/generated_delegates/`) or to `{preview}` if configured that way.

**VALIDATION**: Confirm `preview.md` exists and contains the sections above

### STEP 12: Delta Controller

**INSTRUCTION FILE**: `{md_templates_path}deltacontroller.md`

**VALIDATION**:
- Confirm the DeltaController file is updated in the controllers directory
- Verify Java syntax for the modified file (or request build logs if compilation is required)

---

### STEP 12.1: Register Export Helper

**INSTRUCTION FILE**: `{md_templates_path}exporthelper.md`

**ACTION**:
- Read `exporthelper.md` for exact guidance to add the object entry to `BACExportHelper.java`.
- Update `typesXmlSpec(List<String> types)` in `BACExportHelper.java` by adding a single `spec.append(...)` line for the object template.

Example insertion:
```
spec.append(typeSet.contains(CollectionCategory.WTMyObject) ? "<WTMyObject />" : StringUtils.EMPTY);
```

**LOCATION**:
- File: `\\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\src\com\ptc\windchill\bac\client\helpers\BACExportHelper.java`
- Method: `private static BACSpec typesXmlSpec(List<String> types)`

**VALIDATION**:
- Confirm the `CollectionCategory` enum contains the new entry (CASE-SENSITIVE)
- Confirm the new `spec.append(...)` line was added and is not duplicated
- Verify Java syntax (build in WindchillVM WSL)
- Optionally call `BACSpec.newBACSpec(spec.toString())` in a unit test to validate generated XML

**DRY-RUN DEFAULT**: Make changes locally and verify before committing to repo or production environment

---

### STEP 13: Test Generation
**INSTRUCTION FILE**: 
CollectionTest --> `{test_templates_path}collectionTest.md`
DeleteRecordTest --> `{test_templates_path}deleterecordTest.md`
DeleteRecordAttrTest --> `{test_templates_path}deleterecotrdattrTest.md`
DeleteTrackingTest --> `{test_templates_path}deletetrackingTest.md`
IdentityTest --> `{test_templates_path}identityTest.md`
ProcessorTest --> `{test_templates_path}processorTest.md`
ReportTest --> `{test_templates_path}reportTest.md`
SpecTest --> `{test_templates_path}specTest.md`
TrackingTest --> `{test_templates_path}trackingTest.md`

**ACTION**:
**ACTION FOR EACH FILE**:
1. Read complete MD file from `{test_templates_path}` (no line limits)
2. Generate Java delegate file following template instructions
3. Replace all placeholders with actual values from `selected_object`
4. Save generated file to: `{test_path}{directory_name}\bac\{DelegateTestName}.java`
5. Ensure proper Java syntax and package declarations

**VALIDATION**: 
- Verify 9 delegateTest files created
- Confirm each file has valid Java syntax
- Confirm proper package naming: `wt.{directory_name}.bac`

---



## COMPLETION CHECKLIST
After completing all steps, verify:
- [ ] Admin object registered in `obj_registry.xml`
- [ ] Collection category registered
- [ ] 8 delegate files generated in `{wt_path}{directory_name}\bac\`
- [ ] ExpImp handler generated in `{ixl_handler_path}`
- [ ] All delegates registered in `BAC-service.properties.xconf`
- [ ] BAC specifications updated
- [ ] Delete records updated
- [ ] RB info updated
- [ ] (If applicable) BatchPreviewDelegate generated and saved
- [ ] (If applicable) DeltaController updated/created
- [ ] 9 test delegate files generated in `{test_path}{directory_name}\bac\`
- [ ] All validations for each step confirmed

**final_v2 ACTION**: Report completion status to user with summary of all files created/modified

---

## ERROR HANDLING
- If any step fails, STOP and report error to user
- If information is missing from `object.json`, ASK user for clarification
- If MD template is unclear, ASK user for guidance
- DO NOT proceed with assumptions or placeholder values
- DO NOT hallucinate file contents or configurations

---

## final_v2 BUILD & ENVIRONMENT STEPS
After all registration and file-generation steps are completed and validated, perform the following environment and build steps to compile the BAC sources.

1. **Set Windchill build environment (inside WindchillVM WSL)**:
  - Run it inside the `WindchillVM` WSL distribution (non-interactive example):

```
wsl -d WindchillVM
```

2. Set Windchill environment :

```
wnc_env urel_241
```

3. After Setting the environment, navigate to the BAC module directory:

```
cd /opt/wnc/wcmod/modules/BAC/src
```

4. **Compile BAC module using Ant**:
  - Execute the Ant build command to compile the BAC sources:

```
ant clean clobber all
```

5. **Post-Build Validation**:

``` 
start_windchill
```

- Every command in this should be seperate and new command block as every previous command should be completed before next command is run.

---

## STEP 14: Execute BAC SQL Scripts

**PURPOSE**: After a successful build, execute the required SQL scripts to set up database objects and configurations for the registered BAC object.

**SQL SCRIPTS LOCATION**: 
```
sql_scripts_path = \\wsl.localhost\WindchillVM\opt\wnc\wcmod\modules\BAC\db\sql3\
```

**ACTION**:
1. **Identify SQL Script Files**:
   - Based on the `selected_object` and its template name
   - Locate corresponding SQL scripts in `{sql_scripts_path}` directory
   - Common SQL script patterns:
     - `create_<object_name>_tables.sql` - Creates database tables
     - `insert_<object_name>_metadata.sql` - Inserts metadata records
     - `grant_<object_name>_permissions.sql` - Grants necessary permissions
     - `create_<object_name>_indexes.sql` - Creates database indexes

2. **Database Connection Configuration**:
   - Retrieve database connection details from Windchill configuration:
     - Host: From `wt.properties` or environment variables
     - Port: Default Oracle port (1521) or configured port
     - Service Name/SID: From Windchill database configuration
     - Username: Windchill database user (typically from `wt.properties`)
     - Password: Windchill database password (secure retrieval)

3. **Execute SQL Scripts**:
   - Use `wsl -d WindchillVM` to execute SQL scripts within the WindchillVM environment
   - Execute scripts in the correct order:
     1. Table creation scripts first
     2. Metadata insertion scripts second
     3. Index creation scripts third
     4. Permission grants last
   
   Example execution command pattern:
   ```bash
   wsl -d WindchillVM -- bash -c "source /opt/wnc/wnc_env urel_241 && sqlplus <username>/<password>@<host>:<port>/<service> @/opt/wnc/wcmod/modules/BAC/db/sql3/<script_name>.sql"
   ```

4. **Script Execution Validation**:
   - Verify each script executes successfully (exit code 0)
   - Check for SQL errors in output
   - Validate that expected database objects were created:
     - Query database to confirm tables exist
     - Verify metadata records were inserted
     - Confirm indexes were created

5. **Error Handling**:
   - If script execution fails, capture error message
   - Log failed script name and error details
   - STOP execution and report to user
   - Provide rollback guidance if needed

**VALIDATION CHECKLIST**:
- [ ] SQL scripts identified for selected object
- [ ] Database connection parameters retrieved
- [ ] Scripts executed in correct order
- [ ] All scripts completed without errors
- [ ] Database objects verified to exist
- [ ] Metadata records confirmed in database
- [ ] Indexes created successfully

**EXAMPLE EXECUTION**:
For a `WTChangeOrder2` object:
```bash
# Step 1: Navigate to SQL directory
wsl -d WindchillVM -- bash -c "cd /opt/wnc/wcmod/modules/BAC/db/sql3"

# Step 2: Execute table creation
wsl -d WindchillVM -- bash -c "source /opt/wnc/wnc_env urel_241 && sqlplus wcadmin/password@localhost:1521/windchill @create_wtchangeorder2_tables.sql"

# Step 3: Execute metadata insertion
wsl -d WindchillVM -- bash -c "source /opt/wnc/wnc_env urel_241 && sqlplus wcadmin/password@localhost:1521/windchill @insert_wtchangeorder2_metadata.sql"

# Step 4: Verify database objects
wsl -d WindchillVM -- bash -c "source /opt/wnc/wnc_env urel_241 && sqlplus -s wcadmin/password@localhost:1521/windchill <<< 'SELECT table_name FROM user_tables WHERE table_name LIKE \"%WTCHANGEORDER2%\";'"
```

**POST-EXECUTION**:
- Restart Windchill services to ensure changes are loaded:
```bash
wsl -d WindchillVM -- bash -c "source /opt/wnc/wnc_env urel_241 && windchill stop && windchill start"
```

**NOTES**:
- SQL scripts may need to be generated based on object template if not pre-existing
- Database credentials should be handled securely (environment variables or secure vault)
- Each SQL script execution should be logged for audit purposes
- Consider transaction rollback capabilities for failed executions
