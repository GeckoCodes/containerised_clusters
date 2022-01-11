## For a cluster which requires both R and Python libraries we need to build a custom image from 
# the Python and R base images 

FROM databricksruntime/minimal:9.x

# Suppress interactive configuration prompts
ENV DEBIAN_FRONTEND=noninteractive

# We add RStudio's debian source to install the latest r-base version (4.1)
# We are using the more secure long form of pgp key ID of marutter@gmail.com
# based on these instructions (avoiding firewall issue for some users):
# https://cran.rstudio.com/bin/linux/ubuntu/#secure-apt
RUN apt-get update \
  && apt-get install --yes software-properties-common apt-transport-https \
  && gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 \
  && gpg -a --export E298A3A825C0D65DFD57CBB651716619E084DAB9 | sudo apt-key add - \
  && add-apt-repository -y 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu bionic-cran40/' \
  && apt-get update \
  && apt-get install --yes \
  libssl-dev \
  r-base \
  r-base-dev \
  && add-apt-repository -r 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu bionic-cran40/' \
  && apt-key del E298A3A825C0D65DFD57CBB651716619E084DAB9 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# hwriterPlus is used by Databricks to display output in notebook cells
# Rserve allows Spark to communicate with a local R process to run R code

# Additional instructions to setup rstudio. If you dont need rstudio, you can 
# omit the below commands in your docker file. Even after this you need to use
# an init script to start the RStudio daemon (See README.md for details.)

# Databricks configuration for RStudio sessions.
# COPY Rprofile.site /usr/lib/R/etc/Rprofile.site

# Rstudio installation.
RUN apt-get update \
  # Installation of rstudio in databricks needs /usr/bin/python.
  && apt-get install -y python \
  # Install gdebi-core.
  && apt-get install -y gdebi-core \
  # Download rstudio 1.2 package for ubuntu 18.04 and install it.
  && apt-get install -y wget \
  && wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.2.5042-amd64.deb -O rstudio-server.deb \
  && gdebi -n rstudio-server.deb \
  && rm rstudio-server.deb

# Installs python 3.8 and virtualenv for Spark and Notebooks
RUN apt-get update \
  && apt-get install -y \
  python3.8 \
  virtualenv \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Initialize the default environment that Spark and notebooks will use
RUN virtualenv -p python3.8 --system-site-packages /databricks/python3

# These python libraries are used by Databricks notebooks and the Python REPL
# You do not need to install pyspark - it is injected when the cluster is launched
# Versions are intended to reflect DBR 9.0

# Copy the directory to the image
COPY . /app

# Set the working directory
WORKDIR /app

# Install the python requirements
RUN /databricks/python3/bin/pip install -r requirements.txt

# Install the R requirements
RUN ./r_packages.sh

# Specifies where Spark will look for the python process
ENV PYSPARK_PYTHON=/databricks/python3/bin/python3