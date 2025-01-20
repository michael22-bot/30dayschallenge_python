
1. Understanding HTTP Methods
HTTP (Hypertext Transfer Protocol) defines several methods (also called "verbs") that indicate the desired action to be performed on a resource. The most common HTTP methods are:

GET: Used to request data from a specified resource. It doesn't modify the data on the server.
POST: Sends data to the server to create a new resource. It's typically used for submitting form data.
PUT: Sends data to the server to update an existing resource or create it if it doesn't exist.
DELETE: Requests to delete a specified resource.
2. Using Python's requests Library
Python’s requests library allows you to send HTTP requests and handle responses easily. It supports the most common HTTP methods (GET, POST, PUT, DELETE) and much more.

To use the requests library, you first need to install it if you haven't already:

bash
Copy
pip install requests
3. Script to Fetch a Webpage and Display the Status Code and Headers
Let’s write a Python script that will:

Make a GET request to a URL.
Fetch the status code and headers from the response.
Display the results.
python
Copy
import requests

# URL to request
url = 'https://www.example.com'

# Send GET request
response = requests.get(url)

# Display the status code
print("Status Code:", response.status_code)

# Display the headers of the response
print("\nHeaders:")
for key, value in response.headers.items():
    print(f"{key}: {value}")
How This Works:
requests.get(url): Sends a GET request to the URL.
response.status_code: Accesses the status code of the response, which indicates the result of the HTTP request (e.g., 200 for success, 404 for not found).
response.headers: Contains the HTTP headers returned by the server. It’s a dictionary that contains key-value pairs where the keys are the header names and the values are the corresponding values.
Example Output:
plaintext
Copy
Status Code: 200

Headers:
Date: Sun, 20 Jan 2025 10:00:00 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 1256
Connection: keep-alive
4. HTTP Request Methods in Action
Here’s how you can use different HTTP methods with the requests library:

GET: Retrieve data from the server.
python
Copy
response = requests.get('https://jsonplaceholder.typicode.com/posts/1')
print(response.json())  # Prints the content of the response in JSON format.
POST: Send data to the server.
python
Copy
data = {'title': 'foo', 'body': 'bar', 'userId': 1}
response = requests.post('https://jsonplaceholder.typicode.com/posts', json=data)
print(response.status_code)
print(response.json())
PUT: Update data on the server.
python
Copy
data = {'id': 1, 'title': 'Updated Title', 'body': 'Updated Content', 'userId': 1}
response = requests.put('https://jsonplaceholder.typicode.com/posts/1', json=data)
print(response.status_code)
print(response.json())
DELETE: Delete a resource.
python
Copy
response = requests.delete('https://jsonplaceholder.typicode.com/posts/1')
print(response.status_code)
Summary:
HTTP methods define the type of action you want to perform on a resource.
Python’s requests library makes it easy to send HTTP requests and handle responses.
You can use methods like GET, POST, PUT, and DELETE to interact with APIs or web pages.
Let me know if you need further clarification or additional examples!



