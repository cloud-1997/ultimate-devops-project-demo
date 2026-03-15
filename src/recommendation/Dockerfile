FROM python:3.12-slim-bookworm AS base

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN pip install --upgrade pip
RUN pip install -r requirements.txt

COPY . .

# Debug step: List installed packages to verify setuptools and wheel
RUN pip list

# Ensure setuptools and wheel are installed in the correct environment
RUN pip install --no-cache-dir --upgrade setuptools wheel

# Verify setuptools version to ensure it is installed correctly
RUN python -m pip show setuptools && python -m pip --version

# Reinstall setuptools to ensure pkg_resources is available
RUN pip install --force-reinstall --no-cache-dir setuptools

# Install system dependencies required for Python packages
RUN apt-get update && apt-get install -y \
    gcc \
    libffi-dev \
    python3-dev \
    --no-install-recommends && rm -rf /var/lib/apt/lists/*

# Reinitialize Python environment to ensure all dependencies are available
RUN python -m ensurepip --upgrade && pip install --no-cache-dir --upgrade setuptools wheel

# Verify if pkg_resources is available
RUN python -c "import pkg_resources"

# Run the OpenTelemetry bootstrap command
RUN opentelemetry-bootstrap -a install

# Update ENV instruction to modern format
ENV RECOMMENDATION_PORT=1010

ENTRYPOINT ["python", "recommendation_server.py"]
