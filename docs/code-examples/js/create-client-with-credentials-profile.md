# Create client with credentials profile

```js
import { FooClient } from "@aws-sdk/client-foo"
import { fromIni } from "@aws-sdk/credential-providers"

const client = new FooClient({
  region: "region",
  credentials: fromIni({
    profile: "profile-name",
  }),
})
```
