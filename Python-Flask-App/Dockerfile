# Use the official Python image
FROM python:3.9

# Set the working directory inside the container
WORKDIR /app

# Copy the requirements file into the container
COPY requirements.txt /app/

# Install the dependencies
RUN pip install -- no-cache-dir -r requirements.txt

# Copy the current directory into the container
COPY . /app/

# Run the application
CMD ["python", "app.py"]
