### **Detecting CORS Vulnerabilities Using the Terminal**

You can use several different methods through the terminal to detect CORS (Cross-Origin Resource Sharing) vulnerabilities. These methods typically involve inspecting HTTP requests and headers to identify potential security flaws. Below are some steps and tools you can use to detect CORS vulnerabilities from the terminal.

1. **Inspecting HTTP Headers with `curl`**

`curl` is a powerful tool that allows you to send HTTP requests from the terminal and inspect their headers. To detect CORS headers, you can use the following command.

For example, you can send an `OPTIONS` request to the target server to check which origins are accepted and which HTTP headers are sent.

Use the following command to check CORS headers:

bash
`curl -i -X OPTIONS https://target-api.com -H "Origin: https://your-website.com"`

- `-i`: Shows response headers.
- `-X OPTIONS`: Sends an `OPTIONS` request to the server.
- `-H "Origin: https://your-website.com"`: Specifies the requesting origin, which triggers the CORS policy.

In the output, you can check for the following headers:

- **Access-Control-Allow-Origin**: Specifies which origins are allowed to make requests. If this header is set to `*`, it means all origins are permitted, which could indicate a CORS vulnerability.
- **Access-Control-Allow-Methods**: Specifies which HTTP methods (GET, POST, PUT, DELETE, etc.) are allowed.
- **Access-Control-Allow-Headers**: Specifies which custom headers can be used in requests.
- **Access-Control-Allow-Credentials**: Indicates whether credentials (cookies, HTTP authentication) are allowed with requests.

**Example Output:**

bash
`HTTP/1.1 200 OK Access-Control-Allow-Origin: * Access-Control-Allow-Methods: GET, POST, PUT, DELETE Access-Control-Allow-Headers: Content-Type, Authorization Access-Control-Allow-Credentials: true ...`

Here, since the `Access-Control-Allow-Origin: *` header is permissive, a CORS vulnerability might exist.

2. **Checking Preflight Requests with `curl`**

In some cases, browsers send a preflight (or OPTIONS) request for cross-origin requests. You can check this preflight request by sending another `OPTIONS` request using `curl`.

bash
`curl -i -X OPTIONS https://target-api.com/resource -H "Origin: https://your-website.com"`

This checks whether the preflight request (OPTIONS) is properly handled and whether the CORS headers are present in the response.

3. **Inspecting HTTP Headers with `http` or `wget`**

Alternative tools like `http` and `wget` can also be used to inspect HTTP headers.

**Using the `http` command:** You can use the `http` command (from the HTTPie tool) to view CORS headers:

bash
`http -v OPTIONS https://target-api.com/resource Origin:https://your-website.com`

The `-v` option will show detailed response headers. Ensure you set the correct `Origin` header.

**Using the `wget` command:** Alternatively, `wget` can also display HTTP headers:

bash
`wget --server-response --method=OPTIONS https://target-api.com/resource --header="Origin: https://your-website.com"`

This command will show the server's response headers.

4. **Detecting CORS Vulnerabilities with Burp Suite or OWASP ZAP**

For a more comprehensive security test outside the terminal, you can use tools like Burp Suite or OWASP ZAP. These tools can perform detailed security scans on web applications and automatically detect CORS vulnerabilities.

With these tools, you can:

- Inspect all HTTP requests sent to the server,
- Verify if CORS headers are properly configured,
- Simulate requests from unauthorized origins.

5. **Testing CORS with Python and the `requests` Module**

You can also use Python to inspect CORS headers. The `requests` module allows you to send an `OPTIONS` request to check the headers.

Example Python code:

python
`import requests  url = 'https://target-api.com/resource' headers = {'Origin': 'https://your-website.com'}  response = requests.options(url, headers=headers)  print("Access-Control-Allow-Origin:", response.headers.get('Access-Control-Allow-Origin')) print("Access-Control-Allow-Methods:", response.headers.get('Access-Control-Allow-Methods')) print("Access-Control-Allow-Headers:", response.headers.get('Access-Control-Allow-Headers')) print("Access-Control-Allow-Credentials:", response.headers.get('Access-Control-Allow-Credentials'))`

This Python code sends an `OPTIONS` request to the specified URL and checks the headers. You can analyze the CORS headers in the response to detect potential vulnerabilities.

6. **Manual Exploitation of CORS Vulnerabilities**

If you detect a CORS vulnerability via the terminal, you can attempt exploitation by performing the following actions:

- If the server's `Access-Control-Allow-Origin` header is set to `*`, you can send requests to the API from a different origin (e.g., from another webpage).
- Attempt unauthorized access to the API by including authentication credentials (cookies or authorization headers).
- Using JavaScript in the browser, you can make requests to the target API via a malicious webpage, potentially stealing user data.

However, you must only conduct these tests in a legal and authorized environment when performing security assessments on a target web application.