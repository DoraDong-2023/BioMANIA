
FROM nvidia/cuda:12.1.0-devel-ubuntu22.04
# Install bash and basic network tools
RUN apt-get update && apt-get install -y bash
RUN apt-get update && apt-get install -y net-tools iputils-ping curl
RUN apt-get update && apt-get install -y python3 python3-pip

WORKDIR /app

COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

COPY . .
COPY LICENSE /app/LICENSE

ENV LIB=scanpy
ENV CUDA_VISIBLE_DEVICES=0
ENV PYTHONPATH="/app:${PYTHONPATH}"

EXPOSE 5000

CMD sh -c "LIB=${LIB} CUDA_VISIBLE_DEVICES=${CUDA_VISIBLE_DEVICES} python3 -m deploy.inference_dialog_server"

