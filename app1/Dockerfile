# Use the base Lambda image built from Dockerfile.lambda
# The actual values for ECR_REGISTRY and BASE_IMAGE_TAG will be passed in via build args.
ARG ECR_REGISTRY
ARG BASE_IMAGE_TAG=latest
FROM ${ECR_REGISTRY}/lambda:${BASE_IMAGE_TAG}

# Copy App1's specific code into the Lambda task directory.
# Assume there's a lambda_function.py in the app1 directory with a function 'handler'.
COPY lambda_function.py ${LAMBDA_TASK_ROOT}/

# (Optional) If App1 has additional dependencies, you could copy a requirements.txt and install here.
# COPY requirements.txt .
# RUN pip install -r requirements.txt

# Set the Lambda handler command for App1.
# Here we assume the handler function is lambda_function.handler (filename.function_name).
CMD ["lambda_function.handler"]