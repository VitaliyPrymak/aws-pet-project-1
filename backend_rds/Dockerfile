# Use the official Python 3.9 image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /rdsapp

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    && apt-get clean

# Copy the requirements file into the container
COPY requirements.txt /rdsapp/

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the current directory contents into the container
COPY . /rdsapp/

# Expose the application port
EXPOSE 8000

# Run the Django server
CMD ["sh", "-c", "sleep 10 && python manage.py runserver 0.0.0.0:8000"]