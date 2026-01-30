## System Architecture
Below is the C4 Level 2 diagram rendered directly from DSL.

```kroki-structurizr
workspace {
    model {
        user = person "User"
        softwareSystem = softwareSystem "Software System" {
            webapp = container "Web Application"
            db = container "Database"
        }
        user -> webapp "Uses"
        webapp -> db "Reads/Writes"
    }
    views {
        container softwareSystem "Containers" {
            include *
            autolayout lr
        }
    }
}
```
