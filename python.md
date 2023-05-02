# Python

Small code snippets.

## Base64 library

Can be used to encode/decode hex and not only base64. Since it can handle base16 it makes it fully possible to also work with hex.

Encode string as hex and output as string in hex format:
~~~python
import base64

t = "some data"
tn = base64.b16encode(t.encode())
print(tn.decode())
~~~

Output: `736F6D652064617461`
