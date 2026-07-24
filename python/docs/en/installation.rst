Installation Guide
==================

System Requirements
-------------------

- Python 3.8 or higher
- Linux ARM64 architecture (Hailo-15, RK3588, Jetson)
- AIPC Platform runtime environment

Dependencies
------------

The SDK depends on the following Python packages:

- ``grpcio >= 1.50.0`` - gRPC communication
- ``protobuf >= 4.21.0`` - Protocol Buffers
- ``numpy >= 1.20.0`` - Array processing
- ``Pillow >= 9.0.0`` - Image processing

Installation Methods
--------------------

Install from PyPI (Recommended)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   pip install hailo-ipc-sdk

Install from Source
~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   git clone https://github.com/camthink-ai/ne503-aipc-sdks.git
   cd ne503-aipc-sdks/python
   pip install -e .

Build a Wheel
~~~~~~~~~~~~~

.. code-block:: bash

   git clone https://github.com/camthink-ai/ne503-aipc-sdks.git
   cd ne503-aipc-sdks/python
   python -m pip install --upgrade build
   python -m build --wheel
   ls dist/*.whl

The generated ``.whl`` file is written to ``dist/``. Install it locally to
verify the artifact:

.. code-block:: bash

   pip install dist/hailo_ipc_sdk-*.whl

Install from Tarball
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   # Download the SDK package
   wget https://github.com/camthink-ai/ne503-aipc-sdks/releases/download/v0.3.0/ne503-aipc-sdk-0.3.0-arm64.tar.gz

   # Extract and install
   tar -xzf aipc-sdk-0.2.0-arm64.tar.gz
   cd aipc-sdk-0.2.0-arm64
   pip install .

Verify Installation
-------------------

.. code-block:: python

   import hailo_ipc_sdk
   print(hailo_ipc_sdk.__version__)
   # Output: 0.2.0

Development Environment
-----------------------

If you need to develop or test the SDK, install the development dependencies:

.. code-block:: bash

   pip install -e ".[dev]"

This installs additional tools:

- ``pytest`` - Unit testing
- ``pytest-cov`` - Test coverage
- ``black`` - Code formatting
- ``flake8`` - Code linting
- ``mypy`` - Type checking

Docker Environment
------------------

Using a pre-built Docker image:

.. code-block:: bash

   docker pull registry.local/aipc-sdk:0.2.0
   docker run -it --rm registry.local/aipc-sdk:0.2.0 python3

Or build your own image:

.. code-block:: dockerfile

   FROM python:3.10-slim
   RUN pip install hailo-ipc-sdk
   WORKDIR /app
   COPY app.py .
   CMD ["python3", "app.py"]

Troubleshooting
---------------

Permission Issues
~~~~~~~~~~~~~~~~~

If you encounter Unix socket permission errors:

.. code-block:: bash

   # Ensure the user is in the aipc group
   sudo usermod -aG aipc $USER

   # Re-login or refresh the group
   newgrp aipc

gRPC Connection Failure
~~~~~~~~~~~~~~~~~~~~~~~

Check if platform services are running:

.. code-block:: bash

   # Check service status
   systemctl status ai-runtime
   systemctl status event-bus
   systemctl status device-control

   # Check socket files
   ls -l /run/aipc/*.sock

Dependency Conflicts
~~~~~~~~~~~~~~~~~~~~

If you encounter dependency version conflicts, use a virtual environment:

.. code-block:: bash

   python3 -m venv venv
   source venv/bin/activate
   pip install hailo-ipc-sdk
