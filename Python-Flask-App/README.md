# Deploying a Simple Flask App with PostgreSQL using Docker Compose

## 🚀 Objective

This project demonstrates how to containerize a Python Flask application with PostgreSQL using Docker Compose. The Flask app will be deployed as a web server, while PostgreSQL serves as the database. Additionally, pgAdmin is included for managing the PostgreSQL database.

## 🧰 Overview

- **Flask App**: A simple web application that returns a greeting message.
- **PostgreSQL**: Acts as the backend database for the Flask application.
- **pgAdmin**: A web-based interface to manage the PostgreSQL database.

## 🧩 Prerequisites

Before you begin, ensure you have the following:

- **Docker and Docker Compose** installed on your local machine or EC2 instance.
- Basic knowledge of **Flask** and **PostgreSQL**.
- **Security Groups**: Ensure ports **5000**, **5432**, and **8080** are open for inbound traffic in the EC2 instance security group.

## 📁 Project Structure

Here's an overview of the project directory structure:

├── app.py # Flask application logic ├── Dockerfile # Instructions to build the Flask app Docker image ├── docker-compose.yml # Configuration for Flask, PostgreSQL, and PgAdmin services └── requirements.txt # Flask and dependency versions

### File Descriptions

- **app.py**: Contains the logic for the Flask application, including route definitions.
- **Dockerfile**: Specifies how to build the Docker image for the Flask app.
- **docker-compose.yml**: Defines and configures the services for Flask, PostgreSQL, and pgAdmin.
- **requirements.txt**: Lists the required Python packages and their versions.

## ⚙️ Step-by-Step Guide

### 1. Create `app.py`

Create a simple Flask app by adding the following content in `app.py`:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, Flask with PostgreSQL!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

Why app.py?
**Starts the Flask App**: Runs the web server when the container starts.
**Defines Routes**: Returns a greeting message when accessing the root URL.
**Runs the Server**: Makes the app accessible externally.

2. Create requirements.txt
Add the following lines to requirements.txt:

Flask==2.0.1
Werkzeug==2.0.1

3. Create the Dockerfile
Add the following content to the Dockerfile:

# Use the official Python image
FROM python:3.9

# Set the working directory
WORKDIR /app

# Copy the requirements file
COPY requirements.txt .

# Install the required packages
RUN pip install -r requirements.txt

# Copy the application code
COPY . .

# Command to run the app
CMD ["python", "app.py"]

4. Create docker-compose.yml
Add the following content to docker-compose.yml:

version: '3'
services:
  web:
    build: .
    image: flask-app
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"

  pgadmin:
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "8080:80"
    depends_on:
      - db

5. Build and Run the Application
Run the following command to deploy the application using Docker Compose:

sudo docker-compose up --build

6. Verify Running Containers

## Check that all services are running:

sudo docker ps

🌐 Testing the Flask App
Access the Flask application by visiting http://<EC2-Public-IP>:5000 in your web browser. You should see:

"C:\Users\sandh\Downloads\a02761ad-2e82-4982-a921-abd161be3379.png"

Hello, Flask with PostgreSQL!

🗃️ Testing PostgreSQL Locally (pgAdmin)
Open your browser and go to http://<your-ec2-public-ip>:8080. Log in with the following credentials:

Email: admin@example.com
Password: admin
Add a new server in pgAdmin with these details:

Host: db
Username: user
Password: password

"C:\Users\sandh\Downloads\a6f811ad-fde3-425d-9aac-8933855ddef9.png"

"C:\Users\sandh\Downloads\a100ed24-3b3e-42d2-aebb-82e94b65a968.png"

 🎯 Conclusion
You have successfully deployed a Python Flask app with PostgreSQL using Docker Compose. This project illustrates how to containerize multiple services and link them effectively using Docker Compose. It serves as a solid foundation for developing and testing scalable applications.




