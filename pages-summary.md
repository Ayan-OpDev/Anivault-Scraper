# Application Pages Summary

This application is primarily a backend REST API (AniVault API) and operates as a Single-Page Application (SPA) on the frontend. 

Below is the summary of the frontend pages available:

## `/public/index.html` (API Docs & Tester)
This is the sole frontend page of the application, serving as a comprehensive documentation hub and interactive testing suite for the AniVault API. 

**Key Features:**
- **Navigation Sidebar:** Categorizes and lists all available API endpoints for easy access.
- **Endpoint Documentation:** Provides detailed information on HTTP methods, paths, and required/optional query parameters for each endpoint.
- **Interactive Tester:** Allows developers to input parameters and execute real API requests directly from the browser.
- **Response Previews:** Includes a built-in JSON viewer for data responses, image previews for thumbnail endpoints, and an embedded video player to test HLS streaming links securely.
- **Code Snippets:** Generates ready-to-use fetch code snippets for endpoints.

*(Note: All unrecognized URL routes are caught by the server and redirected to this `index.html` page to ensure the documentation is always accessible).*
