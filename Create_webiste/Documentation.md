## Note

## ✅ Step-by-Step: Setup and Run Node Express App (on localhost)

```
node -v
npm -v
npm init -y
npm install express
npm install ejs
npm install -g nodemon
node app.js

npm cache clean --force
npm install pg
npm init -y
npm install pg express morgan helmet compression ejs

```

```
ping 192.168.43.142
Test-NetConnection 192.168.43.142 -Port 5444
sudo ufw allow 5432/tcp
sudo firewall-cmd --permanent --add-port=5444/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports
```


## Install nodejs for Windows
- [Downlaod link](https://nodejs.org/en/download)

## Run this cmd inside webiste folder 
```
npm install
npm init -y

npm install express pg ejs
```

## Run nodejs server with this cmd 
```
node app.js
```

## To use this enhanced version of your application, you'll need to install a few additional packages:

```
npm install morgan helmet compression
```

> # Server & Jobs Dashboard Documentation
## Overview
### This documentation provides a comprehensive guide to the Server & Jobs Dashboard application, which displays server status and job information from a PostgreSQL database. The application is built using Node.js, Express, and EJS templates.

## Project Structure
```
server-status-website/
├── app.js                 # Main server file
├── package.json           # Project dependencies
├── views/                 # EJS templates
│   ├── index.ejs          # Main dashboard view
│   └── error.ejs          # Error page
└── public/                # Static assets
    └── css/
        └── style.css      # Stylesheet

```

### Database Schema
### Server Status Table
```
CREATE TABLE server_status (
    id SERIAL PRIMARY KEY,
    hostname VARCHAR(100) NOT NULL,
    ip_address VARCHAR(45) NOT NULL,
    uptime INTERVAL,
    service_running_postgres BOOLEAN DEFAULT FALSE
);
```
```
CREATE TABLE jobs (
    id SERIAL PRIMARY KEY,
    hostname VARCHAR(100) NOT NULL,
    ip_address VARCHAR(45) NOT NULL,
    uptime INTERVAL,
    running BOOLEAN DEFAULT FALSE
);

```

## Setup Instructions
### 1. Create Project Structure

```
mkdir server-status-website
cd server-status-website
npm init -y
mkdir views
mkdir public
mkdir public/css
```

## 2. Install Dependencies
```
npm install express pg ejs
```

## 3. Database Setup
### Connect to your PostgreSQL database and create the required tables:
```
psql -U enterprisedb -h 192.168.43.142 -p 5444 -d test_website
```

## Execute the following SQL commands:
```
-- Create server_status table
CREATE TABLE server_status (
    id SERIAL PRIMARY KEY,
    hostname VARCHAR(100) NOT NULL,
    ip_address VARCHAR(45) NOT NULL,
    uptime INTERVAL,
    service_running_postgres BOOLEAN DEFAULT FALSE
);

-- Insert sample data
INSERT INTO server_status (hostname, ip_address, uptime, service_running_postgres)
VALUES ('db-server-1', '192.168.1.10', '5 days 3 hours', TRUE);

-- Create jobs table
CREATE TABLE jobs (
    id SERIAL PRIMARY KEY,
    hostname VARCHAR(100) NOT NULL,
    ip_address VARCHAR(45) NOT NULL,
    uptime INTERVAL,
    running BOOLEAN DEFAULT FALSE
);

-- Insert sample data
INSERT INTO jobs (hostname, ip_address, uptime, running)
VALUES 
    ('job-server-1', '192.168.1.20', '2 days 6 hours', TRUE),
    ('job-server-2', '192.168.1.21', '5 days 12 hours', TRUE),
    ('job-server-3', '192.168.1.22', '1 day 3 hours', FALSE),
    ('job-server-4', '192.168.1.23', '10 days 8 hours', TRUE),
    ('job-server-5', '192.168.1.24', '3 hours 45 minutes', FALSE);

```

## 4. Create Application Files

### Main Server File (app.js)

```
const express = require('express');
const { Pool } = require('pg');
const app = express();
const port = 3000;

// Database connection
const pool = new Pool({
  user: 'enterprisedb',
  host: '192.168.43.142',
  database: 'test_website',
  password: 'admin@123',
  port: 5444,
});

// Set view engine
app.set('view engine', 'ejs');
app.use(express.static('public'));

// Routes
app.get('/', async (req, res) => {
  try {
    // Get both server status and jobs data
    const serverResult = await pool.query('SELECT * FROM server_status');
    const jobsResult = await pool.query('SELECT * FROM jobs');
    
    res.render('index', { 
      servers: serverResult.rows,
      jobs: jobsResult.rows
    });
  } catch (err) {
    console.error('Database query error:', err);
    res.status(500).render('error', { error: 'Failed to fetch data: ' + err.message });
  }
});

// Start server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});

```


## Main View Template (views/index.ejs)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Server & Jobs Dashboard</title>
    <link rel="stylesheet" href="/css/style.css">
</head>
<body>
    <div class="container">
        <h1>Server & Jobs Dashboard</h1>
        
        <h2>Server Status</h2>
        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Hostname</th>
                    <th>IP Address</th>
                    <th>Uptime</th>
                    <th>PostgreSQL Service</th>
                </tr>
            </thead>
            <tbody>
                <% servers.forEach(server => { %>
                <tr>
                    <td><%= server.id %></td>
                    <td><%= server.hostname %></td>
                    <td><%= server.ip_address %></td>
                    <td><%= server.uptime %></td>
                    <td class="<%= server.service_running_postgres ? 'running' : 'stopped' %>">
                        <%= server.service_running_postgres ? 'Running' : 'Stopped' %>
                    </td>
                </tr>
                <% }); %>
            </tbody>
        </table>
        
        <h2>Jobs Status</h2>
        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Hostname</th>
                    <th>IP Address</th>
                    <th>Uptime</th>
                    <th>Status</th>
                </tr>
            </thead>
            <tbody>
                <% jobs.forEach(job => { %>
                <tr>
                    <td><%= job.id %></td>
                    <td><%= job.hostname %></td>
                    <td><%= job.ip_address %></td>
                    <td><%= job.uptime %></td>
                    <td class="<%= job.running ? 'running' : 'stopped' %>">
                        <%= job.running ? 'Running' : 'Stopped' %>
                    </td>
                </tr>
                <% }); %>
            </tbody>
        </table>
    </div>
</body>
</html>
```

## Error View Template (views/error.ejs)

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Error</title>
    <link rel="stylesheet" href="/css/style.css">
</head>
<body>
    <div class="container error">
        <h1>Error</h1>
        <p><%= error %></p>
        <a href="/">Go back to homepage</a>
    </div>
</body>
</html>

Stylesheet (public/css/style.css)

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

.container {
    width: 80%;
    margin: 30px auto;
    overflow: auto;
    padding: 20px;
    background: white;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
    color: #333;
    text-align: center;
    margin-bottom: 30px;
}

h2 {
    color: #444;
    margin-top: 30px;
    margin-bottom: 15px;
    border-bottom: 1px solid #ddd;
    padding-bottom: 10px;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 30px;
}

table, th, td {
    border: 1px solid #ddd;
}

th, td {
    padding: 12px 15px;
    text-align: left;
}

th {
    background-color: #f2f2f2;
    font-weight: bold;
}

tr:nth-child(even) {
    background-color: #f9f9f9;
}

tr:hover {
    background-color: #f1f1f1;
}

.running {
    color: green;
    font-weight: bold;
}

.stopped {
    color: red;
    font-weight: bold;
}

.error {
    text-align: center;
    color: #d9534f;
}

a {
    display: inline-block;
    margin-top: 20px;
    padding: 10px 15px;
    background: #4CAF50;
    color: white;
    text-decoration: none;
    border-radius: 5px;
}

a:hover {
    background: #45a049;
}
```


## 5. Run the Application
```
node app.js
```
