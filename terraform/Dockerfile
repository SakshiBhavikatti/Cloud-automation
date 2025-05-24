#docker build -t terraform-container .
#docker run --rm terraform-container

FROM hashicorp/terraform:1.6.0

WORKDIR /workspace
COPY . .
ENTRYPOINT []
CMD ["sh", "-c", "terraform init && terraform apply -auto-approve"]


