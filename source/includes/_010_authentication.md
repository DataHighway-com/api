
# Authentication

> To gain authorized access where required, use this code:

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
```

```shell
# Pass the correct header with each request
curl "INSERT_DH_API_ENDPOINT"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
```

>  Make sure to replace `INSERT_DH_API_KEY` with your personal API key.

Data Highway uses API keys to allow access to certain parts of the API. Data Sellers of the Data Highway's Inter-chain Data Market may register a new Data Highway API key at our [developer portal](http://datahighway.com/developers).

Data Highway expects for the API key to be included in API requests to the server that required authorization in a header that looks like the following:

`Authorization: INSERT_DH_API_KEY`

<aside class="notice">
You must replace <code>INSERT_DH_API_KEY</code> with your personal API key.
</aside>
