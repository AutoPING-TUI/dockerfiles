FROM openhpi/docker_exec_phusion

LABEL org.opencontainers.image.vendor="AutoPING Team"
LABEL org.opencontainers.image.authors="daniel.fischer@tu-ilmenau.de"
LABEL org.opencontainers.image.title="Python 3.12 execution environment for CodeOcean"
LABEL org.opencontainers.image.description="This image provides a Python 3.12 environment with custom JSON message output and turtle support."
# LABEL org.opencontainers.image.url="https://github.com/openHPI/dockerfiles"

LABEL run_command="python3 -B /usr/lib/python3.12/webpython.py -f %{filename}"
LABEL test_command="python3 -B -m unittest --verbose %{module_name}"

# --------------------------------------------------
# Install Python 3.12
# --------------------------------------------------
RUN add-apt-repository -y ppa:deadsnakes/ppa --no-update

RUN install_clean \
    python3.12 \
    python3.12-dev \
    python3.12-venv \
    python3-pip \
    python3-tk

# --------------------------------------------------
# Set Python 3.12 as default
# --------------------------------------------------
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.12 1

RUN update-alternatives --set python3 /usr/bin/python3.12

# --------------------------------------------------
# Bootstrap pip
# --------------------------------------------------
RUN python3.12 -m ensurepip --upgrade || true

# --------------------------------------------------
# Install Python packages
# --------------------------------------------------
RUN python3.12 -m pip install \
    --break-system-packages \
    --no-cache-dir \
    pylint==3.2.5 \
    bitstring \
    requests \
    py-sudoku \
    matplotlib \
    numpy \
    beautifulsoup4 \
    pandas

# --------------------------------------------------
# Environment variables
# --------------------------------------------------
ENV PYTHONPATH=$PYTHONPATH:/usr/lib/python3.12:/workspace
ENV PATH=/usr/lib/python3.12:$PATH

# --------------------------------------------------
# Add custom files
# --------------------------------------------------
ADD assess.py /usr/lib/python3.12/assess.py

ADD webpython.py /usr/lib/python3.12/webpython.py

# --------------------------------------------------
# Replace turtle implementation
# --------------------------------------------------
RUN rm -f /usr/lib/python3.12/turtle.py

ADD turtle.py /usr/lib/python3.12/turtle.py