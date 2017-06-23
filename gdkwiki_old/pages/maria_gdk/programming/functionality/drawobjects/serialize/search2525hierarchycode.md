# Search C2525

Use C2525CodeParser to find the C2525 code for a hierarchy code.

```csharp
C2525CodeParser parser = new C2525CodeParser();
parser.Load();

//returns true if code is known. A draw object with this code will be "compressed".
var result = parser.FindHierarchyCode("TACGRP.TSK.DLY");
```
