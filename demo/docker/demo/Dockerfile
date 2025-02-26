FROM continuumio/miniconda3
MAINTAINER JP Swinski (jp.swinski@nasa.gov)

# Environment
ENV PYTHONPATH=/usr/local/lib

# Install SlideRule client
COPY environment.yml /environment.yml
RUN conda env create -f environment.yml

# Make RUN commands use the new environment:
SHELL ["conda", "run", "-n", "sliderule_env", "/bin/bash", "-c"]
RUN conda install -c conda-forge voila

# Install latest ipyleaflet
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
  build-essential \
  && rm -rf /var/lib/apt/lists/*
RUN conda install -c conda-forge -y yarn jupyter_packaging geopandas
RUN cd /tmp && git clone https://github.com/jupyter-widgets/ipyleaflet.git
RUN cd /tmp/ipyleaflet && \
    pip install -e . && \
    jupyter labextension develop . --overwrite
RUN cd /tmp/ipyleaflet/js && \
    yarn add webpack webpack-dev-server svg-url-loader --dev && \
    yarn run build

# Install Voila Demo
COPY voila_demo.ipynb /voila_demo.ipynb

# Entry point
COPY docker-entrypoint.sh /usr/local/etc/
ENTRYPOINT ["/bin/bash"]