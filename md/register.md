## BAC Admin Object Registration Instructions

### Directory Configuration
`obj_path` = `\\\\wsl.localhost\\WindchillVM\\opt\\wnc\\wcmod\\modules\\BAC\\src\\com\\ptc\\windchill\\bac\\objreg\\`

### Directory Access Commands
```bash
# Navigate to target directory
cd {obj_path}

# Show directory structure
ls -la
```

### Object Registration Process
- Understand the structure of object registration files in `obj_path` of `obj_registry.xml`
- Identify the appropriate file for registering a new BAC Admin Object
- Based on the object details provided, create a new registration entry at the end of the appropriate registration file
- add this object as new entry in the appropriate registration file within `obj_path`
- Donot alert any other admin object 


