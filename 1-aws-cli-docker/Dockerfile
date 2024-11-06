FROM public.ecr.aws/amazonlinux/amazonlinux:2 as awscli_builder
RUN yum update -y \
  && yum install -y unzip curl \
  && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscli-exe-linux-x86_64.zip" \
  && unzip awscli-exe-linux-x86_64.zip \
  && ./aws/install --bin-dir /aws-cli-bin/ \
  && rm -rf aws awscli-exe-linux-x86_64.zip \
  && yum clean all \
  && rm -rf /var/cache/yum

FROM debian:bookworm-slim
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    less \
    groff \
    ca-certificates \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

COPY --from=awscli_builder /usr/local/aws-cli/ /usr/local/aws-cli/
COPY --from=awscli_builder /aws-cli-bin/ /usr/local/bin/

ENTRYPOINT ["/usr/local/bin/aws"]
