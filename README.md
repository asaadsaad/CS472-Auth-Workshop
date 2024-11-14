## CS472-Auth-Workshop
Create a simple authentiation workflow for users to signup, signin, and request data from a protected route.
## sign-up
* client sends `{email, fullname}` to web server
* server checks if email does not exist: save, else: error
    
## sign-in
* client sends email to the web server
* server generates `enc_data = encode({email, fullname})`
* server generates `hash_data = hash(enc_data, secret)`
* server sends both `{enc_data, hash_data}` to client

## workflow
* client saves `{enc_data, hash_data}` and is expected to send them with every future request header to server
* client can decode and read the data `decode(enc_data)`, reading `email`, and `fullname`
* server has middleware to read `{enc_data, hash_data}` from the request header, and verifies if `hash(enc_data, secret) === hash_data` before allowing access to protected routes

```typescript
import { createHmac } from 'node:crypto';

const secretKey = 'theo';
const data = JSON.stringify({ email: 'asaad@miu.edu', fullname: 'Asaad Saad' });

// encode text to base64
const data_buffer = Buffer.from(data);
const base64 = data_buffer.toString("base64");

// decode base64 to text
const base64_buffer = Buffer.from(base64, "base64");
const decoded_data = base64_buffer.toString("utf-8");

// hash using SHA256, Hash-Based Message Authentication Code
const hmac = createHmac('sha256', secretKey);
hmac.update(data);
const hash = hmac.digest('hex');
```
