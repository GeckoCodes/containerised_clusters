# Custom image for a Gecko Cluster

# Firstly want to use databricks runtime > 9.x
FROM databricksruntime/standard:9.x

# Copy the directory to the image to allow for the custom packages to be installed
COPY . /app

# Set the working directory
WORKDIR /app

# Now want to install any further libraries
RUN /databricks/python3/bin/pip install -r requirements.txt

