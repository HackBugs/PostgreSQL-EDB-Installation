> # Install software - [NODEJS](https://nodejs.org/en/download)

```
Install Nodejs

npm install express pg dotenv
npm install express morgan helmet compression ejs
```

> # 1. Create these table and permission / grant


## 1. Step

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
## 2. Step

```
GRANT ALL ON server_status, jobs, alerts TO enterprisedb;
```

## 3. Step

```
-- Table: public.alerts

-- DROP TABLE public.alerts;

CREATE TABLE IF NOT EXISTS public.alerts
(
    id integer NOT NULL DEFAULT nextval('alerts_id_seq'::regclass),
    message character varying(255) COLLATE pg_catalog."default" NOT NULL,
    severity character varying(50) COLLATE pg_catalog."default" NOT NULL,
    created_at timestamp without time zone DEFAULT current_timestamp,
    CONSTRAINT alerts_pkey PRIMARY KEY (id)
)

TABLESPACE pg_default;

ALTER TABLE public.alerts
    OWNER to enterprisedb;

GRANT ALL ON TABLE public.alerts TO enterprisedb;
```

## 4. step

```
psql -U enterprisedb -h 192.168.1.142 -p 5444 -d test_website
alter_tables.sql
```
