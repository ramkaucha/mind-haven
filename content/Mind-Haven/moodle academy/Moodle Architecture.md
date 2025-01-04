---
title: Moodle Architecture
tags:
  - moodle-academy
  - tutorial
cssclasses:
  - pen-white
author: Ram
---
Identifying the major components of moodle
creating text filter and block plugin

## Application architecture
### Core
Core libraries inside `/lib/` folder
Subsystems and their paths are defined in `lib/components.json` file
snipped:
```json
"subsystems": {  
        "access": null,  
        "admin": "admin",  
        "adminpresets": "admin/presets",  
        "analytics": "analytics",  
        "antivirus": "lib\/antivirus",  
        "auth": "auth",  
        "availability": "availability",  
        "backup": "backup\/util\/ui",  
        "badges": "badges",  
        "block": "blocks",  
        "blog": "blog",  
        "bulkusers": null,  
        "cache": "cache",  
        "calendar": "calendar",  
        "cohort": "cohort",  
        "comment": "comment",  
        "competency": "competency",  
        "completion": "completion",  
        "contentbank": "contentbank",  
        "countries": null,  
        "course": "course",  
        "courseformat": "course\/format",  
        "currencies": null,  
        "customfield": "customfield",  
        ...,  
        ...  
}
```
e.g. `course` is a subsystem and its path is `course`, the `courseformat` subsystem is located at `course/format`, therefore, all course format plugins will be placed in `course/format`

### Third party libraries
list of third-party libraries used from Site administration > Development > Third party libraries
`thirdpartylibs.xml` file contains details of each library used.

### Plugins
the plugin types and their paths are defined in the `lib/components.json` file
an activity plugin will be installed in `mod/` folder, whereas a course format will be installed in the `course/format` folder.

### Database structure
most plugins have their own tables
database structure for each plugin is defined in `install.xml` in the `db/` folder
