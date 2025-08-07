FROM python:3.9-slim
WORKDIR /app
COPY ../app/ .
RUN pip install flask prometheus_flask_exporter
CMD ["python", "app.py"]