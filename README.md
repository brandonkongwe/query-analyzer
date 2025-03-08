# query-analyzer

This application allows users to input SQL queries, execute them against a backend API, and view query results, execution times, and optimization suggestions. The backend can be found here: https://github.com/brandonkongwe/QueryAnalyzer

## Features

- Input SQL queries and execute them.
- View query results in a user-friendly format.
- Display query execution time.
- Show query execution plans and optimization suggestions.

## Technologies

- Frontend Framework: Vue.js
- UI Library: None (plain Vue.js)
- HTTP Client: Axios
- Backend Integration: ASP.NET Core Web API

## Setup

### Prerequisites

1. Node.js: Install [Node.js](https://nodejs.org/en)

2. Backend API: Ensure the [API](https://github.com/brandonkongwe/QueryAnalyzer) is running and accessible.

## Installation

1. Clone the repository:
   
    ```bash
    git clone https://github.com/brandonkongwe/query-analyzer.git
    cd query-analyzer
    ```

2. Install dependencies:

   ```bash
    npm install
    npm install axios
   ```

3. Configure the backend API URL:
    Open the .env file (or create one if it doesn’t exist) and set the backend API URL:
   
     ```bash
      VITE_API_URL=http://localhost:5209/api/query/execute
     ```

5. Run the application:

    ```bash
    npm run dev
    ```


## Strucuture

```bash
query-analyzer/
├── public/
│   └── index.html                 # Main HTML file
├── src/
│   ├── assets/                    # Static assets (e.g., images, styles)
│   ├── components/                # Vue components
│   ├── App.vue                    # Root Vue component
│   └── main.js                    # Application entry point
├── .env                           # Environment variables
├── package.json                   # Project dependencies
└── README.md                      # Project documentation
```

### Main Components

1. App.vue:

    - The root component that handles user input, query execution, and result display.

2. Axios Integration:

    - Used to send HTTP requests to the backend API.

3. Environment Variables:

    - The backend API URL is configured in the .env file.
        
## Usage
1. Input SQL Query
    
    Enter your SQL query in the text area provided on the homepage.

2. Execute Query

    Click the Execute Query button to send the query to the backend API.

3. View Results

    The query results, execution time, and optimization suggestions will be displayed below the input area.
