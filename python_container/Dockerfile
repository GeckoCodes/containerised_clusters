# Custom image for a cluster with Python Libraries only - very simple, just need to add libraries to requirements

# Firstly want to use databricks runtime > 9.x
FROM databricksruntime/standard:9.x

# Copy the directory to the image
COPY . /app

# Set the working directory
WORKDIR /app

# Now want to install any further libraries
RUN /databricks/python3/bin/pip install -r requirements.txt