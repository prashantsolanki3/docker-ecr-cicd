ARG ECR_REGISTRY
ARG BASE_IMAGE_TAG=latest
FROM ${ECR_REGISTRY}/lambda:${BASE_IMAGE_TAG}

# Copy App2's Lambda handler code
COPY lambda_function.py ${LAMBDA_TASK_ROOT}/

# (Optional) install App2-specific requirements if any
# COPY requirements.txt .
# RUN pip install -r requirements.txt

# Set the handler command for App2's function
CMD ["lambda_function.handler"]