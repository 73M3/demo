# Documentation: `LinkLister` Class

## Overview

The `LinkLister` class provides functionality to extract hyperlinks (`<a>` tags) from a given URL. It includes two methods: one for general link extraction and another with additional validation to prevent the use of private IP addresses.

---

## Class: `LinkLister`

### Purpose
The `LinkLister` class is designed to fetch and process hyperlinks from a web page. It uses the `Jsoup` library for HTML parsing and provides two methods for link extraction:
- `getLinks`: Extracts all hyperlinks from a given URL.
- `getLinksV2`: Adds validation to block URLs that resolve to private IP addresses.

---

## Methods

### 1. `getLinks(String url)`

#### Description
Fetches all hyperlinks (`<a>` tags) from the provided URL and returns them as a list of absolute URLs.

#### Parameters
| Name | Type   | Description                     |
|------|--------|---------------------------------|
| `url` | `String` | The URL of the web page to parse. |

#### Returns
| Type          | Description                          |
|---------------|--------------------------------------|
| `List<String>` | A list of absolute URLs extracted from the web page. |

#### Exceptions
| Exception      | Description                                      |
|----------------|--------------------------------------------------|
| `IOException`  | Thrown if there is an issue connecting to the URL or parsing the document. |

#### Logic
1. Connects to the given URL using `Jsoup.connect(url).get()`.
2. Selects all `<a>` elements from the HTML document.
3. Extracts the absolute URL (`absUrl("href")`) for each `<a>` tag.
4. Returns the list of extracted URLs.

---

### 2. `getLinksV2(String url)`

#### Description
Fetches all hyperlinks from the provided URL but includes additional validation to block URLs that resolve to private IP addresses.

#### Parameters
| Name | Type   | Description                     |
|------|--------|---------------------------------|
| `url` | `String` | The URL of the web page to parse. |

#### Returns
| Type          | Description                          |
|---------------|--------------------------------------|
| `List<String>` | A list of absolute URLs extracted from the web page. |

#### Exceptions
| Exception      | Description                                      |
|----------------|--------------------------------------------------|
| `BadRequest`   | Thrown if the URL resolves to a private IP address or if any other error occurs during processing. |

#### Logic
1. Parses the URL using `java.net.URL` to extract the host.
2. Checks if the host starts with any of the following private IP address ranges:
   - `172.`
   - `192.168`
   - `10.`
3. If the host matches a private IP range, throws a `BadRequest` exception with the message "Use of Private IP".
4. Otherwise, calls the `getLinks` method to fetch and return the hyperlinks.

---

## Insights

### Dependencies
- **Jsoup Library**: Used for HTML parsing and extracting elements from the DOM.
- **java.net.URL**: Used for parsing and validating the host of the provided URL.

### Exception Handling
- The `getLinksV2` method introduces a custom exception (`BadRequest`) to handle invalid URLs, specifically those resolving to private IP addresses.
- The `getLinks` method relies on `IOException` for error handling during network operations.

### Security Considerations
- The `getLinksV2` method prevents the use of private IP addresses, which is a common security measure to avoid accessing internal or sensitive network resources.

### Use Cases
- **Web Scraping**: Extracting all hyperlinks from a web page for further processing.
- **URL Validation**: Ensuring that URLs do not point to private IP addresses before processing.

### Limitations
- The `getLinks` method does not perform any validation on the URL, which could lead to potential misuse or security risks.
- The `getLinksV2` method only checks for private IP addresses but does not validate other potentially unsafe URLs (e.g., localhost or loopback addresses).

---

## File Metadata

| Key         | Value              |
|-------------|--------------------|
| **File Name** | `LinkLister.java` |
